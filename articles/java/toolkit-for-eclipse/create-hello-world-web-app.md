---
title: 使用 Eclipse 创建适用于 Azure 应用服务的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for Eclipse 创建适用于 Azure 的 Hello World Web 应用。
services: app-service
keywords: java, eclipse, web 应用, azure 应用服务, hello world, 快速入门
documentationcenter: java
author: selvasingh
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.reviewer: asirveda
ms.date: 08/25/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: 227e1a98bb14474b444a8d2cfb288b62ed5cf8ed
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831625"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>使用 Eclipse 创建适用于 Azure 应用服务的 Hello World Web 应用

本文演示通过使用 [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) 创建基本的 Hello World Web 应用并将 Web 应用发布到 Azure 应用服务所需的步骤。

> [!NOTE]
>
> 如果首选使用 IntelliJ IDEA，请查看[适用于 IntelliJ 的类似教程][intellij-hello-world]。
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> 请勿忘记在完成本教程后清理资源。 在这种情况下，运行本指南不会超出免费帐户配额。
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>安装和登录

以下步骤将指导你完成 Eclipse 开发环境中登录 Azure 的过程。

1. 如果尚未安装该插件，请参阅[安装 Azure Toolkit for Eclipse](installation.md)。

1. 若要登录到 Azure 帐户，请依次单击“工具”、“Azure”和“登录”  。

   :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="在 Eclipse IDE 中登录到 Azure。":::

1. 在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](sign-in-instructions.md)）。  

1. 在“Azure 设备登录”对话框中单击“复制并打开” 。

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

1. 选择 Azure 帐户，完成登录所需的全部身份验证过程。

1. 登录后，关闭浏览器并切换回 Eclipse IDE。 在“选择订阅”对话框中选择要使用的订阅，然后单击“确定” 。

### <a name="install-required-software-optional"></a>安装所需软件（可选）

为确保你具有使用 Web 应用项目所需的组件，请按照下列步骤操作：

1. 单击帮助菜单，然后单击“安装新软件” 。

1. 在“可用软件”对话框中，单击“管理”，并确保已选择最新的 Eclipse 版本（例如 2020-06） 。

1. 单击“应用并关闭”。 展开“使用:”下拉菜单，显示建议的站点。 选择最新的 Eclipse 版本站点以查询可用软件。

1. 向下滚动列表并选择“Web、XML、Java EE 和 OSGi Enterprise 开发”项。 单击“下一步”。

1. 在“安装详细信息”窗口中，单击“下一步”。

1. 在“查看许可证”对话框中，查看许可协议条款。 如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。  

   > [!NOTE]
   > 你可在 Eclipse 工作区的右下角查看安装进度。

1. 如果系统提示你重新启动 Eclipse 以完成安装，请单击 **“立即重新启动”**。

## <a name="creating-a-web-app-project"></a>创建 Web 应用项目

1. 单击“文件”，展开“新建”，然后单击“…项目”  。 在“新建项目”对话框窗口中，展开“Web”，选择“动态 Web 项目”，然后单击“下一步”  。

   > [!TIP]
   > 如果未看到 Web 作为可用项目列出，请参阅[本部分](#install-required-software-optional)，确保你具有所需的 Eclipse 软件。

1. 在本教程中，项目命名为 **MyWebApp**。 屏幕应与下图中所示类似：
   
   ![“新建动态 Web 项目”属性][dynamic-web-project-properties]

1. 单击“完成”。

1. 在左侧的“包资源管理器”窗格中，展开“MyWebApp”。 右键单击“WebContent”，将鼠标悬停在“新建”上，然后单击“其他…”  。

1. 展开“Web”以查找“JSP 文件”选项 。 单击“下一步”。

1. 在“新建 JSP 文件”对话框中，将文件命名为“index.jsp”，将父文件夹保留为“MyWebApp/WebContent”，然后单击“下一步”。   

   ![“新建 JSP 文件”对话框][new-jsp-file-dialog]

1. 基于本教程的目的，在“选择 JSP 模板”对话框中，选择“新建 JSP 文件(html 5)”，然后单击“完成”  。

1. 在 Eclipse 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示 在现有 `<body>` 元素中。 更新后的 `<body>` 内容应类似于以下示例：
   
   ```jsp
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. 保存 index.jsp。

## <a name="deploying-the-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在左侧的“包资源管理器”窗格中，右键单击项目，选择“Azure”，然后选择“发布为 Azure Web 应用” 。
   
   ![发布为 Azure Web 应用][publish-as-azure-web-app]

1. 当“部署 Web 应用”对话框显示时，可选择以下某个选项：

   * 选择现有的 Web 应用（如果存在）。

   * 如果没有现有 Web 应用，请单击“创建”。

      可在此处配置运行时环境、应用服务计划资源组和应用设置。 如有必要，请创建新资源。

      在“创建应用服务”对话框中为 Web 应用指定必要信息，然后单击“创建” 。

1. 选择 Web 应用，然后单击“部署”。

1. 工具包在成功部署 Web 应用后会在“Azure 活动日志”选项卡下显示“已发布”状态 ，这是已部署 Web 应用的 URL 超链接。

1. 可使用状态消息中提供的链接转到 Web 应用。

   ![转到你的 Web 应用][browse-web-app]

## <a name="cleaning-up-resources"></a>清理资源

1. 将 Web 应用发布到 Azure 以后，即可对其进行管理，方法是在 Azure 资源管理器中右键单击，然后在上下文菜单中选择一个选项。 例如，可以**删除**此处的 Web 应用，以便清理本教程的资源。

   ![管理应用服务][manage-app-service]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[Azure Toolkit for IntelliJ]: ../toolkit-for-intellij
[intellij-hello-world]: ../toolkit-for-intellij/create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: create-hello-world-web-app-legacy-version.md

<!-- IMG List -->

[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[dynamic-web-project-properties]: media/create-hello-world-web-app/dynamic-web-project-properties.png
[new-jsp-file-dialog]: media/create-hello-world-web-app/new-jsp-file-dialog.png
[publish-as-azure-web-app]: media/create-hello-world-web-app/publish-as-azure-web-app.png
[publish-status]: media/create-hello-world-web-app/publish-status.png
[manage-app-service]: media/create-hello-world-web-app/manage-app-service.png