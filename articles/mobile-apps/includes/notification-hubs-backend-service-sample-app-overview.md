---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 510218bcbed53dfb4383b6a906911790f7865975
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401341"
---
它允许你通过创建的后端服务从通知中心注册和取消注册。 

当指定了一个操作，并且应用位于前台时，将显示一个警报。 否则，通知中心将显示通知。

> [!NOTE]
> 通常，会在应用程序生命周期中的相应时间点（或在首次运行体验过程中）执行注册（和取消注册）操作，无需进行显式用户注册/取消注册输入。 但是，此示例将需要显式用户输入，以便能够更轻松地探索和测试此功能。
