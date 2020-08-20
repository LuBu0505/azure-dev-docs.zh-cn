---
title: 使用 Jenkins 和 Azure CLI 将应用部署到 Azure Spring Cloud
description: 了解如何使用持续集成和部署管道中的 Azure CLI 将微服务部署到 Azure Spring Cloud 服务
keywords: jenkins, azure, devops, azure spring cloud, azure cli
ms.topic: tutorial
ms.date: 08/10/2020
ms.custom: devx-track-jenkins,devx-track-azurecli
ms.openlocfilehash: 769067c03fce08e462364314d2e4712ab6ffe155
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240829"
---
# <a name="tutorial-deploy-apps-to-azure-spring-cloud-using-jenkins-and-the-azure-cli"></a>教程：使用 Jenkins 和 Azure CLI 将应用部署到 Azure Spring Cloud

[Azure Spring Cloud](/spring-cloud/spring-cloud-overview) 是一种完全托管的微服务开发，内置了服务发现和配置管理。 借助此服务，可以轻松地将基于 Spring Boot 的微服务应用程序部署到 Azure。 本教程演示如何使用 Jenkins 中的 Azure CLI 自动完成 Azure Spring Cloud 的持续集成和交付 (CI/CD)。

在本教程中，你将完成以下任务：

> [!div class="checklist"]
> * 预配服务实例并启动 Java Spring 应用程序
> * 准备 Jenkins 服务器
> * 在 Jenkins 管道中使用 Azure CLI 生成并部署微服务应用程序 

>[!Note]
> Azure Spring Cloud 目前以公共预览版的形式提供。 使用公共预览版产品/服务，客户可以在产品/服务正式发布之前体验新功能。  公共预览功能和服务并非供生产使用。  有关预览期间支持的详细信息，请参阅[常见问题解答](https://azure.microsoft.com/support/faq/)或提交[支持请求](/azure-supportability/how-to-create-azure-support-request)。

## <a name="prerequisites"></a>必备知识

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

**Jenkins**：[在 Linux VM 上安装 Jenkins](configure-on-linux-vm.md)

**GitHub 帐户**：如果没有 GitHub 帐户，请在开始前创建一个[免费帐户](https://github.com/)。

## <a name="provision-a-service-instance-and-launch-a-java-spring-application"></a>预配服务实例并启动 Java Spring 应用程序

我们使用 [Piggy 指标](https://github.com/Azure-Samples/piggymetrics)作为 Microsoft 服务应用程序示例，按照[快速入门：使用 Azure CLI 启动 Java Spring 应用程序](/spring-cloud/spring-cloud-quickstart-launch-app-cli.md)中的步骤来预配服务实例并设置应用程序。 如果你已完成同一过程，则可跳到下一部分。 否则，请执行下面提供的 Azure CLI 命令。 请参阅[快速入门：使用 Azure CLI 启动 Java Spring 应用程序](/spring-cloud/spring-cloud-quickstart-launch-app-cli.md)以获取其他背景信息。

本地计算机需满足与 Jenkins 生成服务器相同的先决条件。 请确保安装以下内容，以便生成并部署微服务应用程序：
    * [Git](https://git-scm.com/)
    * [JDK 8](/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 或更高版本](https://maven.apache.org/download.cgi)
    * [已安装 Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.67 或更高版本

1. 安装 Azure Spring Cloud 扩展：

    ```Azure CLI
    az extension add --name spring-cloud
    ```

1. 创建可包含 Azure Spring Cloud 服务的资源组：

    ```Azure CLI
    az group create --location eastus --name <resource group name>
    ```

1. 预配 Azure Spring Cloud 的实例：

    ```Azure CLI
    az spring-cloud create -n <service name> -g <resource group name>
    ```

1. 将 [Piggy 指标](https://github.com/Azure-Samples/piggymetrics)存储库分叉到你自己的 GitHub 帐户。 在本地计算机中，将存储库克隆到名为 `source-code` 的目录：

    ```bash
    mkdir source-code
    git clone https://github.com/<your GitHub id>/piggymetrics
    ```

1. 设置配置服务器。 请确保将 &lt;your GitHub id&gt; 替换为正确的值。

    ```Azure CLI
    az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/<your GitHub id>/piggymetrics --label config
    ```

1. 生成项目：

    ```bash
    cd piggymetrics
    mvn clean package -D skipTests
    ```

1. 创建三项微服务：**gateway**、**auth-service** 和 **account-service**：

    ```Azure CLI
    az spring-cloud app create --n gateway -s <service name> -g <resource group name>
    az spring-cloud app create --n auth-service -s <service name> -g <resource group name>
    az spring-cloud app create --n account-service -s <service name> -g <resource group name>
    ```

1. 部署应用程序：

    ```Azure CLI
    az spring-cloud app deploy -n gateway -s <service name> -g <resource group name> --jar-path ./gateway/target/gateway.jar
    az spring-cloud app deploy -n account-service -s <service name> -g <resource group name> --jar-path ./account-service/target/account-service.jar
    az spring-cloud app deploy -n auth-service -s <service name> -g <resource group name> --jar-path ./auth-service/target/auth-service.jar
    ```

1. 将公共终结点分配到网关：

    ```Azure CLI
    az spring-cloud app update -n gateway -s <service name> -g <resource group name> --is-public true
    ```

1. 查询网关应用程序以获取 URL，以便验证应用程序是否正在运行。

    ```Azure CLI
    az spring-cloud app show --name gateway | grep url
    ```
    
 1. 导航到上一个命令提供的 URL，以运行 PiggyMetrics 应用程序。

## <a name="prepare-jenkins-server"></a>准备 Jenkins 服务器

在本部分，我们准备 Jenkins 服务器，以便运行一个适合测试的生成项目。 但是，考虑到安全影响，应该使用 [Azure VM 代理](https://plugins.jenkins.io/azure-vm-agents)或 [Azure 容器代理](https://plugins.jenkins.io/azure-container-agents)来运转 Azure 中的代理，以便运行生成项目。 有关详细信息，请参阅有关[在主节点上进行生成所造成的安全影响](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master)的 Jenkins 文章。

### <a name="install-plug-ins"></a>安装插件

1. 登录 Jenkins 服务器。 选择“管理 Jenkins”>“管理插件”。 

1. 在“可用”选项卡上选择以下插件： 

    * [GitHub 集成](https://plugins.jenkins.io/github-pullrequest)
    * [Azure 凭据](https://plugins.jenkins.io/azure-credentials)

    如果这些插件没有显示在列表中，请检查“已安装”  选项卡以查看它们是否已安装。

1. 若要安装插件，请选择“立即下载并在重启后安装”。 

1. 重启 Jenkins 服务器以完成安装。

### <a name="add-your-azure-service-principal-credential-in-jenkins-credential-store"></a>在 Jenkins 凭据存储中添加 Azure 服务主体凭据

1. 部署到 Azure 需要 Azure 服务主体。 有关详细信息，请参阅“部署到 Azure 应用服务”教程中的 [创建服务主体](deploy-from-github-to-azure-app-service.md#create-service-principal)部分。  `az ad sp create-for-rbac` 的输出如下所示：

    ```
    {
        "appId": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "displayName": "xxxxxxxjenkinssp",
        "name": "http://xxxxxxxjenkinssp",
        "password": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "tenant": "xxxxxx--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

1. 在 Jenkins 仪表板中，选择“凭据” > “系统”   。 然后选择“全局凭据(不受限制)”  。

1. 选择“添加凭据”。 

1. 选择“Microsoft Azure 服务主体”作为类型。 

1. 提供以下字段的值：

    - **订阅 ID**：Azure 订阅 ID
    - **客户端 ID**：服务主体 appid
    - **客户端密码**：服务主体密码
    - **租户 ID**：Microsoft 帐户租户 ID
    - **Azure 环境**：选择适合你的环境的值。 例如，对 Azure 全局使用 Azure
    - **ID**：设置为 `azure_service_principal`。 我们将在本文后面的步骤中使用此 ID
    - **说明**：可选填，但建议提供。
    
### <a name="install-maven-and-az-cli-spring-cloud-extension"></a>安装 Maven 和 Az CLI spring-cloud 扩展

示例管道使用 Maven 进行生成，使用 Az CLI 将内容部署到服务实例。 Jenkins 安装完成时，它将创建名为 jenkins 的管理员帐户  。 确保用户 *jenkins* 有权运行 spring-cloud 扩展。

1. 通过 SSH 连接到 Jenkins 主节点。

1. 安装 Maven。

    ```bash
    sudo apt-get install maven
    ```

1. 输入 `az version` 验证是否已安装 Azure CLI。 如果未安装 Azure CLI，请参阅[安装 Azure CLI](/cli/azure/install-azure-cli)。

1. 切换到 `jenkins` 用户：

    ```bash
    sudo su jenkins
    ```

1. 添加 **spring-cloud** 扩展：

    ```bash
    az extension add --name spring-cloud
    ```

## <a name="create-a-jenkinsfile"></a>创建 Jenkinsfile

1. 在你自己的存储库 (https://github.com/&lt ;your GitHub id&gt; /piggymetrics) 的根目录中创建 **Jenkinsfile**。

1. 按如下所示更新文件。 确保替换 \<resource group name> 和 \<service name> 的值 。 将 **azure_service_principal** 替换为正确的 ID（如果你在 Jenkins 中添加凭据时使用了其他值）。

   ```groovy
       node {
         stage('init') {
           checkout scm
         }
         stage('build') {
           sh 'mvn clean package'
         }
         stage('deploy') {
           withCredentials([azureServicePrincipal('azure_service_principal')]) {
             // login to Azure
             sh '''
               az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
               az account set -s $AZURE_SUBSCRIPTION_ID
             '''  
             // Set default resource group name and service name. Replace <resource group name> and <service name> with the right values
             sh 'az configure --defaults group=<resource group name>'
             sh 'az configure --defaults spring-cloud=<service name>'
             // Deploy applications
             sh 'az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar'
             sh 'az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar'
             sh 'az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar'
             sh 'az logout'
           }
         }
       }
   ```

1. 保存并提交更改。

## <a name="create-the-job"></a>创建作业

1. 在 Jenkins 仪表板上，单击“新建项”  。

1. 为作业提供名称 *Deploy-PiggyMetrics* 并选择“管道”  。 单击“确定”。

1. 接下来单击“Pipeline”（管道）  选项卡。

1. 对于“Definition”（定义）  ，选择“Pipeline script from SCM”（来自 SCM 的管道脚本）  。

1. 对于“SCM”  ，选择“Git”  。

1. 输入分叉存储库的 GitHub URL： **https://github.com/&lt ;your GitHub id&gt; /piggymetrics.git**

1. 确保“分支说明符(空白表示‘任意’)”为 * **/Azure** 

1. 保留“脚本路径”  为 **Jenkinsfile**

1. 单击“保存” 

## <a name="validate-and-run-the-job"></a>验证并运行作业

在运行作业之前，让我们将登录输入框中的文本更新为“输入登录 ID”  。

1. 在你自己的存储库的 **/gateway/src/main/resources/static/** 中打开 `index.html`

1. 搜索“输入登录名”，将其更新为“输入登录 ID”

    ```HTML
    <input class="frontforms" id="frontloginform" name="username" placeholder="enter login ID" type="text" autocomplete="off"/>
    ```

1. 提交更改

1. 在 Jenkins 中手动运行作业。 在 Jenkins 仪表板上单击作业 *Deploy-PiggyMetrics*，然后单击“立即生成”。 

作业完成后，导航到**网关**应用程序的公共 IP，并验证应用程序是否已更新。 

![更新的 Piggy Metrics](./media/deploy-to-azure-spring-cloud-using-azure-cli/piggymetrics.png)

## <a name="clean-up-resources"></a>清理资源

如果不再需要本文中创建的资源，请将其删除：

```bash
az group delete -y --no-wait -n <resource group name>
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Jenkins](/azure/jenkins/)