---
title: 使用用于 Node.js 的 Azure 管理模块进行身份验证
description: 在用于 Node.js 的 Azure 管理模块中使用服务主体进行身份验证
ms.topic: how-to
ms.date: 06/17/2017
ms.custom: devx-track-js
ms.openlocfilehash: 150b00c4dbb21d0514d1d7c7d34813272bbf06e1
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586111"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>对适用于 JavaScript 的 Azure 管理模块进行身份验证

有两组可帮助你管理资源的 Azure 服务管理包。
- 用于 Node.js 的 Azure SDK
- 用于 JavaScript 的 Azure SDK

用于 Node.js 的 Azure SDK 是一组较早的 Azure 服务管理包， 
- 只能在 Node.js 中使用，不能在浏览器中使用
- 是使用 JavaScript 编写的，具有手写的类型声明文件
- 不在积极开发中，已被弃用（为了使用用于 JavaScript 的 Azure SDK）
- 包名称以 `azure-arm-` 开头
- 需要 [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 包来创建凭据，这些凭据随后可被传递给包中的客户端类，用于使用 Azure Active Directory 进行身份验证。
- 位于 https://github.com/Azure/azure-sdk-for-node 存储库中

用于 JavaScript 的 Azure SDK 是一组较新的 Azure 服务管理包，
- 可在 Node.js 和浏览器中使用
- 是使用 TypeScript 编写的，可用于 JavaScript 和 TypeScript 项目
- 处于积极开发中，在 Azure 服务更新资源管理 API 时接收更新
- 包名称以 `@azure/arm-` 开头
- 需要 [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) 包来创建凭据，这些凭据随后可被传递给包中的客户端类，用于使用 Azure Active Directory 进行身份验证。 如果应用程序在浏览器中运行，请改用 [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)。
- 位于 https://github.com/Azure/azure-sdk-for-js 存储库中

轻松区分这两个包集的方法是：查看包名称。

在被实例化时，所有服务 API 需要通过 `credentials` 对象进行身份验证。 对于用于 Node.js 的 Azure SDK 和用于 JavaScript 中的包，有多种方法可以进行身份验证和创建需要的凭据。

一些常见的方法是：

- 使用用户名和密码的基本身份验证
- 交互式登录 - 这是最简单的身份验证方法，但要求使用用户帐户登录。
- 服务主体身份验证。 [使用 Node.js 创建 Azure 服务主体](./node-sdk-azure-authenticate-principal.md)主题介绍了创建服务主体的各种方法。 

下面每个包的自述文件都详细介绍了可用于获取凭据对象的各种方式。
- 在 Node.js 中使用用于 JavaScript 的 Azure SDK 中的任何管理包时，使用 [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth)
- 在浏览器中使用用于 JavaScript 的 Azure SDK 中的任何管理包时，使用 [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)
- 使用用于 Node.js 的 Azure SDK 中的任何管理包时，使用 [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="next-steps"></a>后续步骤   

* [从 Visual Studio Code 将静态网站部署到 Azure](tutorial-vscode-static-website-node-01.md)