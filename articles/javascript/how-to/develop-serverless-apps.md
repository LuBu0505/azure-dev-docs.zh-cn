---
title: 使用 Azure Functions 的无服务器 Node.js 代码
description: Azure Functions 提供无服务器代码基础结构，使你能够创建响应式按需 HTTP 终结点。
ms.topic: how-to
ms.date: 10/27/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperfq2
ms.openlocfilehash: eb33717761d492051737b0c4ec86a93a6e2b6256
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93192539"
---
# <a name="use-azure-functions-to-develop-nodejs-serverless-code"></a>使用 Azure Functions 开发 Node.js 无服务器代码

Azure Functions 提供无服务器代码基础结构，使你能够创建响应式按需 HTTP 终结点。 无服务器代码包含为了响应各种事件而运行的 JavaScript 或 TypeScript 代码。 

Functions 以代码或 Docker 容器的形式在 Web 服务的基础上运行，这一点可不予以考虑，因此你可以专注于终结点的代码。 Functions 还使你能够触发另一个函数，这样函数工作流就可以替换现有托管后端服务器功能，且无需管理该服务器。 

## <a name="what-is-a-function-resource"></a>什么是 Function 资源？

Azure Function 资源是一个 Azure 地理位置中所有相关函数的逻辑单元。 资源可包含一个或多个函数，这些函数可以彼此独立，也可以与输入或输出触发器相关。 可以从多个常用函数中进行选择，也可以创建自己的函数。

:::image type="content" source="../media/howto-serverless/portal-screenshot-new-azure-function-type.png" alt-text="可以从多个常用函数中进行选择，也可以创建自己的函数。":::

函数资源设置包含常用的无服务器配置，包括环境变量、身份验证、日志记录和 CORS。  

## <a name="durable-stateful-functions"></a>持久有状态函数 

[Durable Functions](/azure/azure-functions/durable/durable-functions-overview) 在 Azure 中保留状态或管理长期运行的函数。 [使用 JavaScript 创建你的第一个持久函数](/azure/azure-functions/durable/quickstart-js-vscode)。

## <a name="static-web-apps-include-functions"></a>静态 Web 应用包括函数 

开发静态前端客户端应用程序（如 Angular、React 或 Vue）时（也需要无服务器 API），请配合使用[静态 Web 应用](/azure/static-web-apps/getting-started?tabs=react)和函数，以将两者捆绑在一起。 

## <a name="a-simple-javascript-function-for-http-requests"></a>用于 HTTP 请求的简单 JavaScript 函数

函数是含有请求和上下文信息的导出异步函数。 Azure 门户的以下部分屏幕截图显示了函数代码。 

:::image type="content" source="../media/howto-serverless/portal-screenshot-azure-function-http.png" alt-text="Azure 门户中的 Azure Function 的部分屏幕截图。":::

## <a name="develop-functions-locally-with-visual-studio-code-and-extensions"></a>使用 Visual Studio Code 和扩展在本地开发函数

使用 Visual Studio Code 创建[第一个函数](/azure/azure-functions/functions-create-first-function-vs-code)。 Visual Studio Code 通过 [Azure Functions 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)简化了许多细节。

此扩展有助于使用常用模板创建 JavaScript 和 TypeScript 函数。 

Azure 的 HTTP 函数的 JavaScript 示例如下： 

```nodejs
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}
```

Azure 的 HTTP 函数的 TypeScript 示例如下： 

```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };

};

export default httpTrigger;
```

## <a name="configuring-the-function"></a>配置函数

函数通过 function.json 文件进行配置。 通过此配置，你可以配置触发函数的方式（“方向”：in）和函数返回的内容（“方向”：out）。 此外，你还可以设置环境变量，以及运行函数所需的其他信息。 详细了解[触发器和绑定](/azure/azure-functions/functions-triggers-bindings?tabs=javascript.md)。 

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

## <a name="develop-functions-remotely-using-the-azure-portal"></a>使用 Azure 门户远程开发函数

如果[使用 Azure 门户创建 Azure 函数](https://ms.portal.azure.com/#create/Microsoft.FunctionApp)，则可以配置函数，在预填充的模板内编写代码，并测试函数。 

该门户仅可创建 JavaScript 函数，而不是 TypeScript。 如果要使用 TypeScript 进行开发，请下载函数，或利用 Visual Studio Code，通过 Function 扩展在本地创建函数。 

## <a name="next-steps"></a>后续步骤

[Azure Functions JavaScript 开发人员指南](/azure/azure-functions/functions-reference-node)是不错的着手点。 

使用 Microsoft Learn 模块，了解如何[使用 Azure Functions 和 SignalR 服务在 Web 应用中启用自动更新](/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)。

* [在计时器上运行代码](/azure/azure-functions/functions-create-scheduled-function)
* [当文件上传到 Azure Blob 存储中或在 Azure Blob 存储中更新时运行代码](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
* [在消息写入 Azure 队列存储时运行代码](/azure/azure-functions/functions-create-storage-queue-triggered-function)
* [使用 Azure Functions 和 Azure Cosmos DB 存储非结构化数据](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb?tabs=javascript)
* [Node.js + Azure Functions 示例](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)