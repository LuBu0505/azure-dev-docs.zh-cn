---
title: 使用 Azure CLI 将 Node.js 应用部署到 Azure 后清理资源
description: Azure CLI 教程第 7 部分：清理资源
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js
ms.openlocfilehash: 1ebd31a90d7ddbd422bb64b7bb44bb6b8ef33d1e
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365150"
---
# <a name="part-7-clean-up-resources"></a>第 7 部分：清理资源

[上一步：进行更改并重新部署](tutorial-vscode-docker-node-06.md)

创建的应用服务包含一项可能产生费用的后备应用服务计划。 若要清理资源，请在终端或命令提示符下运行以下命令：

```azurecli
az group delete --name myResourceGroup
```

也可访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
