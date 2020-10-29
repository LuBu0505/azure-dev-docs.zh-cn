---
title: 使用 Node.js 创建 Azure 服务主体
description: 了解如何通过 Node.js 和 JavaScript 使用服务主体身份验证
ms.topic: how-to
ms.date: 09/30/2020
ms.custom: devx-track-js
ms.openlocfilehash: 88724ca9ecdb24088e437a5b9b407fbdde69650a
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437156"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>创建适用于 Node.js 的 Azure 服务主体

当有某个应用需要访问资源时，可为应用设置一个标识，并使用其自身的凭据对应用进行身份验证。 此标识称为服务主体。  实质上，这是为 Azure Active Directory 帐户创建了要提供给 SDK 用于身份验证的密钥，这样就不需要用户的干预或提供用户名/密码。

使用服务主体方法可以：
- 将权限分配给应用标识，这些权限不同于自己的权限。 通常情况下，这些权限仅限于应用需执行的操作。
- 运行无人参与的脚本时，使用证书进行身份验证。

本主题介绍三种创建服务主体的方法。

- Azure 门户
- Azure CLI 2.0

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="create-a-service-principal-using-the-azure-portal"></a>使用 Azure 门户创建服务主体

请遵循[使用门户创建可访问资源的 Azure Active Directory 应用程序和服务主体](/azure/active-directory/develop/howto-create-service-principal-portal)主题中所述的步骤来生成服务主体。

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>使用 Azure CLI 2.0 创建服务主体

可执行以下步骤，使用 [Azure CLI 2.0](/cli/azure/install-az-cli2) 创建服务主体：

1. 下载 [Azure CLI 2.0](/cli/azure/install-az-cli2)。

2. 打开终端窗口并键入命令 `az login`以启动登录过程。

3. 调用 `az login` 可生成一个 URL 和一个代码。 浏览到指定的 URL，输入该代码，使用 Azure 标识登录（如果已登录，则此过程可自动完成）。 然后，即可通过 CLI 访问帐户。

4. 使用 `az account list` 命令获取订阅和租户 ID。 使用任何 Azure 包时，你将需要用到它们。 下面显示的是此命令的输出示例：

    ```shell
    {
    "cloudName": "AzureCloud",
    "id": "<subscriptionId>",
    "isDefault": true,
    "name": "<subscriptionName>",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<tenantId>",
        "user": {
            "name": "hello@example.com",
            "type": "user"
        }
    }
    ```

5. 按照主题中所述的步骤（[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)）来生成服务主体。 输出中的 JSON 对象将包含进行 Azure 身份验证所需的信息。


## <a name="using-the-service-principal"></a>使用服务主体

如果拥有服务主体，请查看[对适用于 JavaScript 的 Azure 管理模块进行身份验证](./node-sdk-azure-authenticate.md)主题，了解如何创建凭据对象（可使用该对象通过 Azure Active Directory 对客户端进行身份验证）。
