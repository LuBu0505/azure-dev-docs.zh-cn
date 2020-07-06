---
title: 使用 Visual Studio App Center 和 Azure 通知中心的移动应用中推送通知的重要性
description: 了解可用于与移动应用程序用户互动的服务，如 Visual Studio App Center。
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 6dab56319c04fbf7864094fbaf254621cac82b36
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632439"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>通过发送推送通知与应用程序用户联系

无论你是要生成使用者还是企业应用程序，你都希望用户主动使用你的应用程序。 在很多时候，你希望与用户共享突发新闻、游戏更新、令人兴奋的优惠、产品更新或任何其他相关信息。 若要增加与用户的互动并使他们返回到你的应用程序，你需要一种与用户实时通信的方法。

推送通知便是理想之选，它提供了一种与应用程序用户进行通信的快速而有效的方法。 无论用户是否在使用移动设备，你都可以在适当的时间与用户联系并通知用户其所需的信息（通常以移动设备上的弹出项或对话框形式出现）。

## <a name="value-of-push-notifications"></a>推送通知的价值

推送通知可为用户和企业提供价值。

企业可以：

- 在适当的时间通过支持的平台发送消息，直接与用户通信。
- 通过向用户发送实时更新和提醒来提高用户参与，并鼓励他们定期与应用程序互动。
- 保留用户并与不活跃的用户重新互动（这些用户下载过应用程序，但已不再使用该应用程序），使其再次活跃起来。
- 通过基于移动推送通知分析用户行为、对用户进行细分并利用营销活动，来提高转化率。
- 通过向用户发送推送消息和及时更新来推广产品和服务。
- 通过发送个性化推送消息来对用户进行定位。

对于应用程序用户，推送通知可以：

- 实时提供价值和信息。
- 提醒用户使用应用程序。

使用以下服务在移动应用中启用推送通知。

## <a name="azure-notification-hubs"></a>Azure 通知中心

[通知中心](/azure/notification-hubs/notification-hubs-push-notification-overview)提供了易于使用的横向扩展式推送引擎。 可使用该中心将通知发送到任何平台，以及从云中或本地的任何后端发送通知。

### <a name="azure-notification-hub-features"></a>Azure 通知中心功能

- 从云中或本地的任何后端向任何平台发送推送通知。
- 使用单个 API 调用将广播快速推送到数百万台移动设备。
- 根据客户、语言和位置定制推送通知。
- 动态定义并通知客户细分群。
- 可立即扩展到数百万台移动设备。
- 获得对 iOS、Android、Windows、Kindle 和百度的平台支持。

### <a name="azure-notification-hub-references"></a>Azure 通知中心参考

- [Azure 门户](https://portal.azure.com) 
- [Azure 通知中心入门](/azure/notification-hubs/)
- [快速入门](/azure/notification-hubs/create-notification-hub-portal)
- [示例](/azure/notification-hubs/samples)
