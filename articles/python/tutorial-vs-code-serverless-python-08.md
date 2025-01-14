---
title: 步骤 8：清理 Azure Functions 中与无服务器 Python 代码配合使用的资源
description: 教程步骤 8，清理 Azure 资源，避免持续产生费用。
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperfq2
ms.openlocfilehash: aed7b2857d75f6016434550cc2a58d8230d80615
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96759394"
---
# <a name="8-clean-up-azure-resources-for-azure-functions"></a>8:为 Azure Functions 清理 Azure 资源

[上一步：添加存储绑定](tutorial-vs-code-serverless-python-07.md)

在学习本教程的过程中，使用 Visual Studio Code 创建的 Azure 函数应用包括可能会产生最小开销的资源。 （有关详细信息，请参阅 [Functions 定价](https://azure.microsoft.com/pricing/details/functions/)。）

清理资源的最佳方式是删除包含本教程所用的所有单个资源的资源组。 资源包括函数应用、存储帐户和后备应用服务计划。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

在 Visual Studio Code 中，你可能会注意到 **Azure:Functions** 资源管理器中的函数应用上的上下文菜单有一个“删除函数应用”命令。  但是，该命令仅删除函数应用，而将其他资源留在原处，这可能会产生持续的成本。

## <a name="next-steps"></a>后续步骤

恭喜，你已完成了本演练，已经将 Python 代码部署到 Azure Functions！ 你现在可以创建更多的无服务器函数了。

如前所述，若要详细了解 Functions 扩展，可以访问其 GitHub 存储库：[vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)。 欢迎提交问题和贡献材料。

请阅读 [Azure Functions 概述](/azure/azure-functions/functions-overview)，了解可以使用的不同触发器。

若要详细了解可以通过 Python 使用的 Azure 服务（包括数据存储、AI 和机器学习服务），请访问 [Azure Python 开发人员中心](./index.yml)。

可能还有其他用于 Visual Studio Code 的 Azure 扩展对你有用。 直接在“扩展”资源管理器中搜索“Azure”：

![适用于 Visual Studio Code 的 Azure 扩展](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

下面是一些常用的扩展：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 请参阅[“部署到应用服务”教程](tutorial-deploy-app-service-on-linux-01.md)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure 资源管理器工具](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [我完成了！](/python/azure/?preserve-view=true&view=azure-python)

[存在问题？请告诉我们。](https://aka.ms/python-functions-qs-ms-survey)
