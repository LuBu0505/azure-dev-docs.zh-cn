---
title: 使用 Azure CLI 将 Node.js 应用部署到 Azure 后清理资源
description: 教程第 7 部分：清理资源
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 183539b8e2f0246bd812e5fa364a885b75626819
ms.sourcegitcommit: aa2c66b0fecce51862cc9115f68d39c770f0b2ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709854"
---
# <a name="clean-up-resources"></a>清理资源

[上一步：进行更改并重新部署](tutorial-vscode-docker-node-06.md)

创建的应用服务包含一项可能产生费用的后备应用服务计划。 若要清理资源，请在终端或命令提示符下运行以下命令：

```azurecli
az group delete --name myResourceGroup
```

也可访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
