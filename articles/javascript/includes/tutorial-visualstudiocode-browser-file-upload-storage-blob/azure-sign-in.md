---
title: include 文件 azure-sign-in.md
description: include 文件 azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 085958cd12352273d5433279bae5f3edac934714
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344098"
---
在 Visual Studio Code 中安装 [Azure 存储扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)以后，请通过导航到 Azure 资源管理器登录到 Azure 帐户，选择“登录 Azure”，然后按照提示操作 。 

![通过 VS Code 登录到 Azure](../../media/tutorial-browser-file-upload/azure-sign-in.png)

登录以后，验证 Azure 帐户的电子邮件地址（或者“已登录”）是否显示在状态栏中，以及订阅是否显示在 **Azure** 资源管理器中：

![显示 Azure 帐户的 VS Code 状态栏](../../media/tutorial-browser-file-upload/azure-account-status-bar.png)

![VS Code 的 Azure 资源管理器，其中显示了订阅](../../media/tutorial-browser-file-upload/azure-subscription-view.png)

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
