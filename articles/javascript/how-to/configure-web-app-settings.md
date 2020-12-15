---
title: 托管 Web 应用 - 配置设置
description: 了解如何为 Web 应用设置常见配置。
ms.topic: conceptual
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: 271c2b916062d9cbd2b905fb937c9fb216eb43e5
ms.sourcegitcommit: 1901759f41adfac3c3f2ff135bcf72206543b639
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/09/2020
ms.locfileid: "96934282"
---
# <a name="hosting-web-apps-on-azure"></a>在 Azure 上托管 Web 应用

了解如何为 Web 应用设置常见配置。 如果缺少常见设置，请在反馈中提出问题并告诉我们。 

创建资源时，请求任何所需设置。 如果此时不请求设置，则它具有默认值，你可在创建资源后更改。 

## <a name="what-is-a-web-app"></a>什么是 Web 应用？

Web 应用是使用 Internet URL 访问的任何内容。 有许多 Azure 服务可视为 Web 应用。 通常用于 Web 应用的常见服务包括：

* 应用服务，其中还包括
    * [Static Web Apps](/azure/static-web-apps/)
    * [函数](/azure/azure-functions/)
    * [Web 应用](/azure/app-service/)
    * [容器](/azure/app-service/configure-custom-container?pivots=container-linux)
* 容器 - [Kubernetes](/azure/aks/) 和单个[容器](/azure/container-instances/)
* 虚拟机 - [Windows](/azure/virtual-machines/windows) 和 [Linux](/azure/virtual-machines/linux)

## <a name="how-to-configure-web-app-settings"></a>如何配置 Web 应用设置

大多数 Azure 服务都提供四种配置设置的方法：

* [Azure 门户](https://portal.azure.com)
* 用于服务的 [Azure SDK](https://github.com/Azure/azure-sdk)，通常称为管理
* [Azure CLI](/cli/azure/)
* [Azure PowerShell](/powershell/azure/)

许多设置还可在 Visual Studio Code 中使用[扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)配置。 

## <a name="use-default-domain-name-provided-by-azure"></a>使用 Azure 提供的默认域名

大多数 Azure 服务都会为资源提供 URL。 服务名称确定子域以及来自 Azure 的其他域。 

例如：

* Azure Functions - `https://my-function-app.azurewebsites.net`
* Azure Web 应用 - `https://my-web-app.azurewebsites.net`
* Azure 存储 Blob - `https://mystorage.blob.core.windows.net/`

某些服务（如 Static Web Apps）为你提供了一个相对唯一的子域，使你能够在生产中立即使用它：

* Azure Static Web Apps = `https://gentle-tree-0b08aaf12.azurestaticapps.net`

## <a name="configure-custom-domain-name"></a>配置自定义域名 

每项服务都提供自己的机制来添加自定义域。 

* [Azure Static Web Apps](/azure/static-web-apps/custom-domain)
* [Azure Functions](/azure/app-service/app-service-web-tutorial-custom-domain) & [Azure Web 应用](/azure/app-service/app-service-web-tutorial-custom-domain) - Functions 在 Web 应用的基础上构建，因此它们使用相同的机制
* [Azure 存储 Blob](/azure/storage/blobs/storage-custom-domain-name?tabs=azure-portal)

## <a name="configure-port-forwarding"></a>配置端口转发

如果不是默认端口 `8080`，需要[映射应用的端口号](/azure/app-service/configure-language-nodejs?pivots=platform-windows#get-port-number)。 这使得应用服务能将请求转发到正确的端口。 

## <a name="configure-certificates"></a>配置证书

如果应用立即需要证书，你可通过以下几个选项来[提供证书](/azure/app-service/configure-ssl-certificate#import-an-app-service-certificate)：

* 上传自己的证书
* 管理应用服务中的证书
* 导入 Azure Key Vault 中的证书
* 在[代码](/azure/app-service/configure-ssl-certificate-in-code)中提供证书

## <a name="configure-secrets"></a>配置机密

通常以下列方式提供机密：

* Azure Key Vault - 为此服务创建资源，该服务提供[应用机密](/azure/app-service/app-service-key-vault-references)。 
* 应用设置 - 如果你正在寻找更轻量的解决方案，可通过应用设置方法提供机密，并使用典型 `process.env.VARNAME` 引用这些机密。 

## <a name="configure-logging"></a>配置日志记录

日志记录包括：

* 平台日志记录 - 应用外发生的情况
* 应用日志记录 - 应用内发生的情况

平台日志可用于：
* 了解环境的运行状况。
* 使你能够扩展到不同的定价层或跨区域。 

不会为你提供应用程序日志。 可为内部应用行为添加你自己的日志记录：
* [Azure Monitor](/azure/azure-monitor/overview) 为 [Application Insights](/azure/azure-monitor/app/app-insights-overview) 提供 npm 库，以提供日志记录和存档日志的存储资源。 

## <a name="configure-database-and-storage"></a>配置数据库和存储

通常，与数据库或数据存储的连接以连接字符串开头。 

数据连接的注意事项：
* 使用当前连接
* 新建数据存储 - 如果应用需要新的存储机制，Azure 可提供[多种不同的数据库](integrate-database.md)选择。 需要安全地存储连接。 

## <a name="missing-something"></a>缺少内容？ 

如果此列表中缺少任何内容，请填写反馈，告诉我们。 

## <a name="next-steps"></a>后续步骤

* 请参阅[端到端 Node.js 应用](/azure/developer/javascript/how-to/develop-nodejs-on-azure)开发流中的大部分步骤。 