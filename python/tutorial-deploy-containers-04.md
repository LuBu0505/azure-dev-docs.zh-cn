---
title: 教程：将日志从用于容器的 Azure 应用服务流式传递到 Visual Studio Code 中
description: 教程第 4 部分，查看 Azure 应用服务中的日志以监视其行为。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0f002444d2454b734821e067e65fa513619a68bf
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172223"
---
# <a name="tutorial-stream-logs-from-azure-app-service-for-a-container"></a>教程：从用于容器的 Azure 应用服务流式传输日志

[上一步：进行更改并重新部署](tutorial-deploy-containers-03.md)

在 VS Code 中，可以查看（或“跟踪”）Azure 应用服务上正在运行的站点的日志，该服务捕获从 `print` 语句到控制台的任何输出，并将其路由到 VS Code 的“输出”面板。 

1. 在“Azure:  应用服务”资源管理器中找到该应用，右键单击它，然后选择“开始流式传输日志”。 

1. 当系统提示你启用日志记录并重启应用时，请选择“是”作为答复。  重启应用后，VS Code 的“输出”面板将会打开，其中包含与日志流建立的连接。

1. 几秒钟后，你将看到一条消息，指出已连接到日志流服务。

    ```bash
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    ```

1. 在应用内导航，查看各种 HTTP 请求的其他输出。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-deploy-containers-05.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=04-stream-logs)
