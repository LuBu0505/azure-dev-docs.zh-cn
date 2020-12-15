---
title: 将 Deno 应用从 Azure CLI 部署到 Azure 应用服务
description: 在本教程中，我们使用 Azure CLI 将 Deno 应用程序部署到（Linux 或 Windows 上的）Azure 应用服务。
ms.topic: tutorial
ms.date: 10/13/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: ba2e0a42b6d2dedd2192629562a8415a0d6d7167
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772604"
---
# <a name="deploy-deno-apps-to-azure-app-service-from-the-azure-cli"></a>将 Deno 应用从 Azure CLI 部署到 Azure 应用服务

使用 Azure CLI 将 Deno 应用程序部署到（Linux 或 Windows 上的）Azure 应用服务。 适用于 Azure 应用服务的 Deno 运行时随实验性 Docker 映像一起提供。 

![运行演示服务器](../media/deploy-azure/deno-hello-world.png)

## <a name="1-prepare-your-environment"></a>1.准备环境

- 具有活动订阅的 Azure 帐户。 [免费创建一个](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- 安装 [Visual Studio Code](https://code.visualstudio.com/)
- 安装 [Deno](https://deno.land/#installation)
- 在 bash 环境中使用 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)。

   [![嵌入式启动](https://shell.azure.com/images/launchcloudshell.png "启动 Azure Cloud Shell")](https://shell.azure.com)   
- 如果需要，请[安装](/cli/azure/install-azure-cli) Azure CLI 来运行 CLI 参考命令。
   - 如果使用的是本地安装，请通过 Azure CLI 使用 [az login](/cli/azure/reference-index#az-login) 命令登录。  若要完成身份验证过程，请遵循终端中显示的步骤。  有关其他登录选项，请参阅[使用 Azure CLI 登录](/cli/azure/authenticate-azure-cli)。
  - 出现提示时，请在首次使用时安装 Azure CLI 扩展。  有关扩展详细信息，请参阅[使用 Azure CLI 的扩展](/cli/azure/azure-cli-extensions-overview)。
  - 运行 [az version](/cli/azure/reference-index?#az_version) 以查找安装的版本和依赖库。 若要升级到最新版本，请运行 [az upgrade](/cli/azure/reference-index?#az_upgrade)。

## <a name="2-sign-in-to-azure-cli"></a>2.登录 Azure CLI

如果从本地命令行使用 Azure CLI，则在使用任何 CLI 命令之前，需要使用 [az login](/cli/azure/reference-index#az-login) 登录。

[!INCLUDE [interactive-login](../../azure-cli/includes/interactive-login.md)]

3. 登录后，你将看到与你的 Azure 帐户关联的订阅列表。 在使用 `isDefault: true` 的情况下显示的订阅信息是登录后当前已激活的订阅。 

4. 如需选择另一个订阅，请将 [az account set](/cli/azure/account#az-account-set) 命令与要切换到的订阅 ID 配合使用。 有关订阅选择的详细信息，请参阅[使用多个 Azure 订阅](/cli/azure/manage-azure-subscriptions-azure-cli)。

## <a name="3-create-local-deno-api-app"></a>3.创建本地 Deno API 应用

使用 Deno 的内置 Web 服务器创建 Deno 应用。 然后，在本地运行该应用。

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

    ![运行演示服务器](../media/deploy-azure/deno-hello-world.png)

    还可以通过键入 `deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts` 来运行此代码

1. 在终端中按 **Ctrl**+**C** 停止服务器。

## <a name="4-deploy-deno-app-to-azure"></a>4.将 Deno 应用部署到 Azure

使用 Azure CLI 将 Deno 应用部署到 Azure。

1. 使用以下命令创建名为 `deno-quickstart` 的资源组：

    ```azurecli
    az group create --name deno-quickstart --location eastus
    ```

    如果决定更改资源组的名称，请确保在以下步骤中更新所有 `-g` 标志

1. 使用以下命令创建一个名为 `deno-plan` 的 AppService 计划，该计划将保存你的网站：

    ```azurecli
    az appservice plan create --resource-group deno-quickstart --name deno-plan --is-linux
    ```

1. 接下来，创建 webapp 本身。 此命令将创建一个新的 AppService，并将其绑定到前面创建的计划。 将 `<your-app-name>` 标记更改为要为 Webapp 指定的名称，请记住，它需是唯一名称！

    ```azurecli
    az webapp create -n <your-app-name> --resource-group deno-quickstart -p deno-plan -i anthonychu/azure-webapps-deno:1.0.2
    ```

    此 AppService 运行 `anthonychu/azure-webapps-deno:1.0.2` Docker 映像，该映像提供用于运行 Deno 代码的基本功能。 完成此进程可能需要几秒钟时间。

## <a name="5-configure-the-azure-app-service-webapp"></a>5.配置 Azure 应用服务 webapp

1. 告诉 webapp 在何处获取实验性 Deno 映像名称的 Docker 容器映像：

    ```azurecli
    az webapp config container set --name <your-app-name> --resource-group deno-quickstart -i anthonychu/azure-webapps-deno:1.0.2 -r 'https://index.docker.io' -u '' -p  '' -t true
    ```

1. 将 Web 应用设置为不包含启动文件：

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file ''

1. Set the webapp to have runtime environment variables:

    ```azurecli
    az webapp config appsettings set --name <your-app-name> --resource-group deno-quickstart --settings WEBSITE_RUN_FROM_PACKAGE=1 WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
    ```

## <a name="6-configure-deno-app-deployment-to-web-app"></a>6.配置 Deno 应用到 Web 应用的部署 

Azure Web 应用已配置并且可供使用，但还没有 Deno 包。 用 `.zip` 包打包应用，告诉 Web 应用该文件在本地计算机上，然后设置 zip 文件中的启动文件。 

1. 转到 `deno-demo` 文件夹

    ```bash
    cd deno-demo
    ```

1. 运行命令 `zip`：

    ```bash
    zip demo demo.ts
    ```

    此命令会在 `demo.ts` 文件所在的文件夹中生成一个名为 `demo.zip` 的文件。

1. 将包配置为部署的代码源：

    ```azurecli
    az webapp deployment source config-zip --name <your-app-name> --resource-group deno-quickstart --src ./demo.zip
    ```

1. 配置包中的文件以运行：

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file 'deno run --allow-net demo.ts'
    ```

1. 通过转到 `https://<your-app-name>.azurewebsites.net` 测试应用程序。 

## <a name="7-clean-up-resources"></a>7.清理资源

使用以下命令删除资源组，此操作还会删除 Web 应用资源：

```azurecli
az group delete deno-quickstart
```

## <a name="next-steps"></a>后续步骤

了解有关以下方面的详细信息：
* 使用 Visual Studio Code 扩展[部署到应用服务](../tutorial-vscode-azure-app-service-node-01.md)
* [部署到虚拟机](./nodejs-virtual-machine-vm/introduction.md)
* [将 Deno 函数部署](https://github.com/anthonychu/azure-functions-deno-worker)为[自定义处理程序](/azure/azure-functions/functions-custom-handlers)