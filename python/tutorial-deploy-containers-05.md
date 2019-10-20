---
title: 教程：清理 Azure 资源
description: 教程步骤 5，清理 Azure 资源，避免持续产生费用。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0a3e04759573769d1ed00e59a294caddfc4ef0cc
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278714"
---
# <a name="tutorial-clean-up-azure-resources"></a>教程：清理 Azure 资源

[上一步：流式传输日志](tutorial-deploy-containers-04.md)

本文介绍如何删除在使用 Visual Studio Code 将应用部署到 Azure 应用服务时创建的 Azure 资源。

在本教程中创建的各种 Azure 资源可能会持续产生费用。 若要清理它们，最好是访问 [Azure 门户](https://portal.azure.com)，从左侧导航窗格中选择“资源组”，  接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。 

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
> [我完成了](https://docs.microsoft.com/python/azure/?view=azure-python)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)
