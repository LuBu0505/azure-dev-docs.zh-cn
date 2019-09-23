---
ms.openlocfilehash: d64a5c908915b5d020f27e1c501aee852f04ae38
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019655"
---
安装 Azure 扩展以后，请登录到 Azure 帐户，方法是：导航到“Azure:  应用服务”资源管理器，选择“登录到 Azure”，然后按提示操作。 

![通过 VS Code 登录到 Azure](../media/deploy-azure/azure-sign-in.png)

登录以后，验证 Azure 帐户的电子邮件地址是否显示在状态栏中，以及订阅是否显示在“Azure:  应用服务”资源管理器中：

![显示 Azure 帐户的 VS Code 状态栏](../media/deploy-azure/azure-account-status-bar.png)

![VS Code 的 Azure 应用服务资源管理器，其中显示了订阅](../media/deploy-azure/azure-subscription-view.png)

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
