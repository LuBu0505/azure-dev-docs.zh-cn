---
title: 将 Tomcat 应用程序迁移到 Azure Kubernetes 服务上的容器
description: 本指南介绍在需要迁移现有 Tomcat 应用程序以使之在 Azure Kubernetes 服务容器中运行时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: da516609aaf976db929664bf0402a48f378034d3
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288606"
---
# <a name="migrate-tomcat-applications-to-containers-on-azure-kubernetes-service"></a>将 Tomcat 应用程序迁移到 Azure Kubernetes 服务上的容器

本指南介绍在需要迁移现有 Tomcat 应用程序以使之在 Azure Kubernetes 服务 (AKS) 上运行时应注意的事项。

## <a name="pre-migration-steps"></a>迁移前步骤

* [清点外部资源](#inventory-external-resources)
* [清点机密](#inventory-secrets)
* [清点持久性使用情况](#inventory-persistence-usage)
* [特殊情况](#special-cases)
* [就地测试](#in-place-testing)

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- AKS-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>动态或内部内容

对于经常由应用程序写入和读取的文件（如临时数据文件），或者仅对应用程序可见的静态文件，可以将 Azure 存储共享作为持久卷进行装载。 有关详细信息，请参阅[在 Azure Kubernetes 服务中动态创建永久性卷并将其用于 Azure 文件存储](/azure/aks/azure-files-dynamic-pv)。

### <a name="identify-session-persistence-mechanism"></a>识别会话持久性机制

若要确定正在使用的会话持久性管理器，请在应用程序和 Tomcat 配置中检查 *context.xml* 文件。 请查找 `<Manager>` 元素，然后记下 `className` 属性的值。

Tomcat 的内置 [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) 实现（如 [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) 或 [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components)）不适用于分布式缩放平台，如 Kubernetes。 AKS 可能会在多个 Pod 之间进行负载均衡，并随时以透明方式重启任何 Pod。不建议将可变状态持久保存到文件系统。

如果需要会话持久性，则需使用会将内容写入外部数据存储的备用 `PersistentManager` 实现，例如使用 Redis 缓存的 Pivotal 会话管理器。 有关详细信息，请参阅[将 Redis 作为会话缓存与 Tomcat 配合使用](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)。

### <a name="special-cases"></a>特殊情况

某些生产方案可能需要进行其他更改或施加额外的限制。 虽然这种情况可能不怎么出现，但务必确保它们不适用于应用程序，或者在适用于应用程序的情况下已得到正确解决。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>确定应用程序是否依赖于计划的作业

计划的作业（如 Quartz 计划程序任务或 cron 作业）不能与容器化 Tomcat 部署配合使用。 如果应用程序横向扩展，则一个计划的作业可能会按照计划期间运行多次。 这种情况可能会导致意外的后果。

清点应用程序服务器内外部所有计划的作业。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>确定应用程序是否包含特定于 OS 的代码

如果应用程序包含任何要适应运行应用程序的 OS 的代码，则需对应用程序进行重构，使之不依赖于基础 OS。 例如，可能需要将文件系统路径中使用的 `/` 或 `\` 替换为 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 或 [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)。

#### <a name="determine-whether-memoryrealm-is-used"></a>确定是否使用了 MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) 需要一个持久的 XML 文件。 在 Kubernetes 上，需将此文件添加到容器映像中，或将其上传到[可供容器使用的共享存储](#identify-session-persistence-mechanism)。 `pathName` 参数必须相应地进行修改。

若要确定当前是否在使用 `MemoryRealm`，请检查 *server.xml* 和 *context.xml* 文件，并搜索已在其中将 `className` 属性设置为 `org.apache.catalina.realm.MemoryRealm` 的 `<Realm>` 元素。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>确定是否使用了 SSL 会话跟踪

在容器化部署中，SSL 会话通常在应用程序容器外部由入口控制器卸载。 如果应用程序需要 [SSL 会话跟踪](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)，请确保 SSL 流量直接传递到应用程序容器。

#### <a name="determine-whether-accesslogvalve-is-used"></a>确定是否使用了 AccessLogValve

如果使用 [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html)，则应将 `directory` 参数设置为 [已装载的 Azure 文件共享](/azure/aks/azure-files-dynamic-pv)或其子目录之一。

### <a name="in-place-testing"></a>就地测试

在创建容器映像之前，请将应用程序迁移到要在 AKS 上使用的 JDK 和 Tomcat。 全面测试应用程序，确保兼容性和性能。

### <a name="parametrize-the-configuration"></a>将配置参数化

在预迁移中，你可能已在 *server.xml* 和 *context.xml* 文件中标识了机密和外部依赖项（如数据源）。 对于这样标识的每个项，请将任何用户名、密码、连接字符串或 URL 替换为环境变量。

例如，假设 *context.xml* 文件包含以下元素：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

在这种情况下，可以对其进行更改，如以下示例所示：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

## <a name="migration"></a>迁移

建议你分别为要迁移的每个应用程序（WAR 文件）执行以下步骤，第一步（“预配容器注册表和 AKS”）除外。

> [!NOTE]
> 某些 Tomcat 部署可能会在单个 Tomcat 服务器上运行多个应用程序。 如果部署中出现这种情况，强烈建议在单独的 Pod 中运行每个应用程序。 这样可以优化每个应用程序的资源利用率，同时最大程度地降低复杂性和耦合度。

### <a name="provision-container-registry-and-aks"></a>预配容器注册表和 AKS

在注册表中创建其服务主体具有“读取者”角色的容器注册表和 Azure Kubernetes 群集。 确保根据群集的网络要求[选择相应的网络模型](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```

### <a name="prepare-the-deployment-artifacts"></a>准备部署项目

克隆[容器中的 Tomcat 快速入门 GitHub 存储库](https://github.com/Azure/tomcat-container-quickstart)。 它包含一个 Dockerfile 和多个 Tomcat 配置文件，以及很多建议的优化措施。 在下面的步骤中，我们将在生成容器映像并将其部署到 AKS 之前，概述可能需要对这些文件进行的修改。

#### <a name="open-ports-for-clustering-if-needed"></a>根据需要打开用于聚类分析的端口

若要在 AKS 上使用 [Tomcat 聚类分析](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html)，请确保在 Dockerfile 中公开必要的端口范围。 若要在 `server.xml` 中指定服务器 IP 地址，请确保使用在容器启动时初始化为 Pod 的 IP 地址的变量中的值。

也可将会话状态[持久保存到备用位置](#identify-session-persistence-mechanism)，使之可以跨副本使用。

若要确定应用程序是否使用聚类分析，请在 *server.xml* 文件中查找 `<Host>` 或 `<Engine>` 元素内的 `<Cluster>` 元素。

#### <a name="add-jndi-resources"></a>添加 JNDI 资源

编辑 *server.xml*，添加预迁移步骤中准备的资源，例如数据源。

例如：

```xml
<!-- Global JNDI resources
      Documentation at /docs/jndi-resources-howto.html
-->
<GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
               />

    <!-- Migrated datasources here: -->
    <Resource
        name="jdbc/dbconnection"
        type="javax.sql.DataSource"
        url="${postgresdb.connectionString}"
        driverClassName="org.postgresql.Driver"
        username="${postgresdb.username}"
        password="${postgresdb.password}"
    />
    <!-- End of migrated datasources -->
</GlobalNamingResources>
```

### <a name="build-and-push-the-image"></a>生成并推送映像

若要生成映像并将其上传到 Azure 容器注册表 (ACR) 供 AKS 使用，最简单的方法是使用 `az acr build` 命令。 该命令不需要在计算机上安装 Docker。 例如，如果当前目录中有上述 Dockerfile 和应用程序包 *petclinic.war*，则可通过一个步骤生成 ACR 中的容器映像：

```bash
az acr build -t "${acrName}.azurecr.io/petclinic:{{.Run.ID}}" -r $acrName --build-arg APP_FILE=petclinic.war --build-arg=prod.server.xml .
```

如果 WAR 文件的名称为 *ROOT.war*，则可省略 `--build-arg APP_FILE...` 参数。 如果服务器 XML 文件的名称为 *server.xml*，则可省略 `--build-arg SERVER_XML...` 参数。 这两个文件必须位于 *Dockerfile* 所在的目录中。

也可使用 Docker CLI 在本地生成映像。 此方法可以简化在一开始部署到 ACR 之前对映像进行的测试和优化。 但是，它要求安装 Docker CLI 且 Docker 守护程序处于运行状态。

```bash
# Build the image locally
sudo docker build . --build-arg APP_FILE=petclinic.war -t "${acrName}.azurecr.io/petclinic:1"

# Run the image locally
sudo docker run -d -p 8080:8080 "${acrName}.azurecr.io/petclinic:1"

# Your application can now be accessed with a browser at http://localhost:8080.

# Log into ACR
sudo az acr login -n $acrName

# Push the image to ACR
sudo docker push "${acrName}.azurecr.io/petclinic:1"
```

若要更深入地了解如何在 Azure 中生成和存储容器映像，请参阅相应的 [Microsoft Learn 课程](/learn/modules/build-and-store-container-images/)。

### <a name="provision-a-public-ip-address"></a>预配公共 IP 地址

如果想要跳出内部网络或虚拟网络来访问应用程序，则需要公共静态 IP 地址。 应在群集的节点资源组内预配此 IP 地址。

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```

### <a name="deploy-to-aks"></a>部署到 AKS

[创建并应用 Kubernetes YAML 文件](/azure/aks/kubernetes-walkthrough#run-the-application)。 如果要创建外部负载均衡器（不管是针对应用程序，还是针对入口控制器），请确保提供在上一部分以 `LoadBalancerIP` 形式预配的 IP 地址。

包括[环境变量形式的外部化参数](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)。 不要包括机密（如密码、API 密钥和 JDBC 连接字符串）。 [配置 KeyVault FlexVolume](#configure-keyvault-flexvolume) 部分介绍了机密。

### <a name="configure-persistent-storage"></a>配置持久性存储

如果应用程序需要非易失性存储，请配置一个或多个[持久性卷](/azure/aks/azure-disks-dynamic-pv)。

可能需要[使用 Azure 文件存储创建持久性卷](/azure/aks/azure-files-dynamic-pv)，该卷装载到 Tomcat 日志目录 ( */tomcat_logs*)，目的是集中保留日志。

### <a name="configure-keyvault-flexvolume"></a>配置 KeyVault FlexVolume

[创建 Azure KeyVault](/azure/key-vault/quick-create-cli) 并填充所有必要的机密。 然后，配置 [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md)，使这些机密可供 Pod 访问。

需要修改启动脚本（[容器中的 Tomcat](https://github.com/Azure/tomcat-container-quickstart) GitHub 存储库中的 *startup.sh*），将证书导入容器的本地密钥存储中。

### <a name="migrate-scheduled-jobs"></a>迁移计划的作业

若要在 AKS 群集上执行计划的作业，请按需定义 [Cron 作业](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)。

## <a name="post-migration-steps"></a>迁移后的步骤

将应用程序迁移到 AKS 后，即应验证其运行是否符合预期。 完成此操作后，可以参考我们提供的一些建议，使应用程序的云原生性更好。

1. 考虑向分配给入口控制器或应用程序负载均衡器的 IP 地址[添加 DNS 名称](/azure/aks/ingress-static-ip#configure-a-dns-name)。

1. 考虑[为应用程序添加 HELM 图表](https://helm.sh/docs/topics/charts/)。 可以通过 Helm 图表将应用程序部署参数化，供更多样化的客户使用和自定义。

1. 设计和实施 DevOps 策略。 若要在提高开发速度的同时保持可靠性，请考虑[通过 Azure Pipelines 自动进行部署和测试](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)。

1. 启用[针对群集的 Azure 监视](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)，以便收集容器日志、跟踪利用率等。

1. 考虑通过 Prometheus 公开特定于应用程序的指标。 Prometheus 是在 Kubernetes 社区中广泛采用的开源指标框架。 可以配置 [Azure Monitor 中的 Prometheus 指标抓取](/azure/azure-monitor/insights/container-insights-prometheus-integration)，而不是托管你自己的 Prometheus 服务器，以便进行应用程序指标聚合，并自动响应或升级异常情况。

1. 设计和实施业务连续性和灾难恢复策略。 对于关键应用程序，请考虑[多区域部署体系结构](/azure/aks/operator-best-practices-multi-region)。

1. 查看 [Kubernetes 版本支持策略](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)。 你有责任持续[更新 AKS 群集](/azure/aks/upgrade-cluster)，确保其始终运行受支持的版本。

1. 让所有负责群集管理和应用程序开发的团队成员查看相关的 [AKS 最佳做法](/azure/aks/best-practices)。

1. 评估 *logging.properties* 文件中的项。 考虑消除或减少某些日志记录输出以提高性能。

1. 考虑[监视代码缓存大小](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)并向 Dockerfile 中的 `JAVA_OPTS` 变量添加参数 `-XX:InitialCodeCacheSize` 和 `-XX:ReservedCodeCacheSize`，进一步优化性能。
