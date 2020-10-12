---
title: 在 Visual Studio Code 中将应用部署到 Azure 应用服务
description: Node.js 教程第 3 部分：部署网站
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: 4c96aedf678fb4e28875ceb211c6d2c505afea6f
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764559"
---
# <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

[上一步：创建应用](tutorial-vscode-azure-app-service-node-02.md)

这一步使用通过 VS Code 和 Azure 应用服务扩展执行的 Git deploy 将 Node.js 应用部署到 Azure。 若要实现此目标，请先初始化本地 Git 存储库，接着在 Azure 上创建一个 Web 应用，然后将 VS Code 配置为使用 Git deploy。

1. 在终端中，确保你的位置在 *expressApp1* 文件夹中，然后通过以下命令启动 Visual Studio Code：

    ```bash
    code .
    ```

1. 在 VS Code 中，选择源代码管理图标来打开源代码管理器，然后选择“初始化存储库”来初始化本地 Git 存储库 ：

    ![初始化 git 存储库](media/deploy-azure/git-init.png)

1. 出现提示时，选择 *expressApp1* 作为工作区文件夹。

1. 初始化存储库后，输入“初始提交”消息，并选中与创建源文件的初始提交相对应的复选标记。

    ![完成目标为存储库的初始提交](media/deploy-azure/initial-commit.png)

1. 在命令面板 (**Ctrl**+**Shift**+**P**) 中，键入“创建 Web”并选择“Azure 应用服务:创建新 Web 应用...高级”。 我们使用高级命令来完全控制部署（包括资源组、应用服务计划、操作系统），而不是使用 Linux 默认设置。

1. 响应提示，如下所述：

    - 选择你的“订阅”帐户。
    - 对于“输入全局唯一名称”，请输入在所有 Azure 中都是唯一的名称。 仅使用字母数字字符（“A-Z”、“a-z”和“0-9”）和连字符（“-”）
    - 选择“创建新的资源组”并提供一个名称，例如 `AppServiceTutorial-rg`。
    - 选择操作系统（Windows 或 Linux）
    - 仅 Linux：选择 Node.js 版本。 （对于 Windows，我们在以后使用应用设置来设置版本）。
    - 选择“创建新的应用服务计划”，提供一个名称（例如 `AppServiceTutorial-plan`），然后选择“F1 免费”定价层。
    - 对于 Application Insights 资源，选择“暂时跳过”。
    - 选择附近的位置。

1. 短时间过后，VS Code 会通知你创建已完成。 使用“X”按钮关闭通知：

    ![Web 应用创建完成后的通知](media/deploy-azure/creation-complete.png)

1. Web 应用就绪后，接下来指示 VS Code 部署本地 Git 存储库中的代码。 选择 Azure 图标以打开“Azure 应用服务”资源管理器，展开订阅节点，右键单击刚创建的 Web 应用的名称，然后选择“配置部署源”。

    ![在 Web 应用上配置部署源命令](media/deploy-azure/configure-deployment-source.png)

1. 出现提示时，选择“LocalGit”。

1. 如果部署到 Windows 上的应用服务，则需在部署之前创建两项设置：

    1. 在 VS Code 中展开新应用服务的节点，右键单击“应用程序设置”，然后选择“添加新设置”： 

        ![添加应用设置命令](media/deploy-azure/add-setting.png)

    1. 输入 `WEBSITE_NODE_DEFAULT_VERSION` 作为设置键，输入 `10.15.2` 作为设置值。 此设置设置 Node.js 版本。
    1. 重复此过程，为 `SCM_DO_BUILD_DURING_DEPLOYMENT` 创建值为 `1` 的键。 此设置强制服务器在部署后运行 `npm install`。
    1. 展开“应用程序设置”节点，验证设置是否已就位。

1. 选择蓝色的向上箭头图标，将代码部署到 Azure：

    ![“部署到 Web 应用”图标](media/deploy-azure/deploy.png)

1. 出现提示时，选择 expressApp1 文件夹并再次选择你的订阅帐户，然后选择此前创建的 Web 应用的名称。

1. 部署到 Linux 时，如果系统提示你更新配置以在目标服务器上运行 `npm install`，请选择“是”。

    ![在目标 Linux 服务器上更新配置的提示](media/deploy-azure/server-build.png)

1. 对于 Linux 和 Windows，当系统提示你“始终将工作区 "nodejs-docs-hello-world" 部署到 (应用名称)”时，请选择“是”。  选择“是”就是告知 VS Code 在进行后续部署时自动以同一应用服务 Web 应用为目标。

1. 部署完成后，选择提示中的“浏览网站”，查看全新部署的 Web 应用。 浏览器应会显示“Hello World!”

1. （可选）：可以对代码文件进行更改，然后再次使用部署按钮来更新 Web 应用。

> [!div class="nextstepaction"]
> [我的站点位于 Azure 上](tutorial-vscode-azure-app-service-node-04.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
