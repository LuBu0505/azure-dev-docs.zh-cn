---
title: 部署 Azure Functions 应用程序后删除成本高昂的远程 Azure 资源
description: 删除（清理）远程 Azure 资源，这样它们就不会产生费用。 若要清理资源，请在 Azure Functions 资源管理器中右键单击“函数应用”，然后选择“删除函数应用”。
ms.topic: tutorial
ms.date: 08/31/2020
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: 428330b34b3c315d01c2209840de62001caa747a
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522057"
---
# <a name="5-clean-up-azure-resources-for-azure-functions-tutorial"></a>5.清理 Azure Functions 教程的 Azure 资源

[上一步：部署 Functions 应用](tutorial-vscode-serverless-node-deploy-hosting.md)

## <a name="remove-remote-azure-resources"></a>删除远程 Azure 资源

创建的 Functions 应用包括可能会产生少量费用的资源（请参阅 [Functions 定价](https://azure.microsoft.com/pricing/details/functions/)）。 若要清理资源，请在“Azure:  Functions”资源管理器中右键单击函数应用，然后选择“删除函数应用”。 

也可访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

[!INCLUDE [Next steps for using VSCode extensions](../includes/tutorial-next-steps-vscode-extensions.md)]

[!INCLUDE [Next steps for using JavaScript on Azure](../includes/tutorial-next-steps-js-azure.md)]

## <a name="learn-more-about-azure-functions"></a>详细了解 Azure Functions

* [Azure Functions 开发人员指南](/azure/azure-functions/functions-reference)
* [Azure Functions JavaScript 开发人员指南](/azure/azure-functions/functions-reference-node)
* [保护 Azure Functions](/azure/azure-functions/security-concepts)
* [存储](/azure/azure-functions/storage-considerations)和[性能](/azure/azure-functions/functions-best-practices)注意事项

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [我已完成](../how-to/develop-serverless-apps.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=clean-up-resources)
