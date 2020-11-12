---
title: include 文件 azure-sign-in.md
description: include 文件 azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c8cf3e2f478265a81742093315e2a4041b2f0ed9
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405088"
---
使用 `az webapp up` 命令在本地文件夹 (python-docs-hello-world) 中部署代码：

```azurecli
az webapp up --sku F1 --name <app-name>
```

- 如果无法识别 `az` 命令，请确保按照[设置初始环境](../../quickstart-python-flask-multipage.yml?tutorial-step=1)中所述安装 Azure CLI。
- 如果无法识别 `webapp` 命令，请确保 Azure CLI 版本为 2.0.80 或更高版本。 如果不是，请[安装最新版本](/cli/azure/install-azure-cli)。
- 将 `<app_name>` 替换为在整个 Azure 中均唯一的名称（有效字符为 `a-z`、`0-9` 和 `-`）。 良好的模式是结合使用公司名称和应用标识符。
- `--sku F1` 参数在“免费”定价层上创建 Web 应用。 省略此参数可使用更快的高级层，这会按小时计费。
- 可以选择包含参数 `--location <location-name>`，其中 `<location_name>` 是可用的 Azure 区域。 可以运行 [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations) 命令来检索 Azure 帐户的允许区域列表。
- 如果看到错误“无法自动检测应用的运行时堆栈”，请确保在包含 requirements.txt 文件的 python-docs-hello-world 文件夹 (Flask) 或 python-docs-hello-django 文件夹 (Django) 中运行该命令 。 （请参阅[通过 az webapp up 解决自动检测问题](https://github.com/Azure/app-service-linux-docs/blob/master/AzWebAppUP/runtime_detection.md) (GitHub)。）

此命令可能需要花费几分钟时间完成。 运行时，它提供以下相关信息：创建资源组、应用服务计划和托管应用，配置日志记录，然后执行 ZIP 部署。 然后，它将显示消息“可以在 http://&lt;app-name&gt;.azurewebsites.net（这是 Azure 上应用的 URL）启动应用”。

![az webapp up 命令的示例输出](../../media/quickstart-python/az-webapp-up-output.png)

[!include [az webapp up command note](../app-service-web-az-webapp-up-note.md)]