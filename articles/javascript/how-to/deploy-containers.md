---
title: 将 Node.js 容器部署到 Azure
description: 使用 Docker 容器将 Node.js 应用部署到 Azure
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 6bf8acb66a708433966bdfe90cc358a56d2d3b42
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857805"
---
# <a name="deploy-nodejs-container-to-azure"></a>将 Node.js 容器部署到 Azure 

使用容器构建应用是实现可伸缩性的常见模式。 Azure 提供了多种容器部署方式。

## <a name="host-your-container-app-on-azure"></a>在 Azure 上托管容器应用

通过以下托管选项可以部署容器化应用程序。

| 服务 | 建议用于 |
|--|--|
|[应用服务](/azure/app-service/quickstart-custom-container?pivots=container-linux)|在 Azure 应用服务上部署并运行自定义容器。|
|[容器实例](/azure/container-instances/)|快速设置单个容器。|
|[容器注册表](/azure/container-registry/)|生成、存储和管理自定义或专用容器映像。|
|[Kubernetes 服务](/azure/aks/)|多容器业务流程。|
|[虚拟机](/azure/virtual-machines) (VM)|完全控制 Windows 或 Linux VM。 [查找已背书的 Linux 分发版](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json)或[了解如何在 Azure 市场中查找](/azure/virtual-machines/linux/cli-ps-findimage) Linux VM 映像。|

## <a name="build-containerize-and-deploy-app-to-azure"></a>生成、容器化应用并将其部署到 Azure

若要开始操作，请按照本[教程](develop-nodejs-on-azure.md)了解如何：

* 下载示例代码
* 运行 Node.js 应用
* 在 Visual Studio Code 中调试应用
* 容器化 Node.js MEAN 应用
* 使用 Azure CLI 命令部署应用
* 在 CosmosDB 资源上创建 MongoDB 服务器
* 将容器映像添加到专用容器注册表
* 向 Web 应用添加自定义域名
* 将 Web 应用横向扩展到更大规模
* 创建和删除所有资源的资源组

## <a name="next-steps"></a>后续步骤

Microsoft Learn 模块：

- [使用 Azure 容器实例运行 Docker 容器](/learn/modules/run-docker-with-azure-container-instances/)

- [使用 Azure 容器注册表生成并存储容器映像](/learn/modules/build-and-store-container-images/)
