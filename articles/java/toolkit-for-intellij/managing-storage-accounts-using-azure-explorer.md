---
title: 使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户
description: 了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure 存储帐户。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 051704a83e0535a6754c3c4dbd82eb8dfcf8e3c4
ms.sourcegitcommit: 7be67fb768fb5e19f7de573068cc1376b3d90d1f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85906438"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户

Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 IntelliJ 集成开发环境 (IDE) 内部管理其 Azure 帐户中的存储帐户。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>在 IntelliJ 中创建存储帐户

若要使用 Azure 资源管理器创建存储帐户，请执行以下操作：

1. 按照[用于 IntelliJ 的 Azure 工具包的登录说明]中的步骤登录到 Azure 帐户。 

2. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“存储帐户”，并单击“创建存储帐户”。   

   ![“创建存储帐户”命令][CS01]

3. 在“创建存储帐户”对话框中，指定以下选项：

   ![“创建新存储帐户”对话框][CS02]

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

## <a name="delete-a-storage-account-in-intellij"></a>删除 IntelliJ 中的存储帐户

若要使用 Azure 资源管理器删除存储帐户，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击存储帐户，并选择“删除”。 

   ![“删除存储帐户”菜单][DS01]

2. 在确认窗口中，单击“是”。

   ![删除存储帐户确认窗口][DS02]

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

[Azure 中的 Windows 存储帐户的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 存储帐户的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
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
