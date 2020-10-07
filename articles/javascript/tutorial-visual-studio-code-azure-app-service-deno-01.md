---
title: 将 Deno 应用从 Azure CLI 部署到 Azure 应用服务
description: Deno 教程第 1 部分：简介和先决条件。
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: e4c521edd2f23576842d90979813f96a1812f0ec
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364940"
---
# <a name="deploy-deno-to-azure-app-service-using-visual-studio-code"></a>使用 Visual Studio Code 部署到演示 Azure 应用服务

在本教程中，我们使用 Azure CLI 将 Deno 应用程序部署到（Linux 或 Windows 上的）Azure 应用服务。

## <a name="prerequisites"></a>先决条件

- 具有活动订阅的 Azure 帐户。 [免费创建一个](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Deno](https://deno.land/#installation)
- 已安装并登录到 Azure CLI

## <a name="install-or-run-in-azure-cloud-shell"></a>在 Azure Cloud Shell 中安装或运行

开始使用 Azure CLI 的最简单方法是通过浏览器在 Azure Cloud Shell 环境中运行它。 若要了解 Cloud Shell，请参阅 [bash in Azure Cloud Shell 快速入门](/azure/cloud-shell/quickstart)。

准备好安装 CLI 时，请参阅[安装说明](/cli/azure/install-azure-cli)。

在第一次安装 CLI 后，请通过运行 `az --version` 检查它是否已安装，以及所安装的版本是否正确。

> [!NOTE]
> 如果使用 Azure 经典部署模型，请[安装 Azure 经典 CLI](/cli/azure/install-classic-cli)。

## <a name="sign-in"></a>登录

对本地安装使用任何 CLI 命令之前，需要使用 [az login](/cli/azure/reference-index#az-login) 登录。

[!INCLUDE [interactive-login](../azure-cli/includes/interactive-login.md)]

登录后，你将看到与你的 Azure 帐户关联的订阅列表。 在使用 `isDefault: true` 的情况下显示的订阅信息是登录后当前已激活的订阅。 若要选择另一个订阅，请将 [az account set](/cli/azure/account#az-account-set) 命令与要切换到的订阅 ID 配合使用。 有关订阅选择的详细信息，请参阅[使用多个 Azure 订阅](/cli/azure/manage-azure-subscriptions-azure-cli)。

> [!div class="nextstepaction"]
> [我已安装并登录了 Azure CLI](tutorial-visual-studio-code-azure-app-service-deno-02.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=getting-started)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
