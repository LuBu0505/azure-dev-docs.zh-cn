---
title: 使用 Azure 存储构建高度安全、持久、可缩放的应用程序的云存储
description: 了解在云中存储大型结构化和非结构化移动应用程序数据的服务。
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 7a1171ebe64b0353fc57180adce63ee702431207
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632359"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>使用 Azure 存储作为高度安全、持久、可缩放的应用程序的云存储

[Azure 存储](https://azure.microsoft.com/services/storage/)是用于新式应用程序的 Microsoft 的云存储解决方案，可为数据对象提供可大规模缩放的对象存储，为云提供文件系统服务，并且提供用于可靠消息传送的消息传送存储以及 NoSQL 存储。 Azure 存储：

- **持久且具有高可用性：** 冗余可确保数据在发生短暂的硬件故障时是安全的。 还可以选择在各个数据中心或地理区域之间复制数据，从而在发生本地灾难或自然灾害时提供额外的保护。 以此方式复制的数据在发生意外中断时将保持高可用性。
- **安全：** 该服务将对写入到 Azure 存储的所有数据进行加密。 Azure 存储可以精细地控制谁可以访问你的数据。
- 可缩放：服务设计为可大规模缩放，以满足当今的应用程序在数据存储和性能方面的需求。
- **托管：** Azure 为你处理硬件维护、更新和关键问题。
- **可访问：** 可以通过 HTTP 或 HTTPS 从世界上的任何位置访问数据。 Microsoft 以各种语言（如 .NET、Java、Node.js、Python、PHP、Ruby 和 Go）提供了客户端库以及成熟的 REST API。 Azure PowerShell 或 Azure CLI 支持脚本。 Azure 门户和 Azure 存储资源管理器提供了用于处理数据的简单可视化解决方案。

使用以下服务可在移动应用中启用云存储。

## <a name="azure-blob-storage"></a>Azure Blob 存储

[Azure Blob 存储](https://azure.microsoft.com/services/storage/blobs/)提供适用于云的对象存储解决方案。 Blob 存储经过优化，可存储大量非结构化数据，这些数据不遵循特定数据模型或定义（如文本或二进制文件）。 它支持客户端库使用的各种语言。 Blob 存储用于：

- 直接向浏览器提供图像或文档。
- 存储文件以供分布式访问。
- 对视频和音频进行流式处理。
- 向日志文件进行写入。
- 存储用于备份和还原、灾难恢复及存档的数据。
- 存储数据以供本地或 Azure 托管服务执行分析。

### <a name="azure-blob-storage-references"></a>Azure Blob 存储参考

- [Azure 门户](https://portal.azure.com)
- [Azure Blob 存储文档](/azure/storage/blobs/storage-blobs-introduction)
- [快速入门](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [示例](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure 表存储

[Azure 表存储](https://azure.microsoft.com/services/storage/tables/)是一项用于在云中存储结构化 NoSQL 数据的服务，通过无架构设计提供键或属性存储。 Azure 表存储可存储大量结构化数据。 该服务是一个 NoSQL 数据存储，接受来自 Azure 云内部和外部的通过验证的呼叫。 Azure 表最适合存储结构化非关系型数据。 表存储通常用于：

- 存储 TB 量级的结构化数据，能够为 Web 规模应用程序提供服务。
- 存储无需复杂联接、外键或存储过程，并且可以对其进行非规范化以实现快速访问的数据集。
- 使用聚集索引快速查询数据。
- 将 OData 协议和 LINQ 与 Windows Communication Foundation (WCF) 数据服务 .NET 库配合使用来访问数据。

可使用表存储来存储和查询大量的结构化非关系型数据。 表随着需求的增加而扩展。

### <a name="azure-table-storage-references"></a>Azure 表存储参考

- [Azure 门户](https://portal.azure.com)
- [Azure 表存储文档](/azure/storage/tables/table-storage-overview)
- [示例](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [快速入门](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-queue-storage"></a>Azure 队列存储

[Azure 队列存储](https://azure.microsoft.com/services/storage/queues/)是一项可存储大量消息的服务。 可以使用 HTTP 或 HTTPS 通过经验证的调用从世界任何位置访问消息。 队列消息大小最大可为 64 KB。 一个队列可以包含数百万条消息，直至达到存储帐户的总容量限值。 队列通常用于创建要异步处理的积压工作 (backlog)。

###  <a name="azure-queue-storage-references"></a>Azure 队列存储参考

- [Azure 门户](https://portal.azure.com)
- [Azure 队列存储文档](/azure/storage/queues/)
- [快速入门](/azure/storage/queues/storage-quickstart-queues-portal)
- [示例](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
