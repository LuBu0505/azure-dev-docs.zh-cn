---
title: 在部署到 Azure 应用服务后清理资源
description: 教程第 4 部分：清理资源
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: 1f452b054aa41c714105e79bd3dc971c88acd260
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85791427"
---
# <a name="clean-up"></a>清除

[上一步：部署应用](tutorial-visual-studio-code-azure-app-service-deno-03.md)

在本部分中，我们将删除和清理所有已创建的资源。

## <a name="remove-your-resources"></a>删除资源

创建的应用服务包括一个在免费定价层上运行的后备应用服务计划，因此你不会产生任何持续成本。

需要清理资源时，请访问 [Azure 门户](https://portal.azure.com)，选择“资源组”，找到并选择在本教程中创建的资源组（例如 `deno-quickstart`），然后使用“删除资源组”命令。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
