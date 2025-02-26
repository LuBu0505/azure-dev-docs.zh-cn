---
title: 将 Spring Boot 应用程序部署到 Azure Kubernetes 服务
titleSuffix: Azure Kubernetes Service
description: 本教程将指导用户完成在 Microsoft Azure 的 Kubernetes 群集中部署 Spring Boot 应用程序的步骤。
services: container-service
documentationcenter: java
ms.date: 10/06/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: mvc, devx-track-java, devx-track-azurecli
ms.openlocfilehash: d902219a7dd5f59abc54343ff3aca60f4b7f46e4
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689192"
---
# <a name="deploy-spring-boot-application-to-the-azure-kubernetes-service"></a>将 Spring Boot 应用程序部署到 Azure Kubernetes 服务

[Kubernetes] 和 [Docker] 是开源解决方案，可帮助开发人员自动部署、缩放和管理在容器中运行的应用程序   。

本教程将指导用户将这两种常用的开源技术进行结合，从而将 Spring Boot 应用程序开发和部署到 Microsoft Azure。 具体而言，将使用 [Spring Boot] 进行应用程序开发，使用 [Kubernetes] 进行容器部署，使用 [Azure Kubernetes 服务 (AKS)] 来托管应用程序   。

## <a name="prerequisites"></a>先决条件

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* [Azure 命令行接口 (CLI)]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven] 生成工具（版本 3）。
* [Git] 客户端。
* [Docker] 客户端。
* [ACR Docker 凭据帮助器](https://github.com/Azure/acr-docker-credential-helper)。

> [!NOTE]
> 由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>创建 Docker 上的 Spring Boot 入门 Web 应用

以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。

1. 打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：

   ```bash
   mkdir C:\SpringBoot
   cd C:\SpringBoot
   ```

   \- 或 -

   ```bash
   mkdir /users/$USER/SpringBoot
   cd /users/$USER/SpringBoot
   ```

1. 将 [Docker 上的 Spring Boot 入门]示例项目克隆到目录。

   ```bash
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 将目录更改为已完成项目。

   ```bash
   cd gs-spring-boot-docker
   cd complete
   ```

1. 使用 Maven 生成和运行示例应用。

   ```bash
   mvn package spring-boot:run
   ```

1. 通过浏览到 `http://localhost:8080` 或使用以下 `curl` 命令测试 Web 应用：

   ```bash
   curl http://localhost:8080
   ```

1. 应当会看到显示了以下消息： **Hello Docker World**

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 创建 Azure 容器注册表

1. 打开命令提示符。

1. 登录到 Azure 帐户：

   ```azurecli
   az login
   ```

1. 选择自己的 Azure 订阅：

   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. 为本教程中使用的 Azure 资源创建资源组。

   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. 在资源组中创建私有 Azure 容器注册表。 本教程的后续步骤会将示例应用作为 Docker 映像推送到此注册表。 用注册表的唯一名称替换 `wingtiptoysregistry`。

   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>通过 Jib 将应用推送到容器注册表

1. 通过 Azure CLI 登录到 Azure 容器注册表。

   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. 使用文本编辑器（例如 [VS Code](https://code.visualstudio.com/docs)）打开 pom.xml 文件，。

   ```bash
   code pom.xml
   ```

1. 将 *pom.xml* 文件中的 `<properties>` 集合更新为你的 Azure 容器注册表的注册表名称和 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 的最新版本。

   ```xml
   <properties>
      <!-- Note: If your ACR name contains upper case characters, be sure to convert them to lower case characters. -->
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>2.5.2</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. 更新 pom.xml  文件中的 `<plugins>` 集合，使 `<plugin>` 元素包含 `jib-maven-plugin` 的条目，如以下示例中所示。 请注意，我们将使用 Microsoft 容器注册表 (MCR) 中的基础映像：`mcr.microsoft.com/java/jdk:8-zulu-alpine`，其中包含 Azure 正式支持的 JDK。 有关包含正式支持的 JDK 的其他 MCR 基础映像，请参阅 [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk)、[Java SE JRE](https://hub.docker.com/_/microsoft-java-jre)、[Java SE 无头 JRE](https://hub.docker.com/_/microsoft-java-jre-headless) 以及 [Java SE JDK 和 Maven](https://hub.docker.com/_/microsoft-java-maven)。

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>mcr.microsoft.com/java/jdk:8-zulu-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. 导航到 Spring Boot 应用程序的完成项目目录，然后运行以下命令以生成映像并将映像推送到注册表：

   ```cmd
   az acr login && mvn compile jib:build
   ```

> [!NOTE]
> 出于 Azure Cli 和 Azure 容器注册表的安全考虑，通过 `az acr login` 创建的凭据的有效时间为 1 小时。如果遇到“401 未授权”错误，  则可再次运行 `az acr login -n <your registry name>` 命令，以便重新进行身份验证。

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>使用 Azure CLI 在 AKS 上创建 Kubernetes 群集

1. 在 Azure Kubernetes 服务中创建 Kubernetes 群集。 以下命令在 wingtiptoys-kubernetes 资源组中创建 kubernetes 群集（将 wingtiptoys-akscluster 作为群集名称，附加了 Azure 容器注册表 `wingtiptoysregistry`，并将 wingtiptoys-kubernetes 作为 DNS 前缀     ）：

   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \
    --attach-acr wingtiptoysregistry \
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```

   此命令可能需要一段时间才能完成。

1. 使用 Azure CLI 安装 `kubectl`。 Linux 用户可能必须将 `sudo` 作为此命令的前缀，因为它将 Kubernetes CLI 部署到 `/usr/local/bin`。

   ```azurecli
   az aks install-cli
   ```

1. 下载群集配置信息，以便从 Kubernetes Web 界面和 `kubectl` 管理群集。 

   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>将映像部署到 Kubernetes 群集

本教程使用 `kubectl` 部署应用，以便你可通过 Kubernetes Web 界面浏览部署。

### <a name="deploy-with-kubectl"></a>使用 kubectl 进行部署

1. 打开命令提示符。

1. 使用 `kubectl run` 命令在 Kubernetes 群集中运行容器。 指定 Kubernetes 中应用的服务名称和完整映像名称。 例如：

   ```bash
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```

   在此命令中：

   * 容器名称 `gs-spring-boot-docker` 会立即在 `run` 命令后指定

   * `--image` 参数将组合的登录服务器和映像名称指定为 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. 使用 `kubectl expose` 命令外部公开你的 Kubernetes 群集。 指定你的服务名称、用于访问应用的公开 TCP 端口以及应用侦听的内部目标端口。 例如：

   ```bash
   kubectl expose pod gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```

   在此命令中：

   * 容器名称 `gs-spring-boot-docker` 紧跟在 `expose pod` 命令后指定。

   * `--type` 参数指定此群集使用负载均衡器。

   * `--port` 参数指定面向公众的 TCP 端口 80。 在此端口访问应用。

   * `--target-port` 参数指定内部 TCP 端口 8080。 负载均衡器在此端口将请求转发到你的应用。

1. 将应用部署到群集后，请查询外部 IP 地址并在 Web 浏览器中打开：

   ```bash
   kubectl get services -o=jsonpath='{.items[*].status.loadBalancer.ingress[0].ip}'
   ```

   ![在 Azure 上浏览示例应用][SB02]

### <a name="deploy-with-the-kubernetes-web-interface"></a>使用 Kubernetes Web 界面进行部署

1. 打开命令提示符。

1. 在默认浏览器中打开 Kubernetes 群集的配置网站：

   ```azurecli
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

> [!IMPORTANT]
> 如果 AKS 群集使用 RBAC，则必须先创建 *ClusterRoleBinding* ，然后才能正确访问仪表板。 默认情况下，Kubernetes 仪表板是使用最小读取访问权限部署的，并且显示 RBAC 访问错误。 Kubernetes 仪表板当前不支持使用用户提供的凭据来确定访问权限级别，而是使用授予给服务帐户的角色。 群集管理员可以选择向 *kubernetes-dashboard* 服务帐户授予更多访问权限，但这可能会导致需要进行权限提升。 还可以集成 Azure Active Directory 身份验证来提供更精细的访问权限级别。
>
> 若要创建绑定，请使用 [kubectl create clusterrolebinding] 命令。 下面的示例演示如何创建一个示例绑定，但是，此示例绑定不会应用任何其他身份验证组件，并可能导致使用不安全。 Kubernetes 仪表板将对有权访问该 URL 的任何人开放。 请勿公开 Kubernetes 仪表板。
>
> ```bash
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
>
> 有关使用不同身份验证方法的详细信息，请参阅有关[仪表板身份验证]的 Kubernetes 仪表板 wiki。

1. 在浏览器中打开 Kubernetes 配置网站后，选择“部署容器化应用”的链接  ：

   ![Kubernetes 配置网站，其中显示“没有任何内容要在此处显示”消息][KB01]

1. 显示“资源创建”页时，请指定以下选项  ：

   a. 选择“创建应用”。 

   b. 对于“应用名称”，请输入 Spring Boot 应用程序名称，例如 gs-spring-boot-docker。

   c. 对于“容器映像”，请输入之前的登录服务器和容器映像，例如 wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest。

   d. 选择“外部”服务   。

   e. 在“端口”和“目标端口”文本框中指定外部和内部端口   。

   ![Kubernetes 配置网站 - “创建应用”页面][KB02]

1. 选择“部署”对容器进行部署  。

   ![Kubernetes 部署][KB05]

1. 部署应用程序后，Spring Boot 应用程序将列在“服务”下方  。

   ![Kubernetes 网站 - 服务列表][KB06]

1. 若选择“外部终结点”的链接，则可看见 Spring Boot 应用程序在 Azure 上运行  。

   ![Kubernetes 网站 - 服务列表，其中外部终结点突出显示][KB07]

   ![在 Azure 上浏览示例应用][SB02]

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](./index.yml)

### <a name="additional-resources"></a>其他资源

有关在 Azure 上使用 Spring Boot 的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Linux 的 Azure 应用服务上](deploy-spring-boot-java-app-on-linux.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

有关使用 Visual Studio Code 将 Java 应用程序部署到 Kubernetes 的详细信息，请参阅 [Visual Studio Code Java 教程]。

有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅[Docker 上的 Spring Boot 入门]。

以下链接提供了创建 Spring Boot 应用程序的详细信息：

* 有关创建简单 Spring Boot 应用程序的详细信息，请参阅 https://start.spring.io/ 中的 Spring Initializr。

以下链接提供了将 Kubernetes 与 Azure 配合使用的详细信息：

* [Azure Kubernetes 服务中的 Kubernetes 群集入门](/azure/aks/intro-kubernetes)

有关使用 Kubernetes 命令行接口的详细信息，请在 <https://kubernetes.io/docs/user-guide/kubectl/> 处参阅 **kubectl** 用户指南。

Kubernetes 网站中有多篇文章讨论有关在私有注册表中使用映像的信息：

* [为 Pod 配置服务帐户]
* [命名空间]
* [从私有注册表拉取映像]

有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。

有关使用 Azure Dev Spaces 直接在 Azure Kubernetes 服务 (AKS) 中迭代运行和调试容器的详细信息，请参阅[通过 Java 开始使用 Azure Dev Spaces]

<!-- URL List -->
[kubectl create clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[仪表板身份验证]: https://github.com/kubernetes/dashboard/wiki/Access-control
[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Kubernetes 服务 (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[面向 Java 开发人员的 Azure]: ../index.yml
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service/tutorial-custom-container
[Docker]: https://www.docker.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[为 Pod 配置服务帐户]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空间]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[从私有注册表拉取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java 教程]: https://code.visualstudio.com/docs/java/java-kubernetes/
[通过 Java 开始使用 Azure Dev Spaces]: /azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
