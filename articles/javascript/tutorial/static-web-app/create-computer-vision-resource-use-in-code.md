---
title: 创建计算机视觉资源
description: 创建认知服务计算机视觉资源，并将其设置为环境变量。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 4ac324171f47ab8795169c5dd453d1e6451e8906
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993390"
---
# <a name="3-create-computer-vision-resource-and-use-in-code"></a>3.创建计算机视觉资源并在代码中使用

在此步骤中，创建计算机视觉资源并将其设置为环境变量。 

## <a name="create-azure-resources"></a>创建 Azure 资源

创建资源组使你能够轻松查找资源，并可在完成查找后将其删除。

在这一系列步骤的最后，需要拥有资源的密钥和终结点。

1. 在终端或 bash shell 中输入 [Azure CLI 命令以创建 Azure 资源组](/cli/azure/group?view=azure-cli-latest#az_group_create)，并将其命名为 `rg-demo`：

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo 
    ```
1. 运行以下命令以[创建计算机视觉资源](/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create)：


    ```azurecli
    az cognitiveservices account create \
        --name demo-ComputerVision \
        --resource-group rg-demo \
        --kind ComputerVision \
        --sku F0 \
        --location eastus \
        --yes
    ```

1. 在结果中，查找并复制 `properties.endpoint`。 之后需要此 ID。

    ```json
    ...
    "properties":{
        ...
        "endpoint": "https://eastus.api.cognitive.microsoft.com/",
        ...
    }
    ...
    ```

1. 运行以下[命令](/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list)以获取密钥。 

    ```azurecli
    az cognitiveservices account keys list \
    --name demo-ComputerVision \
    --resource-group rg-demo
    ```

1. 复制其中一个密钥，稍后将需要用到它。

    ```json
    {
      "key1": "8eb7f878bdce4e96b26c89b2b8d05319",
      "key2": "c2067cea18254bdda71c8ba6428c1e1a"
    }
    ```

## <a name="add-environment-variables-to-your-local-environment"></a>将环境变量添加到本地环境

若要使用资源，本地代码需要具有可用的密钥和终结点。 此代码库将它们存储在环境变量中：

* REACT_APP_COMPUTERVISIONKEY
* REACT_APP_COMPUTERVISIONENDPOINT 

1. 运行以下命令，将这些变量添加到环境中。

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    export REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    export REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    
    # <a name="cmd"></a>[cmd](#tab/cmd)
    
    ```cmd
    set REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    set REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    ---

## <a name="add-environment-variables-to-your-remote-environment"></a>将环境变量添加到远程环境

使用 Azure 静态 Web 应用时，需要将环境变量（例如机密）从 GitHub 操作传递到静态 Web 应用。 GitHub 操作会构建应用，包括从相应存储库的 GitHub 机密传入的计算机视觉密钥和终结点，然后将具有环境变量的代码推送到静态 Web 应用。

1. 在 Web 浏览器中的 GitHub 存储库上，依次选择“设置”、“机密”、“新建存储库机密”  。

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-create-new-repository-secret.png" alt-text="用于在设置密钥和终结点之前进行图像分析的 React 认知服务计算机视觉示例的部分浏览器屏幕截图。":::

1. 为在上一部分中使用的终结点输入相同的名称和值。 然后，使用在上一部分中使用的相同密钥名称和值创建另一个机密。 
    
    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-add-secret.png" alt-text="为终结点输入相同的名称和值。然后，创建另一个具有相同密钥名称和值的机密。":::

## <a name="run-react-app-with-computervision-resource"></a>使用计算机视觉资源运行 React 应用

此 React 应用会监视更改，以重新生成和重新运行应用。 若要强制进行重新生成，请进行更改。

1. 在第一个空行（第 4 行）后面的 `./src/VisualAi.js` 中输入新行。 此更改将导致重新生成在本地运行的网站。

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-start-up.png" alt-text="已准备好使用 URL 或按 Enter 的 React 认知服务计算机视觉示例的部分浏览器屏幕截图。":::

1. 将文本字段留空，然后选择“分析”按钮。 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-image-analysis-result.png" alt-text="React 认知服务计算机视觉示例结果的部分浏览器屏幕截图。":::

    此图像是从 `./src/DefaultImages.js` 中定义的图像目录中随机选择的。 

1. 继续选择“分析”按钮，查看其他图像和结果。 

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [创建 Azure 静态 Web 应用](create-static-web-app-visual-studio-code-extension.md)