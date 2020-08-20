---
title: 使用 Node.js、API 和标记在 Azure 上构建静态站点
description: 如何使用 Azure 构建 JAMstack 应用（JavaScript、API 和标记）
ms.topic: article
ms.date: 08/20/2019
ms.custom: seo-javascript-september2019, devx-track-javascript
ms.openlocfilehash: 354c55609092bbeaf21b4e1eefdd125967eccfd6
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218697"
---
# <a name="build-jamstack-static-site-web-apps-on-azure-with-nodejs"></a>使用 Node.js 在 Azure 上构建 JAMstack（静态站点）Web 应用

精彩的 Web 应用可以使用 JavaScript  前端、API  （作为无服务器代码构建的第三方或自定义 API）和作为静态页面提供的模板化标记  （HTML 和 CSS）的组合高效地构建和维护。 通过这种组合（亦称 JAMstack），可以避免编写用于提供网页的复杂后端代码。 相反，系统仅提供静态页面（HTML、CSS 和 JavaScript），其中，这些页面会调用 API 以执行服务器端工作。 由于可以使用自动缩放无服务器技术编写这些 API，因此，完全避免了使用典型的始终可用服务器或 Web 主机所带来的成本和安全问题。 （有关详细信息，请参阅 [jamstack.org](https://jamstack.org/)。）

若要在 Azure 上实现静态/JAMstack 站点，需要使用各种工具和服务：

- 根据需要配置数据库。
- 在 Azure Functions 中实现无服务器 API 代码。 这些 API 通常会使用该数据库。
- 选择要用于前端开发的任何库，如 Angular。 然后，将这些静态 HTML、CSS 和 JavaScript 文件上传到 Azure Blob 存储，该存储用于提供内置的 Web 服务器。
- 创建反向代理，以便所有流量都通过一个 URL 域。

可以观看 Build 2019 大会的[使用 JavaScript、Visual Studio Code 和 Azure 进行高效的前端开发](https://azure.microsoft.com/resources/videos/build-2019-productive-front-end-development-with-javascript-visual-studio-code-and-azure/)分会的过程演示。

> [!VIDEO https://medius.studios.ms/Embed/Video-nc/B19-BRK3021?latestplayer=true]

可以在[将静态网站部署到 Azure](tutorial-vscode-static-website-node-01.md) 中找到一个分步教程。

以下文章还介绍了更多详细信息：

- **数据库**：可以使用任何所需的数据库，如 Azure 上的不同数据库服务，这些数据库服务在[如何将 Azure 数据库集成到 node.js 应用](node-howto-integrate-databases.md)中进行了描述。
  
- **无服务器 API**：

  - 从[在 Visual Studio Code 中部署 Azure Functions](tutorial-vscode-serverless-node-01.md) 开始。该文章在 Visual Studio Code 的上下文中介绍 Azure Functions，以简化很多详细信息。
  - 完成本文后，将拥有一个 Azure Functions 项目（文件夹），其中包含为该函数命名的子文件夹，该子文件夹与该项目的 HTTP 终结点的子文件夹相同。 该函数文件夹中的 index.js  文件包含代码。
  - 可以根据需要修改该函数，还可以向该项目中添加更多函数，然后将其重新部署到 Azure，以供公开发布。
  - 有关无服务器开发的其他资源，请参阅[如何在 Azure 上编写无服务器 Node.js 代码](node-howto-write-serverless-code.md)

- **将前端部署到 Azure 存储**：如果手头有 API，可以立即通过任何所需的框架编写前端代码以使用这些 API。 准备就绪后，请按照本文[教程：在 Blob 存储上托管静态网站](/azure/storage/blobs/storage-blob-static-website-host)的说明，将这些文件上传到 Azure 并打开静态网站托管。

- **创建反向代理**：使用反向代理（如[使用 Azure Functions 代理](/azure/azure-functions/functions-proxies)中所述），可以轻松地将某些请求定向到不同的 URL。 在这种情况下，需要将前端文件请求定向到这些文件已部署到的 Azure 存储 URL，并将 API 请求定向到 Azure Functions URL。

  - 若要创建这些代理，请在 Functions 项目中编辑 proxies.json  文件，以使该文件如下所示，并将 URL 替换为 `<storage_url>` 和 `<api_url>`：
  
    ```json
    {
      "$schema": "http://json.schemastore.org/proxies",
      "proxies": {
        "Static frontend on Azure Storage": {
          "matchCondition": {
            "route": "/{*restOfPath}"
          },
          "backendUri": "<storage_url>/{restOfPath}"
        },
        "Azure Functions API": {
          "matchCondition": {
            "route": "/api/{*restOfPath}"
          },
          "backendUri": "<api_url>/api/{restOfPath}"
        }
      }
    }
    ```
