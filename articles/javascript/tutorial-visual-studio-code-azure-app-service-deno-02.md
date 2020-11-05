---
title: 在 Visual Studio Code 中创建演示 Azure 应用服务
description: Deno 教程第 2 部分：创建 Deno 应用并在本地运行
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: da5ed76459bdd9f00379e9f9c96873589886f8ed
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93233380"
---
# <a name="test-local-deno-apps"></a>测试本地 Deno 应用

[上一步：简介和先决条件](tutorial-visual-studio-code-azure-app-service-deno-01.md)

此步骤中，你将使用 Deno 的内置 web 服务器创建一个简单的 Deno api。 然后，在本地运行该应用。

## <a name="create-and-run-a-local-deno-app"></a>创建和运行本地 Deno 应用

1. 在终端或命令提示符下，导航到要在其中创建应用文件夹的位置，并创建名为 `deno-demo` 的新文件夹。

1. 创建名为 `demo.ts` 的新文件。
1. Deno 接受直接从 URL 运行代码。 编写使用“Hello World”应答所有请求的 HTTP 服务器。 使用以下代码：

    ```typescript
    import { serve } from "https://deno.land/std@0.54.0/http/server.ts"
    const handler = serve({ port: 80 })

    console.log("Serving at 80")

    for await (const req of handler) {
     req.respond({ body: "Hello World!\n" })
    }
    ```

1. 通过运行以下脚本执行应用：

    ```bash
    deno run --allow-net ./demo.ts
    ```

1. 通过打开浏览器访问 `http://localhost:80` 来测试应用。 站点应如下所示：

    ![运行演示服务器](media/deploy-azure/deno-hello-world.png)

    还可以通过键入 `deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts` 来运行此代码

1. 在终端中按 **Ctrl**+**C** 停止服务器。

> [!div class="nextstepaction"]
> [我创建了 Deno 应用](tutorial-visual-studio-code-azure-app-service-deno-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=create-app)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
