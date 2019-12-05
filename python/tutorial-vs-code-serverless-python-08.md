---
title: 教程：清理 Azure 资源- Python 中的 Azure Functions
description: 教程步骤 8，清理 Azure 资源，避免持续产生费用。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: ddc2ab44d3d8865c89cb6cdf8368461b6e4f666a
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465946"
---
# <a name="tutorial-clean-up-azure-resources-for-azure-functions"></a>教程：为 Azure Functions 清理 Azure 资源

[上一步：添加存储绑定](tutorial-vs-code-serverless-python-07.md)

本文介绍如何删除在本教程中创建的 Azure 资源。 使用 Visual Studio Code 创建的 Azure 函数应用包括可能会产生最小开销的资源。

若要清理资源，请在“Azure:  Functions”资源管理器中右键单击函数应用，然后选择“删除函数应用”。  有关详细信息，请参阅 [Functions 定价](https://azure.microsoft.com/pricing/details/functions/)。

也可访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

## <a name="next-steps"></a>后续步骤

恭喜，你已完成了本演练，已经将 Python 代码部署到 Azure Functions！ 你现在可以创建更多的无服务器函数了。

如前所述，若要详细了解 Functions 扩展，可以访问其 GitHub 存储库：[vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)。 欢迎提交问题和贡献材料。

请阅读 [Azure Functions 概述](/azure/azure-functions/functions-overview)，了解可以使用的不同触发器。

若要详细了解可以通过 Python 使用的 Azure 服务（包括数据存储、AI 和机器学习服务），请访问 [Azure Python 开发人员中心](/azure/python/?view=azure-python)。

可能还有其他用于 Visual Studio Code 的 Azure 扩展对你有用。 直接在“扩展”资源管理器中搜索“Azure”：

![适用于 Visual Studio Code 的 Azure 扩展](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

下面是一些常用的扩展：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 请参阅[“部署到应用服务”教程](tutorial-deploy-app-service-on-linux-01.md)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure 资源管理器工具](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [我完成了](https://docs.microsoft.com/python/azure/?view=azure-python)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
