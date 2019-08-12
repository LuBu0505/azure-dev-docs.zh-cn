---
title: 使用 Azure Pipelines 对 MicroProfile 应用进行 CI/CD
description: 了解如何使用 Azure Pipelines 设置一个 CI/CD 发布周期，以便将 MicroProfile 应用部署到用于容器的 Azure Web 应用实例。
services: devops
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 07/31/2019
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: f75a4f32c56b949f7ea4f5e87863a83cbdf4b5d8
ms.sourcegitcommit: bf64ca31b2d4aea3f5c9b36d7c5ed7bde266584a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68755907"
---
# <a name="cicd-for-microprofile-apps-using-azure-pipelines"></a>使用 Azure Pipelines 对 MicroProfile 应用进行 CI/CD

本教程介绍如何轻松地设置 Azure Pipelines 持续集成和持续部署 (CI/CD) 发布周期，以便将 [MicroProfile](http://microprofile.io) Java EE 应用程序部署到用于容器的 Azure Web 应用。 本教程中的 MicroProfile 应用使用 [Payara Micro](https://www.payara.fish/payara_micro) 基础映像来创建 WAR 文件。 

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
开始 Azure Pipelines 容器化过程的方式是：先生成一个 Docker 映像，然后将该容器映像推送到 Azure 容器注册表 (ACR)。 完成此过程的方式是：先创建一个 Azure Pipelines 发布管道，然后将容器映像部署到 Web 应用。

## <a name="prerequisites"></a>先决条件

1. 在 [Azure 门户](https://portal.azure.com)中创建 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry)。
   
1. 在 Azure 门户中创建[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)。 对于“OS”，请选择“Linux”；对于“配置容器”，请选择“快速入门”作为“映像源”。       
   
1. 复制并保存位于 [https://github.com/Azure-Samples/microprofile-hello-azure](https://github.com/Azure-Samples/microprofile-hello-azure) 的示例 GitHub 存储库中的克隆 URL。
   
1. 注册或登录到 [Azure DevOps](https://dev.azure.com) 组织，创建一个新[项目](/vsts/organizations/projects/create-project)。 
   
1. 将示例 GitHub 存储库导入 Azure Repos：
   
   1. 在 Azure DevOps 项目页的左侧导航栏中选择“Repos”。 
   1. 在“导入存储库”下，选择“导入”。   
   1. 在“克隆 URL”下输入保存的 Git 克隆 URL，然后选择“导入”。  
  
## <a name="create-a-build-pipeline"></a>创建生成管道

每次在 Java EE 源应用中进行提交时，Azure Pipelines 中的持续集成生成管道就会自动执行所有生成任务。 在此示例中，Azure Pipelines 使用 Maven 来生成 Java MicroProfile 项目。

1. 在 Azure DevOps 项目页的左侧导航栏中选择“管道” > “生成”。   
   
1. 选择“新建管道”  。
   
1. 选择“在没有 YAML 的情况下使用经典编辑器创建一个管道”。  
   
1. 确保项目名称和导入的 GitHub 存储库显示在字段中，然后选择“继续”。 
   
1. 从模板列表中选择“Maven”，然后选择“应用”   。
   
1. 在右窗格中，确保“托管 Ubuntu 1604”显示在“代理池”下拉列表中  
   
   > [!NOTE]
   > 此设置让 Azure Pipelines 知道要使用哪个生成服务器。  也可使用专用的自定义生成服务器。
   
1. 若要配置用于持续集成的管道，请在左窗格中选择“触发器”选项卡，然后选中“启用持续集成”旁边的复选框。    
   
1. 在页面顶部，选择“保存并排队”旁边的下拉列表，然后选择“保存”  。  

   ![启用持续集成](media/cicd-microprofile/continuous-integration.png)

## <a name="create-a-docker-build-image"></a>创建 Docker 生成映像

Azure Pipelines 将 Dockerfile 与 Payara Micro 提供的基础映像配合使用，以便创建 Docker 映像。  

1. 选择“任务”选项卡，然后选择  “代理作业 1”旁边的加号 **+** ，  以便添加任务。
   
   ![添加新任务](media/cicd-microprofile/add-task.png)
   
1. 从右窗格的模板列表中选择“Docker”，然后选择“添加”   。 
   
1. 在左窗格中选择“buildAndPush”  ，在右窗格的“显示名称”字段中输入说明。 
   
1. 在“容器存储库”  下，选择“容器注册表”字段旁边的“新建”。   
   
1. 填充“添加 Docker 注册表服务连接”对话框，如下所示： 
   
   |字段|值|
   |---|---|
   |**注册表类型**|选择“Azure 容器注册表”  。|
   |**连接名称**|输入连接的名称。|
   |**Azure 订阅**|从下拉列表中选择自己的 Azure 订阅，并在必要时选择“授权”。 |
   |**Azure 容器注册表**|从下拉列表中选择 Azure 容器注册表名称。| 
   
1. 选择“确定”  。
   
   ![添加 Docker 注册表服务连接](media/cicd-microprofile/dockerconnection.png)
   
   > [!NOTE]
   > 如果使用 Docker 中心或其他注册表，请选择“Docker 中心”或“其他”，而不是“注册表类型”旁边的“Azure 容器注册表”。     然后，为容器注册表提供凭据和连接信息。
   
1. 在“命令”下的“命令”下拉列表中选择“生成”。   
   
1. 选择“Dockerfile”字段旁边的省略号“...”，浏览到 GitHub 存储库中的“Dockerfile”并将其选中，然后选择“确定”。     
   
   ![选择 Dockerfile](media/cicd-microprofile/selectdockerfile.png)
   
1. 在“标记”下的新行中输入“最新”。   
   
1. 在页面顶部，选择“保存并排队”旁边的下拉列表，然后选择“保存”  。  

## <a name="push-the-docker-image-to-acr"></a>将 Docker 映像推送到 ACR

Azure Pipelines 将 Docker 映像推送到 Azure 容器注册表，并使用它将 MicroProfile API 应用作为容器化 Java Web 应用运行。

1. 由于你在 Azure Pipelines 中使用 Docker，因此请重复[创建 Docker 生成映像](#create-a-docker-build-image)下的步骤，以便创建另一 Docker 模板。 这次请在“命令”下拉菜列表中选择“推送”。  
   
1. 选择“保存并排队”旁边的下拉列表，然后选择“保存并排队”  。  
   
1. 在“运行管道”弹出窗口中，确保在“代理池”下选择“托管 Ubuntu 1604”，然后选择“保存并运行”。     
   
1. 在生成完成后，即可选择“生成”页上的超链接来验证生成是否成功并查看其他详细信息。 
   
   ![选择生成超链接](media/cicd-microprofile/checkbuild.png)

## <a name="create-a-release-pipeline"></a>创建发布管道

生成成功以后，Azure Pipelines 持续发布管道就会自动触发到目标环境（例如 Azure）的部署。 可以为开发、测试、过渡或生产之类的环境创建发布管道。

1. 在 Azure DevOps 项目页的左侧导航栏中选择“管道” > “发布”。   
   
1. 选择“新建管道”  。
   
1. 在模板列表中选择“将 Java 应用部署到 Azure 应用服务”，然后选择“应用”。   
   
   ![选择“将 Java 应用部署到 Azure 应用服务”模板](media/cicd-microprofile/selectreleasetemplate.png)
   
1. 在弹出窗口中，将“阶段 1”更改为某个阶段名称（例如“开发”、“测试”、“过渡”或“生产”），然后关闭窗口。      
   
1. 在左窗格中的“项目”下选择“添加”，将项目从生成管道链接到发布管道。   
   
1. 在右窗格的“源(生成管道)”下的下拉列表中选择生成管道，然后选择“添加”。  
   
   ![添加生成项目](media/cicd-microprofile/addbuildartifact.png)
   
1. 选择“生产”阶段中的超链接，以便**查看阶段任务**。 
   
   ![选择阶段名称](media/cicd-microprofile/viewstagetasks.png)
   
1. 在右窗格中填充表单，如下所示：
   
   |字段|值|
   |---|---|
   |**Azure 订阅**|从下拉列表中选择自己的 Azure 订阅。|
   |**应用类型**|从下拉列表中选择“用于容器的 Web 应用(Linux)”。 |
   |**应用服务名称**|从下拉列表中选择自己的 ACR 实例。|
   |**注册表或命名空间**|在字段中输入 ACR 名称。 例如，输入 *mymicroprofileregistry.azure.io*。
   |**存储库**|输入包含 Docker 映像的存储库。| 
   
   ![配置阶段任务](media/cicd-microprofile/configurestage.png)
   
1. 在左窗格中选择“将 War 部署到 Azure 应用服务”  ，并在右窗格的“标记”字段中输入“最新”标记。   
   
1. 在左窗格中选择“在代理上运行”  ，并在右窗格的“代理池”下拉列表中选择“托管 Ubuntu 1604”。   

## <a name="set-up-environment-variables"></a>设置环境变量

添加并定义环境变量，以便在部署期间连接到容器注册表。

1. 选择“变量”选项卡，然后选择“添加”，以便为容器注册表 URL、用户名和密码添加以下变量。   
   
   |Name|值|
   |---|---|
   |*registry.url*|输入容器注册表 URL。 例如：*https:\//mymicroprofileregistry.azure.io*|
   |*registry.username*|输入注册表用户名。|
   |*registry.password*|输入注册表密码。 为安全起见，请选择锁状图标，让密码值处于隐藏状态。|
   
   ![添加变量](media/cicd-microprofile/addvariables.png)
   
1. 在“任务”选项卡上  ，选择左窗格中的“将 War 部署到 Azure 应用服务”。  
   
1. 在右窗格中展开“应用程序和配置设置”  ，然后选择“应用设置”字段旁边的省略号“...”  。 
   
1. 在“应用设置”弹出窗口中选择“添加”，定义并分配应用设置变量：  
   
   |Name|值|
   |---|---|
   |*DOCKER_REGISTRY_SERVER_URL*|*$(registry.url)*|
   |*DOCKER_REGISTRY_SERVER_USERNAME*|*$(registry.username)*|
   |*DOCKER_REGISTRY_SERVER_PASSWORD*|*$(registry.password)*|
   
1. 选择“确定”  。
   
   ![添加并设置变量](media/cicd-microprofile/appsettings.png)
   
## <a name="set-up-continuous-deployment"></a>设置连续部署 

若要启用持续部署，请执行以下操作： 

1. 在“管道”选项卡的“项目”下，选择生成项目中的闪电图标  。  
   
1. 在右窗格中，将“持续部署触发器”设置为“启用”。  
   
1. 选择右上角的“保存”，然后再次选择“保存”   。 
   
   ![启用持续部署触发器](media/cicd-microprofile/setcontinuousdeployment.png)
   
## <a name="deploy-the-java-app"></a>部署 Java 应用

启用 CI/CD 以后，修改源代码就会自动创建并运行生成和发布。 也可手动创建并运行发布，如下所示：

1. 在发布管道页的右上角，选择“创建发布”。 
   
1. 在“创建新发布”页的“触发器从自动改为手动的阶段”下选择阶段名称。   
   
1. 选择“创建”  。 
   
1. 选择发布名称，将鼠标悬停在阶段上或将其选中，然后选择“部署”。  
   
## <a name="test-the-java-web-app"></a>测试 Java Web 应用

部署成功完成以后，测试 Web 应用。 

1. 从 Azure 门户复制 Web 应用 URL。
   
   ![Azure 门户中的应用服务应用](media/cicd-microprofile/portalurl.png)
   
1. 在 Web 浏览器中输入运行应用所需的 URL。 网页会显示 **Hello Azure!**
   
   ![Java Web 应用页面](media/cicd-microprofile/webapp.png)

