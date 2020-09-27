---
ms.openlocfilehash: 1e6e3f46fc7dd2f4ddf708be65941deb9ca18096
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772602"
---
安装 Azure 扩展以后，请通过导航到 **Azure** 资源管理器登录到 Azure 帐户，选择“登录 Azure”****，然后按照提示操作。 （如果已安装多个 Azure 扩展，请针对你使用的区域选择一个扩展，如应用服务、Functions 等。）

![通过 VS Code 登录到 Azure](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

登录后，验证状态栏中显示了“Azure：已登录”，且 Azure 资源管理器中显示了你的订阅：

![Visual Studio Code 状态栏，其中显示了 Azure 帐户](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Visual Studio Code 的 Azure 应用服务资源管理器，其中显示了订阅](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> 如果出现错误“找不到名为 [订阅 ID] 的订阅”，这可能是因为你使用了代理，因此无法访问 Azure API。 在终端中使用代理信息配置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量：
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
