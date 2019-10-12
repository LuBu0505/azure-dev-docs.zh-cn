---
title: 教程：使用 Visual Studio Code 在 Python 中创建并部署无服务器 Azure Functions
description: 教程步骤 1，简介和先决条件。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 792c9962d738a8e70f29d5df78c44b6303a63b77
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172290"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>教程：使用 Visual Studio Code 在 Python 中创建并部署无服务器 Azure Functions

在本文中，我们使用 Visual Studio Code 和 Azure Functions 扩展通过 Python 创建一个无服务器 HTTP 终结点，并将连接（或“绑定”）添加到存储。 Azure Functions 在无服务器环境中运行代码，不需预配虚拟机，也不需发布 Web 应用。 用于 Visual Studio Code 的 Azure Functions 扩展可以自动处理许多配置问题，大大简化了使用 Functions 的过程。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”  按钮来提交反馈。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code 与 Azure Functions 扩展](#visual-studio-code-python-and-the-azure-functions-extension)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)一个免费使用 30 天的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python 和 Azure Functions 扩展

安装以下软件：

- Azure Functions 所需要的 Python 3.6.x。 [Python 3.6.8](https://www.python.org/downloads/release/python-368/) 是最新的 3.6.x 版本。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，详见 [Visual Studio Code Python 教程 - 先决条件](https://code.visualstudio.com/docs/python/python-tutorial)。
- [Azure Functions 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 如需常规信息，请访问 [vscode-azurefunctions GitHub 存储库](https://github.com/Microsoft/vscode-azurefunctions)。

> [!NOTE]
> Azure Functions 扩展包含在 [Azure 工具扩展包](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)中。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

按照[使用 Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 一文中适用于你的操作系统的说明操作。 工具本身以 .NET Core 编写，Core Tools 包最好使用 Node.js 包管理器（简称 npm）进行安装。因此，目前需要安装 .NET Core 和 Node.js，即使使用 Python 代码也是如此。 不过，可以使用“扩展捆绑”来规避 .NET Core 要求，详见前述文档。 不管什么情况，你只需安装这些组件一次，然后 Visual Studio Code 就会自动提示你安装任何更新。

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>验证先决条件

若要验证是否已安装所有 Azure Functions 工具，请打开 Visual Studio Code 命令面板 (**F1**)，选择“终端:  创建新的集成终端”命令，等到终端打开后，运行 `func` 命令：

![检查 Azure Functions Core Tools 先决条件](media/tutorial-vs-code-serverless-python/check-prereqs.png)

输出以 Azure Functions 徽标（需向上滚动输出）开头表示 Azure Functions Core Tools 存在。

如果系统无法识别 `func` 命令，则验证在其中安装了 Azure Functions Core Tools 的文件夹是否包含在 PATH 环境变量中。

> [!div class="nextstepaction"]
> [我已登录到 Azure](tutorial-vs-code-serverless-python-02.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
