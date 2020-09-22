---
title: 用于 Eclipse 的 Azure 工具包的登录说明
description: 了解如何使用用于 Eclipse 的 Azure 工具包登录到 Microsoft Azure。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: d0dbc16a16ca3a5ff367e6c67fceabcb37e2cce6
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534417"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>用于 Eclipse 的 Azure 工具包的登录说明

用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：

  - [通过设备登录名登录到 Azure 帐户](#sign-in-to-your-azure-account-by-device-login)
  - [通过服务主体登录到 Azure 帐户](#sign-in-to-your-azure-account-by-service-principal)

还提供了[**注销**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>通过设备登录名登录到 Azure 帐户

本部分将指导你完成通过设备登录名登录 Azure 的过程。

1. 使用 Eclipse 打开项目。

1. 依次单击“工具”、“Azure”、“登录”。************

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="在 Eclipse IDE 中登录到 Azure。":::

1. 在“Azure 登录”窗口中选择“设备登录名”，然后单击“登录”。  

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

1. 在“Azure 设备登录”对话框中单击“复制并打开”。 

> [!NOTE]
>
> 如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 Internet Explorer、Firefox 或 Chrome：
>
> 1. 打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”
>
> 2. 选择首选使用的浏览器
>

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

1. 选择 Azure 帐户，完成登录所需的全部身份验证过程。

1. 登录后，关闭浏览器并切换回 Eclipse IDE。 在“选择订阅”对话框中选择要使用的订阅，然后单击“确定” 。

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>通过服务主体登录到 Azure 帐户

本部分逐步引导创建一个包含服务主体数据的凭据文件。 完成此过程后，在打开项目时，Eclipse 会使用凭据文件将你自动登录到 Azure。

1. 使用 Eclipse 打开项目。

2. 依次单击“工具”、“Azure”、“登录”。************

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="在 Eclipse IDE 中登录到 Azure。":::

3. 在“Azure 登录”窗口中，选择“服务主体”。******** 如果还没有服务主体身份验证文件，请单击“新建”创建一个。**** 否则，可以单击“浏览”将其打开，然后跳到步骤 8。****

   ![已选中“服务主体”的“Azure 登录”窗口][A02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。********

> [!NOTE]
>
> 如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 IE 或 Chrome：
>
> 1. 打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”
>
> 2. 选择首选使用的浏览器
>

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。 

6. 在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。 

7. 成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。 

8. 所创建文件的地址会自动填充在“Azure 登录”窗口中，此时请单击“登录”。********

   ![“Azure 登录”对话框][A06]

9. 最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。 


## <a name="sign-out-of-your-azure-account"></a>注销 Azure 帐户

通过上述步骤配置帐户后，每次启动 Eclipse 时都会自动登录。 但是，若要注销 Azure 帐户，请使用以下步骤。

1. 在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。************

2. 显示“Azure 注销”**** 对话框时，单击“是”****。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png
[A06]: media/sign-in-instructions/A06.png