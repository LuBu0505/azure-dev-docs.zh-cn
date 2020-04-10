---
title: 从 Visual Studio Code 部署 Node.js 应用的容器映像
description: 教程第 5 部分，将映像部署到 Azure 应用服务
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 487110258ed3302e781cfa24a5ae9f518ebb3bda
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740659"
---
# <a name="deploy-the-image-to-azure-app-service"></a>将映像部署到 Azure 应用服务

[上一步：创建应用映像](tutorial-vscode-docker-node-04.md)

在此步骤中，你将直接从 Visual Studio Code 将推送到注册表的映像部署到 [Azure 应用服务](https://azure.microsoft.com/services/app-service/)。

## <a name="enable-admin-access-on-the-registry"></a>在注册表上启用管理员访问权限

若要将映像部署到 Web 应用程序，需要在 Azure 门户中的注册表上启用“管理员”访问权限。

1. 在 **Docker** 资源管理器中，右键单击注册表名称，然后选择“在门户中打开”。 

    ![VS Code 中的“在门户中打开”命令](media/deploy-containers/open-in-portal.png)

    这会在 Azure 门户中打开注册表。

1. 单击侧栏中的“访问密钥”，然后将“管理员用户”设置切换为“启用”。  
    
    ![在 Azure 门户中启用管理员用户设置](media/deploy-containers/access-keys.png)

## <a name="deploy-image"></a>部署映像

1. 在 **DOCKER** 资源管理器中，展开“注册表”  下的映像节点，右键单击 `:latest`，然后选择“将映像部署到 Azure 应用服务”  。

    ![从资源管理器部署](media/deploy-containers/deploy-image-command.png)

1. 出现提示时，为应用服务提供值：

    - 该名称在 Azure 中必须唯一。
    - 选择现有资源组或创建一个新组。 （**资源组**实际上是 Azure 中应用程序资源的命名集合。）
    - 选择现有的应用服务计划或创建新的计划。 （**应用服务计划**定义了托管网站的物理资源。 可以在本教程中使用基本或免费计划层。）

1. 部署完成后，Visual Studio Code 将显示一个通知，其中包含网站 URL：

    ![成功的部署消息](media/deploy-containers/deploy-successful.png)

1. 还可以在 Visual Studio Code 的“输出”  面板的 **Docker** 部分中查看结果：

    ![成功的部署输出](media/deploy-containers/deploy-output.png)

1. 若要浏览部署的网站，可以 **Ctrl**+**Click**“输出”  面板中的 URL。 新的应用服务还出现在 Visual Studio Code 的 **AZURE** 资源管理器中的“应用服务”  部分下，可以在其中右键单击网站并选择“浏览网站”  。

> [!div class="nextstepaction"]
> [我的站点位于 Azure 上](tutorial-vscode-docker-node-06.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
