---
title: 步骤 6：使用 VS Code 向 Azure Functions 添加另一个 Python 无服务器函数
description: 教程步骤 6，通过添加另一个无服务器函数扩展 Azure Functions 项目。
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 18cc5b138a46e4194c82bd0339c1566e20107347
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96441712"
---
# <a name="6-add-a-second-python-function-to-azure-functions"></a>6：向 Azure Functions 添加另一个 Python 函数

[上一步：部署到 Azure](tutorial-vs-code-serverless-python-05.md)

在第一次部署后，可以更改代码（例如添加其他 Python 函数），然后重新部署到同一 Azure Functions 应用。

1. 在“Azure：函数”  Functions”资源管理器中选择“创建函数”命令，  或者使用命令面板中的“Azure Functions:  创建函数”。 为函数指定以下详细信息：

    - 模板：HTTP 触发器
    - 姓名：“DigitsOfPi”
    - 授权级别：匿名

    Azure Functions 资源管理器中的“本地项目”部分现在显示一个 DigitsOfPi 函数。 在编辑器中，可以在函数的 \_\_init\_\_.py、function.json 和 sample.dat 文件之间切换  。

1. 替换 \_\_init\_\_.py 的内容，使之与以下代码匹配，此代码生成的字符串包含的值为 PI，其中的位数在 URL 中指定（此代码仅使用一个 URL 参数）：

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

        return output

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

1. 通过按 F5 或选择“运行” > “启动调试”菜单命令，启动本地调试程序 。 现在，“输出”窗口会显示项目中的两个终结点： 

    <pre>
    Http Functions:
            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi
            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    </pre>

1. 通过浏览器或 curl 向 `http://localhost:7071/api/DigitsOfPi?digits=125` 发出一个请求，然后观察输出。 （你可能会注意到，此代码的算法不是很精确，但我们将改进的任务交给你！）完成后，停止调试程序。

1. 使用  **Azure:Functions** 资源管理器中的“部署到函数应用”命令重新部署代码。 当系统提示时，请选择以前创建的函数应用。

1. 几分钟后，部署完成，“输出”窗口显示可用于重复测试的公共终结点。

> [!div class="nextstepaction"]
> [我添加了另一个函数 - 转到步骤 7 >>>](tutorial-vs-code-serverless-python-07.md)

[存在问题？请告诉我们。](https://aka.ms/python-functions-qs-ms-survey)
