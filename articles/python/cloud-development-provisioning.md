---
title: 在 Azure 上预配、访问和管理资源
description: 概述了用于处理 Azure 资源的方法，包括 Azure 门户、Azure CLI 和 Azure SDK。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 585c40523ba2be311af2d65fc3d4cbb679ba6f60
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404970"
---
# <a name="provisioning-accessing-and-managing-resources-on-azure"></a>在 Azure 上预配、访问和管理资源

[前一篇文章：概述](cloud-development-overview.md)

如本系列前面的文章中所述，开发云应用程序的基本步骤是在 Azure 中预配必要的资源，然后可以将代码和数据部署到这些资源中。

此预配具体是如何操作的？ 如何请求 Azure 为应用程序分配资源，以及如何配置和访问这些资源？ 简而言之，你如何与 Azure 本身通信以获取所有这些资源？

## <a name="means-of-communicating-with-azure"></a>与 Azure 通信的方法

答案非常简单。 与大多数操作系统一样，你可以通过三个路由与 Azure 通信：用户界面、命令行接口和 API。

![与 Azure 进行通信以预配资源的不同方法](media/cloud-development/communication-with-azure.png)

可以使用以上任何一种补充方法或所有方法来创建、配置和管理所需的任何 Azure 资源。 事实上，你通常会在开发项目过程中使用这三个方法，并且需要熟悉每一种方法。

在此开发人员中心，我们主要介绍如何使用使用 Azure SDK 的 CLI 和 Python 代码，因为每个服务的相关文档都详细介绍了门户的使用。

## <a name="azure-portal"></a>Azure 门户

[Azure 门户](https://portal.azure.com)是 Azure 基于浏览器、完全可自定义的用户界面，通过该界面，你可以使用所有 Azure 服务预配和管理资源。 若要访问门户，必须先使用 Microsoft 帐户登录，然后使用订阅创建免费的 Azure 帐户。 （登录后，你可以选择“?” 图标，并选择“启动指导教程”，以获取主要门户功能的简单演练。）

优点：用户界面使你可以轻松地浏览服务及其各种配置选项。 设置配置值是安全的，因为本地工作站上没有存储任何信息。

缺点：使用门户是一个手动过程，无法自动执行。 例如，若要记住更改配置所做的操作，请在单独的文档中记录步骤。

## <a name="azure-cli"></a>Azure CLI

[Azure CLI](/cli/azure/?view=azure-cli-latest) 是 Azure 的[开源](https://github.com/Azure/azure-cli)命令行界面。 登录到 CLI（使用 `az login` 命令）后，可以执行可通过门户执行的相同任务。
  
优点：通过脚本和输出处理轻松实现自动化。 提供更高级别的命令，为常见任务（如部署 Web 应用）同时预配多个资源。 可以在源代码管理中管理脚本。

缺点：与使用门户相比，学习曲线更陡峭，且命令容易出现 bug。 错误消息并非始终有用。

还可以使用 [Azure PowerShell](/powershell/) 来代替 Azure CLI，尽管 Python 开发人员通常更熟悉 Azure CLI 的 Linux 样式命令。

若要代替本地 CLI 或 PowerShell，可以直接通过 [https://shell.azure.com/](https://shell.azure.com/) 使用 Azure Cloud Shell。 但是，因为 Cloud Shell 不是本地环境，所以它更适合一次性操作而非自动化操作。

## <a name="azure-rest-api-and-azure-sdk"></a>Azure REST API 和 Azure SDK

[Azure REST API](/rest/api/?view=Azure) 是 Azure 的编程接口，通过 HTTP 上的安全 REST 提供，因为 Azure 的数据中心本质上都连接到 Internet。 为每个资源分配一个唯一的 URL，该 URL 支持特定于资源的 API，但受制于严格的身份验证协议和访问策略。 （实际上，Azure 门户和 Azure CLI 最终通过 REST API 完成其工作。）

对于开发人员而言，[Azure SDK](https://azure.microsoft.com/downloads/) 提供特定于语言的库，这些库将 REST API 功能转换成更方便的编程模式，如类和对象。 对于 Python，始终使用 `pip install` 安装各个 SDK 库，而不是将 SDK 作为一个整体安装。

优点：精确控制所有操作，包括一种更直接的方法，即使用一个操作的输出作为另一个操作的输入。 对于 Python 开发人员，允许在熟悉的语言模式下工作，而不是使用 CLI。 也可以从应用程序代码中使用，以自动执行管理方案。
  
缺点：可以使用一个 CLI 命令完成的操作通常需要多行代码，所有这些代码都可能出现 bug。 不提供更高级别的操作，如 Azure CLI。

## <a name="automatic-on-demand-provisioning"></a>自动按需预配

许多 Azure 服务允许配置缩放特性以满足可变需求，在这种情况下，Azure 可以在需要时自动预配其他资源，并根据需要取消分配资源。 这种自动缩放是由多个数据中心的资源支持的云平台的主要优点之一。 无需针对高峰需求设计环境，为通常不使用的容量付费，而是可以为基线或平均使用情况设计环境，并只在需要时为额外功能付费。

有关详细信息，请参阅 Azure 体系结构中心的[自动缩放](/azure/architecture/best-practices/auto-scaling)。

## <a name="subscriptions-resource-groups-and-regions"></a>订阅、资源组和区域

在 Azure 资源模型中，你可以想像，随着时间的推移，你将为不同应用程序跨多个 Azure 服务预配许多不同的资源。 可以使用三个层次结构级别来组织这些资源：

1. 订阅：每个 Azure 订阅都有自己的计费帐户，通常表示组织中的不同团队或部门。 通常情况下，可以为同一订阅中的任何给定应用程序预配所需的所有资源，以便它们可以从共享身份验证等功能中获益。 但是，因为所有资源都可以通过公共 URL 和必要的授权令牌进行访问，所以当然可以将资源分布到多个订阅。

1. 资源组：在订阅内，资源组是其他资源的容器，可以作为一个组进行管理。 （因此，资源组通常与特定项目相关。）事实上，在预配资源时，必须指定所属的组。 新项目的第一步通常是创建相应的资源组。 删除资源组会取消分配其包含的所有资源，而无需单独删除每个资源。 相信我们，如果忽视对资源组的组织，那么当你后面不记得哪个资源属于哪个项目时，可能会遇到很多麻烦！

1. 资源命名：在资源组中，你可以使用任何你喜欢的命名策略来表达资源之间的共性或关系。 由于名称通常在资源的 URL 中使用，因此可能会对可使用的字符有限制。 （例如，某些名称只允许使用字母和数字，而其他名称则允许使用连字符和下划线。）

使用 Azure 时，你将制定自己的首选项来组织资源，并制定自己的约定来命名订阅、资源组和单个资源组。

### <a name="regions-and-geographies"></a>区域和地理位置

资源组的一项重要特征是，它始终与特定的 Azure 区域相关联，后者是特定数据中心的位置。 同一组中的所有资源都在该数据中心共存，因此，可以比位于不同区域的资源更有效地进行交互。 开发人员通常会选择离客户最近的区域，从而优化应用程序的响应能力。 Azure 还提供了异地复制功能，可跨多个区域同步应用程序和数据库的副本，这样就可以更好地为全球客户群服务。

由于本地法律和法规（由你在其中创建订阅的地域确定），你可能只能访问特定区域，而这些区域可能不支持所有 Azure 服务。 有关详细信息，请参阅 [Azure 全球基础结构](https://azure.microsoft.com/global-infrastructure/)。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 开发流程 >>>](cloud-development-flow.md)