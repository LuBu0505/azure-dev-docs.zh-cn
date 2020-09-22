---
title: 使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机
description: 了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure 虚拟机。
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 058842e8f7d50d885d2a5d28c56ee144072e637a
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534344"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a>使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机

Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 IntelliJ 集成开发环境 (IDE) 内部管理其 Azure 帐户中的虚拟机。

本文演示如何在 IntelliJ 上通过 Azure 资源管理器创建和管理虚拟机。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-virtual-machine"></a>创建虚拟机

若要使用 Azure 资源管理器创建虚拟机，请执行以下操作： 

1. 按照[用于 IntelliJ 的 Azure 工具包的登录说明]一文中的步骤登录到 Azure 帐户。

2. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“虚拟机”，并单击“创建 VM”。    
 
   :::image type="content" source="media/managing-virtual-machines-using-azure-explorer/CR01.png" alt-text="Azure 资源管理器中的“创建 VM”选项。":::

3. 在“选择订阅”窗口中选择订阅，并单击“下一步”。 

4. 在“选择虚拟机映像”窗口中输入以下信息：

   * 位置：指定将创建虚拟机的位置（例如“美国西部”）。 

   * **建议的映像**：指定要从常用映像的简化列表中选择映像。

   * **自定义映像**：指定要选择自定义映像，为此将需要提供以下信息：

      * **发布者**：指定创建了用于创建虚拟机的映像的发布者（例如 *Microsoft*）。

      * **产品/服务**：指定所选发布者提供的可以使用的虚拟机产品/服务（例如 *JDK*）。

      * **Sku**：从所选产品/服务中指定要使用的库存单位 (SKU)（例如 *JDK_8*）。

      * **版本号**：指定要使用所选 SKU 的哪个版本。

5. 单击“下一步”。 

6. 在“虚拟机基本设置”窗口中输入以下信息：

   * **虚拟机名称**：指定新虚拟机的名称，该名称必须以字母开头并仅包含字母、数字和连字符。

   * **Size**：指定要为虚拟机分配的内核数和内存。

   * 用户名：指定要创建的用于管理虚拟机的管理员帐户。

   * 密码：指定管理员帐户的密码。 在“确认”框中重新输入密码以验证凭据。

7. 单击“下一步”。 

8. 在“关联的资源”窗口输入以下信息：

   * 资源组：指定虚拟机的资源组。 选择以下选项之一：
      * **新建**：指定要创建新的资源组。
      * **使用现有项**：指定要从与 Azure 帐户关联的资源组列表中进行选择。

   * **存储帐户**：指定用于存储虚拟机的存储帐户。 可以选择现有存储帐户，也可以创建新的帐户。 如果选择“新建”，会显示以下对话框：

   * **虚拟网络**和**子网**：指定虚拟机将连接到的虚拟网络和子网。 可使用现有网络和子网，也可以创建新网络和子网。 如果选择“新建”，会显示以下对话框：

   * **公共 IP 地址**：为虚拟机指定面向外部的 IP 地址。 可选择创建新 IP 地址，也可以选择“(无)”（如果虚拟机将不具有公共 IP 地址）。 

   * **网络安全组**：指定虚拟机的可选网络防火墙。 可以选择现有防火墙，也可以选择“(无)”（如果虚拟机不使用网络防火墙）。 

   * **可用性集**：指定虚拟机可能属于的可选可用性集。 可以选择现有的可用性集，也可以创建新的可用性集，或选择“(无)”（如果虚拟机将不属于可用性集）。

9. 单击“完成”。 新虚拟机随即显示在“Azure 资源管理器”工具窗口中。 

## <a name="restart-a-virtual-machine"></a>重启虚拟机

若要在 IntelliJ 中使用 Azure 资源管理器重新启动虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“重新启动”。

2. 在确认窗口中，单击“是”。 

   ![确认重新启动虚拟机窗口][RE02]

## <a name="shut-down-a-virtual-machine"></a>关闭虚拟机

若要在 IntelliJ 中使用 Azure 资源管理器关闭正在运行的虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“关闭”。

2. 在确认窗口中，单击“是”。 

   ![确认关闭虚拟机窗口][SH02]

## <a name="delete-a-virtual-machine"></a>删除虚拟机

若要在 IntelliJ 中使用 Azure 资源管理器重新删除虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“删除”。

2. 在确认窗口中，单击“是”。 

   ![确认删除虚拟机窗口][DE02]

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

[用于 IntelliJ 的 Azure 工具包的登录说明]: ./sign-in-instructions.md
[Azure 中 Windows 虚拟机的大小]: https://docs.microsoft.com/azure/virtual-machines/sizes
[Azure 中 Linux 虚拟机的大小]: https://docs.microsoft.com/azure/virtual-machines/sizes
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
