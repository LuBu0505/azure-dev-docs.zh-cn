---
ms.openlocfilehash: 90ac4162e1a6032bcb1f3cfab7f249e4538314f7
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441472"
---
安装 Azure 扩展以后，请通过导航到 **Azure** 资源管理器登录到 Azure 帐户，选择“登录 Azure”  ，然后按照提示操作。 （如果已安装多个 Azure 扩展，请针对你使用的区域选择一个扩展，如应用服务、Functions 等。）

![通过 VS Code 登录到 Azure](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

登录以后，验证 Azure 帐户的电子邮件地址（或者“已登录”）是否显示在状态栏中，以及订阅是否显示在 **Azure** 资源管理器中：

![Visual Studio Code 状态栏，其中显示了 Azure 帐户](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Visual Studio Code 的 Azure 应用服务资源管理器，其中显示了订阅](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> 如果出现错误“找不到名为 [订阅 ID] 的订阅”，这可能是因为你使用了代理，因此无法访问 Azure API。  在终端中使用代理信息配置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量：
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
