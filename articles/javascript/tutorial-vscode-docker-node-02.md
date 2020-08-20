---
title: 通过 Visual Studio Code 使用容器注册表
description: 教程第 2 部分：使用容器注册表
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: c0eaa345af7d439cda47672f597ce1d82e78a355
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218418"
---
# <a name="use-a-container-registry"></a>使用容器注册表

[上一步：简介和先决条件](tutorial-vscode-docker-node-01.md)

在此步骤中，将为应用映像设置合适的容器注册表。 然后，支持容器的托管服务（如 Azure 应用服务）就可以从注册表中提取映像。

本教程使用 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)，这是用于映像的专用且安全的托管注册表。 但是，此处显示的工具和过程也可以与其他注册表（例如 [Docker Hub](https://hub.docker.com/)）一起使用。

## <a name="create-an-azure-container-registry"></a>创建 Azure 容器注册表

1. 在 Visual Studio Code 中选择 F1  键，打开命令面板。

1. 在搜索框中输入“注册表”。  从结果中选择“Azure 容器注册表:  创建注册表”。

   ![Visual Studio Code 中的 Docker 资源管理器](media/deploy-containers/docker-create-registry.jpg)

1. 输入或选择下列值：

    - 在“注册表名称”中输入一个名称，该名称在 Azure 中独一无二，包含 5-50 个字母数字字符。 
    - 在“SKU”中，  选择“基本”。 
    - 在“资源组”中输入一个在订阅中独一无二的值。 
    - 在“位置”中，  选择离你近的区域。

    Visual Studio Code 会启动在 Azure 中创建注册表的过程。 完成后，会看到如下所示的通知。 此通知确认已成功创建注册表。

   ![Visual Studio Code 中的通知确认已创建注册表](media/deploy-containers/registry-created.jpg)

1. 打开 Docker  资源管理器。 确保刚设置的注册表终结点在“注册表”下可见。 

   ![验证注册表是否显示在 Docker 资源管理器中](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>登录到 Azure 容器注册表

虽然可以在 Docker 扩展中看到自己的 Azure 注册表，但在登录到容器注册表之前，无法向其推送映像。

1. 在 Visual Studio 中选择“Ctrl+`”，打开集成终端。  

1. 运行以下 Azure CLI 命令，登录到容器注册表。 将 `<your-registry-name>` 替换为已创建的注册表的名称。

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [我创建了注册表](tutorial-vscode-docker-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
 