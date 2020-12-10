---
title: 步骤 2：准备从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务的应用
description: 教程步骤 2，设置应用程序
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 7197f8afc28bd62e7247c3955c888199ee69c509
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559191"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2:对应用进行部署到 Azure 应用服务的准备

[上一步：配置环境](tutorial-deploy-app-service-on-linux-01.md)

本文介绍如何为本教程准备要部署到 Azure 应用服务的应用。 可以使用现有应用，也可以创建或下载应用。

## <a name="if-you-already-have-an-app"></a>如果已有应用

如果已有要使用的应用，请确保项目根目录中包含一个 requirements.txt 文件，其中列出了依赖项，包括 Flask 或 Django 等框架。 你可以使用自己选择的任何框架。

> [!div class="nextstepaction"]
> [我自己的应用已准备就绪 - 转到步骤 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

## <a name="if-you-dont-already-have-an-app"></a>如果还没有应用

如果还没有应用，请使用以下选项之一。 确保验证该应用是否能够在本地运行。

本教程的其余部分使用[选项 3](#option-3-create-a-minimal-flask-app) 中显示的代码。

### <a name="option-1-use-the-vs-code-flask-tutorial-sample"></a>选项 1：使用 VS Code Flask 教程示例

下载或克隆 [https://github.com/Microsoft/python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial)，这是按 [Flask 教程](https://code.visualstudio.com/docs/python/tutorial-flask)操作后获得的结果。 请注意，应用代码位于“hello_app”文件夹中。 查看示例的 readme.md 文件，获取本地运行应用的说明。

### <a name="option-2-use-the-vs-code-django-tutorial-sample"></a>选项 2：使用 VS Code Django 教程示例

下载或克隆 [https://github.com/Microsoft/python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial)，这是按 [Django 教程](https://code.visualstudio.com/docs/python/tutorial-django)操作后获得的结果。

理想情况下，部署到云的 Django 应用同样使用基于云的数据库，例如 PostgreSQL for Azure。 有关详细信息，请参阅[教程：通过 Azure 门户使用 PostgreSQL 部署 Django Web 应用](tutorial-python-postgresql-app-portal.md)。

如果 Django 应用使用类似本示例的本地 SQLite 数据库，则本教程中最简单的方法是在存储库中包含 db.sqlite3 文件的预初始化且预填充的副本。 否则，需要配置生成后命令，以便在部署应用的容器中运行 Django 的 `migrate` 命令。 有关详细信息，请参阅[应用服务配置 - 自定义生成自动化](/azure/app-service/configure-language-python#customize-build-automation)。

### <a name="option-3-create-a-minimal-flask-app"></a>选项 3：创建最小 Flask 应用

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
    Flask
    ```

1. 使用菜单命令 Terminal > New Terminal，打开一个终端。

1. 在该终端中，创建并激活名为 `.venv` 的虚拟环境。 

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux)

    ```bash
    sudo apt-get install python3-venv    # If needed
    python3 -m venv .venv
    source .venv/bin/activate
    ```

    # <a name="windows"></a>[Windows](#tab/windows)

    ```cmd
    py -3 -m venv .venv
    .venv\scripts\activate
    ```

    ---

1. 当 VS Code 提示你激活新创建的环境时，请回答“是”。

1. 安装应用依赖项：

    ```cmd
    pip install -r requirements.txt
    ```

1. 设置 FLASK_APP 环境变量，该变量告知 Flask 在何处查找应用对象：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set FLASK_APP=hello:myapp
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```ps
    $env:FLASK_APP = "hello:myapp"
    ```

   # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    export FLASK_APP=hello:myapp
    ```

    ---

1. 运行应用：

    ```cmd
    flask run
    ```

1. 使用 URL `http://127.0.0.1:5000/` 在浏览器中打开应用。 在适用于 Linux 的 Azure 应用服务上应会显示消息“Hello Flask”。

1. 通过在终端中按 Ctrl + C 来停止 Flask 服务器 。

> [!div class="nextstepaction"]
> [我的应用已准备就绪 - 转到步骤 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

[存在问题？请告诉我们。](https://aka.ms/FlaskVSCQuickstartHelp)
