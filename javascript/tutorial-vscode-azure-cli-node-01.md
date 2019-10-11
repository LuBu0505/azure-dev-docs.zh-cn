---
title: 使用 Azure CLI 将 Node.js 应用部署到 Azure 应用服务
description: 教程第 1 部分：简介和先决条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 2c566e43bce15672d4ae73310f96c91f4a6f137a
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686174"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>使用 Azure CLI 部署到 Azure 应用服务

在本教程中，将使用在所有操作系统上运行的 [Azure 命令行接口 (CLI)](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) 将 Node.js 应用程序部署到 Azure 应用服务。 使用 CLI 可以创建 Azure 资源，在 Git 存储库和 Azure 之间建立部署管道，并查看应用的 `console.log` 输出。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Node.js 和 npm 6.x 或更高版本](https://nodejs.org/en/download)，即 Node.js 包管理器。
- [Git](https://git-scm.com/downloads)，其后命令 `git --version` 应显示版本号。
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

可以选择使用 [Visual Studio Code 的 Azure CLI 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)，在编写 Azure CLI 脚本时该扩展提供语法彩色显示、IntelliSense (完成) 和代码片段。

另一种替代方法是 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)，可以使用 [Azure 帐户扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)从 Visual Studio Code 中使用它。

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

### <a name="sign-in-to-the-azure-cli"></a>登录 Azure CLI

安装 Azure CLI 后，请从终端或命令提示符运行以下命令：

```bash
az login
```

该命令将打开一个浏览器窗口，要求你在其中登录 Azure。 登录后，终端窗口会显示包含订阅详细信息的 JSON 输出。

## <a name="check-npm-version"></a>检查 npm 版本

如果已安装了 Node.js，请运行 `node -v` 命令并验证版本是否为 10.x 或更高版本。 如果使用的是旧版本，请[升级](https://nodejs.org/en/download/)到最新的 LTS（“长期支持”）版本。

> [!div class="nextstepaction"]
> [我安装了必备项](tutorial-vscode-azure-cli-node-02.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)