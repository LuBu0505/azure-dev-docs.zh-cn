---
title: Azure 术语速查表
description: 使用 Microsoft Azure 时，需要了解的最重要的术语和概念的简短列表。
ms.date: 12/07/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 0be44d246af0f34b60b9ca1403f1889cc5708e9c
ms.sourcegitcommit: 1901759f41adfac3c3f2ff135bcf72206543b639
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/09/2020
ms.locfileid: "96934283"
---
# <a name="azure-terminology-in-brief"></a>Azure 术语概述

| 术语 | 简短说明 |
| --- | --- |
| [帐户和订阅](#account-and-subscriptions) | 有关管理 Azure 上资源的计费信息和基本组织结构。 [更多信息...](#account-and-subscriptions)
| [资源](#resource) | Azure 数据中心内任何特定功能分配的通用名称。 [更多信息...](#resource) |
| [资源组](#resource-group) | 其他资源的逻辑容器，可将其作为一个单元进行管理。 [更多信息...](#resource-group) |
| [区域](#region-location) | 对在其中分配资源的特定 Azure 数据中心的引用。 [更多信息...](#region-location) |
| [Azure 应用服务](#azure-app-service) | Azure 管理的适用于 Web 应用程序的托管服务。 [更多信息...](#azure-app-service) |
| [应用服务计划](#app-service-plan) | 定义 Azure 应用服务使用的虚拟机的资源。 [更多信息...](#app-service-plan) |
| [Azure 门户](#azure-portal) | 用于创建和管理 Azure 资源的基于 Web 的 UI。 [更多信息...](#azure-portal) |
| [Azure CLI](#azure-command-line-interface-cli) | 用于创建和管理 Azure 资源的一组基于文本的命令。 [更多信息...](#azure-command-line-interface-cli) |
| [az webapp、az webapp up](#az-webapp-az-webapp-up) | 用于 Azure 应用服务的 Azure CLI 命令。 [更多信息…](#az-webapp-az-webapp-up) |

## <a name="account-and-subscriptions"></a>帐户和订阅

Azure 帐户包含基本联系信息（电话号码、电子邮件地址）和计费信息（信用卡）。

在单个计费帐户中，你可将活动组织到一个或多个订阅中。 每个订阅都有自己的权限，因此你可为个人或部门创建单独的订阅，而所有订阅全都保留在同一帐户下。 个人可以访问任意数量的订阅。

在 Azure 上创建任何资源始终在订阅的上下文中完成。 可以[在订阅之间移动资源](/azure/azure-resource-manager/management/move-resource-group-and-subscription)，但不能在帐户之间移动。

## <a name="resource"></a>资源

资源是 Azure 数据中心内任何特定功能分配的通用名称。 资源可以是虚拟机、虚拟网络、各种级别的存储、数据库、机器学习模型、IoT 引入中心等等。

由于资源分配的是实际的计算功能，因此每个资源可能会持续产生费用，这取决于所需的性能级别。 对于开发和测试，很多资源可使用免费层进行创建。 有关详细详细，请参阅[定价计算器](https://azure.microsoft.com/pricing/calculator/)。

## <a name="resource-group"></a>资源组

在订阅中，资源组是其他资源的逻辑容器，可将其作为一个单元进行管理。

资源组通常与特定的项目相关，在预配资源时，必须始终指定资源组。 新项目的第一步通常是创建相应的资源组。

删除资源组会取消分配并删除其包含的所有资源，这比单独删除每个资源更方便。

## <a name="region-location"></a>区域（位置）

区域标识在其中预配资源的 Azure 数据中心的特定位置。

不同的资源可以始终跨区域通信，但当资源位于同一区域时，通信会更高效。

为全球客户提供服务的应用程序可以让 Azure 跨多个区域自动复制资源，以提高应用程序的总体响应能力和性能。

## <a name="azure-app-service"></a>Azure 应用服务

应用服务是 Azure 管理的适用于 Web 应用程序的托管服务。 Azure 管理所有基础硬件和服务器基础结构，而你提供代码和配置。

借助应用服务，你可预配托管（称为应用服务 Web 应用）并上传代码。 还可配置托管的各种特征，例如负载均衡、缩放、服务器端环境变量等。

应用服务 Web 应用的直接 URL 始终是 `<web-app-name>.azurewebsites.net`。 还可将 Web 应用配置为使用自定义域。

## <a name="app-service-plan"></a>应用服务计划

应用服务计划是一种资源，用于定义 Azure 应用服务所使用的虚拟机，从而确定托管应用程序的核心成本。 在部署代码之前，将应用服务计划定义为预配应用服务的一部分。

## <a name="azure-portal"></a>Azure 门户

[Azure 门户](https://portal.azure.com)是基于 Web 的用户界面，通过该界面可使用 Azure 帐户、订阅和资源。

## <a name="azure-command-line-interface-cli"></a>Azure 命令行接口 (CLI)

[Azure CLI](/azure/what-is-azure-cli) 是一组用来创建和管理 Azure 资源的命令，这对自动化特别有用。 Azure CLI 可在所有操作系统上使用，并且可在大多数 Azure 服务中运行。

如果你更愿意使用 PowerShell，可使用 [Azure PowerShell 模块](/powershell/azure)。

## <a name="az-webapp-az-webapp-up"></a>az webapp、az webapp up

az webapp 命令是通过 Azure CLI 与 Azure 应用服务的所有方面进行交互的方式。

具体来说，az webapp up 命令可简化 Web 应用程序的部署。 此单个命令可以预配资源组、应用服务计划和应用服务 Web 应用，然后再上传代码，所有这些都是一次性完成的。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [云部署概述 >>>](cloud-development-overview.md)
