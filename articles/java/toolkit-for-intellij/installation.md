---
title: 安装 Azure Toolkit for IntelliJ
description: 了解如何安装用于 IntelliJ 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。
documentationcenter: java
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: fe8b07257ff3a9fc5523d13dd13e19982103ab05
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534444"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>安装 Azure Toolkit for IntelliJ

借助 Azure Toolkit for IntelliJ 提供的模板和功能，可以轻松使用 IntelliJ IDEA 开发环境来创建、开发、测试云应用程序并将其部署到 Azure。

> [!NOTE] 
> 
> Azure Toolkit for IntelliJ 是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为： 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

可以通过两种方法来安装用于 IntelliJ 的 Azure 工具包：一种是使用“设置”对话框，另一种是使用开始屏幕上的“配置”菜单。  两种安装方法都会在以下步骤中演示。

## <a name="prerequisites"></a>先决条件

使用用于 IntelliJ 的 Azure 工具包需要以下软件组件：

* 已安装的 Java 开发工具包 (JDK) 8+，例如：[OpenJDK](https://openjdk.java.net/) 或 [Oracle JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* 已安装的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) 旗舰版或社区版

> [!NOTE]
> 
> JetBrains 插件存储库中的 [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 页列出了与该工具包兼容的内部版本。
> 

<!--
> [!IMPORTANT]
> 
> If you are using the Azure Toolkit for IntelliJ on Windows, the toolkit requires installing the Azure SDK 2.9.6 or later in order to use the Azure emulator. You have two options for installing the Azure SDK:
> 
> * You can download and install the Azure SDK by using the [Web Platform Installer (WebPI)](https://go.microsoft.com/fwlink/?LinkID=252838).
> * If you do not have the Azure SDK installed when you create your first Azure deployment project, you will be prompted to automatically download install the requisite version of the Azure SDK.
> 
> Note that the Azure SDK is only required on Windows.
> 
-->


## <a name="from-the-settings-dialog-box"></a>从“设置”对话框

1. 在 IntelliJ 工具栏中，依次单击“文件”和“设置” 。

1. 在“设置”对话框的左侧导航菜单中，单击“插件”。

1. 在“市场”搜索栏中，键入“Azure”以筛选插件列表。 选择“Azure Toolkit for IntelliJ”，然后单击“安装” 。 阅读 IntelliJ 的第三方插件隐私说明并单击“接受”。

   :::image type="content" source="media/installation/03-intellij-search-plugin.png" alt-text="搜索 Azure Toolkit for IntelliJ 插件。"::: 

1. 安装完成后，单击“重启 IDE”。

1. 当系统提示重启 IntelliJ IDEA 时，请单击“重启”。
   
   :::image type="content" source="media/installation/07-restart-intellij.png" alt-text="重启 IntelliJ IDEA。"::: 

## <a name="from-the-start-screen"></a>从开始屏幕

1. 在 IntelliJ IDEA 开始屏幕中，依次单击“配置”和“插件” 。

   :::image type="content" source="media/installation/01-intellij-configure-dropdown.png" alt-text="来自开始屏幕的插件。"::: 

1. 在“市场”搜索栏中，键入“Azure”以筛选插件列表。 选择“Azure Toolkit for IntelliJ”，然后单击“安装” 。 阅读 IntelliJ 的第三方插件隐私说明并单击“接受”。

   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace.png" alt-text="来自开始屏幕的插件市场。":::

1. 安装完成后，单击“重启 IDE”。

1. 当系统提示重启 IntelliJ IDEA 时，请单击“重启”。
   
   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace-restart.png" alt-text="重启以从开始屏幕安装。":::

## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

