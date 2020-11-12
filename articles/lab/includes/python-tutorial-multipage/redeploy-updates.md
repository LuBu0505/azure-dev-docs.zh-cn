---
title: include 文件 azure-sign-in.md
description: include 文件 azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: e500a760430d92fa6640d964320a3ef3f161b7cc
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405116"
---
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

![在 Azure 中运行更新的示例 Python 应用](../../media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code 为 Python 和 Azure 应用服务提供了功能强大的扩展，简化了将 Python Web 应用部署到应用服务的过程。 有关详细信息，请参阅[将 Python 应用从 Visual Studio Code 部署到应用服务](/azure/python/tutorial-deploy-app-service-on-linux-01)。