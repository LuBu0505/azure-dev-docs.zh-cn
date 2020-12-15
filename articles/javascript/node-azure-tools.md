---
title: 工具选择 - JavaScript - Azure
description: 安装用于在 Azure 上进行 Node.js 和 JavaScript 开发的各个工具
ms.topic: reference
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.openlocfilehash: 714d096e4afde345bffa2582026f28fa2126a38c
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857786"
---
# <a name="tools-for-javascript-developers-on-azure"></a>Azure 上的面向 JavaScript 开发人员的工具 

JavaScript 是一个包含很多工具的生态系统。 本文为 JavaScript 开发人员提供了一些由 Microsoft 构建和维护的工具。 工具针对新的 Azure 服务和部署/托管方案进行了改进。 

你不需要使用这些工具来使用 Azure，它只是在功能和支持方面提供了更好的体验。 

## <a name="azure-portal"></a>Azure 门户

通过 [Azure 门户](https://portal.azure.com/)，你可访问帐户的所有订阅和资源。 

## <a name="azure-cli"></a>Azure CLI
Azure CLI 经过优化，可从命令行管理 Azure 资源。 

Azure CLI 提供以下使用方案：

* [本地安装](/cli/azure/install-az-cli2)
* [Web shell](https://shell.azure.com/)
* [容器](/cli/azure/run-azure-cli-docker)

如果使用 Azure 门户，可从门户顶部的导航栏中获取 Azure CLI。

:::image type="content" source="media/azure-tools/azure-portal-select-azure-cloud-shell.png" alt-text="如果使用 Azure 门户，可从门户顶部的导航栏中获取 Azure CLI。":::

## <a name="typescript"></a>TypeScript

[TypeScript](https://www.typescriptlang.org/download) 提供了 JavaScript 具有的所有功能，并在此基础上增加了一层：TypeScript 的类型系统。 你现有的有效 JavaScript 代码也是 TypeScript 代码。 TypeScript 的主要优点在于，它可以突出显示代码中的意外行为，从而降低 bug 出现的几率。

## <a name="typescript-and-the-azure-sdk-client-libraries"></a>TypeScript 和 Azure SDK 客户端库

Azure SDK 客户端库参考文档是针对 TypeScript 编写的，因为客户端库是用 TypeScript 编写的。 不必用 TypeScript 来使用 Azure SDK 客户端库。 

详细了解 [Azure SDK 的 TypeScript 准则](https://azure.github.io/azure-sdk/typescript_introduction.html)。

## <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com) 是适用于 Azure 的 JavaScript 开发的首选 IDE。 界面、功能和扩展协同工作，以缩短开发时间并减少开发问题。 

在本地开发项目的根目录下创建一个项目工作区，然后添加所有相关的配置、设置和扩展。 签入项目的工作区文件，以使每个团队成员都有权访问项目所需的设置和工具。

使用 Visual Studio Code 有以下几个好处：

* Visual Studio Code 以内联方式显示 Azure 引用文档
* Visual Studio Code 提供语句完成
* 很少有不明确的类型或对象

Visual Studio code 提供丰富的文档，以供 [JavaScript 项目使用](https://code.visualstudio.com/docs/nodejs/working-with-javascript)。 

## <a name="visual-studio-code-extensions"></a>Visual Studio Code 扩展
在 Visual Studio Code 中使用以下免费扩展来直接与 Azure 服务对接。

| 工具 | 说明  |
|:---------:|---------|
| [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions "Azure Functions 扩展的链接") <br> [![Azure Functions 工具](media/node-azure-tools/icon-azure-functions.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) | 创建、管理、查看、调试和部署函数|
| [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice "Azure 应用服务扩展的链接") <br> [![应用服务工具](media/node-azure-tools/icon-azure-app-service.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) | 浏览站点和 Azure 门户，创建新站点并部署到槽 |
| [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb "Cosmos DB 扩展的链接" )  <br> [![Cosmos DB 工具](media/node-azure-tools/icon-cosmos-db.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)| 在 Azure 中创建、浏览和更新全球分布式多模型数据库 |
| [Docker](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)   <br> [![Docker](media/node-azure-tools/icon-docker.png)](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)| 管理 Docker 容器和映像、Docker 中心及 Azure 容器注册表 |

> [!div class="nextstepaction"]
> [在 Visual Studio Code Marketplace 中获取更多的 Azure 扩展](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)
