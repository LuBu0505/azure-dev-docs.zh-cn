---
title: 通过 Azure CLI 创建用于托管应用的 Azure 应用服务
description: 教程第 3 部分：创建应用服务
ms.topic: conceptual
ms.date: 09/24/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: d48ee9ba1b1ddb813b0dd7f0f48da52e0cf96e15
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88217936"
---
# <a name="create-the-app-service"></a>创建应用服务

[上一步：创建应用](tutorial-vscode-azure-cli-node-02.md)

这一步通过 Azure CLI 创建用于托管应用代码的 Azure 应用服务。

1. 在终端或命令提示符处，使用以下命令创建应用服务的**资源组**。 资源组实际上是 Azure 中应用资源的命名集合，例如网站、数据库、Azure Functions 等。

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    以上命令在 `westus` 数据中心创建名为 `myResourceGroup` 的资源组。 可以根据需要更改这些值。

    该命令成功运行以后，会显示包含资源组详细信息的 JSON 输出。

1. 运行以下命令，为后续命令设置默认资源组和区域。 这样做就不需每次都指定这些值。 （此命令在成功运行后没有输出。）

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

1. 运行以下命令，创建一个**应用服务计划**，用于定义应用服务所使用的底层虚拟机：

    ```azurecli
    az appservice plan create --name myPlan --sku F1
    ```

    以上命令指定一个使用共享虚拟机的免费托管计划 (`--sku F1`)，并将计划命名为 `myPlan`。 同样，该命令在成功运行后会显示 JSON 输出。

1. 运行以下命令来创建应用服务，将 `<your_app_name>` 替换为会成为 URL (`http://<your_app_name>.azurewebsites.net`) 的唯一名称。 请注意，PowerShell 命令稍有不同。 `--runtime "node|6.9"` 参数告知 Azure 在服务器上使用 Node 版本 6.9.x。

    ```azurecli
    az webapp create --name <your_app_name> --plan myPlan --runtime "node|6.9"
    ```

    > [!TIP]
    > 也可在 `package.json` 中声明所需的 Node 版本。 Azure 在部署过程中应用此设置。 例如，以下 `package.json` 条目告知 Azure 使用至少 7.0.0 版的 Node：
    >
    > ``` json
    > "engines": {
    >     "node": ">7.0.0"
    > },
    > ```

1. 运行以下命令，将浏览器打开到新创建的应用服务。同样，将 `<your_app_name>` 替换为你所使用的名称：

    ```azurecli
    az webapp browse --name <your_app_name>
    ```

1. 由于尚未为应用部署任何自定义代码，因此浏览器会显示默认页：

    ![默认应用服务页](media/azure-cli/azure-default-page.png)

> [!div class="nextstepaction"]
> [我创建了应用服务](tutorial-vscode-azure-cli-node-04.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
