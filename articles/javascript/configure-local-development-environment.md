---
title: 为 Azure 开发配置本地 JavaScript 环境
description: 如何设置适用于 Azure 的本地 JavaScript 开发环境，包括编辑器、Azure SDK 库、可选工具以及库身份验证所需的凭据。
ms.date: 07/01/2020
ms.topic: conceptual
ms.custom: devx-track-javascript
ms.openlocfilehash: 1071985de770d8c1d9e5e78a25e8556048857a3e
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218928"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>为 Azure 配置本地 JavaScript 开发环境

创建云应用程序时，开发人员通常更喜欢在其本地工作站测试代码，然后再将代码部署到 Azure 等云环境中。 本地环境的优势是可以提供更多各种各样的工具以及熟悉的开发环境。

本文提供了用于创建和验证适用于 Azure 上的 JavaScript 的本地开发环境的安装说明。

## <a name="one-time-subscription-creation"></a>一次性创建订阅

Azure 资源是在订阅中创建的，订阅是使用 Azure 的计费单位。 虽然你可以创建免费资源（每个订阅都会为大多数服务提供免费资源），但如果希望将资源部署到生产环境，应创建付费层资源。

* 如果你已有订阅，则无需创建新订阅。 使用 [Azure 门户](https://portal.azure.com)访问现有订阅。
* [开始免费试用订阅]()

## <a name="one-time-installation"></a>一次性安装

若要在本地工作站上结合 JavaScript 使用 Azure 资源进行开发，需要一次性安装以下各项：

|名称/安装程序|说明|
|--|--|
|[Node.js]()|安装最新的长期支持 (LTS) 运行时环境进行本地工作站开发。 |
| NPM（与新版 Node.js 一起安装）或 [Yarn]()|安装 Azure SDK 库的包管理器。|
|[VSCode](https://aka.ms/vscode-deploy)| VSCode 将为你提供强大的 JavaScript 集成和编码体验，但它不是必需的。 你可以使用任何代码编辑器。 对于本文档，如果你使用的是其他编辑器，请检查是否与 Azure 集成或使用 Azure CLI。|
|[Azure CLI]()|Azure CLI 可用于从命令行、终端或 bash shell 重新创建和管理 Azure 资源。|

> [!CAUTION]
> 如果计划使用 Azure 资源（如 Azure Web 应用或 Azure 容器实例）作为代码的运行时环境，则应验证本地 Node.js 开发环境是否与计划使用的 Azure 资源运行时匹配。

### <a name="optional-local-installations"></a>可选的本地安装

你可以视情况选择以下常见的本地工作站安装来帮助执行本地开发任务。

|名称/安装程序|说明|
|--|--|
| [git](https://git-scm.com/downloads) | 用于源代码管理的命令行工具。 可以根据需要使用不同的源代码管理工具。 |

## <a name="one-time-configuration-of-service-principal"></a>一次性配置服务主体

每项 Azure 服务都有一个身份验证机制。 这可以包括密钥和终结点、连接字符串或其他机制。 若要遵循最佳做法，请使用服务主体创建资源并对资源进行身份验证。 使用服务主体可以具体定义即时开发需要的访问范围。

从概念上讲，创建和使用服务主体的步骤包括：

* 使用你个人的用户帐户（如 joe@microsoft.com）登录 Azure。
* 创建具有特定范围的命名服务主体。 由于大多数快速入门都要求创建 Azure 资源，因此服务主体需要能够创建资源。
* 注销 Azure 用户帐户。
* 使用服务主体以编程方式向 Azure 进行身份验证。
* 服务主体将创建一个 Azure 资源，并使用与该服务关联的服务。

### <a name="create-service-principal"></a>创建服务主体

为了更轻松地创建服务主体，请使用以下步骤和提供的脚本来创建要与 Azure 快速入门配合使用的服务主体。 以下步骤使用名称 `JOE` 作为示例用户名。 将此名称替换为你自己的姓名或电子邮件别名。

1. 打开 VSCode 并安装 [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)扩展。 通过此扩展，你可以从脚本文件逐行执行 Azure CLI 命令。 运行每个命令时，系统都会在 VSCode 中打开一个相邻的文档来显示结果。

1. 创建一个名为 `create-service-principal.sh` 的新文件，并将以下 Azure 命令复制到该文件中：

    ```azurecli
    # Replace ALL-CAPS variables with your own values

    ####################################
    # Login as you
    ####################################

    # Login - command opens browser, select your account to finish authentication, then close browser
    az login

    ####################################
    # Optional, set default subscription
    ####################################

    # If you have more than 1 subscription, use the `list` command to find the subscription, then use the `set` command to set the default by name or id
    az account list
    az account set --subscription MYCOMPANYSUBSCRIPTION

    ####################################
    # Create service principal
    ####################################

    # Create a service principal with a name that indicates its purpose and owner - the response includes the `appId` which is necessary in some of the remaining commands
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment

    ####################################
    # Add role of contributor
    ####################################

    # Add contributor role to service principal so it can create Azure resources
    az role assignment create --assignee APP-ID --role CONTRIBUTOR

    ####################################
    # Optional, verify role assignment
    ####################################

    # Verify role assignment for service principal
    az role assignment list --assignee APP-ID

    ####################################
    # Logout
    ####################################

    # Logout off Azure CLI
    az logout
    ```

    在此过程中的剩余步骤中，对于并未以 `#` 开头的文件中的每一行，将 VSCode 游标悬停在对应行上，然后右键单击并选择“在编辑器中运行行”  。

    :::image type="content" source="media/development-setup/vscode-rightclick-run-line-in-editor.png" alt-text="在此过程中的剩余步骤中，对于不是以“#”开头的文件中的每一行，将 VSCode 游标悬停在对应行上，然后右键单击并选择“在编辑器中运行行”。":::

1. 对以下行使用右键单击/“在编辑器中运行行”，以使用 Azure CLI 通过你自己的用户帐户向 Azure 进行身份验证。 此命令将打开 Internet 浏览器。 选择你的 Azure 帐户。 对帐户进行身份验证后，关闭浏览器窗口，后续任务不需要此窗口。

    ```azurecli
    az login
    ```

    响应包含你有权访问的所有订阅，以 JSON 数组形式显示在 VSCode 文档窗口中。 查找 `name` 或 `id` 属性。 要执行剩余命令，需要下列值之一。

    ```json
    [  {
    "cloudName": "AzureCloud",
    "id": "320d9379-aaaa-bbbb-cccc-52f2b0fc40ac",
    "isDefault": false,
    "name": "contoso-development-team",
    "state": "Enabled",
    "tenantId": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47",
    "user": {
      "name": "joe@contoso.com",
      "type": "user"
    }
    }]
    ```

    标记为 `isDefault: true` 的订阅可接收剩余命令。 如果需要更改默认订阅，请使用 `az account set --subscription <name or id>` 命令。


<a name='create-service-principal-command'></a>

1. 对以下行使用右键单击/“在编辑器中运行行”，以创建与用户帐户关联的服务主体。 由于 `--skip-assignment` 参数，此服务主体尚不具有任何作用域权限。


    ```azurecli
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment
    ```

    服务主体名称是 `JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS`。 你可在 Azure 门户中 Active Directory 服务的应用程序列表下查看与 Azure 用户帐户相关联的所有服务主体列表。

    结果包含所需的信息：`appId` 和 `password`。 使用 `create-service-principal.json` 名称保存文件

    ```json
    {
      "appId": "93453d56-aaaa-bbbb-cccc-db600ecc4f6a",
      "displayName": "JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "name": "http://JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "password": "d88b21e0-aaaa-bbbb-cccc-e1e9b06d50f6",
      "tenant": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47"
    }
    ```

1. 对以下行使用右键单击/“在编辑器中运行行”，以分配用于创建 Azure 资源的作用域权限。 通过 `CONTRIBUTOR` 范围，服务主体可创建 Azure 资源。

    ```azurecli
    az role assignment create --assignee APP-ID --role CONTRIBUTOR
    ```

    结果应类似如下所示：

    ```json
    {
      "canDelegate": null,
      "id": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleAssignments/3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "name": "3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "principalId": "c05d56c9-aaaa-bbbb-cccc-0535d6167ed4",
      "principalType": "ServicePrincipal",
      "roleDefinitionId": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleDefinitions/b24988ac-aaaa-bbbb-cccc-20f7382dd24c",
      "scope": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0",
      "type": "Microsoft.Authorization/roleAssignments"
    }
    ```

    目前，服务主体已准备就绪，可供使用。

1. 对以下行使用右键单击/“在编辑器中运行行”，以使用以下命令退出 Azure CLI：

    ```azurecli
    az logout
    ```

## <a name="steps-for-each-new-development-project-setup"></a>设置每个新开发项目的步骤

由于 Azure SDK 库是为每个服务单独提供的，因此没有可用于访问所有 Azure 资源的可下载包。 根据要使用的 Azure 服务安装每个库。

使用 Azure 的每个新项目都应执行以下操作：
- 创建 Azure 资源或查找现有 Azure 资源的身份验证信息
- 从 NPM 或 Yarn 安装 Azure SDK 库。 了解[库版本](#library-versions)。
- 安全管理项目中的身份验证信息。 一种常见方法是使用 [Dotenv](https://www.npmjs.com/package/dotenv) 从 `.env` 文件读取环境变量。 请确保将 `.env` 文件添加到 `.gitignore` 文件，以便 `.env` 文件不会被签入到源代码管理中。

### <a name="library-versions"></a>库版本

所有 Azure 库都将移动到 `@azure` 范围。

| 库类型 | 说明|
|--|--|
|新式|范围为 `@azure`，例如 [@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob) 和 [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos)，并包含 TypeScript 类型。|
|更旧的包|通常以 `azure-` 开头。 许多包都以此名称开头，说明它们不是由 Microsoft 生成的。 验证包的所有者是 Microsoft 还是 Azure。|

### <a name="create-resource-using-service-principal"></a>使用服务主体创建资源

以下部分提供了有关如何使用服务主体创建 Azure 服务资源的示例。 若要使用服务主体登录，需要从[创建服务主体](#create-service-principal)过程保存到 `create-service-principal.json` 的`appId`、`tenant` 和 `password`。

1. 打开 VSCode，并使用以前安装的 [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)扩展。 通过此扩展，你可以从脚本文件逐行执行 Azure CLI 命令。 运行每个命令时，系统都会在 VSCode 中打开一个相邻的文档来显示结果。

1. 创建一个名为 `create-service-resource.sh` 的新文件，并将以下 Azure 命令复制到该文件中：

    ```azurecli
    ####################################
    # Login as service principal
    ####################################
    # User name for command is the app id
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

    ####################################
    # Create resource group
    ####################################

    # Create resource group in westus region - check your quickstart if it requires a specific region, then change this value to the appropriate region
    # Common naming convention for resource group is `USERNAME-REGION-PURPOSE`
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP

    ####################################
    # Create specific service resource
    ####################################

    # Create resource in westus
    # This is an example of creating a Cognitive Services LUIS resource
    # Review your quickstart to find the exact command
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes

    ####################################
    # Get resource keys
    ####################################

    # Get resource keys
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

1. 对以下行使用右键单击/“在编辑器中运行行”，以使用服务主体登录。 [上一个服务主体创建命令](#create-service-principal-command)的响应中返回了所有字母均大写的变量。

    ```azurecli
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
    ```

1. 对以下行使用右键单击/“在编辑器中运行行”，为你需要为快速入门创建的所有资源创建一个资源组。 资源组区域只能包含该区域的资源。

    ```azurecli
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

    使用完快速入门资源后，可以删除资源组，此操作可一次性删除这些资源。

1. 对以下行使用右键单击/“在编辑器中运行行”，以创建认知服务 LUIS 资源。 这是一个示例，你自己的资源将具有不同的命令。

    ```azurecli
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes
    ```

    LUIS 资源使用密钥和终结点，在使用 LUIS 快速入门时需要用到它们。

1. 对以下行使用右键单击/“在编辑器中运行行”，以获取 LUIS 密钥和终结点。 向 LUIS 服务进行身份验证需使用该密钥和终结点。

    ```azurecli
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

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

### <a name="install-npm-packages"></a>安装 NPM 包

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
    npm install @azure/cognitiveservices-luis-runtime
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

* [从 Visual Studio Code 将静态网站部署到 Azure](tutorial-vscode-static-website-node-01.md)