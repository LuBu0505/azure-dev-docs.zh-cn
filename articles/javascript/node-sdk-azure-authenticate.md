---
title: 使用用于 Node.js 的 Azure 管理模块进行身份验证
description: 在用于 Node.js 的 Azure 管理模块中使用服务主体进行身份验证
ms.topic: article
ms.date: 06/17/2017
ms.custom: devx-track-javascript
ms.openlocfilehash: 1d3f0d2930d397c24177f0cee7a9e276c4df9d67
ms.sourcegitcommit: b03cb337db8a35e6e62b063c347891e44a8a5a13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91110428"
---
# <a name="authenticate-with-the-azure-modules-for-nodejs"></a>使用用于 Node.js 的 Azure 模块进行身份验证

在被实例化时，所有服务 API 需要通过 `credentials` 对象进行身份验证。 可以使用三种方法通过用于 Node.js 的 Azure SDK 进行身份验证和创建所需的凭据：

- 基本身份验证
- 交互式登录
- 服务主体身份验证

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="basic-authentication"></a>基本身份验证

若要使用 Azure 帐户凭据以编程方式进行身份验证，请使用 `loginWithUsernamePassword` 函数。 以下 JavaScript 代码片段演示如何使用存储为环境变量的凭据执行基本身份验证。

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithUsernamePassword(process.env.AZURE_USER,
                                 process.env.AZURE_PASS,
                                 (err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials,
                                                             '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="interactive-login"></a>交互式登录

交互式登录提供一个链接和一个代码让用户从浏览器进行身份验证。 如果同一个脚本使用多个帐户或者希望用户干预，可使用此方法。

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.interactiveLogin((err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="service-principal-authentication"></a>服务主体身份验证

[交互式登录](#interactive-login)是最简单的身份验证方法。 但是，在使用 Node.js SDK 时，我们可能想要使用服务主体身份验证，而不是提供自己的帐户凭据。 [使用 Node.js 创建 Azure 服务主体](./node-sdk-azure-authenticate-principal.md)主题介绍了创建（和使用）服务主体的各种方法。

## <a name="next-steps"></a>后续步骤

* [从 Visual Studio Code 将静态网站部署到 Azure](tutorial-vscode-static-website-node-01.md)