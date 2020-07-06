---
title: 通过 Visual Studio App Center 和 Azure 服务了解移动应用程序使用情况和用户行为
description: 了解 App Center 等服务，可使用这些服务了解用户如何使用你的移动应用程序，从而便于你制定明智的业务决策。
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 744e41200a5f7e5ff898e7a0786a4284de176b7a
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632589"
---
# <a name="analyze-and-understand-mobile-application-use"></a>分析和了解移动应用程序使用情况

你有多了解用户是如何使用你的应用程序的？ 你的应用程序有多少活跃用户，随时间推移，他们的使用情况有怎样的变化？ 他们使用哪些功能，最常使用哪些功能？ 这些用户位于何处？ 有多少用户在使用最新版本的应用程序？ 所有这些问题都非常重要，可帮助你将应用转变为成功的业务。 若要回答与“使用情况分析”有关的问题，需要从应用中收集使用情况数据。

越深入得了解数据，可能发现的可改进应用程序和使用户满意的方法就越多。 使用数据找到可操作的见解并保持用户满意度，这一点很重要。

## <a name="importance-of-analytics"></a>分析的重要性

- 了解你的用户、他们与你的应用程序的交互方式以及是什么让他们愿意再次使用你的应用程序，通过这种方式优化应用程序，提供出色体验，从而推进业务增长。
- 跟踪使用情况指标，以便能够明智决定如何营销应用程序并为客户提供更好的服务。
- 度量应用程序性能。
- 了解是应用程序的哪些元素在推动价值增长和性能优化。
- 获取客户流失和维系问题的数据驱动的见解。

使用以下服务启用移动应用程序分析。

## <a name="visual-studio-app-center"></a>Visual Studio App Center

借助 [App Center 分析](/appcenter/analytics/)，可通过专注于重要内容，赢得更多用户。 它提供有关用户会话、热门设备、OS 版本的深度报告和见解，以及行为分析。 轻松创建自定义事件，通过大量应用程序分析指标进行跟踪和分析。

### <a name="visual-studio-app-center-features"></a>Visual Studio App Center 功能

- 免费跟踪使用模式、用户采用情况和其他用户参与相关指标。
- 通过自定义事件确定趋势、用户行为和参与情况。
- 集中通过一个仪表板，获取有关应用程序使用情况（每天、每周、每月）、会话、设备属性和用户人口统计信息的现成指标和详细见解。
- 连续将所有 App Center 分析数据导出到 Azure 中，以便实现无限制的保留。 “App Center 分析”支持导出到 Azure Blob 存储和 Azure Application Insights。
- 与 Azure Application Insights 集成，以获得更深入的见解，如保留情况、漏斗分析和队列信息。
- 使用单行 SDK 集成，在几分钟内即可开始进行集成。
- 获取对 iOS、Android、macOS、tvOS、Xamarin、React Native、Unity 和 Cordova 的平台支持。

### <a name="visual-studio-app-center-references"></a>Visual Studio App Center 参考

- [注册 App Center](https://appcenter.ms/signup)
- [App Center Analytics 入门](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure Monitor

Azure Monitor 包含了 [Application Insights](/azure/azure-monitor/app/app-insights-overview)，后者提供所需工具用于收集和分析遥测，以便最大程度提高移动应用程序性能和进行监视。 可以通过使用 App Center Analytics 设置为导出到 Application Insights 来利用 Application Insights。 Application Insights 可以查询、分段、筛选和分析来自应用程序的自定义事件遥测，相比 App Center 提供的分析工具，其功能更为强大。

### <a name="azure-monitor-features"></a>Azure Monitor 功能

- 查询自定义事件遥测。
- 利用强大的分段功能，筛选事件遥测。
- 分析应用程序中的转换、保留和导航模式。 可用工具如下：
  - 漏斗图，用于分析和监视转换率。
  - 保留，用于分析随时间推移应用程序的用户保留情况。
  - 工作簿，用于将可视化效果和文本合并为可共享的报表。
  - 队列，用于命名和保存特定用户或事件组，以便可以轻松地通过其他分析工具引用它们。

### <a name="azure-monitor-references"></a>Azure Monitor 参考

- [Azure 门户](https://portal.azure.com/)
- [使用 App Center 和 Application Insights 分析移动应用程序](/azure/azure-monitor/learn/mobile-center-quickstart)
- [结合使用 App Center 分析和 Application Insights](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab

[Azure PlayFab](https://playfab.com/) 提供配置了游戏服务、实时分析功能的完整后端平台以及用于创建与云连接的世界级游戏的 LiveOps。 这些服务为游戏开发人员扫除了一部分开发障碍。 它们提供基于游戏规模、经济高效的大型和小型工作室开发解决方案。 这些服务可以帮助工作室吸引、留住玩家并实现盈利。 借助 PlayFab，开发人员可以使用智能云来构建和运营游戏、分析游戏数据和改进整体游戏体验。

### <a name="azure-playfab-features"></a>Azure PlayFab 功能

- 监视实时仪表板。
- 通过热门指标评估游戏性能。
- 通过自动生成的报表查看游戏的每日和每月性能摘要。 可以在游戏管理器中查看报表，并每日将其下载或发送到收件箱。
- 使用 A/B 测试来运行试验，并确定特定变量的最佳设置。
- 使用玩家分段功能定义玩家的自动分组。

### <a name="azure-playfab-references"></a>Azure PlayFab 参考

- [PlayFab 门户](https://developer.playfab.com/en-US/sign-up)
- [分析](/gaming/playfab/#pivot=documentation&panel=analytics)
- [快速入门](/gaming/playfab/#pivot=documentation&panel=quickstarts)
