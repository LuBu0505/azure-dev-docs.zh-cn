---
title: 将 Hello World Web 应用部署到 Linux 容器
titleSuffix: Azure Toolkit for IntelliJ
description: 在 Linux 容器中运行一个基本的 Hello World Web 应用，并使用用于 IntelliJ 的 Azure 工具包将它部署到云中。
services: app-service\web
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 080ea6565a05860d930d8dd22bca5328fd34a545
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405836"
---
# <a name="deploy-java-app-to-azure-web-apps-for-containers-using-azure-toolkit-for-intellij"></a>使用 Azure Toolkit for IntelliJ 将 Java 应用部署到用于容器的 Web 应用

[Docker] 容器广泛用于部署 Web 应用程序。 开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。 Azure Toolkit for IntelliJ 通过添加用于将容器部署到 Microsoft Azure 的功能，为 Java 开发人员简化了部署过程。

本文演示如何创建基本的 Hello World Web 应用和使用用于 IntelliJ 的 Azure 工具包将 Linux 容器中的 Web 应用发布到 Azure。

[!INCLUDE [prerequisites](includes/prerequisites.md)]
* [Docker] 客户端。

> [!NOTE]
>
> 若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。 可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。
>
> ![Docker 设置菜单][docker-settings-menu]
>

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

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>创建 Azure 容器注册表，用作专用 Docker 注册表

以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。

> [!NOTE]
>
> 如果你想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。
>

1. 浏览到 [Azure 门户]并登录。

   登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。

1. 依次单击“+创建资源”菜单图标、“容器”类别和“容器注册表”  。

1. 显示“创建容器注册表”页面后，请指定以下信息：

   * 订阅：指定要用于新容器注册表的 Azure 订阅。

   * **资源组** ：指定容器注册表的资源组。 选择以下选项之一：
      * **新建** ：指定要创建新的资源组。
      * **使用现有** ：指定将从与 Azure 帐户关联的资源组列表中进行选择。

   * **注册表名称** ：指定新容器注册表的名称。

   * 位置：指定将在其中创建容器注册表的区域（例如“美国西部”）。

   * **SKU** ：指定容器注册表的服务层。 对于本教程，请选择“基本”。 有关详细信息，请参阅 [Azure 容器注册表服务层级](/azure/container-registry/container-registry-skus)。

1. 单击“查看 + 创建”，验证信息是否正确。 单击“创建”以完成。

## <a name="deploy-your-web-app-in-a-docker-container"></a>在 Docker 容器中部署 Web 应用

以下步骤将引导你为 Web 应用配置 Docker 支持并将 Web 应用部署到 Docker 容器中。

1. 导航到左侧“项目”选项卡上的项目，然后右键单击项目。 展开“Azure”，然后单击“添加 Docker 支持” 。

   将使用默认配置自动创建 Docker 文件。

   :::image type="content" source="media/hello-world-web-app-linux/docker-support-file.png" alt-text="Docker 支持文件。":::

1. 添加 Docker 支持后，右键单击项目资源管理器中的项目，展开“Azure”，然后单击“在用于容器的 Web 应用上运行” 。

1. 在“在用于容器的 Web 应用上运行”对话框中，填写以下信息：

   * 名称：指定在 Azure Toolkit 中显示的易记名称。 

   * **容器注册表** ：从下拉菜单中选择在本文的上一部分创建的容器注册表。 “服务器 URL”、“用户名”和“密码”字段会自动填充。

   * **映像和标记** ：指定容器映像名称；通常使用以下语法：“ *registry*.azurecr.io/ *appname* :latest”，其中： 
      * 注册表是上文所述的容器注册表 
      * appname 是 Web 应用的名称 

   * “使用现有的 Web 应用”或“创建新的 Web 应用”：用于指定是将容器部署到现有 Web 应用还是创建新的 Web 应用 。 指定的“应用名称”将创建 Web 应用的 URL，例如 *wingtiptoys.azurewebsites.net* 。

   * **资源组** ：指定是要使用现有资源组还是创建新的资源组。 

   * **应用服务计划** ：指定是要使用现有应用服务计划还是创建新的应用服务计划。 

1. 配置完上面列出的设置后，单击“运行”。 成功部署 Web 应用以后，状态会显示在“运行”窗口中。

1. 发布 Web 应用后，可浏览到之前为 Web 应用指定的 URL，例如 wingtiptoys.azurewebsites.net。

   ![浏览到 Web 应用][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a>可选：修改 Web 应用发布设置

1. 发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。 可单击 Web 应用的下拉菜单，然后单击“编辑配置”来修改这些设置。

    :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="编辑配置菜单。":::

1. 出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定” 。

## <a name="next-steps"></a>后续步骤

有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure 门户]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: ../index.yml
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[browsing-to-web-app]:  media/hello-world-web-app-linux/browsing-to-web-app.png
[docker-settings-menu]: media/hello-world-web-app-linux/docker-settings-menu.png