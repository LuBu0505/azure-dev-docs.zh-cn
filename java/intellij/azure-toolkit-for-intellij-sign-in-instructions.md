---
title: 用于 IntelliJ 的 Azure 工具包的登录说明
description: 了解如何使用用于 IntelliJ 的 Azure 工具包登录到 Microsoft Azure。
services: ''
documentationcenter: java
author: bmitchell287
manager: douge
editor: ''
ms.assetid: ''
ms.author: brendm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 7c2cec950c8a8bd51a7e1c6f9e5de390e1799c15
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68278858"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>用于 IntelliJ 的 Azure 工具包的登录说明

[安装](https://www.jetbrains.com/help/idea/managing-plugins.html)后，[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 提供两种登录 Azure 帐户的方法：

  - [通过设备登录名登录到 Azure 帐户](#sign-in-to-your-azure-account-by-device-login)
  - [通过服务主体登录到 Azure 帐户](#sign-in-to-your-azure-account-by-service-principal)

还提供了[**注销**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>通过设备登录名登录到 Azure 帐户

若要通过设备登录名登录 Azure，请执行以下操作：

1. 使用 IntelliJ IDEA 打开项目。

2. 打开边栏中的“Azure 资源管理器”，然后单击顶部栏中的“Azure 登录”图标（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 登录”）。   

   ![“IntelliJ Azure 登录”命令][I01]

3. 在“Azure 登录”窗口中选择“设备登录名”，然后单击“登录”。   

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][I03]

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][I04]

6. 在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。  

   ![“选择订阅”对话框][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>通过服务主体登录到 Azure 帐户

本部分逐步引导创建一个包含服务主体数据的凭据文件。 完成此过程后，在你打开项目时，IntelliJ 会使用凭据文件将你自动登录到 Azure。

1. 使用 IntelliJ IDEA 打开项目。

1. 打开边栏中的“Azure 资源管理器”，然后单击顶部栏中的“Azure 登录”图标（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 登录”）。   
   ![“IntelliJ Azure 登录”命令][A01]

1. 在“Azure 登录”窗口中选择“服务主体”，然后单击“新建”。   

   ![已选中“服务主体”的“Azure 登录”窗口][A02]

1. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][A03]

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][A04]

1. 在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。  

   ![“创建身份验证文件”窗口][A05]

1. 成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。  

   ![“服务主体创建状态”对话框][A06]

1. 在“Azure 登录”窗口中单击“登录”。   

   ![“Azure 登录”对话框][A07]

1. 在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。  

   ![“选择订阅”对话框][A08]

> 创建服务主体身份验证文件以后，即可从步骤 8 开始操作，选择身份验证文件并登录。

## <a name="sign-out-of-your-azure-account"></a>注销 Azure 帐户

通过上述步骤配置帐户后，每次启动 IntelliJ IDEA 时都会自动登录。 但是，若要注销 Azure 帐户，请执行以下操作。

* 在 IntelliJ IDEA 中打开“Azure 资源管理器”边栏，单击“Azure 注销”图标并进行确认（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 注销”）。  

   ![“IntelliJ Azure 注销”命令][L01]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png
[A09]: media/azure-toolkit-for-intellij-sign-in-instructions/A09.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
