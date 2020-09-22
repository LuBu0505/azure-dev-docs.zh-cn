---
title: 安装用于 Eclipse 的 Azure 工具包
description: 了解如何安装用于 Eclipse 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。
documentationcenter: java
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 27793d827b60a5977968529377b20c7033170ffe
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534648"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>安装用于 Eclipse 的 Azure 工具包

Azure Toolkit for Eclipse 提供了一些功能。通过它们，你可使用 Eclipse 开发环境轻松创建、开发、配置、测试和部署高度可用且可缩放的轻量级 Java Web 应用和 HDInsight Spark 作业。

> [!NOTE] 
> 
> 用于 Eclipse 的 Azure 工具包是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为： 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

可通过两种方法来安装 Azure Toolkit for Eclipse：访问 Eclipse Marketplace，以及使用帮助菜单上的“安装新软件”选项 。 这两种安装方法都将在以下部分中进行演示。

## <a name="eclipse-marketplace"></a>Eclipse Marketplace

通过 Eclipse IDE 中的 Eclipse Marketplace 向导，用户可浏览 [Eclipse Marketplace](https://marketplace.eclipse.org/) 并安装解决方案。 下面两个选项会将你转到 Eclipse Marketplace：

   * 将以下按钮拖到正在运行的 Eclipse 工作区。 此按钮会打开 Eclipse Marketplace，其中已选择 Azure Toolkit for Eclipse。

      [![拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端")

   * 在 Eclipse IDE 上，单击帮助菜单，导航到 Eclipse Marketplace，搜索“Azure Toolkit for Eclipse”，然后单击“安装”  。

      :::image type="content" source="media/installation/eclipse-marketplace-button.png" alt-text="帮助菜单中的“Marketplace”窗口。"::: 

1. Eclipse Marketplace 向导将弹出安装说明，其中包括要安装的组件的列表。 验证是否已选择所有功能，然后单击“确认 >”。

   | Feature | 说明 | 
   |---|---| 
   | **用于 Java 的 Application Insights 插件** | 可通过此组件将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。 | 
   | **Azure 常用插件** | 提供其他工具包组件所需的常见功能。 | 
   | **用于 Eclipse 的 Azure 容器工具** | 用于生成 .WAR 并将其作为 Docker 容器部署到 Docker 计算机。 | 
   | **用于 Eclipse 的 Azure 容器** | 用于将 .WAR 或 .JAR 项目作为 Docker 容器部署到 Azure 虚拟机。 | 
   | **用于 Eclipse 的 Azure 资源管理器** | 提供一个资源管理器样式的界面，用于管理 Azure 资源。 | 
   | **用于 Java 的 Azure HDInsight 插件** | 在 Scala 中实现 Apache Spark 应用程序开发。 |
   | **Microsoft JDBC Driver 6.1 for SQL Server** | 提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。 | 
   | **Microsoft Azure Java 库包** | 提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等）的 API。 | 
   | **用于 Eclipse 的 WebApp 插件** | 你可用它来将 Web 应用程序部署为 Azure 应用服务。 | 

1. 在“查看许可证”对话框中，查看许可协议条款。 如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成” 。 

   > [!NOTE]
   > 你可在 Eclipse 工作区的右下角查看安装进度。

4. 安装完成后，系统将提示你重启 Eclipse IDE 来应用软件更新。 单击“立即重新启动”****。

## <a name="install-new-software"></a>安装新软件

你可以新软件的形式直接从帮助菜单安装 Azure Toolkit for Eclipse。

1. 单击帮助菜单，然后单击“安装新软件” 。

   :::image type="content" source="media/installation/eclipse-install-software-button.png" alt-text="帮助菜单中的“安装新软件”。"::: 

1. 在“可用软件”对话框中，在“使用”文本框中键入 `http://dl.microsoft.com/eclipse/` 。

1. 在“名称”窗格中，选中“用于 Java 的 Azure 工具包”，并取消选中“在安装过程中访问所有更新站点以查找所需的软件”。   屏幕应与下图中所示类似：

   ![安装用于 Eclipse 的 Azure 工具包][02]

1. 如果展开 Azure Toolkit for Java，会看到一个要安装的组件列表，例如：

   | Feature | 说明 | 
   |---|---| 
   | **用于 Java 的 Application Insights 插件** | 可通过此组件将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。 | 
   | **Azure 常用插件** | 提供其他工具包组件所需的常见功能。 | 
   | **用于 Eclipse 的 Azure 容器工具** | 用于生成 .WAR 并将其作为 Docker 容器部署到 Docker 计算机。 | 
   | **用于 Eclipse 的 Azure 容器** | 用于将 .WAR 或 .JAR 项目作为 Docker 容器部署到 Azure 虚拟机。 | 
   | **用于 Eclipse 的 Azure 资源管理器** | 提供一个资源管理器样式的界面，用于管理 Azure 资源。 | 
   | **用于 Java 的 Azure HDInsight 插件** | 在 Scala 中实现 Apache Spark 应用程序开发。 |
   | **Microsoft JDBC Driver 6.1 for SQL Server** | 提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。 | 
   | **Microsoft Azure Java 库包** | 提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等）的 API。 | 
   | **用于 Eclipse 的 WebApp 插件** | 你可用它来将 Web 应用程序部署为 Azure 应用服务。 | 

1. 单击“下一步”。 （如果在安装该工具包时遇到不正常的延迟，请确保未选中“在安装过程中访问所有更新站点以查找所需的软件”。）

1. 在“安装详细信息”对话框中，单击“下一步”。 

1. 在“查看许可证”对话框中，查看许可协议条款。 如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。  （剩余步骤假定你接受许可协议条款。 如果不接受许可协议条款，请退出安装过程。）

   > [!NOTE]
   > 你可在 Eclipse 工作区的右下角查看安装进度。

1. 如果系统提示你重新启动 Eclipse 以完成安装，请单击 **“立即重新启动”**。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[02]: media/installation/eclipse-installation-02.png
