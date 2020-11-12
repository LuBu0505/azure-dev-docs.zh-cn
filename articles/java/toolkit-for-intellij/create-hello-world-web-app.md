---
title: 使用 IntelliJ 创建适用于 Azure 应用服务的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for IntelliJ 创建 Azure 的 Hello World Web 应用。
services: app-service
keywords: java, intellij, web 应用, azure 应用服务, hello world, 快速入门
documentationcenter: java
author: selvasingh
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.reviewer: asirveda
ms.date: 09/09/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: 992ce6dfd32de9fc20016542b11f2792bf0a8e9b
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405846"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>使用 IntelliJ 创建适用于 Azure 应用服务的 Hello World Web 应用

本文演示通过使用 [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 创建基本的 Hello World Web 应用并将 Web 应用发布到 Azure 应用服务所需的步骤。

> [!NOTE]
>
> 如果首选使用 Eclipse，请查看[适用于 Eclipse 的类似教程][eclipse-hello-world]。
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> 请勿忘记在完成本教程后清理资源。 在这种情况下，运行本指南不会超出免费帐户配额。
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>安装和登录

以下步骤将指导你完成 IntelliJ 开发环境中登录 Azure 的过程。

1. 如果尚未安装该插件，请参阅[安装 Azure Toolkit for IntelliJ](./index.yml)。

1. 若要登录 Azure 帐户，请导航到左侧的 Azure 资源管理器边栏，然后单击“Azure 登录”图标 。 或者，你可导航到“工具”，展开“Azure”，然后单击“Azure 登录”  。

   :::image type="content" source="media/sign-in-instructions/I01.png" alt-text="在 IntelliJ 上登录到 Azure。"::: 

1. 在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](sign-in-instructions.md)）。  

1. 在“Azure 设备登录”对话框中单击“复制并打开” 。

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

1. 选择 Azure 帐户，完成登录所需的全部身份验证过程。

1. 登录后，关闭浏览器并切换回 IntelliJ IDE。 在“选择订阅”对话框中，选择要使用的订阅，然后单击“选择” 。

## <a name="creating-a-new-web-app-project"></a>创建新的 Web 应用项目

1. 单击“文件”，展开“新建”，然后单击“项目”  。

1. 在“新建项目”对话框中，选择“Maven”，并确保已选中“从原型创建”选项  。 从列表中选择“maven-archetype-webapp”，然后单击“下一步” 。

   :::image type="content" source="media/create-hello-world-web-app/maven-archetype-webapp.png" alt-text="选择“maven-archetype-webapp”选项。"::: 

1. 展开“项目坐标”下拉列表查看所有输入字段，并为新的 Web 应用指定以下信息，然后单击“下一步” ：

   * 名称：Web 应用的名称。 这将自动填写 Web 应用的 ArtifactId 字段。
   * GroupId：项目组的名称，通常为公司域。 （例如 com.microsoft.azure）
   * **版本** ：我们将保留默认版本 1.0-SNAPSHOT。

1. 自定义任何 Maven 设置或接受默认设置，然后单击“完成”。

1. 导航到左侧的“项目”选项卡上的项目，然后打开 rc/main/webapp/index.jsp 文件 。 将代码替换为以下内容并保存更改：

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```
   :::image type="content" source="media/create-hello-world-web-app/open-index-page.png" alt-text="打开 index.jsp 文件。":::

## <a name="deploying-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在“项目资源管理器”视图下右键单击项目，展开“Azure”，然后单击“部署到 Azure Web 应用” 。

1. 在“部署到 Azure”对话框中，可将应用程序部署到现有的 Tomcat Web 应用中，也可创建一个新的 Web 应用。

   a. 单击“没有可用的 Web 应用，单击以新建一个”，创建一个新的 Web 应用。 如果订阅中已存在现有的 Web 应用，则请从“Web 应用”下拉列表中选择“创建新的 Web 应用”。

      :::image type="content" source="media/create-hello-world-web-app/deploy-to-azure-webapps.png" alt-text="“部署到 Azure”对话框窗口。":::

   在弹出的“创建 Web 应用”对话框中，指定以下信息，然后单击“确定” ： 

      * 名称：Web 应用的域名字符串。
      * 订阅：指定要用于新的 Web 应用的 Azure 订阅。
      * Platform：选择“Linux”。
      * Web 容器：选择“TOMCAT 9.0-jre8”或视情况而定。
      * **资源组** ：指定 Web 应用的资源组。 可选择与你的 Azure 帐户关联的现有资源组，也可创建一个新的资源组。
      * **应用服务计划** ：指定 Web 应用的应用服务计划。 可选择与你的 Azure 帐户关联的现有计划，也可创建一个新计划。

   b. 若要部署到现有的 Web 应用，请从“Web 应用”下拉菜单中选择 Web 应用，然后单击“运行”。

1. 成功部署 Web 应用后，工具包会显示一条状态消息，以及成功部署的 Web 应用的 URL。

1. 可使用状态消息中提供的链接转到 Web 应用。

   :::image type="content" source="media/create-hello-world-web-app/browse-web-app.png" alt-text="浏览 Web 应用。":::

## <a name="managing-deploy-configurations"></a>管理部署配置

> [!TIP]
> 发布 Web 应用后，可单击工具栏上的绿色箭头图标来运行部署。

1. 在运行 Web 应用的部署之前，可单击 Web 应用的下拉菜单并选择“编辑配置”来修改默认设置。

   :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="编辑配置菜单。":::

1. 在“运行/调试配置”对话框中，可修改任何默认设置。 单击“确定”保存设置。

## <a name="cleaning-up-resources"></a>清理资源

1. 若要删除 Web 应用，请导航到左侧的“Azure 资源管理器”边栏并找到“Web 应用”项 。 

   > [!NOTE]
   > 如果“Web 应用”菜单项未展开，请单击 Azure 资源管理器工具栏上的“刷新”图标，或者右键单击“Web 应用”菜单项并选择“刷新”来手动刷新列表 。

1. 右键单击要删除的 Web 应用，然后单击“删除”。

1. 若要删除应用服务计划或资源组，请访问 [Azure 门户](https://portal.azure.com)并手动删除订阅下的资源。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[Azure Toolkit for IntelliJ]: /azure/developer/java/tookit-for-intellij
[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[eclipse-hello-world]: ../toolkit-for-eclipse/create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[intelliJ-sign-in-instructions]: sign-in-instructions.md

<!-- IMG List -->
[marketplace]:media/create-hello-world-web-app/marketplace.png
[file-new-project]: media/create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: media/create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: media/create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: media/create-hello-world-web-app/maven-options.png
[project-name]: media/create-hello-world-web-app/project-name.png
[open-index-page]: media/create-hello-world-web-app/open-index-page.png
[edit-index-page]: media/create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: media/create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: media/create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: media/create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: media/create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: media/create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: media/create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: media/create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: media/create-hello-world-web-app/clean-resource.png