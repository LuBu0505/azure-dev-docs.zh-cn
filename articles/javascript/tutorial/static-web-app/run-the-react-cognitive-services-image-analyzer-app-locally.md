---
title: 下载示例 React 应用
description: GitHub 存储库中提供了完整的示例应用。 创建存储库的分支，安装依赖项，然后在本地运行。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5ddfd00be1c6160ec8c0c9630a8e950f13677bdd
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993426"
---
# <a name="2-download-and-run-the-react-cognitive-services-image-analyzer-app"></a>2.下载并运行 React 认知服务图像分析器应用

GitHub 存储库中提供了完整的示例应用。 创建存储库的分支，安装依赖项，然后在本地运行。

## <a name="fork-the-sample-repo"></a>创建示例存储库的分支

创建存储库的分支，而不是仅将其克隆到本地计算机，以便拥有自己的 GitHub 存储库，可以向其中推送更改。 

1. 打开单独的浏览器窗口或选项卡，然后登录到 <a href="https://github.com/login" target="_blank">GitHub</a>。 
1. 通过 Web 浏览器导航到 <a href="https://github.com/Azure-Samples/js-e2e-client-cognitive-services" target="_blank">GitHub 示例存储库</a>。 

    ```http
    https://github.com/Azure-Samples/js-e2e-client-cognitive-services
    ```

1. 在页面的右上部分，选择“分支”。 
1. 选择“代码”，然后复制分支的位置 URL。 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-clone-github-sample-repository-fork.png" alt-text="GitHub 网站的部分屏幕截图，选择“代码”，然后复制分支的位置。":::    

## <a name="create-local-development-environment"></a>创建本地开发环境

1. 在终端或 bash 窗口中，将分支克隆到本地计算机。 将 `YOUR-ACCOUNT-NAME` 替换为 GitHub 帐户名。

    ```bash
    git clone https://github.com/YOUR-ACCOUNT-NAME/js-e2e-client-cognitive-services
    ```

1. 更改为新目录并安装依赖项。 

    ```bash
    cd js-e2e-client-cognitive-services && npm install
    ```

## <a name="run-sample"></a>运行示例

1. 运行该示例。 

    ```bash
    npm start
    ```

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-cognitive-services-app-before-authentication.png" alt-text="用于在设置密钥和终结点之前进行图像分析的 React 认知服务计算机视觉示例的部分浏览器屏幕截图。":::    
    
1. 停止应用。 关闭终端窗口或在终端上使用 `control+c`。 
    
## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [创建计算机视觉资源并在代码中使用](create-computer-vision-resource-use-in-code.md) 