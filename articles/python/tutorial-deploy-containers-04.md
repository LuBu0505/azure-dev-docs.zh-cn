---
title: 步骤 4：将日志从用于容器的 Azure 应用服务流式传递到 Visual Studio Code 中
description: 教程第 4 部分，查看 Azure 应用服务中的日志以监视其行为。
ms.topic: conceptual
ms.date: 12/09/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: b4e380fa556f6cc806205e8fb446545328e35976
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522025"
---
# <a name="4-stream-logs-from-azure-app-service-for-a-container"></a>4：从用于容器的 Azure 应用服务流式传输日志

[上一步：进行更改并重新部署](tutorial-deploy-containers-03.md)

按照此过程操作可将容器的 Azure 应用服务中的日志流式传输到 Visual Studio Code。

在 VS Code 中，可以查看（或“跟踪”）Azure 应用服务上正在运行的站点的日志，该服务捕获从 `print` 语句到控制台的任何输出，并将其路由到 VS Code 的“输出”面板。 

1. 在“Azure:  应用服务”资源管理器中找到该应用，右键单击它，然后选择“开始流式传输日志”。 

1. 当系统提示你启用日志记录并重启应用时，请选择“是”作为答复。  重启应用后，VS Code 的“输出”面板将会打开，其中包含与日志流建立的连接。

1. 几秒钟后，你将在输出中看到一条消息，指出已连接到日志流服务：

    <pre>
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    </pre>

1. 在应用内导航，查看各种 HTTP 请求的其他输出。

> [!div class="nextstepaction"]
> [我看到了日志 - 转到步骤 5 >>>](tutorial-deploy-containers-05.md)

