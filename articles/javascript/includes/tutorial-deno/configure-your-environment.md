---
title: include 文件 1
description: include 文件 1
ms.topic: include
ms.date: 06/01/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 8f0bda8929699f61fd3c79f9ae6fb38762e622c9
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278632"
---
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

[!INCLUDE [interactive-login](../../../azure-cli/includes/interactive-login.md)]

登录后，你将看到与你的 Azure 帐户关联的订阅列表。 在使用 `isDefault: true` 的情况下显示的订阅信息是登录后当前已激活的订阅。 若要选择另一个订阅，请将 [az account set](/cli/azure/account#az-account-set) 命令与要切换到的订阅 ID 配合使用。 有关订阅选择的详细信息，请参阅[使用多个 Azure 订阅](/cli/azure/manage-azure-subscriptions-azure-cli)。
