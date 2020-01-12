---
title: 教程：使用 VS Code 通过 Python 创建并部署无服务器 Azure Functions
description: 教程步骤 1，简介和先决条件。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: a380a447150f29653a1f94a3fe1f6464dd495a81
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2019
ms.locfileid: "75190994"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>教程：使用 Visual Studio Code 在 Python 中创建并部署无服务器 Azure Functions

在本文中，我们使用 Visual Studio Code 和 Azure Functions 扩展通过 Python 创建一个无服务器 HTTP 终结点，并将连接（或“绑定”）添加到存储。

Azure Functions 在无服务器环境中运行代码，不需预配虚拟机，也不需发布 Web 应用。 用于 Visual Studio Code 的 Azure Functions 扩展可以自动处理许多配置问题，大大简化了使用 Functions 的过程。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”  按钮来提交反馈。

## <a name="prerequisites"></a>必备条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。
- [Visual Studio Code 与 Azure Functions 扩展](#visual-studio-code-python-and-the-azure-functions-extension)。

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)一个免费使用 30 天的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

按照[使用 Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 一文中适用于你的操作系统的说明安装 Azure Functions Core Tools。 忽略有关 Chocolately 包管理器的一文中的注释，这些注释对于完成本教程而言并非必需。

安装 Node.js 时，请使用默认选项，  不要选择用于自动安装必要工具的选项。  另外，请务必将 `-g` 选项与 `npm install` 命令一起使用，以使 Core Tools 可用于后续命令。

    > [!TIP]
    > The Core Tools are written in .NET Core, and the Core Tools package is best installed using the Node.js package manager, npm, which is why you need to install .NET Core and Node.js at present, even for working with Azure Functions in Python. You can, however bypass the .NET Core requirement using "extension bundles" as described in the aforementioned documentation. Whatever the case, you need install these components only once, after which Visual Studio Code automatically prompts you to install any updates.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python 和 Azure Functions 扩展

安装以下软件：

- Azure Functions 所需的 Python 3.7 或 Python 3.6。 [Python 3.7.5](https://www.python.org/downloads/release/python-375/) 和 [Python 3.6.8](https://www.python.org/downloads/release/python-368/) 是最新的兼容版本。 在这些页面上向下滚动，找到安装程序。 安装时，请选择“向 PATH 添加 Python 3.x”并通过选择“立即安装”选项来使用默认选项   。 在 Windows 上，还需在此过程结束时选择“禁用路径长度限制”  。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，详见 [Visual Studio Code Python 教程 - 先决条件](https://code.visualstudio.com/docs/python/python-tutorial)。
- [Azure Functions 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 如需常规信息，请访问 [vscode-azurefunctions GitHub 存储库](https://github.com/Microsoft/vscode-azurefunctions)。

    > [!NOTE]
    > Azure Functions 扩展包含在 [Azure 工具扩展包](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)中。

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>验证先决条件

若要验证是否已安装所有 Azure Functions 工具，请打开 Visual Studio Code 命令面板 (**F1**)，选择“终端:  创建新的集成终端”命令，等到终端打开后，运行 `func` 命令：

![检查 Azure Functions 核心工具先决条件](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

输出以 Azure Functions 徽标（需向上滚动输出）开头表示 Azure Functions Core Tools 存在。

如果无法识别 `func` 命令，请再次运行 `npm install -g azure-functions-core-tools`，并验证安装是否成功。 另外，还请确保将 `-g` 开关用于 install 命令；否则，npm 只会在当前文件夹中安装包。

`func` 命令通过安装在 node.js 全局文件夹中的 *func* 文件发挥作用。 若要查看此文件夹的位置，请运行 `npm -l`，并检查输出末尾的位置。

> [!div class="nextstepaction"]
> [我已登录到 Azure](tutorial-vs-code-serverless-python-02.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
