---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 6b2ad5c8490cd4b1c450426f8e7d728cd39eaea3
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062042"
---
现在你已完成迁移，请验证应用程序是否按预期运行。 然后可借助以下建议，提高应用程序的云原生性。

* 请考虑将应用程序与 Spring Cloud 注册表配合使用。 这将允许其他已部署的微服务和客户端动态发现你的应用程序。 有关详细信息，请参阅[教程：准备 Java Spring 应用以进行部署](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)。 然后修改任何应用程序客户端，以使用 Spring Client 负载均衡器。 这允许客户端获取应用程序的所有正在运行的实例地址，并查找在其他实例损坏或无响应时可以使用的实例。 有关详细信息，请参阅 Spring 博客中的 [Spring 使用技巧：Spring Cloud 负载均衡器](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer)。

* 请考虑添加 [Spring Cloud 网关](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/current/reference/html/)实例，而不是公开你的应用程序。 Spring Cloud 网关为 Azure Spring Cloud 实例中部署的所有应用程序/微服务提供单一终结点。 如果已部署 Spring Cloud 网关，请确保将其配置为将流量路由到新部署的应用程序。

* 请考虑添加一个 Spring Cloud Config 服务器来集中管理和版本控制所有 Spring Cloud 微服务的配置。 首先，创建一个 Git 存储库来容纳配置，并配置 Azure Spring Cloud 实例以使用该配置。 有关详细信息，请参阅[教程：为服务设置 Spring Cloud 配置服务器实例](/azure/spring-cloud/spring-cloud-tutorial-config-server)。 然后使用以下步骤迁移配置：

  1. 在应用程序的 src/main/resources 目录内，创建一个包含以下内容的 bootstrap.yml 文件 ：

        ```yml
          spring:
            application:
              name: <your-application-name>
        ```

  1. 在配置 Git 存储库中，创建一个 <your-application-name>.yml 文件，其中，`your-application-name` 与之前的步骤中相同。 将设置从 src/main/resources 中的 application.yml 文件移动到刚创建的新文件 。 如果这些设置以前在 .properties 文件中，则先将其转换为 YAML。 可以查找联机工具或 IntelliJ 插件来执行此转换。

  1. 在上述目录中创建一个 application.yml 文件。 可以使用此文件来定义将在 Azure Spring Cloud 实例的所有应用程序之间共享的设置和资源。 此类设置通常包括数据源、日志记录设置、Spring Boot Actuator 配置等。

  1. 提交这些更改并将其推送到 Git 存储库。

  1. 从应用程序中删除 application.properties 或 application.yml 应用程序 。

* 请考虑为自动、一致的部署添加部署管道。 提供有关 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) 和 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) 的说明。

* 请考虑使用过渡部署在生产中测试代码更改，然后再将其提供给一些或所有最终用户。 有关详细信息，请参阅[在 Azure Spring Cloud 中设置过渡环境](/azure/spring-cloud/spring-cloud-howto-staging-environment)。

* 请考虑添加服务绑定，以便将应用程序连接到受支持的 Azure 数据库。 如果使用这些服务绑定，则无需向 Spring Cloud 应用程序提供连接信息（包括凭据）。

* 请考虑使用分布式跟踪和 Azure App Insights 来监视应用程序的性能和交互。 有关详细信息，请参阅[将分布式跟踪与 Azure Spring Cloud 配合使用](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)。

* 请考虑添加 Azure Monitor 预警规则和操作组，以快速检测并解决异常情况。 有关详细信息，请参阅[教程：使用警报和操作组监视 Spring Cloud 资源](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)。

* 请考虑将 Azure Spring Cloud 部署复制到其他区域，以降低延迟，提高可靠性和容错。 使用 [Azure 流量管理器](/azure/traffic-manager)在部署之间实现负载均衡，或使用 [Azure Front Door](/azure/frontdoor) 添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。

* 如果不需要异地复制，请考虑添加 [Azure 应用程序网关](/azure/application-gateway)，以便添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。
