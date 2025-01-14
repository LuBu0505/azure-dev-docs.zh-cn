---
title: 为 Azure 开发配置本地 JavaScript 环境
description: 如何设置适用于 Azure 的本地 JavaScript 开发环境，包括编辑器、Azure SDK 库、可选工具以及库身份验证所需的凭据。
ms.date: 11/05/2020
ms.topic: conceptual
ms.custom: devx-track-js, azure-sdk-javascript-ai-text-analytics-5.0.0
ms.openlocfilehash: 0da3357fc24f65a43303f6c23d5aa6b679f211d4
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94333817"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>为 Azure 配置本地 JavaScript 开发环境

创建云应用程序时，开发人员通常更喜欢在其本地工作站测试代码，然后再将代码部署到 Azure 等云环境中。 本地环境的优势是可以提供更多各种各样的工具以及熟悉的开发环境。

本文提供了用于创建和验证适用于 Azure 上的 JavaScript 的本地开发环境的安装说明。

## <a name="one-time-subscription-creation"></a>一次性创建订阅

Azure 资源是在订阅中创建的，订阅是使用 Azure 的计费单位。 虽然你可以创建免费资源（每个订阅都会为大多数服务提供免费资源），但如果希望将资源部署到生产环境，应创建付费层资源。

|类型|说明|
|--|--|
|试用订阅|创建一个免费的[试用订阅](https://azure.microsoft.com/free/)。|
|现有订阅|如果你已经拥有订阅，请在 [Azure 门户](https://portal.azure.com)、[Azure CLI]() 或 JavaScript 中访问你的现有订阅。|
|跨多个订阅|如果你需要管理多个订阅，请[了解如何](/azure/governance/management-groups/create-management-group-javascript)使用 JavaScript 创建管理组。|

## <a name="one-time-software-installation"></a>一次性软件安装

若要在本地工作站上结合 JavaScript 使用 Azure 资源进行开发，需要一次性安装以下各项：

|名称/安装程序|说明|
|--|--|
[!INCLUDE [Node.js](../includes/environment-nodejs-table-row-2-columns.md)]
|[Visual Studio Code](https://code.visualstudio.com/)| Visual Studio Code 将为你提供强大的 JavaScript 集成和编码体验，但它不是必需的。 你可以使用任何代码编辑器。 对于本文档，如果你使用的是其他编辑器，请检查是否与 Azure 集成或使用 Azure CLI。|

> [!CAUTION]
> 如果计划使用 Azure 资源（如 Azure Web 应用或 Azure 容器实例）作为代码的运行时环境，则应验证本地 Node.js 开发环境是否与计划使用的 Azure 资源运行时匹配。

### <a name="recommended-local-installations"></a>推荐的本地安装

推荐使用以下常见的本地工作站安装来帮助执行本地开发任务。

|名称/安装程序|说明|
|--|--|
|[Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) 或[适用于 Azure 的 Visual Studio Code 扩展](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance) |通常通过 [Azure 门户](https://ms.portal.azure.com/)、Azure CLI 或特定 Visual Studio Code 扩展来使用 Azure。 虽然无需使用 Azure CLI（除非在快速入门或教程中指定），但它是一种使用 Azure 的单一工具（Visual Studio Code 可基于每个服务的扩展提供相同的功能）。|
| [git](https://git-scm.com/downloads) | 用于源代码管理的命令行工具。 可以根据需要使用不同的源代码管理工具。 |


## <a name="one-time-configuration-of-service-principal"></a>一次性配置服务主体

每项 Azure 服务都有一个身份验证机制。 这可以包括密钥和终结点、连接字符串或其他机制。 若要遵循最佳做法，请使用[服务主体](node-sdk-azure-authenticate-principal.md)创建资源并对资源进行身份验证。 使用服务主体可以具体定义即时开发需要的访问范围。

## <a name="working-with-azure-and-the-azure-sdk-client-libraries"></a>使用 Azure 和 Azure SDK 客户端库

[Azure SDK 库](../azure-sdk-library-package-index.md)是为每项服务单独提供的。 根据需要使用的 Azure 服务安装每个库。

使用 Azure 的每个新项目都应执行以下操作：
- 创建 Azure 资源，并将关联的密钥或配置保存到[安全位置](#securing-configuration-information)。
- 从 NPM 或 Yarn 安装 Azure SDK 库。 
- 使用服务主体向 Azure SDK 进行身份验证，然后使用配置信息访问特定服务。

## <a name="securing-configuration-information"></a>保护配置信息

有多个选项可用来存储配置信息：
- [Dotenv](https://www.npmjs.com/package/dotenv) 是一种常用的 npm 包，它用于从 `.env` 文件中读取环境变量。 请确保将 `.env` 文件添加到 `.gitignore` 文件，以便 `.env` 文件不会被签入到源代码管理中。
- Azure [Key Vault](/azure/key-vault/) 可创建并维护用于访问和加密云资源、应用和解决方案的密钥

### <a name="create-environment-variables-for-the-azure-libraries"></a>为 Azure 库创建环境变量

若要使用 Azure SDK 库访问 Azure 云所需的 Azure 设置，请将最常见的值设置为环境变量。 以下命令将环境变量设置为本地工作站。 另一种常见机制是使用 `DOTENV` NPM 包为这些设置创建 `.env` 文件。 如果计划使用 `.env`，请确保不将该文件签入到源代码管理。 将 `.env` 文件添加到 git 的 `.ignore` 文件是确保将这些设置签入到源代码管理的标准方法。

在下面的示例中，客户端 ID 是服务主体 ID 和服务主体密码。

# <a name="bash"></a>[bash](#tab/bash)

```bash
AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
```

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
```

---

将这些命令中显示的值替换为特定服务主体的值。

## <a name="install-npm-packages"></a>安装 npm 包

对于每个项目，我们建议你始终使用以下步骤创建单独的文件夹及其各自的 `package.json` 文件：

1. 打开终端、命令提示符或 bash shell，并为项目创建一个新文件夹。 然后移动到该新文件夹。

    ```console
    mkdir MY-NEW-PROJECT && cd MY-NEW-PROJECT
    ```

1. 初始化包文件：

    ```console
    npm init -y
    ```

    此命令运行 NPM 命令来创建 package.json 文件并初始化最小属性。 通过 NPM 或 Yarn 安装 Azure SDK 库包之后，package.json 文件将捕获该安装信息。

1. 安装快速入门所需的 Azure SDK 库。 以下命令是一个示例。

    ```console
    npm install @azure/ai-text-analytics@5.0.0
    ```

## <a name="use-source-control"></a>使用源代码管理

建议在每次启动项目时都创建一个源代码管理存储库。 如已安装 Git，则运行以下命令：

```bash
git init
```

可以在其中运行 `git add` 和 `git commit` 等命令以提交更改。 通过定期提交更改，可以创建提交历史记录，并可将其恢复到任何以前的状态。

若要对项目进行联机备份，还建议将存储库上载到 [GitHub](https://github.com) 或 [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops)。 如果已先初始化本地存储库，请使用 `git remote add` 将本地存储库附加到 GitHub 或 Azure DevOps。

有关 git 的文档，请参阅 [git-scm.com/docs](https://git-scm.com/docs) 和 Internet 上的所有文档。

Visual Studio Code 包含许多内置的 git 功能。 有关详细信息，请参阅[使用 VS Code 中的版本控制](https://code.visualstudio.com/docs/editor/versioncontrol)。

还可以使用所选的任何其他源代码管理工具；Git 只是使用和支持范围最广的工具之一。

## <a name="next-steps"></a>后续步骤

* [创建和使用服务主体](node-sdk-azure-authenticate-principal.md)
* [使用用于 Node.js 的 Azure 模块进行身份验证](node-sdk-azure-authenticate.md)
* [从 Visual Studio Code 将静态网站部署到 Azure](../tutorial-vscode-static-website-node-01.md)