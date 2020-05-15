---
title: Spring Data Azure Cosmos DB 开发人员指南
description: 本指南介绍在使用 Spring Data Azure Cosmos DB SDK 时应注意的事项。
author: kushagraThapar
ms.author: kuthapar
ms.topic: conceptual
ms.date: 1/9/2019
ms.openlocfilehash: 838fb4efa79f5d3ef8a97977a0d239a809e2506d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81674283"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Spring Data Azure Cosmos DB 开发人员指南

本主题介绍使用 SQL API 的 [Spring Data Cosmos DB](https://github.com/microsoft/spring-data-cosmosdb) 的功能。 本主题还包括有关常见问题、解决方法和诊断步骤的指导。

[Azure Cosmos DB](/azure/cosmos-db/introduction) 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API 处理数据。 Spring Data Cosmos DB SDK 基于 [Spring Data](https://spring.io/projects/spring-data) 框架，并提供与使用 SQL API 的 Azure Cosmos DB 的集成。 以下主题介绍对其他 API 的支持：

- [如何将 Spring Data MongoDB API 用于 Azure Cosmos DB](/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
- [如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB](/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
- [如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用](/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)

Spring Data Cosmos DB SDK 在 GitHub 上的 [spring-data-cosmosdb](https://github.com/microsoft/spring-data-cosmosdb) 存储库中作为开放源代码提供。 此存储库有一个活动[问题](https://github.com/microsoft/spring-data-cosmosdb/issues)列表，你可以在其中提交 Bug 或检查已提交问题的解决方法。 你还可以检查[版本](https://github.com/microsoft/spring-data-cosmosdb/releases)列表，看是否已在更新的版本中解决了某个问题。 Spring Data Cosmos DB SDK 2.2.x 版的版本序列支持 spring-data-commons 版本 2.2.0.RELEASE，而该 SDK 的 2.1.x 版的版本序列则支持 spring-data-common 版本 2.1.0.RELEASE。

## <a name="available-features"></a>可用功能

以下部分介绍当前的可用功能。

### <a name="crudrepository-and-reactivecrudrepository-support"></a>CrudRepository 和 ReactiveCrudRepository 支持

Spring Data Cosmos DB SDK 提供 `CosmosRepository` 和 `ReactiveCosmosRepository` 接口，用于扩展 Spring Data 的 `CrudRepository` 和 `ReactiveCrudRepository` 接口。

以下示例演示如何扩展这些接口。

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {
    List<SampleEntity> findByName(String name);
}

@Repository
public interface ReactiveSampleRepository extends ReactiveCosmosRepository<SampleEntity, String> {
    Flux<SampleEntity> findByName(String name);
}
```

需要在 `Configuration` 类中单独启用这两个存储库，具体取决于使用情况。 例如：

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>定义简单实体

可以通过添加 `@Document` 批注并指定与集合相关的属性（如集合名称、请求单位 (RU)、生存时间和自动创建集合标志）来定义实体。

默认情况下，集合名称将是用户域类的类名。 若要对其进行自定义，请将 `@Document(collection="myCustomCollectionName")` 批注添加到域类中。 集合字段还支持 [Spring 表达式语言](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) (SpEL) 表达式，方便你通过配置属性以编程方式提供集合名称。 例如，可以使用 `collection = "${dynamic.collection.name}"` 和 `collection = "#{@someBean.getCollectionName()}"` 之类的表达式。

可以通过两种方式将域类中的字段映射到 Azure Cosmos DB 文档的 `id` 字段：

- 使用 `@Id` 批注字段。
- 将字段的名称设置为 `id`。

以下示例演示如何使用 `@Document` 和 `@Id` 批注。

```java
@Document(collection = "myCollection")
class MyDocument {

    @Id
    private String myId;

    @PartitionKey
    private String data;

    @Version
    private String _etag;
}
```

默认情况下，`IndexingPolicy` 由 Azure 服务设置。 若要对其进行自定义，请将批注 `@DocumentIndexingPolicy` 添加到域类中。 此批注有四个属性：

```java
boolean automatic;     // Indicates whether the indexing policy is automatic.
IndexingMode mode;     // The indexing policy mode; the options are Consistent, Lazy, or None.
String[] includePaths; // Included paths for indexing.
String[] excludePaths; // Excluded paths for indexing.
```

SDK 还支持分区。 有关详细信息，请参阅 [Azure Cosmos DB 中的分区和水平缩放](/azure/cosmos-db/partition-data)。 若要指定域类的字段作为分区键字段，请使用 `@PartitionKey` 来批注它。 然后，在执行 CRUD 操作时指定分区值。

以下示例演示如何在执行 CRUD 操作时使用 `@PartitionKey` 批注。

```java
@Document(ru = "400")
public class Address {
    @Id
    String postalCode;

    @PartitionKey
    String city;

    String street;
    String country;
    String phoneNumber;

    ...
}

class AddressService {

    @Autowired
    AddressRepository repository;

    final Address newAddress = new Address("12345", "city");

    // There's no need to specify a partition key in the save operation.
    repository.save(updatedAddress);

    // Provide a partition key when performing a find-by-id operation.
    final Optional<Address> addressById = repository.findById("12345", new PartitionKey("city"));

    final Address foundAddress = addressById.get();

    // Provide a partition key when performing a delete-by-id operation.
    repository.deleteById(foundAddress.getPostalCode(), new PartitionKey(foundAddress.getCity())); 
}
```

SDK 还支持 Spring Data 自定义查询查找操作，如 `findByAFieldAndBField`。 有关详细信息，请参阅 Spring 文档中的[定义查询方法](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.query-methods.details)。

## <a name="best-practices"></a>最佳做法

以下部分介绍了使用 SDK 时的最佳做法。

### <a name="configuring-the-application"></a>配置应用程序

请通过以下步骤来配置应用程序：

1. 扩展 `AbstractCosmosConfiguration` 类，以设置应用程序的配置（Cosmos DB 密钥、URL、数据库名称等）。
1. 添加 `@Configuration` 批注。
1. 根据存储库的使用情况，添加 `@EnableCosmosRepositories` 和/或 `@EnableReactiveCosmosRepositories` 批注。

可以利用 `CosmosKeyCredential` 功能动态轮换密钥。 可以使用 `switchToSecondaryKey` 方法来切换密钥。

以下示例代码演示了一项应用程序配置，并演示了如何使用 `switchToSecondaryKey`。

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    @Value("${azure.cosmosdb.uri}")
    private String uri;

    @Value("${azure.cosmosdb.key}")
    private String key;

    @Value("${azure.cosmosdb.secondaryKey}")
    private String secondaryKey;

    @Value("${azure.cosmosdb.database}")
    private String dbName;

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private CosmosKeyCredential cosmosKeyCredential;

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri,
            this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

还可以通过自定义配置来更改连接模式、连接池最大大小、请求超时等，如以下示例所示。

```java
public CosmosDBConfig getConfig() {

    this.cosmosKeyCredential = new CosmosKeyCredential(key);
    ConnectionPolicy customizedConnectionPolicy = new ConnectionPolicy();

    // Set the connection mode to Direct (TCP).
    customizedConnectionPolicy.setConnectionMode(ConnectionMode.DIRECT);

    // Set the maximum number of HTTP/TCP connections to 1000 per application.
    customizedConnectionPolicy.setMaxPoolSize(1000);

    // Set the request timeout to 10 seconds.
    customizedConnectionPolicy.requestTimeoutInMillis(10000);

    // Set the idle connection timeout to two minutes.
    customizedConnectionPolicy.idleConnectionTimeoutInMillis(120000);
    CosmosDBConfig cosmosDbConfig = CosmosDBConfig.builder(uri,   this.cosmosKeyCredential, dbName)
                                                  .connectionPolicy  (customizedConnectionPolic  y)
                                                  .build();
    return cosmosDbConfig;
}
```

### <a name="response-diagnostics-and-query-metrics"></a>响应诊断和查询指标

2\.2.x 版 Spring Data Cosmos DB SDK 支持响应诊断字符串和查询指标。

若要启用查询指标，请在 `application.properties` 文件中将 `populateQueryMetrics` 标志设置为 **true**。 然后，扩展 `ResponseDiagnosticsProcessor` 接口并实现 `processResponseDiagnostics` 方法来记录诊断信息。 最后，将实现的实例传递给 `CosmosDbConfig.setResponseDiagnosticsProcessor` 方法。 以下代码演示示例实现。

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    ...

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            log.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri, this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }
}
```

### <a name="pagination-and-sorting"></a>分页和排序

Spring Data Cosmos DB SDK 支持 Spring Data 分页和排序。 有关详细信息，请参阅 Spring 文档中的[特殊参数处理](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters)。

根据数据库帐户上提供的请求单位 (RU)，Cosmos DB 可能返回小于或等于请求的大小的文档。 有关详细信息，请参阅 [Azure Cosmos DB 中的请求单位](/azure/cosmos-db/request-units)。

由于每次迭代返回的文档的数目都是可变的，因此不应依赖于 `totalPageSize` 值， 而应循环访问 `Pageable` 对象，如以下示例所示。

```java
final Sort sort = Sort.by(Sort.Direction.DESC, "name");
final CosmosPageRequest pageRequest = new CosmosPageRequest(0, pageSize,   null, sort);
Page<T> page = tRepository.findAll(pageRequest);
List<T> pageContent = page.getContent();
while(page.hasNext()) {
    Pageable nextPageable = page.nextPageable();
    page = repository.findAll(nextPageable);
    pageContent = page.getContent();
}
```

## <a name="common-issues-and-workarounds"></a>常见问题和解决方法

以下部分介绍在使用 Spring Data Cosmos DB SDK 时应注意的问题。

### <a name="getting-the-correct-cosmos-db-configuration"></a>获取正确的 Cosmos DB 配置

扩展 `AbstractCosmosConfiguration` 接口可能会很棘手，因为该类中存在各种批注和配置。 最常见的问题出现在 `Enable Repositories` 批注中。

如果存储库扩展 `CosmosRepository`，请确保添加批注 `@EnableCosmosRepositories`。 如果存储库扩展 `ReactiveCosmosRepository`，请确保添加批注 `@EnableReactiveCosmosRepositories`。 以下示例演示了如何使用这些批注。

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

创建或自定义 `CosmosDBConfig` bean 时，请确保使用 `CosmosKeyCredential` 对象，而不是直接使用密钥。

可以利用 `CosmosKeyCredential` 功能动态轮换密钥。 可以使用 `switchToSecondaryKey` 方法来切换密钥。

`CosmosKeyCredential` 应该是单一实例对象，因为 Cosmos DB SDK 会在内部使用同一对象来检测该对象中键值的更改。

### <a name="custom-query-execution"></a>自定义查询执行

Spring Data Cosmos DB SDK 目前尚不支持查询批注功能。 在该功能推出之前，可以直接在由 Spring 应用程序上下文公开的 `cosmosClient` bean 上执行自定义查询和复杂查询。

以下代码演示了一个简单的示例，说明如何使用 `cosmosClient` bean 执行 offset 和 limit 查询。

```java
final FeedOptions feedOptions = new FeedOptions();

// Enable cross-partition queries.
feedOptions.enableCrossPartitionQuery(true);

// Set the page size.
feedOptions.maxItemCount(20);

// Set the number of parallel operations on the client-side SDK when executing parallel queries.
feedOptions.maxDegreeOfParallelism(2);

// Populate query metrics from Cosmos DB.
feedOptions.populateQueryMetrics(true);

final String query = "SELECT * from c OFFSET " + skipCount + " LIMIT " + takeCount;

final CosmosClient cosmosClient = applicationContext.getBean(CosmosClient.class);

Flux<FeedResponse<CosmosItemProperties>> feedResponseFlux =
    cosmosClient.getDatabase(databaseId)
                .getContainer(collectionId)
                .queryItems(query, feedOptions);
    feedResponseFlux.subscribeOn(Schedulers.parallel())
                    .flatMap(feedResponse -> {
                        List<CosmosItemProperties> results =
                        feedResponseFlux.results();
                        log.info("Results are {}", results);
                        return feedResponse;
                    })
                    .subscribe();
```

### <a name="enable-diagnostics-and-query-metrics"></a>启用诊断和查询指标

调试时，从 Cosmos DB SDK 获得响应诊断字符串和查询指标会很有用。 Cosmos DB SDK 在客户端记录响应诊断字符串。 后端记录查询指标，并将其提供给 Cosmos DB SDK。

在 Spring Data Cosmos DB SDK 中，`ResponseDiagnosticsProcessor.processResponseDiagnostics` 方法在每次进行 API 调用后调用。 因此，实现必须没有 Bug 且不复杂，确保良好的性能，这很重要。 例如，不应在此方法中记录整套诊断信息，因为所涉及的信息量会产生极大的性能成本。 还应使用 `Debug` 日志记录级别，以免影响应用程序性能。

以下代码演示了一个示例，介绍如何实现 `ResponseDiagnosticsProcessor` 接口。

```java
private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

    @Override
    public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {

        // To log everything:
        if (log.isDebugEnabled()) {
            log.debug("Response diagnostics {}", responseDiagnostics);
        }

        // To log Cosmos DB response diagnostics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Cosmos DB response diagnostics {}", cosmosResponseDiagnostics);
        }

        // To log just the request latency:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Request latency {}", cosmosResponseDiagnostics.requestLatency());
        }

        // To log query metrics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            FeedResponseDiagnostics feedResponseDiagnostics =
                responseDiagnostics.getFeedResponseDiagnostics();
            log.debug("Query metrics {}", feedResponseDiagnostics);
        }
    }
}
```

## <a name="how-to-troubleshoot"></a>如何进行故障排除

以下部分介绍排查常见问题的方法。

### <a name="connection-issues"></a>连接问题

如果遇到连接问题，请确保配置类中的所有必需批注都存在且正确，如[获取正确的 Cosmos DB 配置](#getting-the-correct-cosmos-db-configuration)部分所述。

### <a name="api-exceptions"></a>API 异常

2\.2.1 版 Spring Data Cosmos DB SDK 提供对异常处理的以下改进：

- 所有 API 都引发 `CosmosDBAccessException`，这将通过 getter 来公开 `cosmosClientException` 字段。
- Cosmos DB SDK 引发 `CosmosClientException`，你可以使用它在客户端实现任何重试逻辑。
- 重试的常见异常会出现 `Resource already exists`、`Request rate too large`、`Request timeout exception` 等消息。

### <a name="api-or-query-slowness"></a>API 或查询速度缓慢

如果在 API 上或查询执行过程中遇到严重的延迟，请尝试记录诊断字符串和查询指标，如[启用诊断和查询指标](#enable-diagnostics-and-query-metrics)部分所述。 请检查 CPU 使用率、网络带宽和 I/O 磁盘空间，这可能是客户端速度缓慢的根本原因。
