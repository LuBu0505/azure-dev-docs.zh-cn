---
title: 步骤 5：清理 Azure 资源
description: 教程步骤 5，清理 Azure 资源，避免持续产生费用。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: cc5540570bc914d39a17bd102dc0f86f6674025a
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473522"
---
# <a name="5-clean-up-azure-resources"></a>5：清理 Azure 资源

[上一步：流式传输日志](tutorial-deploy-containers-04.md)

在本教程中创建的 Azure 资源可能会产生持续的成本。 若要避免此类成本，请删除包含所有这些资源的资源组。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

若要详细了解 Docker 和应用服务扩展，可以访问它们各自在 GitHub 上的存储库：[vscode-docker](https://github.com/Microsoft/vscode-docker) 和 [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)。 欢迎提交问题和贡献材料。

若要详细了解可以通过 Python 使用的 Azure 服务（包括数据存储、AI 和机器学习服务），请访问 [Azure Python 开发人员中心](https://docs.microsoft.com/python/azure/?view=azure-python)。

可能还有其他用于 VS Code 的 Azure 扩展对你有用。 直接在“扩展”资源管理器中搜索“Azure”：

![用于 VS Code 的 Azure 扩展](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

下面是一些常用的扩展：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure 资源管理器工具](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [我完成了！](https://docs.microsoft.com/python/azure/?view=azure-python)

问题？ 使用页面底部的“此页面”反馈提交 GitHub 问题。
