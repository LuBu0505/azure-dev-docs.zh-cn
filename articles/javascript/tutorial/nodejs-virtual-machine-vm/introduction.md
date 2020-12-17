---
title: Azure CLI Linux 虚拟机
description: 创建 Azure Linux 虚拟机，并从 GitHub 存储库克隆一个基于 Express.js 的应用。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 674bf37acda9fcd9f6df7b84602600ad65ada3d9
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522127"
---
# <a name="1-create-linux-virtual-machine-with-expressjs-app-using-azure-cli"></a>1.使用 Azure CLI 创建 Linux 虚拟机以及 Express.js 应用

在本教程中，将为 Express.js 应用创建一个 Linux 虚拟机 (VM)。 VM 是使用 cloud-init 配置文件配置的，并且它包含 NGINX 以及 Express.js 应用的 GitHub 存储库。 VM 运行后，可使用 SSH 连接到 VM，将 Web 应用更改为包含跟踪日志记录，并在 Web 浏览器中查看公共 Express.js 服务器应用。

本教程包括以下任务：

* 使用 Azure CLI 登录到 Azure
* 使用 Azure CLI 创建 Azure Linux VM 资源
    * 打开公用端口 80
    * 从 GitHub 存储库安装演示版 Express.js Web 应用
    * 安装 Web 应用依赖项
    * 启动 Web 应用
* 使用 Azure CLI 创建 Azure 监视资源
    * 使用 SSH 连接到 VM
    * 使用 npm 安装 Azure SDK 客户端库
    * 添加 Application Insights 客户端库代码以创建自定义跟踪
* 从浏览器查看 Web 应用
    * 请求 `/trace` 路由以在 Application Insights 日志中生成自定义跟踪
    * 使用 Azure CLI 查看日志中收集的跟踪计数
    * 使用 Azure 门户查看跟踪列表
* 使用 Azure CLI 删除资源

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="prerequisites"></a>先决条件

- 通过 SSH 连接到 VM：使用新式终端，如 bash shell（其中包括 SSH）。
[!INCLUDE [Azure CLI](../../../includes/azure-cli-prepare-your-environment-no-header.md)]


## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [为 Azure 资源创建资源组](create-azure-monitoring-application-insights-web-resource.md) 