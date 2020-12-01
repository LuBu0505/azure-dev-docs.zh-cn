---
ms.openlocfilehash: c78b25e2111e48ed9337bcd85f6585c4bdd4a081
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035507"
---
安装 Azure 扩展以后，请登录到 Azure 帐户：

1. 导航到 Azure 资源管理器
1. 选择“登录到 Azure”并按照提示操作。 （如果已安装多个 Azure 扩展，请针对你使用的区域选择一个扩展，如应用服务、Functions 等。）

    ![通过 VS Code 登录到 Azure](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

1. 登录后，验证状态栏中显示了“Azure：已登录”，且 Azure 资源管理器中显示了你的订阅：

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
