---
title: Azure CLI 产品之间的差异
description: 了解如何对 Azure CLI 产品执行命名、版本控制和升级操作。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 3cd61d2166d03f7b9d58db1ee1cee77d17b5b336
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030829"
---
# <a name="differences-between-azure-cli-products"></a>Azure CLI 产品之间的差异

在 2018 年 6 月末，显式版本号已从 Azure CLI 产品名称中去除。 这种更改有助于消除用户有时候在阅读文档时遇到的困惑，具体说来就是，用户在被告知使用“Azure CLI”时，并不清楚这是指哪个版本的产品。 如果熟悉旧的产品名称，则应该容易理解下面列出的具体更改：

* Azure CLI 2.0 及更高版本现在直接称为“Azure CLI”。
* 更早的 Azure CLI 版本（1.x 及更低版本）现在称为“Azure 经典 CLI”。

将名称更改为“Azure 经典 CLI”清楚地表明，此工具仅适用于经典部署模型。 经典 CLI 也不再进行更新或维护。 由于这个原因以及更多其他原因，建议放弃经典部署模型而改用 Azure 资源管理器模型，并迁移到最近提供的 Azure CLI 版本。

如果仍使用经典 CLI，可通过以下文章了解迁移过程：

* [从经典部署迁移到 Azure 资源管理器部署](/azure/virtual-machines/linux/migration-classic-resource-manager-overview)
* [安装 Azure CLI](install-azure-cli.md)
* [Migrating from Azure classic CLI to Azure CLI](https://github.com/Azure/azure-cli/blob/dev/doc/classic_cli_migration.md)（从 Azure 经典 CLI 迁移到 Azure CLI）
