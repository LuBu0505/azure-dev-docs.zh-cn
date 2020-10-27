---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: include 文件 run-site-locally.md
description: include 文件 run-site-locally.md
ms.openlocfilehash: 129546c7f6a845c335405c6fc615f907848138f6
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344096"
---
在本教程的这一部分中，将示例应用程序下载到本地计算机，并从 Visual Studio Code 终端运行它。 然后在浏览器中查看本地运行的应用。

## <a name="clone-and-run-the-initial-react-app"></a>克隆并运行初始 React 应用

已为你提供了完整的 React 应用。 在此过程中，克隆应用，安装依赖项并运行应用。 如果在代码中配置了 Azure 存储，则初始应用会尝试连接到 Azure 存储，如果不可用，则会显示消息 `Storage is not configured`。 

1. 打开 Visual Studio Code。
1. 通过选择 Git 图标并选择“克隆存储库”来克隆 GitHub 存储库。 此过程要求你登录到 GitHub。 使用 `https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob` 的存储库 URL，然后在本地计算机上选择一个文件夹将示例克隆到其中。 出现提示时，打开克隆的存储库。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/vscode-git-clone-repository.png" alt-text="通过选择 Git 图标，然后选择“克隆存储库”来克隆 GitHub 存储库。":::

1. 在 Visual Studio Code 中，打开终端窗口，然后运行以下命令以安装示例的依赖项。

    ```javascript
    npm install
    ```

1. 在同一终端窗口中，运行命令以运行 Web 应用。

    ```javascript
    npm start
    ```

1. 打开 Web 浏览器并使用以下 url 查看本地计算机上的 Web 应用。

    ```url
    http://localhost:3000/
    ```

    如果在浏览器中看到这一简单的 Web 应用，其中文本显示“未配置存储”，则本教程的此部分已成功完成。

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-no-azure-storage-resource-configured.png" alt-text="通过选择 Git 图标，然后选择“克隆存储库”来克隆 GitHub 存储库。":::

1. 在 Visual Studio Code 终端中使用 Control+C 停止代码。
