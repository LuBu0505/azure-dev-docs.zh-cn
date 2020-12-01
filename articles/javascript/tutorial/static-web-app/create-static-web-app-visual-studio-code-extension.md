---
title: 创建静态 Web 应用资源
description: 为相应服务创建带有 Visual Studio Code 扩展的静态 Web 应用资源。
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c6daa3f2a7ceb7f981cdaf57bdba61a722edbaa3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993433"
---
# <a name="4-create-azure-static-web-app-resource"></a>4.创建 Azure 静态 Web 应用资源

在本教程的此部分中，为相应服务创建带有 Visual Studio Code 扩展的静态 Web 应用资源，并将本地代码推送到远程存储库中以进行构建，然后将该应用部署到 Azure。

## <a name="create-a-new-branch-dedicated-to-deployment"></a>创建专用于部署的新分支

Azure 静态 Web 应用从 GitHub 存储库的特定分支接收构建。 目前，本教程使用 `main` 分支。 在 Visual Studio Code 的新终端中，创建仅用于构建和部署应用的 `live` 分支。

```bash
git checkout -b live
```

## <a name="push-the-live-branch-to-github"></a>将 live 分支推送到 GitHub

在 Visual Studio Code 终端中，将本地分支 `live` 推送到远程存储库。

```bash
git push origin live
```

## <a name="create-a-static-web-app-resource"></a>创建静态 Web 应用资源

1. 选择 Azure 图标，然后右键单击“Static Web Apps”服务，然后选择“创建静态 Web 应用…”  。 

    :::image type="content" source="../../media/static-web-app/visualstudiocode-storage-extension-create-static-web-resource.png" alt-text="带有 Visual Studio 扩展的 Visual Studio Code 屏幕截图":::

1. 在后续字段中输入以下信息，每次显示一个字段。 

    |字段名称| 值|
    |--|--|
    |静态 Web 应用的名称。|`Demo-ComputerVisionAnalyzer`|
    |为存储库选择分支|`live`| 
    |选择应用程序代码的位置。|`/`|
    |选择 Azure Functions 代码的位置。|选择“现在跳过”|
    |输入构建输出相对于应用的位置的路径。|`build`|
    |选择新资源的位置|选择靠近自己的 Azure 位置。|

## <a name="update-the-github-action-with-secret-environment-variables"></a>使用机密环境变量更新 GitHub 操作

计算机视觉密钥和终结点位于存储库的机密集合中，但还不在 GitHub 操作中。 此步骤会将密钥和终结点添加到操作中。

1. 下拉创建 Azure 资源所做的更改，以获取 GitHub 操作文件。

    ```bash
    git pull origin live
    ```

1. 在 Visual Studio Code 编辑器中，编辑在 `./.github/workflows/` 中找到的 GitHub 操作文件以添加机密。 

    :::code language="yml" source="~/../js-e2e-client-cognitive-services/.github/workflows/sample-github-workflow.yml" highlight="34-36" :::

    
1. 添加并提交对本地 `live` 分支所做的更改。

    ```bash
    git add . && git commit -m "add secrets to action"
    ```

1. 将更改推送到远程存储库，对 Azure 静态 Web 应用启动新的 build-and-deploy 操作。

    ```bash
    git push origin live
    ```

## <a name="view-the-github-action-build-process"></a>查看 GitHub 操作构建过程

1. 在 Web 浏览器中，打开用于本教程的 GitHub 存储库，然后选择“操作”。 

1. 在列表中选择顶部的构建，然后在左侧菜单上选择“构建并部署作业”来监视构建过程。 等待“构建并部署”成功完成。

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-action-build-react-computer-vision-app.png" alt-text="在列表中选择顶部的构建，然后在左侧菜单上选择“构建并部署作业”来监视构建过程。等待构建成功完成。":::

## <a name="view-azure-static-web-site-in-browser"></a>在浏览器中查看 Azure 静态网站

1. 在 Visual Studio Code 中，选择最右侧菜单中的 Azure 图标，选择静态 Web 应用，右键单击“浏览站点”，然后选择“打开”查看公共静态网站  。 

:::image type="content" source="../../media/static-web-app/visualstudiocode-browse-static-web-app.png" alt-text="选择“浏览站点”，然后选择“打开”以查看公共静态网站。":::

还可以在以下位置找到该站点的 URL：
* “概述”页上的资源的 Azure 门户。
* GitHub 操作的 build-and-deploy 输出在脚本的末尾包含该站点的 URL 

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [查看 React 代码和认知服务计算机视觉分析](add-computer-vision-react-app.md)