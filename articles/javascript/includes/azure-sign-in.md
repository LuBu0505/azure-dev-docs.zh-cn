---
ms.custom: devx-track-js
ms.topic: include
ms.date: 10/16/2020
ms.openlocfilehash: 624ba9739549b260018e05450598d17460f2e5d8
ms.sourcegitcommit: c1ef7aa8ed2e88e98b190e42cffde52cf301958d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97096460"
---
如果你已使用 Azure 服务扩展，则应该已经登录，可以跳过此步骤。 如果你不使用 Azure 服务扩展，请继续阅读本部分以登录到 Azure。

在 Visual Studio Code 中安装 Azure 服务扩展以后，需要通过导航到 Azure 资源管理器登录到 Azure 帐户，选择“登录 Azure”，然后按照提示操作 。 （如果已安装多个 Azure 扩展，请针对你使用的区域选择一个扩展，如应用服务、Functions 等。）

![通过 VS Code 登录到 Azure](../media/deploy-azure/azure-sign-in.png)

登录以后，验证 Azure 帐户的电子邮件地址（或者“已登录”）是否显示在状态栏中，以及订阅是否显示在 **Azure** 资源管理器中：

![显示 Azure 帐户的 VS Code 状态栏](../media/deploy-azure/azure-account-status-bar.png)

![VS Code 的 Azure 资源管理器，其中显示了订阅](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> 如果出现错误“找不到名为 [订阅 ID] 的订阅”，这可能是因为你使用了代理，因此无法访问 Azure API。 在终端中使用代理信息配置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量：
>
> # <a name="bash"></a>[bash](#tab/bash)
>
> ```bash
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> # <a name="powershell"></a>[PowerShell](#tab/powershell)
>
> ```powershell
> $env: HTTPS_PROXY = "https://username:password@proxy:8080"
> $env: HTTP_PROXY = "http://username:password@proxy:8080"
> ```
>
> # <a name="cmd"></a>[Cmd](#tab/cmd)
>
> ```cmd
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ---
