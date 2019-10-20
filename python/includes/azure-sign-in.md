---
ms.openlocfilehash: fcbce1c0f17721c7f3fd90e8d396baba6ae100c4
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278829"
---
安装 Azure 扩展以后，请通过导航到 **Azure** 资源管理器登录到 Azure 帐户，选择“登录 Azure”  ，然后按照提示操作。 （如果已安装多个 Azure 扩展，请针对你使用的区域选择一个扩展，如应用服务、Functions 等。）

![通过 VS Code 登录到 Azure](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

登录以后，验证 Azure 帐户的电子邮件地址（或者“已登录”）是否显示在状态栏中，以及订阅是否显示在 **Azure** 资源管理器中：

![Visual Studio Code 状态栏，其中显示了 Azure 帐户](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Visual Studio Code 的 Azure 应用服务资源管理器，其中显示了订阅](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> 如果出现错误“找不到名为 [订阅 ID] 的订阅”，这可能是因为你使用了代理，因此无法访问 Azure API。  在终端中使用代理信息配置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量：
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
