---
title: 步骤 4：使用 VS Code 在本地调试 Azure Functions Python 代码
description: 教程步骤 4，在本地运行用于检查 Python 代码的 VS Code 调试程序。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 51ef666bd529194670279deaae51ee073633fd3a
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441592"
---
# <a name="4-debug-the-azure-functions-python-code-locally"></a>4：在本地调试 Azure Functions Python 代码

[上一步：检查代码文件](tutorial-vs-code-serverless-python-03.md)

可以在 Visual Studio Code 中本地调试 Azure Functions Python 代码。

1. 创建 Functions 项目时，Visual Studio Code 扩展也在 `.vscode/launch.json` 中创建启动配置，该文件包含名为“附加到 Python 含”的单个配置。  此配置意味着，若要启动项目，只需按 F5 或使用调试资源管理器：

    ![用于启动 Python 项目的调试浏览器的配置](media/tutorial-vs-code-serverless-python/configuration-to-start-a-python-project-for-debugging.png)

1. 启动调试程序时，会打开一个终端，显示来自 Azure Functions 的输出，其中包含可用终结点的摘要。 如果使用的名称不是“HttpExample”，则 URL 可能有所不同：

    <pre>
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    </pre>

1. 在 Visual Studio Code 的“输出”窗口  中对 URL 使用 **Ctrl+单击**或 **Cmd+单击** 即可将浏览器打开到该地址，也可启动浏览器后在其中粘贴同一 URL。 不管什么情况，终结点都是 `api/<function_name>`，在此示例中为 `api/HttpExample`。 但是，由于该 URL 不含名称参数，因此浏览器窗口会直接显示“请传递查询字符串或请求正文中的名称”，此名称与代码中的该路径相对应。

    > [!TIP]
    > 如果你无法访问该 URL 并在企业代理后运行（很可能已设置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量），可将名为 `NO_PROXY` 的环境变量设置为 `localhost,127.0.0.1`，然后重试。

1. 现在，请尝试添加一个要使用的名称参数，例如 `http://localhost:7071/api/HttpExample?name=VS%20Code`，然后浏览器窗口会显示“Hello Visual Studio Code!”消息，表示你已运行该代码路径。

1. 若要在 JSON 请求正文中传递名称值，可以使用 curl 之类的内联了 JSON 的工具：

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data '{"name":"Visual Studio Code"}' http://localhost:7071/api/HttpExample
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```powershell
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    在 PowerShell 中，还可以使用 [Invoke-WebRequest cmdlet](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-6)。

    ---

    也可创建 *data.json* 之类的包含 `{"name":"Visual Studio Code"}` 的文件，并使用 `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample` 命令。

1. 若要对函数调试功能进行测试，请在行中设置一个断点 (`name = req.params.get('name')`)，然后再次向该 URL 发出请求。 Visual Studio Code 调试程序会在该行中暂停，让你可以检查变量并对代码进行步进调试。 （若要快速了解基本的调试，请参阅 [Visual Studio Code 教程 - 配置和运行调试程序](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger)。）

1. 如果已在本地对函数进行详细的测试并对测试结果满意，请停止调试程序（使用“调试”   >   “停止调试”菜单命令，或者使用调试工具栏上的“断开连接”命令  ）。

> [!div class="nextstepaction"]
> [我在本地运行了调试程序 - 转到步骤 5 >>>](tutorial-vs-code-serverless-python-05.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)