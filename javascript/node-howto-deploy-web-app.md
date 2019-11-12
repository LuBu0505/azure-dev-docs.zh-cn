---
title: 将 Node.js Web 应用部署到 Azure
description: 开始使用 Azure 应用服务和其他用于托管 Web 应用（包括渐进式 Web 应用 (PWA)）的选项
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 07112f988856df2598d1f336c779a3982f9342c1
ms.sourcegitcommit: 7e5392a0af419c650225cfaa10215d1e0e56ce71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568183"
---
# <a name="deploy-nodejs-web-apps-to-azure-app-service"></a>将 Node.js Web 应用部署到 Azure 应用服务

在 Azure 上，有下面几个用于部署和托管 Web 应用的选项：

- 用于托管 Web 应用的最佳选项是 Azure 应用服务，这是一种平台即服务 (PaaS) 产品。 若要开始，请试用以下资源之一：

  - [在 Azure 中创建 Node.js Web 应用](/azure/app-service/app-service-web-get-started-nodejs)
  - [试用 Azure 应用服务 - 从模板创建 Express 应用](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [使用 Azure 应用服务托管 Web 应用 - Learn 模块](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [在 Azure 中生成 Node.js 和 MongoDB 应用](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [应用服务示例](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- 可以使用 Azure 容器注册表和 Azure Kubernetes 服务构建自己的容器并将其部署到 Azure。 有关详细信息，请参阅[如何将 Node.js 容器部署到 Azure](node-howto-deploy-containers.md)。

- 如果希望主要使用无服务器代码，请参阅[如何在 Azure 上编写无服务器 Node.js 代码](node-howto-write-serverless-code.md)。

- 有关创建 JAMstack（静态）站点的详细信息，请参阅[如何使用 Azure 构建 JAMstack（静态站点）Web 应用](node-howto-create-static-site-jamstack.md)。

- 如果希望控制基础结构，只需使用虚拟机即可。 若要开始，请遵循 Microsoft Learn 中的[使用 Azure 虚拟机部署网站](/learn/paths/deploy-a-website-with-azure-virtual-machines/)路径。

有关不同托管选项的完整概述，请参阅 [Azure 计算服务的决策树](/azure/architecture/guide/technology-choices/compute-decision-tree)以及 Microsoft Learn 上的[核心云服务 - Azure 计算选项](/learn/modules/intro-to-azure-compute/)模块。
