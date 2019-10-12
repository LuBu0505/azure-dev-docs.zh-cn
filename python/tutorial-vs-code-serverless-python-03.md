---
title: 教程：在 Visual Studio Code 中检查用于 Azure Functions 的 Python 代码文件
description: 教程步骤 3，了解 Azure Functions 提供的模板 Python 代码。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: c09323e35c20a0b9fb5162c08f7fa223969d83fe
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172113"
---
# <a name="tutorial-examine-the-python-code-files-in-visual-studio-code"></a>教程：在 Visual Studio Code 中检查 Python 代码文件

[上一步：创建函数](tutorial-vs-code-serverless-python-02.md)

在新创建的函数子文件夹中有三个文件： *\_\_init\_\_.py* 包含函数的代码，*function.json* 介绍 Azure Functions 的函数，*sample.dat* 是一个示例数据文件。 可以删除 *sample.dat*（如果你希望删除它），因为它存在只是为了演示你可以向子文件夹添加其他文件。

让我们先看看 *function.json*，然后看看 *\_\_init\_\_.py* 中的代码。

## <a name="functionjson"></a>function.json

function.json 文件提供 Azure Functions 终结点的必需配置信息：

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
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
      "name": "$return"
    }
  ]
}
```

`scriptFile` 属性标识代码的启动文件，该代码必须包含名为 `main` 的 Python 函数。 可以将代码分解成多个文件，只要此处指定的文件包含 `main` 函数即可。

`bindings` 元素包含两个对象，一个用于描述传入请求，另一个用于描述 HTTP 响应。 对于传入请求 (`"direction": "in"`)，此函数响应 HTTP GET 或 POST 请求，不需要身份验证。 响应 (`"direction": "out"`) 是一个 HTTP 响应，它返回从 `main` Python 函数返回的任意值。

## <a name="__initpy__"></a>\_\_init.py\_\_

当你创建新函数时，Azure Functions 在 *\_\_init\_\_.py* 中提供默认 Python 代码：

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

代码的重要部分如下所示：

- 必须从 `azure.functions` 导入 `func`；导入日志记录模块为可选操作，但建议你执行它。
- 必需的 `main` Python 函数接收名为 `req` 的 `func.HttpRequest` 对象，并返回类型为 `func.HttpResponse` 的值。 若要详细了解这些对象的功能，可参阅 [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) 和 [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python)。
- 然后，`main` 的主体会处理请求并生成响应。 在这种情况下，代码会在 URL 中查找 `name` 参数。 如果这样做失败，它会检查请求正文是否包含 JSON（使用 `func.HttpRequest.get_json`），以及 JSON 是否包含 `name` 值（使用 `get_json` 返回的 JSON 对象的 `get` 方法）。
- 如果找到一个名称，代码会返回追加了该名称的字符串“Hello”，否则会返回错误消息。

> [!div class="nextstepaction"]
> [我检查了代码文件](tutorial-vs-code-serverless-python-04.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=03-examine-code-files)
