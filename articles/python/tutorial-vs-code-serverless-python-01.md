---
title: 教程：使用 VS Code 通过 Python 创建并部署无服务器 Azure Functions
description: 教程步骤 1, 使用 Azure Functions, 简介和先决条件。
ms.topic: conceptual
ms.date: 05/19/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 740a64785c57694be34f37ef6aa6571b0b3304b7
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473602"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>教程：使用 Visual Studio Code 在 Python 中创建并部署无服务器 Azure Functions

在本文中，我们使用 Visual Studio Code 和 Azure Functions 扩展通过 Python 创建一个无服务器 HTTP 终结点，并将连接（或“绑定”）添加到存储。

Azure Functions 在无服务器环境中运行代码，不需预配虚拟机，也不需发布 Web 应用。 用于 Visual Studio Code 的 Azure Functions 扩展可以自动处理许多配置问题，大大简化了使用 Functions 的过程。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”按钮来提交反馈。

有关演示视频，请观看虚拟 PyCon 2020 中的<a href="https://www.youtube.com/watch?v=9bMsdBYy-D0&feature=youtu.be&ocid=AID3006292" target="_blank">使用 VS Code 生成 Azure Functions</a> (youtube.com)。 你可能还会对<a href="https://www.youtube.com/watch?v=PV7iy6FPjAY&feature=youtu.be&t=13&ocid=AID3006292" target="_blank">通过 Azure Functions 轻松处理数据</a> (youtube.com) 会话感兴趣，该会话时间比较长。 

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。
- [Visual Studio Code 与 Azure Functions 扩展](#visual-studio-code-python-and-the-azure-functions-extension)。

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)一个免费使用 30 天的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

按照[使用 Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 一文中适用于你的操作系统的说明安装 Azure Functions Core Tools。 忽略有关 Chocolatey 包管理器的文章中的注释，这些注释对于完成本教程而言并非必需。

安装 Node.js 时，请使用默认选项，不要选择用于自动安装必要工具的选项。  另外，请务必将 `-g` 选项与 `npm install` 命令一起使用，以使 Core Tools 可用于后续命令。

> [!TIP]
> Core Tools 以 .NET Core 编写，并且 Core Tools 包最好使用 Node.js 包管理器（简称 npm）进行安装。因此，目前需要安装 .NET Core 和 Node.js，即使要通过 Python 使用 Azure Functions 也是如此。 不过，可以使用“扩展捆绑”来规避 .NET Core 要求，详见前述文档。 不管什么情况，你只需安装这些组件一次，然后 Visual Studio Code 就会自动提示你安装任何更新。

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python 和 Azure Functions 扩展

安装以下软件：

- Azure Functions 所需的 Python 3.6、3.7 或 3.8 的 64 位版本。 从 [python.org](https://www.python.org/downloads) 安装 Python安装时，请选择“向 PATH 添加 Python 3.x”并通过选择“立即安装”选项来使用默认选项 。 在 Windows 上，还需在此过程结束时选择“禁用路径长度限制”。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，详见 [Visual Studio Code Python 教程 - 先决条件](https://code.visualstudio.com/docs/python/python-tutorial)。
- [Azure Functions 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 如需常规信息，请访问 [vscode-azurefunctions GitHub 存储库](https://github.com/Microsoft/vscode-azurefunctions)。

    > [!NOTE]
    > Azure Functions 扩展包含在 [Azure 工具扩展包](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)中。

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>验证先决条件

若要验证是否已安装所有 Azure Functions 工具，请打开 Visual Studio Code 命令面板 (**F1**)，选择“终端:创建新的集成终端”命令，等到终端打开后，运行 `func` 命令：

![检查 Azure Functions 核心工具先决条件](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

输出以 Azure Functions 徽标（需向上滚动输出）开头表示 Azure Functions Core Tools 存在。

如果无法识别 `func` 命令，请再次运行 `npm install -g azure-functions-core-tools`，并验证安装是否成功。 另外，还请确保将 `-g` 开关用于 install 命令；否则，npm 只会在当前文件夹中安装包。

`func` 命令通过安装在 node.js 全局文件夹中的 *func* 文件发挥作用。 若要查看此文件夹的位置，请运行 `npm -l`，并检查输出末尾的位置。

> [!div class="nextstepaction"]
> [我已登录到 Azure - 转到步骤 2 >>>](tutorial-vs-code-serverless-python-02.md)

问题？ 使用页面底部的“此页面”反馈提交 GitHub 问题。
