---
title: 步骤 2：准备从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务的应用
description: 教程步骤 2，设置应用程序
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: b53b774cb7747b8f6cca10d493f820bef5306fcc
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441942"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2:对应用进行部署到 Azure 应用服务的准备

[上一步：先决条件](tutorial-deploy-app-service-on-linux-01.md)

本文介绍如何为本教程准备要部署到 Azure 应用服务的应用。 可以使用现有应用，也可以创建或下载应用。

如果已经有一个要使用的应用，请确保有一个 *requirements.txt* 文件，该文件介绍依赖项，包括 Flask 或 Django 之类的框架。

如果还没有应用，请使用以下选项之一。 确保验证该应用是否能够在本地运行。

## <a name="minimal-flask-app"></a>最小 Flask 应用

本部分介绍在此演练中使用的最小 Flask 应用。

1. 创建一个新文件夹，在 VS Code 中打开它，然后添加名为 *hello.py* 的文件，其内容如下。 应用对象有意命名为 `myapp`，目的是演示如何在应用服务的启动命令中使用这些名称，详见后面的介绍。

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. 创建一个具有以下内容的名为 *requirements.txt* 的文件：

    ```text
    Flask==1.1.1
    ```

1. 按照 [Flask 教程 - 为 Flask 创建项目环境](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask)中的说明操作，创建一个在其中安装了 Flask 的虚拟环境，以便在本地运行应用。

1. 设置 FLASK_APP 环境变量，该变量告知 Flask 在何处查找应用对象：

   # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    export FLASK_APP=hello:myapp
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```ps
    $env:FLASK_APP = "hello:myapp"
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```bash
    set FLASK_APP=hello:myapp
    ```

    ---

1. 运行应用：

    ```cmd
    flask run
    ```

1. 然后，你可以使用 URL `http://127.0.0.1:5000/` 在浏览器中打开应用。

## <a name="vs-code-flask-tutorial-sample"></a>VS Code Flask 教程示例

下载或克隆 [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial)，这是按 [Flask 教程](https://code.visualstudio.com/docs/python/tutorial-flask)操作后获得的结果。

## <a name="vs-code-django-tutorial-sample"></a>VS Code Django 教程示例

下载或克隆 [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial)，这是按 [Django 教程](https://code.visualstudio.com/docs/python/tutorial-django)操作后获得的结果。

如果 Django 应用使用本地 SQLite 数据库（例如此示例），则需在存储库中包括一个已预先初始化并预先填充的 *db.sqlite3* 文件副本。 这样做的原因是，目前，用于 Linux 的应用服务无法在部署过程中运行 Django 的 `migrate` 命令，因此必须部署预先准备的数据库。 即使到了那个时候，数据库也只能进行有效读取；向数据库写入内容还是会导致错误。

不管什么情况，最佳选择是使用单独的数据库，该数据库的部署和初始化独立于应用代码。

> [!div class="nextstepaction"]
> [我的应用已准备就绪 - 转到步骤 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)