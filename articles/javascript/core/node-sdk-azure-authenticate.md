---
title: 使用用于 Node.js 的 Azure 管理模块进行身份验证
description: 在用于 Node.js 的 Azure 管理模块中使用服务主体进行身份验证
ms.topic: how-to
ms.date: 09/29/2020
ms.custom: devx-track-js
ms.openlocfilehash: 084a7154bf52314886c0dfb57db3e9c879c8e2c0
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437131"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>对适用于 JavaScript 的 Azure 管理模块进行身份验证

在被实例化时，所有 [SDK 客户端库](../azure-sdk-library-package-index.md)都需要通过 `credentials` 对象进行身份验证。 可使用多种方法来验证和创建所需凭据。

创建所需凭据的常见方法包括：

- 服务主体身份验证（推荐方法）。 了解[如何创建 Azure 服务主体](node-sdk-azure-authenticate-principal.md)。 
- 交互式登录，这是最简单的身份验证方法，但要求在浏览器中使用用户帐户进行登录。
- 基本身份验证，使用用户名和密码。 该方法的安全性最低。 

## <a name="samples"></a>示例

|身份验证包|示例身份验证脚本|
|--|--|
|[@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) <br>（建议）|[使用证书的服务主体](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpCert.ts)<br>[来自文件的服务主体](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpSecret.ts)<br>[交互式](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/interactivePersonalAccount.ts)<br>[基本](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/usernamePassword.ts)|
|[@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)<br>（建议）|[带有弹出窗口的身份验证 (create-react-app)](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/authentication-with-popup)<br>[无弹出窗口回应](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/react-app)<br>[带登录按钮的 HTML](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/vanilla)|
|[ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|[服务主体](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#service-principal-authentication)<br>[交互式](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#interactive-login)<br>[基本](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#basic-authentication)|

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="next-steps"></a>后续步骤   

* [从 Visual Studio Code 将静态网站部署到 Azure](../tutorial-vscode-static-website-node-01.md)