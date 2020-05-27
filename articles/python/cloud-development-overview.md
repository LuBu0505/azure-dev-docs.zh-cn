---
title: 使用 Azure 进行云开发 - 什么是 Azure？
description: 概述了如何在 Microsoft Azure 上开发云应用，首先从介绍数据中心、服务和资源之间的关系开始。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 815da765aaed1e8364c37f621f17f279212bf77f
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404950"
---
# <a name="cloud-development-on-azure"></a>在 Azure 上进行云开发

你是一名 Python 开发人员，已经准备好开发 Microsoft Azure 云应用。 为了帮助你为长期且富有成效的职业生涯做好准备，本系列的三篇文章将向你介绍在 Azure 上进行云开发的基本情况。

## <a name="what-is-azure-data-centers-services-and-resources"></a>什么是 Azure？ 数据中心、服务和资源

Microsoft 首席执行官 Satya Nadella 经常将 Azure 称为“世界计算机”。 如你所知，计算机是由操作系统管理的硬件集合，这提供了一个平台，你可以在此平台上生成软件，有助于人们将系统的计算能力应用于任意数量的任务。 （正因为此，我们使用“应用”一词来描述此类软件。）

对于 Azure，计算机的硬件不是一台计算机，而是[全世界数十个海量数据中心](https://azure.microsoft.com/global-infrastructure/regions/)中包含的虚拟化服务器计算机的巨型池。 然后，Azure“操作系统”由服务组成，这些服务根据应用需求来动态分配和取消分配此资源池的不同部分。 每个分配（计算能力（CPU 核心和内存）、存储、数据库、网络等）都被称为“资源”。 而且，每个离散资源相应地分配有一个唯一对象标识符（即 GUID）和唯一 URL。

![Azure 层（从数据中心到用于分配资源的 Azure 服务）](media/cloud-development/azure-layers.png)

资源是云应用的构建基块。 因此，云开发流程从创建适当的环境开始，你可以在其中部署应用的不同部分。 简而言之，在分配并配置（即预配）合适的目标资源（如虚拟机、数据库、存储帐户、容器注册表、容器业务流程协调程序、Web 主机、虚拟网络、AI 和分析引擎等）之前，你无法将任何代码或数据部署到 Azure。

然后，为应用创建环境的过程涉及到确定相关的服务和资源类型，然后预配这些资源（此时开始从 Azure 租用它们）。 事实上，有数百个不同类型的资源可供你任意使用，包括从让你保留对所部署软件的完全控制和责任的基本“基础结构”资源（如虚拟机），到提供让你只关注数据和应用代码且更易于管理的环境的更高级“平台”服务。

为应用找到合适的服务，并平衡它们的相对成本，这可能很有挑战性，但也是云开发的创造性乐趣的一部分。 此开发人员中心内的其他文章有助于你了解如何进行选择。 在此期间，让我们讨论一下如何实际使用所有这些服务和资源。

> [!NOTE]
> 你可能已经见过并厌倦了“IaaS”（基础结构即服务）、“PaaS”（平台即服务）等术语。 “即服务”部分反映了这样一个事实，即你通常没有对数据中心本身的物理访问权限。 相反，你使用 Azure 门户、Azure CLI 或 Azure REST API 等工具来预配“基础结构”资源、“平台”资源等。 作为一项“服务”，Azure 始终随时等待接收你的请求。
>
> 在此开发人员中心内，我们为你省去了 IaaS、PaaS 等术语，因为“即服务”一开始就是云所固有的！

> [!NOTE]
> “混合云”是指私有计算机和数据中心与 Azure 等云资源的组合，除了前面讨论的内容之外，它还有自己的注意事项。 此外，本讨论假定是进行新的应用开发；这里不讨论涉及重新架构和迁移现有本地应用的方案。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [预配、访问和管理资源 >>>](cloud-development-provisioning.md)
