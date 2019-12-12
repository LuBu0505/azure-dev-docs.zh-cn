---
title: 使用 Azure Functions 编写无服务器 Node.js 代码
description: 有关如何使用 Azure Functions 创建和部署无服务器代码的指南。
ms.topic: article
ms.date: 08/19/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: d1b17e33b5ae4aa51a84ceae8005a5385c162967
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992470"
---
# <a name="use-azure-functions-to-write-serverless-nodejs-code-on-azure"></a>使用 Azure Functions 编写 Azure 上的无服务器 Node.js 代码

在 Azure 上，无服务器产品称为 Azure Functions。 通过无服务器代码，可以在 Internet 上创建响应式按需终结点，而不用为预配或管理基础结构担心。 无服务器代码包含为了响应各种事件而运行的脚本或其他代码内容。 

首先，立即开始：

- [使用 Visual Studio Code 创建第一个函数](/azure/azure-functions/functions-create-first-function-vs-code)。 本文在 Visual Studio Code 的上下文中介绍 Azure Functions，简化了很多详细信息。

接下来，查看以下文章，进一步了解 Azure Functions 所能执行的任务：

- [Azure Functions 简介](/azure/azure-functions/functions-overview)，其中描述了无服务器开发的优点、成本以及可用于运行无服务器代码的不同触发器。

- [Azure Functions 触发器和绑定概念](/azure/azure-functions/functions-triggers-bindings)

- [Azure Functions 开发人员指南](/azure/azure-functions/functions-reference)，然后是 [Azure Functions JavaScript 开发人员指南](/azure/azure-functions/functions-reference-node)

- 如果有兴趣在无服务器环境中编写有状态  函数，请查看[什么是 Durable Functions？](/azure/azure-functions/durable/durable-functions-overview)以及[在 JavaScript 中创建第一个 Durable 函数](/azure/azure-functions/durable/quickstart-js-vscode)。

从这里，可以享用许多其他资源，以帮助进一步探索无服务器代码：

- Microsoft Learn 模块：[使用 Azure Functions 和 SignalR 服务在 Web 应用中启用自动更新](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- 了解如何使用各种触发器运行无服务器代码：

  - [在计时器上运行代码](/azure/azure-functions/functions-create-scheduled-function)
  - [当文件上传到 Azure Blob 存储中或在 Azure Blob 存储中更新时运行代码](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [在消息写入 Azure 队列存储时运行代码](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [使用 Azure Functions 和 Azure Cosmos DB 存储非结构化数据](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md?tabs=javascript)。 有关其他数据库的信息，请参阅[如何将 Azure 数据库集成到 Node.js 代码中](node-howto-integrate-databases.md)

- [在本地对 Azure Functions 进行编码和测试](/azure/azure-functions/functions-develop-local)

- [在 Azure Functions 中测试代码的策略](/azure/azure-functions/functions-test-a-function)和[错误处理](/azure/azure-functions/functions-bindings-error-pages)

- [使用 Azure Active Directory 配置身份验证](/azure/app-service/configure-authentication-provider-aad.md?toc=%2fazure%2fazure-functions%2ftoc.json)

- [使用 Azure Pipelines 设置持续部署](/azure/azure-functions/functions-how-to-azure-devops)

- [Node.js + Azure Functions 示例](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)
