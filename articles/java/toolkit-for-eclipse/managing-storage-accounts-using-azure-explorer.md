---
title: 使用用于 Eclipse 的 Azure 资源管理器管理存储帐户
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure 存储帐户。
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7a2a3406e43e1bc65ac61b7197399cdce08ef29b
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846408"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>使用用于 Eclipse 的 Azure 资源管理器管理存储帐户

> [!NOTE]
> Azure 资源管理器中的存储帐户功能已被弃用。 你可使用 Azure 门户来创建和管理存储帐户和容器。 有关如何管理存储帐户的快速入门，请参阅 [Azure 存储](/azure/storage/blobs/storage-quickstart-blobs-portal)文档。

Azure 资源管理器是 Azure Toolkit for Eclipse 的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse 集成开发环境 (IDE) 内部管理其 Azure 帐户中的存储帐户。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>创建存储帐户

1. 按照[用于 Eclipse 的 Azure 工具包的登录说明](./sign-in-instructions.md)中的步骤登录到 Azure 帐户。

1. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“存储帐户”，并单击“创建存储帐户”。   

1. 在“创建存储帐户”对话框中，指定以下选项：

   * 名称：指定要用于新存储帐户的名称。

   * 订阅：指定要用于新存储帐户的 Azure 订阅。

   * **资源组**：指定虚拟机的资源组。 选择以下选项之一：
      * **新建**：指定要创建新的资源组。
      * **使用现有**：指定将从与 Azure 帐户关联的资源组列表中进行选择。

   * **区域**：指定将创建存储帐户的位置（例如“美国西部”）。

   * **帐户类型**：指定要创建的存储帐户的类型（例如“常规用途 v1”）。 有关详细信息，请参阅[有关 Azure 存储帐户]。

   * **性能**：提供要使用所选发布者提供的哪一个存储帐户（例如“标准”）。 有关详细信息，请参阅 [Azure 存储可伸缩性和性能目标]。

   * **复制**：指定存储帐户的复制（例如“本地冗余”）。 有关详细信息，请参阅 [Azure 存储复制]。

1. 指定了上述所有选项后，单击“创建”。

## <a name="create-and-manage-storage-containers"></a>创建和管理存储容器

若要创建和管理存储容器，请访问 Azure 门户或以编程方式预配资源。

要查看分步教程了解如何使用 Azure 门户在 Azure 存储中创建容器以及在该容器中上传和下载块 Blob，请参阅[使用 Azure 门户上传、下载和列出 Blob](/azure/storage/blobs/storage-quickstart-blobs-portal)。

## <a name="delete-a-storage-account"></a>删除存储帐户

1. 在 **Azure 资源管理器**视图中，右键单击存储帐户，并单击“删除”。

1. 在确认窗口中，单击“确定”。


## <a name="next-steps"></a>后续步骤

有关 Azure 存储帐户大小和定价的详细信息，请参阅以下资源：

* [Microsoft Azure 存储简介]
* [有关 Azure 存储帐户]
* Azure 存储帐户大小
  * [Azure 中的 Windows 存储帐户的大小]
  * [Azure 中的 Linux 存储帐户的大小]
* Azure 存储帐户定价
  * [Windows 存储帐户定价]
  * [Linux 存储帐户定价]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Microsoft Azure 存储简介]: /azure/storage/common/storage-introduction
[有关 Azure 存储帐户]: /azure/storage/storage-create-storage-account
[Azure 存储复制]: /azure/storage/storage-redundancy
[Azure 存储可伸缩性和性能目标]: /azure/storage/storage-scalability-targets
[Naming and referencing containers, blobs, and metadata]: /rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata

[Azure 中的 Windows 存储帐户的大小]: /azure/virtual-machines/sizes
[Azure 中的 Linux 存储帐户的大小]: /azure/virtual-machines/sizes
[Windows 存储帐户定价]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Linux 存储帐户定价]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/managing-storage-accounts-using-azure-explorer/DC02.png