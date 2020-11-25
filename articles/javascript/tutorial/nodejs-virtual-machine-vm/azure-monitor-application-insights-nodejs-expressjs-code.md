---
title: 安装 Application Insights 客户端库
description: 将 Azure SDK 客户端库添加到虚拟机上的代码，以开始在 Azure 云中收集应用日志。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: a6fb29dd059922ead67b4cef5107c9407f40e294
ms.sourcegitcommit: ed749b136f0d6b876fd5866ba4a151c73af5b71f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674707"
---
# <a name="5-install-azure-sdk-client-library-to-monitor-web-app"></a>5.安装 Azure SDK 客户端库以监视 Web 应用

在此步骤中，将 Azure SDK 客户端库添加到虚拟机上的代码，以开始在 Azure 云中收集应用日志。

## <a name="edit-indexjs-for-logging-with-azure-monitor-application-insights"></a>使用 Azure Monitor Application Insights 编辑 index.js 以进行日志记录

1. 使用虚拟机中提供的 [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) 文本编辑器来编辑 `index.js`。 

    ```bash
    sudo nano index.js
    ```

1. 编辑 `index.js` 文件以添加客户端库和日志记录代码，如下所示。 很多 bash shell 允许直接复制并粘贴到 nano 中。 

    :::code language="JavaScript" source="~/../js-e2e-vm/index-logging.js" highlight="5-28" :::

1. 完成后，请按 `Control+x` 退出，然后按 `y` 保存更改。 对 Web 应用的更改通过 PM2 进行监视；此更改导致了应用重启，但无需重启 VM。 

1. 在 Web 浏览器中，使用新的 `trace` 路由测试应用：

    ```http
    http://REPLACE-WITH-YOUR-IP/trace
    ```

    浏览器将显示响应 `tracing...` 以及你的 IP 地址。

## <a name="viewing-the-vm-logs-for-nginx-and-pm2"></a>查看 NGINX 和 PM2 的 VM 日志

VM 将收集 NGINX 和 PM2 的日志，以供查看。

| 服务 | 日志位置|
|--|--|
|NGINX| /var/log/nginx/access.log|
|PM2| /var/log/pm2.log|

1. 查看 NGINX 代理服务的 VM 日志。 在同一 bash shell 中，使用以下命令查看日志：

    ```bash
     cat /var/log/nginx/access.log
    ```

    日志包含来自本地计算机的调用。 

    ```console
     "GET /trace HTTP/1.1" 200 10 "-"
    ```

1. 查看 PM2 服务的 VM 日志。 在同一 bash shell 中，使用以下命令查看日志：

    ```bash
     cat /var/log/pm2.log
    ```

    日志包含来自本地计算机的调用。 

    ```console
    Hello world app listening on port 3000!
    testing from trace route 76.22.73.183
    ```

1. 本教程不会再连接到 VM。 在 bash shell 中使用以下命令退出 SSH 连接。 

    ```bash
    exit
    ```

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [在 Azure 门户中查看日志](azure-monitor-application-insights-logs.md) 