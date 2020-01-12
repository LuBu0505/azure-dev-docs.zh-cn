---
title: 通过 Visual Studio Code 使用容器注册表
description: 教程第 2 部分：使用容器注册表
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: c5e9ff3cd803ef4d57408199682c71e4b57f2d77
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191008"
---
# <a name="use-a-container-registry"></a>使用容器注册表

[上一步：简介和先决条件](tutorial-vscode-docker-node-01.md)

在此步骤中，将为应用映像设置合适的容器注册表。 然后，支持容器的托管服务（如 Azure 应用服务）将从注册表中提取映像。

本教程使用 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/) (ACR)，这是用于映像的专用、安全的托管注册表。 但是，此处显示的工具和过程也可以与其他注册表（例如 [Docker Hub](https://hub.docker.com/)）一起使用。

## <a name="create-an-azure-container-registry"></a>创建 Azure 容器注册表

1. 登录到 [Azure 门户](https://portal.azure.com)，然后选择“创建资源”  。

    ![在 Azure 门户上创建新资源](media/deploy-containers/portal-01a.png)

1. 在下一页上，选择“容器”   >   “容器注册表”。

    ![在 Azure 门户中创建容器注册表](media/deploy-containers/portal-01b.png)

1. 在出现的“创建容器注册表”  表单中，输入适当的值：

    - **注册表名称**在 Azure 中必须唯一，并且包含 5-50 个字母数字字符。
    - 在**订阅**中选择自己的订阅。
    - **资源组**仅在订阅中必须唯一。
    - 在位置“位置”  中，选择靠近你的区域。
    - 将**管理员用户**设置为“启用”  。
    - 选择“基本”  作为 **SKU**。

    ![容器注册表表单的值](media/deploy-containers/portal-02.png)

1. 选择“创建”  以创建注册表。

1. 创建注册表后，请在门户上打开通知，然后对注册表选择“转到资源”  ：

    ![打开新创建的注册表资源](media/deploy-containers/portal-03.png)

1. 在注册表页上，选择“访问密钥”  并记下管理员凭据：

    ![Azure 门户上的注册表的注册表凭据](media/deploy-containers/portal-04.png)

1. 在命令提示符下或终端中，使用以下命令登录 Docker，将 `<registry_name>` 替换为注册表名称，并将 `<username>` 和 `<password>` 替换为 Azure 门户中为管理员用户显示的值：

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

    为提高安全性，请使用 `--password-stdin` 而不是 `-p <password>`，然后在出现提示时粘贴密码。

1. 在 Visual Studio Code 中，打开 **Docker** 资源管理器，并确保刚刚设置的注册表终结点在**注册表**下可见：

    ![验证注册表是否显示在 Docker 资源管理器中](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [我已创建了注册表](tutorial-vscode-docker-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
