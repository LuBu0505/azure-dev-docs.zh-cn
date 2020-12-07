---
title: Spring Data Azure Cosmos DB 开发人员指南
description: 本指南介绍在使用 Spring Data Azure Cosmos DB SDK 时应注意的事项。
author: anfeldma-ms
ms.author: anfeldma
ms.topic: conceptual
ms.date: 11/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 45872eba2fd6929cf406df4a551559fd5ad78c40
ms.sourcegitcommit: 63732132cb88206b99876f0bcd035b52c301f315
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523132"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Spring Data Azure Cosmos DB 开发人员指南

本主题介绍使用 SQL API 的 [Spring Data Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos) 的功能。 本主题还包括有关常见问题、解决方法和诊断步骤的指导。

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API 处理数据。 Spring Data Azure Cosmos DB SDK 基于 [Spring Data](https://spring.io/projects/spring-data) 框架，并提供与使用 SQL API 的 Azure Cosmos DB 的集成。 以下主题介绍对其他 API 的支持：

- [如何将 Spring Data MongoDB API 用于 Azure Cosmos DB](./configure-spring-data-mongodb-with-cosmos-db.md)
- [如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB](./configure-spring-data-apache-cassandra-with-cosmos-db.md)
- [如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用](./configure-spring-data-gremlin-java-app-with-cosmos-db.md)

Spring Data Azure Cosmos DB SDK在 GitHub 上的 [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos) 存储库中作为开放源代码提供。 此存储库有一个活动[问题](https://github.com/Azure/azure-sdk-for-java/issues)列表，你可以在其中提交 Bug 或检查已提交问题的解决方法。 你还可以检查[版本](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-spring-data-cosmos/CHANGELOG.md)列表，看是否已在更新的版本中解决了某个问题。 

## <a name="available-features"></a>可用功能

以下部分介绍当前的可用功能。

### <a name="crudrepository-and-reactivecrudrepository-support"></a>CrudRepository 和 ReactiveCrudRepository 支持

Spring Data Azure Cosmos DB SDK 提供 `CosmosRepository` 和 `ReactiveCosmosRepository` 接口，用于扩展 Spring Data 的 `CrudRepository` 和 `ReactiveCrudRepository` 接口。

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
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>定义简单实体

可以通过添加 `@Container` 批注并指定与集合相关的属性（如集合名称、请求单位 (RU)、生存时间和自动创建集合标志）来定义实体。

默认情况下，集合名称将是用户域类的类名。 若要对其进行自定义，请将 `@Container(containerName="myCustomCollectionName")` 批注添加到域类中。 `containerName` 字段还支持 [Spring 表达式语言](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) (SpEL) 表达式，方便你通过配置属性以编程方式提供集合名称。 例如，可以使用 `containerName = "${dynamic.container.name}"` 和 `containerName = "#{@someBean.getContainerName()}"` 之类的表达式。

可以通过两种方式将域类中的字段映射到 Azure Cosmos DB 文档的 `id` 字段：

- 使用 `@Id` 批注字段。
- 将字段的名称设置为 `id`。

以下示例演示如何使用 `@Container` 和 `@Id` 批注。

```java
@Container(containerName = "myContainer")
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

SDK 还支持分区。 有关详细信息，请参阅 [Azure Cosmos DB 中的分区和水平缩放](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#:~:text=%20Partitioning%20and%20horizontal%20scaling%20in%20Azure%20Cosmos,partitions.%20Azure%20Cosmos%20DB%20transparently%20and...%20More%20)。 若要指定域类的字段作为分区键字段，请使用 `@PartitionKey` 来批注它。 然后，在执行 CRUD 操作时指定分区值。

以下示例演示如何在执行 CRUD 操作时使用 `@PartitionKey` 批注。

```java
@Container(ru = "400")
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

    final Address newAddress = new Address("12345", "Seattle");

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

## <a name="best-practices"></a>最佳实践

以下部分介绍了使用 SDK 时的最佳做法。

### <a name="pulling-configuration-properties-into-the-application"></a>将配置属性拉取到应用程序中

你可以创建一个属性类，用于将 application.properties 设置作为 Java 访问方法公开。 application.properties 的结构可能是

```xml
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

# Populate query metrics
cosmos.queryMetricsEnabled=true
```

镜像此结构，创建一个结构如下的 Java 类 `CosmosProperties`。

```java
@ConfigurationProperties(prefix = "cosmos")
public class CosmosProperties {

    private String uri;

    private String key;

    private String secondaryKey;

    private boolean queryMetricsEnabled;

    public String getUri() {
        return uri;
    }

    public void setUri(String uri) {
        this.uri = uri;
    }

    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    public String getSecondaryKey() {
        return secondaryKey;
    }

    public void setSecondaryKey(String secondaryKey) {
        this.secondaryKey = secondaryKey;
    }

    public boolean isQueryMetricsEnabled() {
        return queryMetricsEnabled;
    }

    public void setQueryMetricsEnabled(boolean enableQueryMetrics) {
        this.queryMetricsEnabled = enableQueryMetrics;
    }
}
```

注意，此类有一个与每个 application.properties 配置属性对应的成员，并且对于每个成员，`CosmosProperties` 都会公开“get”和“set”方法 。 `@ConfigurationProperties` 注释将该类标识为表示配置属性，`prefix = "cosmos"` 参数表示 `CosmosProperties` 的给定成员映射到 application.properties 中的 `cosmos.member` 属性。

在下一部分中，我们将展示如何将 `CosmosProperties` 类合并到自动化配置流中。 在配置时，将创建一个 `CosmosProperties` 实例，并使用 application.properties 中的配置设置来填充其实例方法。 此属性实例允许应用程序在运行时读取和修改配置属性。

### <a name="configuring-the-application-based-on-properties"></a>基于属性配置应用程序

下一步是创建一个自动配置应用程序的配置类。 我们将通过下面的示例介绍其创建方法

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class AppConfiguration extends AbstractCosmosConfiguration {

    private static final Logger logger = LoggerFactory.getLogger(QuickstartSampleConfiguration.class);

    @Autowired
    private CosmosProperties properties;

    private AzureKeyCredential azureKeyCredential;

    @Bean
    public CosmosClientBuilder cosmosClientBuilder() {
        this.azureKeyCredential = new AzureKeyCredential(properties.getKey());
        return new CosmosClientBuilder()
            .endpoint(properties.getUri())
            .key(this.azureKeyCredential)
    }

    @Bean
    public CosmosConfig cosmosConfig() {
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig();        
        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig);                           
                           .build();
    }

    @Override
    protected String getDatabaseName() {
        return "testdb";
    }

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            logger.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

创建配置类的结构：

1. 扩展 `AbstractCosmosConfiguration` 类，以设置应用程序的配置（Cosmos DB 密钥、URL、数据库名称等）。
1. 添加 `@Configuration` 批注。
1. 根据存储库的使用情况，添加 `@EnableCosmosRepositories` 和/或 `@EnableReactiveCosmosRepositories` 批注。
1. 添加 `@PropertySource("classpath:application.properties")` 注释，该注释指示从 application.properties 提取属性的键/值对
1. 添加 `@EnableConfigurationProperties` 注释，该注释将 Spring Data 指向可存储 application.properties 中的键/值对的类。 此注释将类定义作为参数；你应传递 `CosmosProperties.class`。

配置类将使用以下成员：

1. 声明并定义 log4j2 `logger` 成员，Spring Data 会将该成员用于所有日志输出
1. 声明 `@Autowired` `CosmosProperties` 成员，这是存放 application.properties 设置的位置

可以利用 `AzureKeyCredential` 功能动态轮换密钥。 要启用此功能，请定义 `AzureKeyCredential` 成员。 可以通过添加 `switchToSecondaryKey` 方法来切换键，如上面的示例所示。

接下来，需要定义如何执行自动化配置。
1. 定义 `@Bean` `cosmosClientBuilder` 方法，以使用 `CosmosClientBuilder` 来处理客户端初始化。 此方法的目的是执行基本客户端设置，即指定帐户终结点 URI 和访问密钥。 通常，帐户终结点 URI 和访问密钥是在 application.properties 中定义的，而这些内容又将填充到 `properties` 中。 可以使用 `properties.getKey()` 初始化 `azureKeyCredential` 成员，然后分别将 `properties.getUri()` 和 `this.azureKeyCredential` 提供给 `endpoint` 和 `key` 生成器方法。 

请注意，在上面的示例中，`cosmosClientBuilder` 没有在客户端生成器上调用 `build()`，而是返回未完成的生成器结构。 Spring Data 允许分两个阶段执行配置 - 首先，`cosmosClientBuilder` 可以应用基本配置并返回配置结构，然后 Spring Data 调用 `cosmosConfig` 方法，通过该方法，可定义更高级的配置，如指标和诊断。 接下来，我们将通过 `cosmosConfig` 方法介绍这个高级配置：
1. 按如上所示创建 `@Bean` `cosmosConfig` 方法。
1. Azure Cosmos DB 可以返回与每个请求关联的服务器端诊断。 借助 Spring Data，可通过定义客户诊断处理器在记录原始诊断输出之前转换原始诊断输出。 如上所示，定义一个实现 `ResponseDiagnosticsProcessor` 并替代 `processResponseDiagnostics` 方法的类。 可以定义 `processResponseDiagnostics` 以控制如何处理诊断输出。 以上示例只是记录了原始诊断。
1. 要启用诊断并初始化诊断处理器，请调用 `responseDiagnosticsProcessor` 生成器方法，传递客户处理器类的新实例：

    ```java
    return CosmosConfig.builder()
                       .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
    ```
1. Azure Cosmos DB 还为查询提供了更具体的性能指标功能，称为查询指标。 如前一节所示，最佳做法是使用 application.properties 设置来启用/禁用查询指标。 通过在 `cosmosConfig` 中附加 `.enableQueryMetrics(properties.isQueryMetricsEnabled())` 生成器方法来应用此配置设置。
1. 建议使用直接模式连接，以最大程度降低延迟并最大程度提高吞吐量，以便你也可以在客户端生成器中进行配置。

`cosmosConfig` 中的高级配置完成后，必须通过在配置结构上调用 `build()` 来触发客户端创建；这会根据配置设置生成一个 Azure Cosmos DB 客户端实例。

定义配置过程的最后一步是添加 `@Override` 方法 `getDatabaseName()`，该方法以字符串的形式返回 Azure Cosmos DB 数据库的名称。

### <a name="customizing-the-configuration"></a>自定义配置

还可以通过自定义配置来更改连接模式、连接池最大大小、请求超时等，如以下示例所示。

```java
    @Bean
    public CosmosConfig cosmosConfig() {

        // Set the connection mode to Direct (TCP) which applies to data plane operations
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig(); 

        // Even in Direct mode, some control plane operations always pass through the gateway as HTTP requests (i.e. container/database CRUD.)
        // Optionally, you can customize connection properties for these specific operations which are
        // always Gateway mode
        GatewayConnectionConfig gatewayConnectionConfig = GatewayConnectionConfig.getDefaultConfig(); 

        // Set the maximum number of HTTP connections to 1000 per application.
        gatewayConnectionConfig.setMaxConnectionPoolSize(1000);

        // Set the request timeout to 10 seconds.
        gatewayConnectionConfig.setIdleConnectionTimeout(Duration.ofMillis(10000));

        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig, gatewayConnectionConfig); // directMode() has an override which accepts Gateway config                        
                           .build();
    }
```

### <a name="response-diagnostics-and-query-metrics"></a>响应诊断和查询指标

自版本 2 起，Spring Data Azure Cosmos DB SDK 支持响应诊断字符串和查询指标。

若要启用查询指标，请在 `application.properties` 文件中将 `queryMetricsEnabled` 标志设置为 **true**。 然后，按照上一部分所述的过程来扩展 `ResponseDiagnosticsProcessor` 接口并实现 `processResponseDiagnostics` 方法，以记录诊断信息。 最后，将实现的实例传递给 `CosmosDbConfig.setResponseDiagnosticsProcessor` 方法。

### <a name="pagination-and-sorting"></a>分页和排序

Spring Data Azure Cosmos DB SDK 支持 Spring Data 分页和排序。 有关详细信息，请参阅 Spring 文档中的[特殊参数处理](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters)。

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

以下部分介绍在使用 Spring Data Azure Cosmos DB SDK 时应注意的问题。

### <a name="getting-the-correct-cosmos-db-configuration"></a>获取正确的 Cosmos DB 配置

扩展 `AbstractCosmosConfiguration` 接口可能会很棘手，因为该类中存在各种批注和配置。 最常见的问题出现在 `Enable Repositories` 批注中。

如果存储库扩展 `CosmosRepository`，请确保添加批注 `@EnableCosmosRepositories`。 如果存储库扩展 `ReactiveCosmosRepository`，请确保添加批注 `@EnableReactiveCosmosRepositories`。 以下示例演示了如何使用这些批注。

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

创建或自定义 `CosmosDBConfig` bean 时，请确保使用 `AzureKeyCredential` 对象，而不是直接使用密钥。

可以利用 `AzureKeyCredential` 功能动态轮换密钥。 可以使用 `switchToSecondaryKey` 方法来切换密钥。

`AzureKeyCredential` 应该是单一实例对象，因为 Cosmos DB SDK 会在内部使用同一对象来检测该对象中键值的更改。

### <a name="custom-query-execution"></a>自定义查询执行

Spring Data Azure Cosmos DB SDK 3.x.x 支持使用 `@query` 注释定义自定义查询！

以下代码演示了一个简单的示例，说明如何使用 `@query` 注释定义 offset 和 limit 查询：

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {

    ...

    @Query(value = "SELECT * from c OFFSET @skipCount LIMIT @takeCount")
    List<SampleEntity> findByName(@Param("skipCount") int skipCount, @Param("takeCount") int takeCount);
}
```

### <a name="enable-diagnostics-and-query-metrics"></a>启用诊断和查询指标

调试时，从 Cosmos DB SDK 获得响应诊断字符串和查询指标会很有用。 Cosmos DB SDK 在客户端记录响应诊断字符串。 后端记录查询指标，并将其提供给 Cosmos DB SDK。

在 Spring Data Azure Cosmos DB SDK 中，每次 API 调用后都会调用 `ResponseDiagnosticsProcessor.processResponseDiagnostics` 方法。 因此，实现必须没有 Bug 且不复杂，确保良好的性能，这很重要。 例如，不应在此方法中记录整套诊断信息，因为所涉及的信息量会产生极大的性能成本。 还应使用 `Debug` 日志记录级别，以免影响应用程序性能。

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

### <a name="naming-changes"></a>命名变更

3\.1.0+ 版 Spring Data Azure Cosmos DB SDK 对类、方法、注释和 Maven 项目的名称/接口进行了以下显著更改：
* 已将组 ID 更新为 `com.azure`。
* 已将项目 ID 更新为 `azure-spring-data-cosmos`。
* 同步 API 返回类型更新为 `Iterable` 类型，而非 `List`。
* `CosmosDbFactory` 重命名为 `CosmosFactory`。
* `CosmosDBConfig` 重命名为 `CosmosConfig`。
* `CosmosDBAccessException` 重命名为 `CosmosAccessException`。
* `Document` 注释重命名为 `Container` 注释。
* `DocumentIndexingPolicy` 注释重命名为 `CosmosIndexingPolicy` 注释。
* `DocumentQuery` 重命名为 `CosmosQuery`。
* application.properties 将 `populateQueryMetrics` 标记为 `queryMetricsEnabled`。

### <a name="key-bug-fixes"></a>关键 bug 修复

3\.1.0+ 版 Spring Data Azure Cosmos DB SDK 修复了以下关键 bug
* 修复了带批注的查询不选取带批注的容器名称的问题。
* 将诊断日志记录任务安排到并行线程，以避免阻塞 Netty I/O 线程。
* 修复了对删除操作的乐观锁定。
* 修复了转义 IN 子句查询时的问题。
* 通过允许对 @Id 使用 long 数据类型修复了一项问题。
* 通过允许对 @PartitionKey 注释使用 boolean、long、int、double 数据类型修复了一项问题。
* 修复了忽略大小写的查询的 IgnoreCase 和 AllIgnoreCase 关键字问题。
* 删除了自动创建容器时的默认请求单位值 4000。
* 修复了与 @GeneratedValue 批注一起使用时的嵌套分区键 bug。

### <a name="api-or-query-slowness"></a>API 或查询速度缓慢

如果在 API 上或查询执行过程中遇到严重的延迟，请尝试记录诊断字符串和查询指标，如[启用诊断和查询指标](#enable-diagnostics-and-query-metrics)部分所述。 请检查 CPU 使用率、网络带宽和 I/O 磁盘空间，这可能是客户端速度缓慢的根本原因。