---
title: 从 Visual Studio Code 创建 Azure Functions 应用程序
description: 创建一个本地 Azure Functions（无服务器）应用程序，其中包含一个使用 HTTP 触发器的函数。 Azure Functions 应用可以包含多个具有不同触发器的 Functions。 HTTP 触发器专门处理传入的 HTTP 流量。
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: b1e357bc3329c2dc19fdf4988b22182576575cf5
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522081"
---
# <a name="2-create-the-local-functions-app-with-the-visual-studio-code-_functions_-extension"></a>2.使用 Visual Studio Code Functions 扩展来创建本地 Functions 应用

[上一步：简介和先决条件](tutorial-vscode-serverless-node-install.md)

在此步骤中，将创建一个本地 Azure Functions（无服务器）应用程序，其中包含一个使用 [HTTP 触发器](/azure/azure-functions/functions-reference-node#http-triggers-and-bindings)的函数。 Azure Functions 应用可以包含多个具有[不同触发器](/azure/azure-functions/functions-triggers-bindings)的函数。 HTTP 触发器专门处理传入的 HTTP 流量。

1. 在终端中或命令提示符下，从适合项目的文件夹中运行 Visual Studio Code：

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. 在 Visual Studio Code 中，选择 Azure 徽标以打开“Azure Functions”资源管理器，然后选择 Create Project 命令 ：

    ![在 VS Code 中创建本地函数应用](../media/functions-extension/create-function-app-project.png)

1. 对于前两个提示，选择当前文件夹，然后选择 **JavaScript** 作为语言。

1. 在提示“为项目的第一个函数选择模板”  时，选择“HTTP 触发器”  ：

    ![选择函数的触发器](../media/functions-extension/create-function-choose-template.png)

1. 在提示“提供函数名称”  时，输入 **HttpExample**。 （避免使用默认的“HttpTrigger”名称，因为它与触发器相同，这可能会使人混淆。）

    ![输入函数名称](../media/functions-extension/create-function-name.png)

1. 在提示“授权级别”  时，选择“匿名”  ：

    ![ 在提示“授权级别”时，选择“匿名”](../media/functions-extension/create-function-anonymous-auth.png)

1. 几分钟后，VS Code 完成了项目的创建。 你有一个为函数命名的文件夹 *HttpExample*，其中有三个文件：

    | 文件名 | 说明 |
    | --- | --- |
    | *index.js* |  响应 HTTP 请求的源代码。 |
    | *function.json* | HTTP 触发器的[绑定配置](/azure/azure-functions/functions-triggers-bindings)。 |
    | *sample.dat* | 一个占位符数据文件，用于演示文件夹中可以有其他文件。 如果需要，可以删除此文件，因为本教程中没有使用它。 |

    ![创建函数应用的结果](../media/functions-extension/create-function-app-results.png)

## <a name="http-function-javascript-template-code"></a>HTTP 函数 JavaScript 模板代码

为你提供了用于响应 HTTP 请求的基本代码。 如果你熟悉 HTTP 请求（req 参数）和响应对象，那么该函数应该看起来很熟悉。 使用 `res` 属性上的 context 对象返回响应信息。  

```javascript
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

每个 Function [触发](/azure/azure-functions/functions-triggers-bindings?tabs=csharp)类型都提供了一个模板函数，使你可以立即专注于应用程序的代码。 从 Express.js 移至 Azure Functions 时，[查看应用程序的必要更改](/azure/azure-functions/shift-expressjs?tabs=javascript)。 

> [!div class="nextstepaction"]
> [我创建了 Functions 应用](tutorial-vscode-serverless-node-test-local.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
