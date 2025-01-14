---
title: 教程：使用 VS Code 通过 Python 创建并部署无服务器 Azure Functions
description: 教程步骤 1，为无服务器 Azure Functions 配置本地环境
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperfq2
ms.openlocfilehash: a4fbf9d46a4158fe67660db9f9b7eae8ff83e810
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96759434"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>教程：使用 Visual Studio Code 在 Python 中创建并部署无服务器 Azure Functions

在本文中，我们使用 Visual Studio Code 和 Azure Functions 扩展通过 Python 创建一个“无服务器”HTTP 终结点，并将连接（或“绑定”）添加到存储。 用于 Visual Studio Code 的 Azure Functions 扩展可以自动处理许多配置问题，大大简化了使用 Functions 的过程。

Azure Functions 的无服务器环境意味着 Azure 可提供应用的终结点和公共 URL，而无需预配虚拟机、发布 Web 应用或以其他方式管理服务器和资源。 Azure 为你高效地管理所有这些资源，显著降低了托管应用程序的开销和成本。 （有关详细信息，请参阅 [Azure Functions 概述](/azure/azure-functions/functions-overview)。）

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 使用每篇文章末尾的 **此页** 反馈按钮。

有关演示视频，请观看虚拟 PyCon 2020 中的<a href="https://www.youtube.com/watch?v=9bMsdBYy-D0&feature=youtu.be&ocid=AID3006292" target="_blank">使用 VS Code 生成 Azure Functions</a> (youtube.com)。 你可能还会对<a href="https://www.youtube.com/watch?v=PV7iy6FPjAY&feature=youtu.be&t=13&ocid=AID3006292" target="_blank">通过 Azure Functions 轻松处理数据</a> (youtube.com) 会话感兴趣，该会话时间比较长。

## <a name="configure-your-environment"></a>配置环境

- 如果没有具备有效订阅的 Azure 帐户，请[免费创建一个](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)。

- 按照后续部分中的说明操作：

  - [安装 Azure Functions Core Tools](#azure-functions-core-tools)。

  - [安装 Python、Visual Studio Code 和 Azure Functions 扩展](#visual-studio-code-python-and-the-azure-functions-extension)。

  - [登录到 Azure](#sign-in-to-azure)

  - [验证环境](#verify-your-environment)
 
### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

按照[使用 Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 一文中适用于你的操作系统的说明操作。 忽略有关 Chocolatey 包管理器的文章中的注释，这些注释对于完成本教程而言并非必需。

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

安装 Azure 扩展以后，请通过导航到 **Azure** 资源管理器登录到 Azure 帐户，选择“Functions”下的“登录 Azure”，然后按照浏览器中的提示操作。

![通过 VS Code 登录到 Azure](media/tutorial-vs-code-serverless-python/azure-sign-in.png)

登录后，验证状态栏中是否显示“Azure：已登录”，且 Azure 资源管理器中显示了你的订阅：

![Visual Studio Code 状态栏，其中显示了 Azure 帐户](media/tutorial-vs-code-serverless-python/azure-account-status-bar.png)

![Visual Studio Code 的 Azure 应用服务资源管理器，其中显示了订阅](media/tutorial-vs-code-serverless-python/azure-subscription-view.png)

[!INCLUDE [proxy-config](includes/proxy-config.md)]

### <a name="verify-your-environment"></a>验证环境

若要验证是否已安装所有 Azure Functions 工具，请打开 Visual Studio Code 命令面板 (**F1**)，选择“终端:创建新的集成终端”命令，等到终端打开后，运行 `func` 命令：

![检查 Azure Functions 核心工具先决条件](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

输出以 Azure Functions 徽标（需向上滚动输出）开头表示 Azure Functions Core Tools 存在。

如果无法识别 `func` 命令，请再次运行 `npm install -g azure-functions-core-tools`，并验证安装是否成功。 另外，还请确保将 `-g` 开关用于 install 命令；否则，npm 只会在当前文件夹中安装包。

`func` 命令通过安装在 node.js 全局文件夹中的 *func* 文件发挥作用。 若要查看此文件夹的位置，请运行 `npm -l`，并检查输出末尾的位置。

> [!div class="nextstepaction"]
> [我已登录到 Azure - 转到步骤 2 >>>](tutorial-vs-code-serverless-python-02.md)

[存在问题？请告诉我们。](https://aka.ms/python-functions-qs-ms-survey)
