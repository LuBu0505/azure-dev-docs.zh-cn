---
title: 使用 Azure CLI 将 Node.js 应用部署到 Azure 后清理资源
description: Azure CLI 教程第 7 部分：清理资源
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 0c3404067f6324a37d7863568e715b806511a226
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405986"
---
# <a name="part-7-clean-up-resources"></a>第 7 部分：清理资源

[上一步：进行更改并重新部署](tutorial-vscode-docker-node-06.md)

创建的应用服务包含一项可能产生费用的后备应用服务计划。 若要清理资源，请在终端或命令提示符下运行以下命令：

```azurecli
az group delete --name myResourceGroup
```

也可访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

> [!div class="nextstepaction"]
> [我已完成](./how-to/deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)