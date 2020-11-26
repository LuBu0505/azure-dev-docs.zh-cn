---
title: 步骤 7：在从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务后清理资源
description: 教程步骤 7，清理 Azure 资源
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: e9fe39d97487cb58523e2fd33a8acb74c51aa00b
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96035477"
---
# <a name="7-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>7:在从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务后清理资源

[上一步：流式传输日志](tutorial-deploy-app-service-on-linux-06.md)

创建的 Azure 应用服务包含一项可能产生费用的后备应用服务计划。 若要避免此类成本，请删除包含所有资源的资源组。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

恭喜，你已完成了本演练，已经将 Python 代码部署到 Linux 上的应用服务！

如前所述，若要详细了解应用服务扩展，可以访问其 GitHub 存储库：[vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)。 欢迎提交问题和贡献材料。

若要详细了解可以通过 Python 使用的 Azure 服务（包括数据存储、AI 和机器学习服务），请访问 [Azure Python 开发人员中心](/python/azure/)。

可能还有其他用于 VS Code 的 Azure 扩展对你有用。 直接在“扩展”资源管理器中搜索“Azure”：

![适用于 Visual Studio Code 的 Azure 扩展](media/deploy-azure/azure-extensions-for-visual-studio-code.png)

下面是一些常用的扩展：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure 资源管理器 (ARM) 工具](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [我完成了！](/python/azure/) 

[存在问题？请告诉我们。](https://aka.ms/FlaskVSCQuickstartHelp)
