---
title: 快速入门：在 Linux 上创建 Python 应用
description: 将第一个 Python 应用部署到 Azure 应用服务中的 Linux 容器即可开始使用 Azure 应用服务。
ms.topic: quickstart
ms.date: 09/22/2020
ms.custom: seo-python-october2019, cli-validate, devx-track-python, devx-track-azurecli
robots: noindex
ms.openlocfilehash: a37ad0969308ace65a4654cc4244d280abd286ea
ms.sourcegitcommit: 12f80b1e0fe08db707c198271d0c399c3aba343a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515208"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service"></a>快速入门：在 Azure 应用服务中创建 Python 应用 

在本快速入门中，需将 Python Web 应用部署到 [Linux 上的应用服务](/azure/app-service/overview#app-service-on-linux)，该版本提供了一项高度可缩放、自我修补的 Azure Web 托管服务。 在 Mac、Linux 或 Windows 计算机上，可使用本地 [Azure 命令行界面 (CLI)](/cli/azure/install-azure-cli) 通过 Flask 或 Django 框架来部署示例。 配置的 Web 应用使用免费的应用服务层，因此本文中的操作不会产生任何费用。

> [!TIP]
> 如果更喜欢使用 Visual Studio Code，请按照 [Visual Studio Code 应用服务快速入门](/azure/developer/python/tutorial-deploy-app-service-on-linux-01)进行操作。

<details>
<summary >1.设置初始环境</summary>

<a name="set-up"></a>

1. 具有活动订阅的 Azure 帐户。 [免费创建帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
1. 安装 <a href="https://www.python.org/downloads/" target="_blank">Python 3.6 或更高版本</a>。
1. 安装 <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.0.80 或更高版本，使用它可以在任何 shell 中运行命令来预配和配置 Azure 资源。

打开终端窗口并检查 Python 版本是否为 3.6 或更高版本：

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```cmd
py -3 --version
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 --version
```

---

检查 Azure CLI 版本是否为 2.0.80 或更高版本：

```azurecli
az --version
```

然后通过 CLI 登录到 Azure：

```azurecli
az login
```

此命令将打开浏览器以获取凭据。 命令完成后，会显示包含订阅相关信息的 JSON 输出。

登录后，可以使用 Azure CLI 运行 Azure 命令，处理订阅中的资源。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)

</details>

<details>
<summary>2.克隆示例</summary>

使用以下命令克隆示例存储库，并导航到示例文件夹。 （如果尚未安装 git，请[安装 git](https://git-scm.com/downloads)。）

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

然后导航到该文件夹：

```terminal
cd python-docs-hello-world
```

示例包含 Azure 应用服务在启动应用时可以识别的框架特定代码。 有关详细信息，请参阅[容器启动过程](/azure/app-service/configure-language-python#container-startup-process)。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)

</details>

<details>
<summary>3.运行示例</summary>

1. 确保位于 python-docs-hello-world 文件夹中。 

1. 创建虚拟环境并安装依赖项：

    [!include [virtual environment setup](includes/app-service-quickstart-python-venv.md)]

    如果遇到“[Errno 2] 没有此类文件或目录:“requirements.txt”。”，请确保位于 python-docs-hello-world 文件夹中。

1. 运行开发服务器。

    ```terminal  
    flask run
    ```
    
    默认情况下，该服务器假定应用的条目模块位于示例使用的 app.py 中。 （如果使用其他模块名称，请将 `FLASK_APP` 环境变量设置为该名称。）

1. 打开 Web 浏览器并转到 `http://localhost:5000/` 处的示例应用。 该应用显示“Hello, World!”消息。

    ![在本地运行示例 Python 应用](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)
    
1. 在终端窗口中，按 Ctrl+C 退出开发服务器 。


[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)

</details>

<details>
<summary>4.部署示例</summary>

使用 `az webapp up` 命令在本地文件夹 (python-docs-hello-world) 中部署代码：

```azurecli
az webapp up --sku F1 --name <app-name>
```

- 如果无法识别 `az` 命令，请确保按照[设置初始环境](#set-up)中所述安装 Azure CLI。
- 如果无法识别 `webapp` 命令，请确保 Azure CLI 版本为 2.0.80 或更高版本。 如果不是，请[安装最新版本](/cli/azure/install-azure-cli)。
- 将 `<app_name>` 替换为在整个 Azure 中均唯一的名称（有效字符为 `a-z`、`0-9` 和 `-`）。 良好的模式是结合使用公司名称和应用标识符。
- `--sku F1` 参数在“免费”定价层上创建 Web 应用。 省略此参数可使用更快的高级层，这会按小时计费。
- 可以选择包含参数 `--location <location-name>`，其中 `<location_name>` 是可用的 Azure 区域。 可以运行 [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations) 命令来检索 Azure 帐户的允许区域列表。
- 如果看到错误“无法自动检测应用的运行时堆栈”，请确保在包含 requirements.txt 文件的 python-docs-hello-world 文件夹 (Flask) 或 python-docs-hello-django 文件夹 (Django) 中运行该命令 。 （请参阅[通过 az webapp up 解决自动检测问题](https://github.com/Azure/app-service-linux-docs/blob/master/AzWebAppUP/runtime_detection.md) (GitHub)。）

此命令可能需要花费几分钟时间完成。 运行时，它提供以下相关信息：创建资源组、应用服务计划和托管应用，配置日志记录，然后执行 ZIP 部署。 然后，它将显示消息“可以在 http://&lt;app-name&gt;.azurewebsites.net（这是 Azure 上应用的 URL）启动应用”。

![az webapp up 命令的示例输出](./media/quickstart-python/az-webapp-up-output.png)

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)

[!include [az webapp up command note](includes/app-service-web-az-webapp-up-note.md)]
</details>

<details>
<summary>5.浏览到应用</summary>

在 Web 浏览器中使用以下 URL 浏览到已部署的应用程序：`http://<app-name>.azurewebsites.net`。 最初启动应用需要几分钟时间。

Python 示例代码在使用内置映像的应用服务中运行 Linux 容器。

![在 Azure 中运行示例 Python 应用](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

祝贺你！ 现已将 Python 应用部署到应用服务。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)
</details>

<details>
<summary>6.重新部署更新</summary>

在本部分中，你将对代码进行少量更改，然后将代码重新部署到 Azure。 代码更改包括 `print` 语句，用于生成要在下一部分使用的日志记录输出。

在编辑器中打开 app.py，并更新 `hello` 函数以匹配以下代码。 

```python
def hello():
    print("Handling request to home page.")
    return "Hello, Azure!"
```

    
保存更改，然后再次使用 `az webapp up` 命令重新部署应用：

```azurecli
az webapp up
```

此命令使用本地缓存在 .azure/config 文件中的值，包括应用名称、资源组和应用服务计划。

部署完成后，切换回打开到 `http://<app-name>.azurewebsites.net` 的浏览器窗口。 刷新页面，刷新后的页面应显示修改后的消息：

![在 Azure 中运行更新的示例 Python 应用](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)

> [!TIP]
> Visual Studio Code 为 Python 和 Azure 应用服务提供了功能强大的扩展，简化了将 Python Web 应用部署到应用服务的过程。 有关详细信息，请参阅[将 Python 应用从 Visual Studio Code 部署到应用服务](/azure/python/tutorial-deploy-app-service-on-linux-01)。
</details>

<details>
<summary>7.流式传输日志</summary>

可以访问应用内和运行应用的容器所生成的控制台日志。 这些日志包括使用 `print` 语句生成的任何输出。

若要流式传输日志，请运行 [az webapp log tail](/cli/azure/webapp/log?view=azure-cli-latest&preserve-view=true#az_webapp_log_tail) 命令：

```azurecli
az webapp log tail
```

还可以在 `az webapp up` 命令中包含 `--logs` 参数，以在部署时自动打开日志流。

在浏览器中刷新应用以生成控制台日志，其中包括描述对应用的 HTTP 请求的消息。 如果未立即显示输出，请在 30 秒后重试。

也可通过浏览器在 `https://<app-name>.scm.azurewebsites.net/api/logs/docker` 中检查日志文件。

若要随时停止日志流式处理，请在终端中按 Ctrl+C。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)
</details>

<details>
<summary>8.管理 Azure 应用</summary>

转到 <a href="https://portal.azure.com" target="_blank">Azure 门户</a>管理已创建的应用。 搜索并选择“应用服务”。

![在 Azure 门户中导航到应用服务](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

选择 Azure 应用名称。

![在 Azure 门户的应用程序服务中导航到 Python 应用](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

选择该应用将打开其“概述”页，你可以在其中执行基本的管理任务，例如浏览、停止、启动、重启和删除。

![在 Azure 门户的“概述”页中管理 Python 应用](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

应用服务菜单提供了用于配置应用的不同页面。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)
</details>

<details>
<summary>9.清理资源</summary>

在前面的步骤中，你在资源组中创建了 Azure 资源。 资源组根据位置得名，例如“appsvc_rg_Linux_CentralUS”。 如果使用 F1 免费层以外的应用服务 SKU，这些资源会持续产生费用（请参阅[应用服务定价](https://azure.microsoft.com/pricing/details/app-service/linux/)）。

如果你认为将来不再需要这些资源，请运行以下命令删除资源组：

```azurecli
az group delete --no-wait
```

此命令使用 .azure/config 文件中缓存的资源组名称。

`--no-wait` 参数允许此命令在操作完成之前返回。

[存在问题？请告诉我们。](https://aka.ms/FlaskCLIQuickstartHelp)
</details>

<details>
<summary>后续步骤</summary>

> [!div class="nextstepaction"]
> [配置 Python 应用](/azure/app-service/configure-language-python)

> [!div class="nextstepaction"]
> [将用户登录添加到 Python Web 应用](/azure/active-directory/develop/quickstart-v2-python-webapp)

> [!div class="nextstepaction"]
> [教程：在自定义容器中运行 Python 应用](/azure/app-service/tutorial-custom-container)
</details>