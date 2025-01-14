---
title: 将 JavaScript 应用部署到 Azure
description: 托管选项和部署方案包括多个用于 Azure 的服务和工具。 在 Azure 上发布应用并为其提供服务。
ms.topic: how-to
ms.date: 12/09/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperf-fy21q2
ms.openlocfilehash: 9f9f28204abf8537aeda933083ca5802210b6c20
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522327"
---
# <a name="deploy-and-host-your-nodejs-apps-on-azure"></a>在 Azure 上部署和托管 Node.js 应用

托管选项和部署方案包括多个用于 Azure 的服务和工具。 Azure 有许多用于托管的选项和工具，有助于你将应用从本地或云存储库迁移到 Azure。 

## <a name="choose-a-recommended-azure-host-provider"></a>选择推荐的 Azure 托管提供商

在 Azure 上托管客户端、服务器或后台任务应用，有多种解决方案供你选择。 使用下表进行选择。 大多数用例的推荐解决方案是 [Azure 应用服务](/azure/app-service/overview)。 

有关不同托管选项的完整概述，请参阅 [Azure 计算服务的决策树](/azure/architecture/guide/technology-choices/compute-decision-tree)以及 Microsoft Learn 上的[核心云服务 - Azure 计算选项](/learn/modules/intro-to-azure-compute)模块。


 服务 |支持的应用类型| 建议用于 |
|--|--|--|
|[*应用服务](/azure/app-service/overview) - **推荐**|客户端、服务器、客户端/服务器、API、服务器呈现|通过代码或容器托管应用。 这使你可以管理 Web 服务器，而无需管理基础环境。|
|[（预览）Static Web Apps](/azure/static-web-apps/)|静态前端、预呈现、使用服务器 API 的静态前端|托管你的静态客户端应用（如 Angular、Vue、React）。 也可选择添加无服务器函数终结点来托管完整堆栈应用。 这个简单的服务抽象出了很多 Web 服务器的功能，让你可以专注于对客户端应用程序重要的功能。 |
|[函数](/azure/azure-functions/)|服务器 API|托管无服务器 API 终结点。 Azure 提供了许多称为触发器的模板来启动常见方案。|

## <a name="host-web-apps-with-more-control"></a>使用更多控制托管 Web 应用

以下选择为你提供了对应用程序环境的更多控制。 

| 服务 | 建议用于 |
|--|--|
|[虚拟机](/azure/virtual-machines) (VM)|完全控制 Windows 或 Linux VM。 [查找已背书的 Linux 分发版](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json)或[了解如何在 Azure 市场中查找](/azure/virtual-machines/linux/cli-ps-findimage) Linux VM 映像。|
|[容器实例](/azure/container-instances/)|快速设置单个容器。|
|[Kubernetes 服务](/azure/aks/)|多容器业务流程。|

## <a name="alternative-choices-for-web-app-hosting-on-azure"></a>在 Azure 上托管的 Web 应用的替代选项

这些选项是根据特定用例定制的。 

| 服务 | 建议用于 |
|--|--|
|[存储](/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-portal)|Azure 存储也可以托管静态 Web 应用。 如果需要在可靠存储和客户端应用程序之间进行紧密集成，可以使用此功能。|
|[内容分发网络](/azure/cdn/) (CDN)|提供预呈现的网站。 通过使用距离最近的接入点 (POP) 服务器来缓存从 Azure Blob 存储、Web 应用程序或任何可公开访问的 Web 服务器加载的静态对象。 Azure CDN 也可以通过利用各种网络和路由优化来加速不能缓存的动态内容。|

## <a name="deploy-your-web-app-to-azure"></a>将 Web 应用部署到 Azure

选择托管应用程序的服务后，请选择部署流程和工具。 将客户端和服务器应用部署到 Azure 服务意味着将一个文件或一组文件迁移到 Azure 以通过 HTTP 终结点提供服务。 

将文件移动到 Azure 云的常用方法如下：

| 方法 | 详细信息 |
|--|--|
|[GitHub Actions](/azure/app-service/deploy-github-actions?tabs=applevel)|对于自动或触发的连续部署使用此方法。|
|[Visual Studio Code 扩展](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)|对于手动、测试或很少的部署使用此方法。 要求在本地安装服务的扩展。|
|[Azure CLI](../tutorial-vscode-azure-cli-node-04.md)|对于手动或很少的部署使用此方法。 要求在本地安装服务的扩展。|

基于特定服务，可能存在其他部署方法。 例如，Azure 应用服务支持多种部署方法：
* [从 ZIP 文件](/azure/app-service/deploy-zip)
* [使用 FTP](/azure/app-service/deploy-ftp)
* [Dropbox 或 OneDrive](/azure/app-service/deploy-content-sync)
* [本地 Git](/azure/app-service/deploy-local-git)
* [cURL](/azure/app-service/deploy-zip#with-curl)
* [SSH](/azure/app-service/configure-linux-open-ssh-session)

## <a name="verify-your-deployment-with-your-http-endpoint"></a>使用 HTTP 终结点验证部署

若要验证部署，请访问 HTTP 终结点。 HTTP 终结点在“概览”页面上的所有服务上都可见。 

### <a name="view-http-endpoint-in-azure-portal"></a>在 Azure 门户中查看 HTTP 终结点

从 Azure 门户上服务的“概览”页面查看 HTTP 终结点。 

:::image type="content" source="../media/howto-deploy/azure-portal-hosting-url.png" alt-text="从 Azure 门户上服务的“概览”页面查看 HTTP 终结点。":::

## <a name="next-steps"></a>后续步骤

* [使用容器部署](deploy-containers.md)
