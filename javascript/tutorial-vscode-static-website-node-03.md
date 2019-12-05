---
title: 从 Visual Studio Code 为静态 Node.js 网站创建 Azure 存储帐户
description: 教程第 3 部分：创建 Azure 存储帐户
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 42badfc649d7cc43eb1a58ab20c8ff639eff5354
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466502"
---
# <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

[上一步：创建应用](tutorial-vscode-static-website-node-02.md)

在此步骤中，将创建一个 Azure 存储帐户，该帐户使用内置 Web 服务器充当简单的文件存储（或 CDN）。 此内置服务器使 Azure 存储成为快速托管静态站点的理想选择。

1. 从上一步中创建的 `my-static-app` 文件夹中，启动 Visual Studio Code，以便它自动打开该文件夹：

    ```bash
    code .
    ```

1. 在 VS Code 中，选择 Azure 徽标以打开 **Azure** 资源管理器。 在 **Azure 存储**下，右键单击 Azure 订阅，然后选择“创建存储帐户”  ：

    ![在 VS Code 中创建存储帐户](media/static-website/create-storage-account.png)

1. 在提示“输入新存储帐户的名称”时，为存储帐户输入全局唯一的名称，然后按 Enter。 应用名称的有效字符包括“a-z”和“0-9”。

1. 在提示“选择资源组”时，选择“创建新的资源组”  并接受默认名称。

1. 在提示“选择位置”时，选择附近的[区域](https://azure.microsoft.com/regions/)。

1. 创建存储帐户后，进度将显示在 VS Code 的“输出”  面板中：

1. 完成存储帐户后，右键单击该帐户并选择“配置静态网站”  。 启用静态网站托管意味着 Azure 存储会自动为索引文档和任何其他静态资产提供服务。

    ![创建存储帐户](media/static-website/configure-static-website.png)

1. 出现提示时，输入 *index.html* 作为索引文档名称和 404 错误文档名称。 我们将 *index.html* 用于错误文档，因为诸如 React、Angular和 Vue 之类的新式单页应用 (SPA) 可处理客户端中的错误。 对于经典静态网站，请使用自定义 404 错误页。

> [!div class="nextstepaction"]
> [我创建了一个存储容器](tutorial-vscode-static-website-node-04.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
