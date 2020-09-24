---
title: 使用用于 Eclipse 的 Azure 资源管理器管理虚拟机
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure 虚拟机。
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: e457d4fe152f9fa5fa64bafaa4f49311e8ff4475
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831883"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>使用用于 Eclipse 的 Azure 资源管理器管理虚拟机

Azure 资源管理器是用于 Eclipse 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse 集成开发环境 (IDE) 内部管理其 Azure 帐户中的虚拟机。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-virtual-machine"></a>创建虚拟机

1. 按照[用于 Eclipse 的 Azure 工具包的登录说明](./sign-in-instructions.md)中的步骤登录到 Azure 帐户。

1. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“虚拟机”，并单击“创建 VM”。

   :::image type="content" source="media/managing-virtual-machines-using-azure-explorer/CR01.png" alt-text="Azure 资源管理器中的“创建 VM”选项。":::

1. 在“选择订阅”窗口中选择订阅，并单击“下一步”。

1. 在“选择虚拟机映像”窗口中，选择“位置”（例如“美国西部”） 。 可选择继续使用推荐映像，也可选择自定义映像。 在本快速入门中，我们将继续使用推荐映像。 

   如果选择选择自定义映像，请输入以下信息：
   * 发布者：指定创建了用于创建虚拟机的映像的发布者（例如“Microsoft”）。

   * **产品/服务**：指定所选发布者提供的可以使用的虚拟机产品/服务（例如 *JDK*）。

   * **Sku**：从所选产品/服务中指定要使用的库存单位 (SKU)（例如 *JDK_8*）。

   * **版本号**：指定要使用所选 SKU 的哪个版本。

1. 单击“下一步”。

1. 在“虚拟机基本设置”窗口中输入以下信息：

   * **虚拟机名称**：指定新虚拟机的名称，该名称必须以字母开头并仅包含字母、数字和连字符。

   * **Size**：指定要为虚拟机分配的内核数和内存。

   * 用户名：指定要创建的用于管理虚拟机的管理员帐户。

   * **密码**：指定管理员帐户的密码。 在“确认”框中重新输入密码以验证凭据。

1. 单击“下一步”。

1. 在“关联的资源”窗口输入以下信息：
   * **资源组**：指定虚拟机的资源组。 选择以下选项之一：
      * **新建**：指定要创建新的资源组。
      * 使用现有资源：指定选择已与 Azure 帐户关联的资源组。

   * **存储帐户**：指定用于存储虚拟机的存储帐户。 可使用现有存储帐户，也可以创建新帐户。

   * **虚拟网络**和**子网**：指定虚拟机将连接到的虚拟网络和子网。 可使用现有网络和子网，也可以创建新网络和子网。 如果选择“新建”，会显示以下对话框：

   * **公共 IP 地址**：为虚拟机指定面向外部的 IP 地址。 可选择创建新 IP 地址，也可以选择“(无)”（如果虚拟机将不具有公共 IP 地址）。

   * **网络安全组**：指定虚拟机的可选网络防火墙。 可以选择现有防火墙，也可以选择“(无)”（如果虚拟机不使用网络防火墙）。

   * **可用性集**：指定虚拟机可能属于的可选可用性集。 可选择现有可用性集，或创建新可用性集，也可选择“(无)”（如果虚拟机将不属于可用性集）。

10. 单击“完成”。  

      > [!NOTE]
      > 可在 Eclipse 工作区的右下角查看创建进度。

## <a name="restart-a-virtual-machine"></a>重启虚拟机

若要在 Eclipse 中使用 Azure 资源管理器重启虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“重新启动”。

1. 在确认窗口中，单击“确定”。

   ![“重启虚拟机”确认窗口](media/managing-virtual-machines-using-azure-explorer/RE02.png)

## <a name="shut-down-a-virtual-machine"></a>关闭虚拟机

若要在 Eclipse 中使用 Azure 资源管理器关闭正在运行的虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“关闭”。

1. 在确认窗口中，单击“确定”。

   ![“关闭虚拟机”确认窗口](media/managing-virtual-machines-using-azure-explorer/SH02.png)

## <a name="delete-a-virtual-machine"></a>删除虚拟机

若要在 Eclipse 中使用 Azure 资源管理器删除虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“删除”。

1. 在确认窗口中，单击“是”。

   ![“删除虚拟机”确认窗口](media/managing-virtual-machines-using-azure-explorer/DE02.png)

## <a name="next-steps"></a>后续步骤

有关 Azure 虚拟机大小和定价的详细信息，请参阅以下资源：

* Azure 虚拟机大小
  * [Azure 中 Windows 虚拟机的大小]
  * [Azure 中 Linux 虚拟机的大小]
* Azure 虚拟机定价
  * [Windows 虚拟机定价]
  * [Linux 虚拟机定价]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure 中 Windows 虚拟机的大小]: /azure/virtual-machines/sizes
[Azure 中 Linux 虚拟机的大小]: /azure/virtual-machines/sizes
[Windows 虚拟机定价]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Linux 虚拟机定价]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/managing-virtual-machines-using-azure-explorer/CR08.png