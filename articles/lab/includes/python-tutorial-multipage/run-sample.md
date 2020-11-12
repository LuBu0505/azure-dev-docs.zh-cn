---
title: include 文件 azure-sign-in.md
description: include 文件 azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 6f9e5e8f30c5108996f14bef9dc4c90a668c60c1
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405128"
---
1. 确保位于 python-docs-hello-world 文件夹中。 

1. 创建虚拟环境并安装依赖项：

    [!include [virtual environment setup](../app-service-quickstart-python-venv.md)]

    如果遇到“[Errno 2] 没有此类文件或目录:“requirements.txt”。”，请确保位于 python-docs-hello-world 文件夹中。

1. 运行开发服务器。

    ```terminal  
    flask run
    ```
    
    默认情况下，该服务器假定应用的条目模块位于示例使用的 app.py 中。 （如果使用其他模块名称，请将 `FLASK_APP` 环境变量设置为该名称。）

1. 打开 Web 浏览器并转到 `http://localhost:5000/` 处的示例应用。 该应用显示“Hello, World!”消息。

    ![在本地运行示例 Python 应用](../../media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)
    
1. 在终端窗口中，按 Ctrl+C 退出开发服务器 。
