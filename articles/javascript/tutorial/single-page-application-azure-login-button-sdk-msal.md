---
title: 教程：向 React SPA 添加 Microsoft 登录按钮
description: 本教程中介绍的 Azure Active Directory 身份验证是一个登录和注销按钮，可用于访问用户的用户名（电子邮件）。 使用 Azure 客户端 SDK `@azure/msal-browser` 开发应用程序，以在单页应用程序 (SPA) 中管理用户的交互。
ms.topic: tutorial
ms.date: 12/01/2020
ms.custom: devx-track-js, "azure-sdk-javascript-@azure/msal-browser-2.7.0"
ms.openlocfilehash: a5c07696c6c774408bf2772542234e59f5c0b58c
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96563694"
---
# <a name="add-microsoft-login-button-to-a-single-page-application-for-authentication"></a>将 Microsoft 登录按钮添加到单页应用程序以进行身份验证

本教程中介绍的 Azure 身份验证是一个登录和注销按钮，可用于访问用户帐户。 使用 Azure 客户端 SDK `@azure/msal-browser` 开发应用程序，以在单页应用程序 (SPA) 中管理用户的交互。

* [源代码](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)

## <a name="application-architecture-and-functionality"></a>应用程序体系结构和功能

本教程中构建的 SPA 是一款具有以下任务的 React 应用 (create-react-app)：

- 使用 Microsoft 支持的登录名（如 Office 365 或 Outlook.com）登录
- 注销应用程序

为了提供一个快速简单的单页应用程序，该示例将 create-react-app 与 TypeScript 配合使用。 此前端框架在使用 Azure SDK 进行典型的客户端开发中提供了多种快捷方式：

- 捆绑，客户端应用程序中使用的 Azure SDK 需要
- `.env` 文件中的环境变量

## <a name="1-set-up-development-environment"></a>1.设置开发环境

验证本地计算机上是否安装了以下项。

- 具有活动订阅的 Azure 用户帐户。 [免费创建一个](https://azure.microsoft.com/free/)。
- [Node.js 和 npm](https://nodejs.org/en/download) - 已安装到本地计算机。
- [Visual Studio Code](https://code.visualstudio.com/) 或 Bash shell 或终端的等效 IDE - 已安装到本地计算机。 

## <a name="2-keep-value-for-environment-variable"></a>2.保留环境变量的值

留出一个位置来复制应用客户端 ID 值。 

## <a name="3-create-app-registration-for-authentication"></a>3.创建用于身份验证的应用注册

1. 登录到 [Azure 门户](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)以进行默认目录的应用注册。
1. 选择“+ 新建注册”。
1. 使用下表输入应用注册数据：

   | 字段                   | 值                                                                                                                                                                      |描述|
   | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |--|
   | “属性”                    | `Simple Auth Tutorial`|这是应用名称，用户登录到应用时将在权限窗体上看到。                                                 |
   | 支持的帐户类型 | 任何组织目录中的帐户（任何 Azure AD 目录 - 多租户）和个人 Microsoft 帐户|这会涵盖大多数帐户类型。 |
   | 重定向 URI 类型           | **单页面应用程序 (SPA)**                                                                                        | |
   | 重定向 URI 值           | `http://localhost:3000` | 身份验证成功或失败后要返回到的 URL。                                                                                        |

    :::image type="content" source="../media/tutorial-login-button/azure-active-directory-create-new-app-registration.png" alt-text="Azure 新应用注册。":::  

1. 选择“注册”  。 等待应用注册过程完成。
1. 从应用注册的“概述”部分复制应用程序（客户端）ID。 稍后会将此值添加到客户端应用的环境变量中。

## <a name="4-create-react-single-page-application-for-typescript"></a>4.为 TypeScript 创建 React 单页应用程序

1. 在 Bash shell 中，使用 TypeScript 语言创建 create-react-app：

   ```bash
   npx create-react-app tutorial-demo-login-button --template typescript
   ```

1. 更改为新目录并安装 `@azure/msal-browser` 包：

   ```bash
   cd tutorial-demo-login-button && npm install @azure/msal-browser
   ```

1. 在根级别文件中创建 `.env` 并添加以下行：

    :::code language="env" source="~/../js-e2e-client-azure-login-button/.env"  :::

    `.env` 文件当做 create-react-app 框架的一部分。

1. 将应用程序（客户端）ID 复制到第二个值中。

## <a name="5-add-login-and-logoff-buttons"></a>5.添加登录和注销按钮

1. 在 `./src` 文件夹中为 Azure 特定文件创建一个名为 `azure` 的子文件夹。 

1. 在 `azure` 文件夹中创建一个用于身份验证配置的新文件，将其命名为 `azure-authentication-config.ts`，然后复制以下代码：

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-config.ts"  highlight="3-4,, 11-12":::

    此文件从 `.env` 文件中读取应用程序 ID，将会话设置为浏览器存储而不是 cookie，并提供略去个人信息的日志记录。

1. 在 `azure` 文件夹中为 Azure 身份验证中间件创建一个新文件，将其命名为 `azure-authentication-context.ts`，然后复制以下代码：

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-context.ts"  highlight="43, 58, 65":::

    此文件通过 `login` 和 `logout` 函数为用户提供对 Azure 的身份验证。

1. 在 `azure` 文件夹中为用户界面按钮组件文件创建一个新文件，将其命名为 `azure-authentication-component.tsx`，然后复制以下代码：

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-component.tsx"  highlight="3, 11, 23, 29, 36":::

   此按钮组件可实现用户登录，并将用户帐户传递回调用/父组件。

   按钮文本和功能根据用户当前是否已登录进行切换，由 `onAuthenticated` 函数捕获，作为属性传入组件。

   当用户登录时，按钮将调用 Azure 身份验证库方法 `authenticationModule.login`，并使用 `returnedAccountInfo` 作为回调函数。 然后使用 `onAuthenticated` 函数将返回的用户帐户传递回父组件。


1. 打开 `./src/App.tsx` 文件，将所有代码替换为以下代码，以合并登录/注销按钮组件：

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/App.tsx"  highlight="10, 37-42":::

    用户登录并且身份验证重定向回此应用后，将显示 currentUser 对象。 

## <a name="6-run-react-spa-app-with-login-button"></a>6.使用登录按钮运行 SPA 应用

1. 在 Visual Studio Code 终端，启动应用：

    ```bash
    npm run start
    ```

1. 在 Web 浏览器中选择“登录”按钮。 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-before-authentication-login-button-display.png" alt-text="选择“登录”按钮。":::  

1. 选择用户帐户。 用户帐户不必与用于访问 Azure 门户的帐户相同，但它应该是提供 Microsoft 身份验证的帐户。

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-select-user-account.png" alt-text="选择用户帐户。用户帐户不必与用于访问 Azure 门户的帐户相同，但它应该是提供 Microsoft 身份验证的帐户。":::

1. 查看显示 1) 用户名、2) 应用名、3) 你同意的权限的弹出窗口，然后选择“是”。

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-let-this-app-access-your-info.png" alt-text="查看显示 1) 用户名、2) 应用名、3) 你同意的权限的弹出窗口，然后选择“是”":::。

1. 查看用户帐户信息。 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-after-authentication-login-button-succeeds.png" alt-text="查看用户帐户信息。":::  

1. 在应用中选择“注销”按钮。 该应用还提供了指向 Microsoft 用户应用以用于撤消权限的便捷链接。 

## <a name="7-clean-up-resources"></a>7.清理资源

根据本教程完成操作后，请在 Azure 门户[应用注册列表](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)中删除该应用程序。

如果你希望保留该应用，但要撤销特定用户帐户授予该应用的权限，请使用以下链接之一：

* [撤销 AAD 权限](https://myapps.microsoft.com/)
* [撤销使用者权限](https://account.live.com/consent/manage)

## <a name="next-steps"></a>后续步骤

此应用为你的应用提供用户身份验证，并返回用户信息。 对于简单版本的应用，可在此处停止身份验证功能，也可添加功能来管理应用的用户管理和用户对应用功能的授权。 

用户管理可存储在 Azure Active Directory 或自己的数据库中，具体取决于你选择的功能和工具。 

用户授权可由 Azure 提供，你也可在不使用 Azure 的情况下开发授权，还可结合使用这两种方式获得授权、角色和应用功能的自定义体验。 

* 继续使用 [MSAL 库](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) 获取用户配置文件并提供无提示登录
* 添加 [Microsoft Graph](https://docs.microsoft.com/graph/overview) 以访问 Microsoft 365 中的用户帐户，包括电子邮件和日历约会