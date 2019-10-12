---
title: 教程：使用 Visual Studio Code 向 Azure Functions 添加另一个 Python 函数
description: 教程步骤 6，通过添加另一个函数扩展 Azure Functions 项目。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 07f806dc58eb5aede65f0dca67e1bc59ce495a25
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172434"
---
# <a name="tutorial-add-a-second-python-function-to-azure-functions"></a>教程：向 Azure Functions 添加另一个 Python 函数

[上一步：部署到 Azure](tutorial-vs-code-serverless-python-05.md)

在第一次部署后，可以更改代码（例如添加其他函数），然后重新部署到同一 Functions 应用。

1. 在“Azure：函数”  Functions”资源管理器中选择“创建函数”命令，  或者使用命令面板中的“Azure Functions:  创建函数”。 为函数指定以下详细信息：

    - 模板：HTTP 触发器
    - 姓名：“DigitsOfPi”
    - 授权级别：匿名

1. 在 Visual Studio Code 文件资源管理器中是一个使用函数名称的子文件夹，同样，其中包含名为 *\_\_init\_\_.py*、*function.json* 和 *sample.dat* 的文件。

1. 替换 *\_\_init\_\_.py* 的内容，使之与以下代码匹配，此代码生成的字符串包含的值为 PI，其中的位数在 URL 中指定（此代码仅使用一个 URL 参数）

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. 由于代码仅支持 HTTP GET，请修改 *function.json*，使 `"methods"` 集合仅包含 `"get"`（即，删除 `"post"`）。 整个文件应如下所示：

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
            "get"
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

1. 启动调试程序，方法是：按 F5 或选择“调试”   >   “启动调试”菜单命令。 现在，“输出”窗口会显示项目中的两个终结点： 

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. 通过浏览器或 curl 向 `http://localhost:7071/api/DigitsOfPi?digits=125` 发出一个请求，然后观察输出。 （你可能会注意到，此代码算法不是很精确，但我们将改进的任务交给你了！）完成后，停止调试程序。

1. 使用  **Azure:Functions** 资源管理器中的“部署到函数应用”命令重新部署代码。 当系统提示时，请选择以前创建的函数应用。

1. 部署完成（需要数分钟！）后，“输出”窗口会显示可以用来重复你的测试的公共终结点。 

> [!div class="nextstepaction"]
> [我添加了另一个函数](tutorial-vs-code-serverless-python-07.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=06-second-function)
