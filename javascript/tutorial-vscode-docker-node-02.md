---
title: 通过 Visual Studio Code 使用容器注册表
description: 教程第 2 部分：使用容器注册表
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: e6dde135a2e6482284488fb83d9f811b02249c4d
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740547"
---
# <a name="use-a-container-registry"></a>使用容器注册表

[上一步：简介和先决条件](tutorial-vscode-docker-node-01.md)

在此步骤中，将为应用映像设置合适的容器注册表。 然后，支持容器的托管服务（如 Azure 应用服务）将从注册表中提取映像。

本教程使用 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/) (ACR)，这是用于映像的专用、安全的托管注册表。 但是，此处显示的工具和过程也可以与其他注册表（例如 [Docker Hub](https://hub.docker.com/)）一起使用。

## <a name="create-an-azure-container-registry"></a>创建 Azure 容器注册表

1. 在 Visual Studio Code 中，按 <kbd>F1</kbd> 键打开命令面板。 

1. 在搜索框中键入“注册表”，选择“Azure 容器注册表:  创建注册表”。

   ![VS Code 中的 Docker 资源管理器](media/deploy-containers/docker-create-registry.jpg)

1. 在提示中提供以下值...

    - **注册表名称**在 Azure 中必须唯一，并且包含 5-50 个字母数字字符。
    - 选择“基本”  作为 **SKU**。
    - **资源组**仅在订阅中必须唯一。
    - 在位置“位置”  中，选择靠近你的区域。

    Visual Studio Code 会启动在 Azure 中创建注册表的过程。 完成后，会出现如下所示的通知，确认注册表已成功创建。

   ![Visual Studio Code 中的一个表明已创建注册表的确认通知](media/deploy-containers/registry-created.jpg)

1. 打开 **Docker** 资源管理器，确保刚设置的注册表终结点在“注册表”  下可见：

   ![验证注册表是否显示在 Docker 资源管理器中](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>登录到 Azure 容器注册表

虽然可以在 Docker 扩展中看到自己的 Azure 注册表，但在登录到 Azure 容器注册表 (ACR) 之前，无法向其推送映像。

1. 按“Ctrl + `”，在 VS Code 中打开集成终端。<kbd></kbd> 

1. 执行以下 Azure CLI 命令，登录到 ACR。 将“<your-registry-name>”替换为刚创建的注册表的名称。

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [我已创建了注册表](tutorial-vscode-docker-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
