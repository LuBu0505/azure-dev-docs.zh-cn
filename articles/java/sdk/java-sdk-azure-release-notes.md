---
title: 用于 Java 的 Azure 管理库发行说明 | Microsoft Docs
description: 了解用于 Java 的 Azure 管理库的新增功能，并留意其中的重大更改
keywords: Azure,  Java, API, 参考, 说明, 更新, 弃用
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.topic: reference
ms.date: 3/06/2016
ms.openlocfilehash: 69c2c29935f9333dd1d31b26b0941e0446ca5504
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82105198"
---
# <a name="release-notes"></a>发行说明 

## <a name="october-5-2017---130"></a>2017 年 10 月 5 日 - 1.3.0 

版本 1.3.0 与先前版本中已达到正式版（稳定版）发布阶段的服务和功能向后兼容。

预览版中这些服务的任何重大更改带有 @Beta 批注。


### <a name="generally-available-in-v13"></a>在 V1.3 中正式发布

先前版本中仍处于 Beta 版状态的某些 API 现已推出正式版，具体而言：

- 异步方法
- CDN 中以前处于 Beta 版状态的所有方法
- 应用程序网关中以前处于 Beta 版状态的所有方法和接口

  库的某些部件仍处于预览版状态。 请参阅下表了解库的当前状态：

服务或功能 | 以正式版提供 | 以预览版提供 
---------|---------|---------|-
计算  | 虚拟机和 VM 扩展、虚拟机规模集、托管磁盘   | Azure 容器服务、Azure 容器注册表 
存储   |  存储帐户       |    加密     
SQL 数据库  | 数据库、防火墙、弹性池              
网络    |  虚拟网络、网络接口、IP 地址、路由表、网络安全组、DNS、流量管理器、应用程序网关  |    负载均衡器、网络对等互连、虚拟网络网关、网络观察程序 
其他服务    |  资源管理器、Key Vault、Redis、CDN、Batch       |  Web 应用、函数应用、服务总线、Graph RBAC、Cosmos DB、搜索  
基本     |   身份验证 - 核心、异步方法、托管服务标识      |      |

> 预览版功能在库中的类、接口或方法级别带有 `@Beta` 批注。 这些功能随时会变化。 将来可能会以任何方式对其进行修改甚至删除。

### <a name="import-with-maven"></a>使用 Maven 导入

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

请查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社区文章，获得有关在自己的代码中使用库的帮助。 如果发现任何 Bug 或者想要提出建议来改进这些库，请通过 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出问题。


