---
title: include 文件 tutorial-azure-web-app-mongodb-03.md
description: include 文件 tutorial-azure-web-app-mongodb-03.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9d6fb0beaa19503541196c72ecbdae52a642648b
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755536"
---
本教程的这一部分将示例应用程序部署到 Azure。 然后可以在浏览器中查看远程运行的应用。 

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](../azure-sign-in.md)]

## <a name="create-web-app-resource"></a>创建 Web 应用资源

使用 Visual Studio Code 扩展创建应用服务资源，并将 Web 应用部署到资源。

1. 导航到 Azure 资源管理器。 右键单击该订阅，然后选择 `Create new web app...`。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/create-web-app-with-extension.png" alt-text="Visual Studio Code 的部分屏幕截图 - 使用 Azure 应用服务扩展创建 Web 应用。":::

1. 使用下表按照提示进行操作，理解如何使用你的值。

    |属性|“值”|
    |--|--|
    |为新 Web 应用输入全局唯一名称。| 为应用服务资源输入一个值，如 `web-app-with-mongodb-YOUR-NAME`。 将 `<YOUR-NAME>` 替换为你的名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。|
    |为 Linux 应用选择运行时。|选择 `Node 12 LTS`。|

1. 应用创建过程完成后，将在 Visual Studio Code 的右下角显示一条状态消息，可选择 `Deploy` 或 `View output`。 选择 `Deploy`。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-extension-create-web-app-deploy-web-app.png" alt-text="Visual Studio Code 的部分屏幕截图 - 使用 Azure 应用服务扩展创建 Web 应用。":::

    如果状态消息不再可见，则可以通过选择 Azure 资源管理器进行部署，然后右键单击资源名称，并选择“部署到 Web 应用…”。

1. 在部署过程中，可通过选择显示的通知来查看“输出窗口”。  该操作将显示部署的滚动状态。 

1. 部署完成后，将显示一条通知。 选择“流式传输日志”以查看滚动日志。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-deployed.png" alt-text="Visual Studio Code 的部分屏幕截图 - 使用 Azure 应用服务扩展创建 Web 应用。":::

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-stream-logs.png" alt-text="Visual Studio Code 的部分屏幕截图 - 使用 Azure 应用服务扩展创建 Web 应用。":::    

1. 在浏览器中打开该网站，将文本 `YOUR-RESOURCE_NAME` 替换为你自己的资源名称：`https://YOUR-RESOURCE_NAME.azurewebsites.net`。
    
    网站现在可以在本地和远程运行，但仍无法连接到数据库。 

## <a name="want-to-know-more"></a>想要了解更多信息？

初始 Web 服务配置为在端口 8080 上运行并公开可用。 这些类型的网站设置是可配置的。
* [应用设置](/azure/app-service/configure-common)
* [身份验证](/azure/app-service/configure-authentication-provider-microsoft)
* [按网络限制访问](/azure/app-service/app-service-ip-restrictions)

使用此应用服务扩展将网站部署到 Azure 云时，可能需要详细了解如何[配置该部署](https://github.com/microsoft/vscode-azureappservice/wiki/Configuring-Zip-Deployment#additional-zip-deploy-configuration-settings)
