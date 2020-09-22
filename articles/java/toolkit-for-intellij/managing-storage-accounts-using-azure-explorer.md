---
title: 使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户
description: 了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure 存储帐户。
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 5152b1bfedd02c821d2a9138fa3e20325b5ea086
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534315"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户

> [!NOTE]
> Azure 资源管理器中的存储帐户功能已被弃用。 你可使用 Azure 门户来创建和管理存储帐户和容器。 有关如何管理存储帐户的快速入门，请参阅 [Azure 存储](/azure/storage/blobs/storage-quickstart-blobs-portal)文档。

Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 IntelliJ 集成开发环境 (IDE) 内部管理其 Azure 帐户中的存储帐户。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>创建存储帐户

若要使用 Azure 资源管理器创建存储帐户，请执行以下操作：

1. 按照[用于 IntelliJ 的 Azure 工具包的登录说明]中的步骤登录到 Azure 帐户。 

2. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“存储帐户”，并单击“创建存储帐户”。   

3. 在“创建存储帐户”对话框中，指定以下选项：

   * 名称：指定要用于新存储帐户的名称。

   * **帐户类型**：指定要创建的存储帐户的类型（例如“Blob 存储”）。 有关详细信息，请参阅[有关 Azure 存储帐户]。 

   * **性能**：指定要使用哪一个从所选发布者提供的存储帐户（例如，“高级”）。 有关详细信息，请参阅 [Azure 存储可伸缩性和性能目标]。 

   * **复制**：指定存储帐户的复制（例如“区域冗余”）。 有关详细信息，请参阅 [Azure 存储复制]。 

   * 订阅：指定要用于新存储帐户的 Azure 订阅。

   * 位置：指定将创建存储帐户的位置（例如“美国西部”）。

   * **资源组**：指定虚拟机的资源组。 选择以下选项之一：
      * **新建**：指定要创建新的资源组。
      * **使用现有项**：指定将从与 Azure 帐户关联的资源组列表中进行选择。

4. 指定了上述所有选项后，单击“确定”。

## <a name="delete-a-storage-account"></a>删除存储帐户

若要使用 Azure 资源管理器删除存储帐户，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击存储帐户，并选择“删除”。 

2. 在确认窗口中，单击“是”。


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

[用于 IntelliJ 的 Azure 工具包的登录说明]: ./sign-in-instructions.md
[Microsoft Azure 存储简介]: /azure/storage/common/storage-introduction
[有关 Azure 存储帐户]: /azure/storage/storage-create-storage-account
[Azure 存储复制]: /azure/storage/storage-redundancy
[Azure 存储可伸缩性和性能目标]: /azure/storage/storage-scalability-targets
[Naming and referencing containers, blobs, and metadata]: https://go.microsoft.com/fwlink/?LinkId=255555

[Azure 中的 Windows 存储帐户的大小]: https://docs.microsoft.com/azure/virtual-machines/sizes
[Azure 中的 Linux 存储帐户的大小]: https://docs.microsoft.com/azure/virtual-machines/sizes
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
