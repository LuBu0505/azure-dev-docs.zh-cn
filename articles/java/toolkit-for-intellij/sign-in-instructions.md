---
title: 用于 IntelliJ 的 Azure 工具包的登录说明
description: 了解如何使用用于 IntelliJ 的 Azure 工具包登录到 Microsoft Azure。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 27442efda23ba24abe6b40f20f7685f6ac7a524e
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010250"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>用于 IntelliJ 的 Azure 工具包的登录说明

[安装](https://www.jetbrains.com/help/idea/managing-plugins.html)后，[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 提供两种登录 Azure 帐户的方法：

  - [通过设备登录名登录到 Azure 帐户](#sign-in-to-your-azure-account-by-device-login)
  - [通过服务主体登录到 Azure 帐户](#sign-in-to-your-azure-account-by-service-principal)

还提供了[**注销**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>通过设备登录名登录到 Azure 帐户

若要通过设备登录名登录 Azure，请执行以下操作：

1. 使用 IntelliJ IDEA 打开项目。

1. 打开边栏中的 Azure 资源管理器，然后单击顶部栏中的“Azure 登录”图标（或者在 IntelliJ 菜单中导航到“工具”>“Azure”>“Azure 登录”）  。

   ![“IntelliJ Azure 登录”命令][I01]

1. 在“Azure 登录”窗口中选择“设备登录名”，然后单击“登录”。  

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

1. 在“Azure 设备登录”对话框中单击“复制并打开”。 

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

1. 选择 Azure 帐户，完成登录所需的全部身份验证过程。

1. 在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。 


## <a name="sign-in-to-your-azure-account-by-service-principal"></a>通过服务主体登录到 Azure 帐户

本部分逐步引导创建一个包含服务主体数据的凭据文件。 完成此过程后，当打开项目时，IntelliJ 会使用凭据文件将你自动登录到 Azure。

1. 使用 IntelliJ IDEA 打开项目。

1. 打开边栏中的 Azure 资源管理器，然后单击顶部栏中的“Azure 登录”图标（或者在 IntelliJ 菜单中导航到“工具”>“Azure”>“Azure 登录”）  。

   ![“IntelliJ Azure 登录”命令][I01]

1. 在“Azure 登录”窗口中选择“服务主体”，然后单击“新建”。  

   ![已选中“服务主体”的“Azure 登录”窗口][A02]

1. 在“Azure 设备登录”对话框中单击“复制并打开”。 

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

1. 选择 Azure 帐户，完成登录所需的全部身份验证过程。 身份验证后，关闭浏览器并切换回 IntelliJ。

1. 在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。 

1. 成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。 

1. 在“Azure 登录”窗口中单击“登录”。  

1. 在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。 

   > [!TIP]
   > 创建服务主体身份验证文件后，可从步骤 3 开始操作，选择身份验证文件并进行登录。

## <a name="sign-out-of-your-azure-account"></a>注销 Azure 帐户

通过上述步骤配置帐户后，每次启动 IntelliJ IDEA 时都会自动登录。 

但是，如果想要注销 Azure 帐户，请导航到“Azure 资源管理器”边栏，单击“Azure 注销”图标或在 IntelliJ 菜单中导航到“工具”>“Azure”>“Azure 注销” 。


## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png

