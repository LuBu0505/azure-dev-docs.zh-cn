---
title: 为 Azure 开发配置本地 Python 环境
description: 如何设置适用于 Azure 的本地 Python 开发环境，包括 Visual Studio Code、Azure SDK 库以及库身份验证所需的凭据。
ms.date: 05/29/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: d95584758900eae2c50df5e731fd84f8bca00897
ms.sourcegitcommit: 800c5e05ad3c0b899295d381964dd3d47436ff90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88614504"
---
# <a name="configure-your-local-python-dev-environment-for-azure"></a>为 Azure 配置本地 Python 开发环境

创建云应用程序时，开发人员通常更喜欢在其本地工作站测试代码，然后再将代码部署到 Azure 等云环境中。 本地开发可提供更快的速度和各种调试工具。

本文提供了用于创建和验证适用于 Azure 上的 Python 的本地开发环境的一次性安装说明：

- [安装所需组件](#required-components)，即 Azure 帐户、Python 和 Azure CLI。
- 为使用 Azure 库来预配、管理和访问 Azure 资源的场景[配置身份验证](#configure-authentication)。
- 查看每个项目[使用 Python 虚拟环境](#use-python-virtual-environments)的过程。

配置好工作站后，只需添加最少的配置即可完成此开发中心和 Azure 文档中其他部分的各种快速入门和教程。

这种用于本地开发的设置独立于在 Azure 上组成应用程序云环境的[预配资源](cloud-development-flow.md)。 在开发过程中，你可以在本地开发环境中运行可访问这些云资源的代码，但你的代码尚未部署到云中的[适用托管服务](quickstarts-app-hosting.md)。 该部署步骤稍后再介绍， [Azure 开发流](cloud-development-flow.md)一文中有详细说明。

## <a name="install-components"></a>安装组件

### <a name="required-components"></a>所需组件

| 名称/安装程序 | 说明 |
| --- | --- |
| [具有活动订阅的 Azure 帐户](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=python-dev-center&mktingSource=environment-setup) | 帐户/订阅免费提供，其中包括许多免费使用的服务。 |
| [Python 2.7+ 或 3.5.3+](https://www.python.org/downloads) | Python 语言运行时。 建议使用最新版本的 Python 3.x，除非具有特定版本要求。 |
| [Azure 命令行接口 (CLI)](/cli/azure/install-azure-cli) | 提供一套完整的 CLI 命令，用于预配和管理 Azure 资源。 Python 开发人员通常将 Azure CLI 与使用 Azure 管理库的自定义 Python 脚本结合使用。 |

说明：

- 可以根据需要按项目安装单个 Azure 库包。 建议为每个项目[使用 Python 虚拟环境](#use-python-virtual-environments)。 对于 Python，没有独立的“SDK”安装程序。
- 尽管 Azure PowerShell 通常等效于 Azure CLI，但在使用 Python 时，建议使用 Azure CLI。

### <a name="recommended-components"></a>推荐的组件

| 名称/安装程序 | 说明 |
| --- | --- |
| [Visual Studio Code](https://code.visualstudio.com) | 尽管可以使用任何适合的编辑器或 IDE，但 Microsoft 提供的免费、轻量级 IDE 在 Python 开发人员中广受欢迎。 有关介绍，请参阅 [VS Code 中的 Python](https://code.visualstudio.com/docs/python/python-tutorial)。 |
| [适用于 VS Code 的 Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python) | 向 VS Code 添加 Python 支持。 |
| [适用于 VS Code 的 Azure 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) | 向 VS Code 添加对各种 Azure 服务的支持。 还可以单独安装对特定服务的支持。 |
| [git](https://git-scm.com/downloads) | 用于源代码管理的命令行工具。 可以根据需要使用不同的源代码管理工具。 |

### <a name="optional-components"></a>可选组件

| 名称/安装程序 | 说明 |
| --- | --- |
| [适用于 VS Code 的 Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python) | 向 VS Code 添加 Docker 支持，如果经常使用容器，该扩展会很有帮助。 |

### <a name="verify-components"></a>验证组件

1. 打开终端或命令提示符。
1. 通过运行命令 `python --version` 来验证 Python 版本。
1. 通过运行 `az --version` 来验证 Azure CLI 版本。
1. 验证 VS Code 安装：
    1. 运行 `code .` 以打开当前文件夹的 VS Code。
    1. 在 VS Code 中，选择“视图” > “扩展”命令以打开“扩展”视图，然后验证是否可在列表中看到“Python”和“Azure 帐户”，如果同时安装了“Azure”扩展和“Docker”，则一并检查它们是否也在列表中。

## <a name="sign-in-to-azure-from-the-cli"></a>从 CLI 登录 Azure

在终端或命令提示符中，登录到 Azure 订阅：

```azurecli
az login
```

`az` 命令是 Azure CLI 的根命令。 `az` 后面是一个或多个特定命令，例如 `login`。 请参阅 [az login](/cli/azure/authenticate-azure-cli) 命令参考。

Azure CLI 通常会维护多个会话的登录，但最好在每次打开新的终端或命令提示符时运行 `az login`。

## <a name="configure-authentication"></a>配置身份验证

如[如何对应用进行身份验证](azure-sdk-authenticate.md#identity-when-running-the-app-locally)中所述，每个开发人员都需要一个服务主体，以便在本地测试应用代码时将其用作应用程序标识。

以下各部分介绍了如何创建服务主体，以及如何在需要时向 Azure 库提供服务主体属性的环境变量。

组织中的每个开发人员都应单独执行这些步骤。

### <a name="create-a-service-principal-and-environment-variables-for-development"></a>创建用于开发的服务主体和环境变量

1. 打开终端或命令提示符以登录到 Azure CLI (`az login`)。

1. 创建服务主体：

    ```azurecli
    az ad sp create-for-rbac --name localtest-sp-rbac --skip-assignment --sdk-auth > local-sp.json
    ```

    此命令会将其输出保存到 local-sp.json 中。 有关该命令及其参数的更多详细信息，请参阅 [create-for-rbac 命令的功能](#what-the-create-for-rbac-command-does)。

    如果你在组织中，则可能没有在订阅中运行此命令的权限。 在这种情况下，请与订阅所有者联系，让他们为你创建服务主体。

1. 使用以下命令创建 Azure 库所需的环境变量。 （azure-identity 库的 `DefaultAzureCredential` 对象将查找这些变量）。

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
    set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
    set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
    set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
    AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
    AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
    AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
    ```

    ---

    将这些命令中显示的值替换为特定服务主体的值。

    若要检索订阅 ID，请运行 [`az account show`](/cli/azure/account?view=azure-cli-latest#az-account-show) 命令并查看输出结果中的 `id` 属性。

    为了方便起见，请创建包含这些命令的命令行脚本文件（例如在 macOS/Linux 上为 setenv.sh，在 Windows 上为 setenv.cmd） 。 然后，在每次打开终端或命令提示符进行本地测试时，都可运行此脚本来设置变量。 同样，不要将此脚本文件添加到源代码管理中，这样它只会保留在用户帐户中。

1. 保护客户端 ID 和客户端密码（以及存储它们的所有文件），以便它们始终保留在工作站上的特定用户帐户中。 永远不要将这些属性保存在源代码管理中或与其他开发人员共享。 可以在需要时删除服务主体并创建新的服务主体。

    为了增加安全性，可以创建一个策略来定期删除和重新创建服务主体，从而使以前的 ID 和密码失效。

    此外，开发服务主体最好仅对非生产资源授权，或在仅用于开发目的的 Azure 订阅中创建。 然后，生产应用程序将使用一个单独的订阅和单独的生产资源，这些资源只对部署的云应用程序授权。

1. 若要在以后修改或删除服务主体，请参阅[如何管理服务主体](how-to-manage-service-principals.md)。

#### <a name="what-the-create-for-rbac-command-does"></a>create-for-rbac 命令的功能

`az ad create-for-rbac` 命令将为“基于角色的身份验证”(RBAC) 创建服务主体。

- `ad` 表示 Azure Active Directory，`sp` 表示“服务主体”，`create-for-rbac` 表示“创建基于角色的访问控制”，这是 Azure 的主要授权形式。 请参阅 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令参考。

- `--name` 参数在组织中应该是唯一的，通常采用使用服务主体的开发人员的名字。 如果省略此参数，则 Azure CLI 使用 `azure-cli-<timestamp>` 格式的通用名称。 可根据需要在 Azure 门户上重命名服务主体。

- `--skip-assignment` 参数创建没有默认权限的服务主体。 然后，必须将特定的权限分配给服务主体，以允许本地运行的代码访问任何资源。 不同的快速入门和教程提供了为所涉及的资源授权服务主体的详细信息。

- 此命令提供 JSON 输出，在这一示例中，该输出保存在名为 local-sp.json 的文件中。

- `--sdk-auth` 参数生成类似于以下值的 JSON 输出。 你的 ID 值和密码都将不同）：

    <pre>
    {
      "clientId": "12345678-1111-2222-3333-1234567890ab",
      "clientSecret": "abcdef00-4444-5555-6666-1234567890ab",
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "tenantId": "00112233-7777-8888-9999-aabbccddeeff",
      "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
      "resourceManagerEndpointUrl": "https://management.azure.com/",
      "activeDirectoryGraphResourceId": "https://graph.windows.net/",
      "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
      "galleryEndpointUrl": "https://gallery.azure.com/",
      "managementEndpointUrl": "https://management.core.windows.net/"
    }
    </pre>

    如果没有 `--sdk-auth` 参数，该命令将生成更简单的输出：

    <pre>
    {
      "appId": "12345678-1111-2222-3333-1234567890ab",
      "displayName": "localtest-sp-rbac",
      "name": "http://localtest-sp-rbac",
      "password": "abcdef00-4444-5555-6666-1234567890ab",
      "tenant": "00112233-7777-8888-9999-aabbccddeeff"
    }
    </pre>

    在这种情况下，`tenant` 是租户 ID，`appId` 是客户端 ID，`password` 是客户端密码。

    > [!IMPORTANT]
    > 此命令的输出是查看客户端机密/密码的唯一位置。 稍后不能检索机密/密码。 不过，可以在需要时添加新密码，而不会使服务主体或现有密码失效。

## <a name="use-python-virtual-environments"></a>使用 Python 虚拟环境

对于每个项目，建议始终使用以下步骤创建和激活虚拟环境：

1. 打开终端或命令提示符。

1. 为项目创建一个文件夹。

1. 创建虚拟环境：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```bash
    python -m venv .venv
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    python -m venv .venv
    ```

    ---

    此命令运行 Python `venv` 模块，并在名为 `.venv` 的文件夹中创建虚拟环境。

1. 激活虚拟环境：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```bash
    .venv\scripts\activate
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    source .venv/scripts/activate
    ```

    ---

虚拟环境是项目内的一个文件夹，用于隔离特定 Python 解释器的副本。 激活该环境（Visual Studio Code 自动执行该操作）后，运行 `pip install` 将库安装到该环境中。 然后，在运行 Python 代码时，它将在环境的确切上下文中运行，且每个库都有特定的版本。 运行 `pip freeze` 时，将获取这些库的准确列表。 （在本文档的很多示例中，你为所需的库创建了“requirements.txt”文件，然后使用 `pip install -r requirements.txt`。 将代码部署到 Azure 时，通常需要一个 requirements 文件。）

如果不使用虚拟环境，Python 会在其全局环境中运行。 尽管使用全局环境既快速又便捷，但随着时间的推移，为任何项目或实验所安装的所有库的数量都会膨胀。 此外，如果更新一个项目的库，则可能会中断依赖于该库的不同版本的其他项目。 而且由于环境是由任意数量的项目共享的，因此不能使用 `pip freeze` 来检索任一项目的依赖项的列表。

全局环境是要在其中安装要在多个项目中使用的工具包的位置。 例如，你可能会在全局环境中运行 `pip install gunicorn` 以使 Gunicorn Web 服务器可在任意位置使用。

## <a name="use-source-control"></a>使用源代码管理

建议在每次启动项目时都创建一个源代码管理存储库。 如已安装 Git，只需运行以下命令：

```cmd
git init
```

可以在其中运行 `git add` 和 `git commit` 等命令以提交更改。 通过定期提交更改，可以创建提交历史记录，并可将其恢复到任何以前的状态。

若要对项目进行联机备份，还建议将存储库上载到 [GitHub](https://github.com) 或 [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops)。 如果已先初始化本地存储库，请使用 `git remote add` 将本地存储库附加到 GitHub 或 Azure DevOps。

有关 git 的文档，请参阅 [git-scm.com/docs](https://git-scm.com/docs) 和 Internet 上的所有文档。

Visual Studio Code 包含许多内置的 git 功能。 有关详细信息，请参阅[使用 VS Code 中的版本控制](https://code.visualstudio.com/docs/editor/versioncontrol)。

还可以使用所选的任何其他源代码管理工具；Git 只是使用和支持范围最广的工具之一。

## <a name="next-step"></a>后续步骤

准备好本地开发环境后，请快速查看 Azure 库的常见使用模式：

> [!div class="nextstepaction"]
> [查看常见使用模式 >>>](azure-sdk-library-usage-patterns.md)
