---
title: 在 Visual Studio Code 中将 Deno 应用部署到 Azure 应用服务
description: Deno 教程第 3 部分：部署网站
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: ceb55fb184582a4cb355c25f42c798713335b45c
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364851"
---
# <a name="deploy-deno-apps-to-azure"></a>将 Deno 应用部署到 Azure

[上一步：创建应用](tutorial-visual-studio-code-azure-app-service-deno-02.md)

在此步骤中，将使用 Azure CLI 将 Deno 应用部署到 Azure。

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

1. 使用以下命令创建名为 `deno-quickstart` 的资源组：

    ```bash
    az groups create -n deno-quickstart -l eastus
    ```

    如果决定更改资源组的名称，请确保在以下步骤中更新所有 `-g` 标志

1. 使用以下命令创建一个名为 `deno-plan` 的 AppService 计划，该计划将保存你的网站：

    ```bash
    az appservice plan create -g deno-quickstart -n deno-plan --is-linux
    ```

1. 接下来，创建 webapp 本身。 此命令将创建一个新的 AppService，并将其绑定到前面创建的计划。 将 `<your-app-name>` 标记更改为要为 Webapp 指定的名称，请记住，它需是唯一名称！

    ```bash
    az webapp create -n <your-app-name> -g deno-quickstart -p deno-plan -i anthonychu/azure-webapps-deno:1.0.2
    ```

    此 AppService 运行 Docker 映像，该映像提供用于运行 Deno 代码的基本功能。 完成此进程可能需要几秒钟时间。

1. 创建完成后，需要配置一些变量。 可以通过发出以下命令来执行此操作：

    ```bash
    az webapp config container set -n <your-app-name> -g deno-quickstart -i anthonychu/azure-webapps-deno:1.0.2 -r 'https://index.docker.io' -u '' -p  '' -t true && \
    az webapp config set -n <your-app-name> -g deno-quickstart --startup-file '' && \
    az webapp config appsettings set -n <your-app-name> -g deno-quickstart --settings WEBSITE_RUN_FROM_PACKAGE=1 WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
    ```

现在已配置了 AppService，它正在等待接收上一步中的应用。 但若要运行该应用，需要将其打包到 `.zip` 包中。 可通过以下步骤执行此操作：

1. 转到 `deno-demo` 文件夹

    ```bash
    cd deno-demo
    ```

1. 运行命令 `zip`：

    ```bash
    zip demo demo.ts
    ```

    此命令会在 `demo.ts` 文件所在的文件夹中生成一个名为 `demo.zip` 的文件。

1. 打包后，可以将文件上传到要执行的 AppService 中：

    ```bash
    az webapp deployment source config-zip -n <your-app-name> -g deno-quickstart --src ./demo.zip && \
    az webapp config set -n <your-app-name> -g deno-quickstart --startup-file 'deno run --allow-net demo.ts'
    ```

1. 通过转到 `https://<your-app-name>.azurewebsites.net` 测试应用程序

> [!div class="nextstepaction"]
> [我的站点位于 Azure 上](tutorial-visual-studio-code-azure-app-service-deno-04.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=deploy-app)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
