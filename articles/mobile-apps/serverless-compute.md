---
title: 使用 Azure Functions 和其他服务构建无服务器移动应用程序后端
description: 了解用于构建固态无服务器移动应用程序后端的计算服务。
author: codemillmatt
ms.service: mobile-services
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 297ed418ffb3a3b4101f2b9506b849a32ec506bc
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632429"
---
# <a name="build-mobile-back-end-components-with-compute-services"></a>使用计算服务构建移动后端组件

每个移动应用程序都需要一个后端来负责数据存储、业务逻辑和安全性。 管理用于托管和执行后端代码的基础结构涉及对多个服务器进行调整、预配和缩放。 还需要管理操作系统更新和涉及的硬件并应用安全修补程序。 然后，需要监视所有这些基础结构组件的性能、可用性和容错能力。 

为这种类型的方案使用无服务器体系结构会非常方便，因为没有需要管理的服务器，也没有需要管理的操作系统或相关软件或硬件更新。 无服务器体系结构可节省开发人员的时间和成本，这意味着可加快市场投放，并将精力和时间集中于构建应用程序。

## <a name="benefits-of-compute"></a>计算的优点

- 服务器抽象意味着无需考虑托管、修补和安全性问题，这样可将精力和时间集中在代码上。
- 即时且高效的缩放可确保自动预配资源或按所需的任何规模预配资源。
- 高可用性和容错能力。
- 微计费可确保仅当代码实际运行时才计费。
- 在云中运行使用你选择的语言编写的代码。

使用以下服务在移动应用中启用无服务器计算功能。

## <a name="azure-functions"></a>Azure Functions

[Azure Functions](https://azure.microsoft.com/services/functions/) 是一种事件驱动型计算体验，可用于执行以你选择的编程语言编写的代码，且无需考虑服务器相关问题。 无需管理用于运行代码的应用程序或基础结构。 所使用的函数按需缩放，只需为运行中的代码付费。 Azure 函数是实现移动应用程序 API 的绝佳方法。 它们易于实现和维护，可通过 HTTP 访问。

### <a name="azure-functions-key-features"></a>Azure Functions 主要功能

- 它是事件驱动型的和可缩放的，可以使用触发器和绑定来定义何时调用函数以及函数要连接什么数据。
- 可引入自己的依赖项，因为Functions 支持 NuGet 和 NPM，因此用户可以使用自己的常用库。
- 提供集成的安全性，可以使用 OAuth 提供程序（如 Azure Active Directory、Facebook、Google、Twitter 和 Microsoft 帐户）保护 HTTP 触发的函数。
- 可利用不同的 [Azure 服务](/azure/azure-functions/functions-overview)和软件即服务 (SaaS) 产品/服务来简化集成。
- 灵活开发，可以直接在 Azure 门户中编写函数代码，或者通过 GitHub、Azure DevOps Services 和其他受支持的开发工具设置持续集成和部署代码。
- Functions 运行时是开源的，可通过 [GitHub](https://github.com/azure/azure-webjobs-sdk-script) 获取。
- 增强的开发体验：可通过结合使用所需编辑器或易于使用的 web 界面和由集成工具和内置 DevOps 功能提供的监视功能，在本地进行编码、测试和调试。
- 提供各种用于开发的编程语言和托管选项，例如 C#、Node.js、Java、JavaScript 或 Python。
- 按使用付费的定价模型意味着只需为运行代码所用的时间付费。

### <a name="azure-functions-references"></a>Azure Functions 参考

- [Azure 门户](https://portal.azure.com)
- [Azure Functions 文档](/azure/azure-functions/)
- [Azure Functions 开发人员指南](/azure/azure-functions/functions-reference)
- [快速入门](/azure/azure-functions/functions-create-first-function-vs-code)
- [示例](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="azure-app-service"></a>Azure 应用服务

使用 [Azure 应用服务](https://azure.microsoft.com/services/app-service/)，可以采用所选编程语言构建和托管 Web 应用和 RESTful API，无需管理基础结构。 它提供自动缩放和高可用性，支持 Windows 和 Linux，并支持从 GitHub、Azure DevOps 或任何 Git 存储库进行自动部署。

### <a name="azure-app-service-key-features"></a>Azure 应用服务重要功能

- 支持多种语言和框架，包括 ASP.NET、ASP.NET Core、Java、Ruby、Node.js、PHP 或 Python。 我们还能以后台服务的形式运行 PowerShell 和其他脚本或可执行文件。
- 可通过 Azure DevOps、GitHub、BitBucket、Docker Hub 或 Azure 容器注册表设置持续集成和部署，从而实现 DevOps 优化。 在应用服务中，利用 Azure PowerShell 或跨平台命令行接口 (CLI) 来管理应用。
- 提供具有高可用性的全局缩放功能，可以手动或自动方式进行横向或纵向扩展。
- 与 SaaS 平台和本地数据建立连接，可从适用于企业系统（例如 SAP）的 50 多个连接器、SaaS 服务（例如 Salesforce）以及 Internet 服务（例如 Facebook）中进行选择。 使用混合连接和 Azure 虚拟网络访问本地数据。
- Azure 应用服务符合 ISO、SOC 和 PCI 要求。 使用 Azure Active Directory 或社交媒体（Google、Facebook、Twitter 和 Microsoft）登录方式对用户进行身份验证。 创建 IP 地址限制和管理服务标识。
- 应用程序模板：可从 Azure 市场的大量应用程序模板列表中进行选择，例如 WordPress、Joomla 和 Drupal。
- Visual Studio 集成：可使用 Visual Studio 中的专用工具简化创建、部署和调试工作。

### <a name="azure-app-service-references"></a>Azure 应用服务参考

- [Azure 门户](https://portal.azure.com/)
- [Azure 应用服务文档](/azure/app-service/)
- [快速入门](/azure/app-service/app-service-web-get-started-dotnet)
- [示例](/azure/app-service/samples-cli)
- [教程](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service"></a>Azure Kubernetes 服务

[Azure Kubernetes 服务 (AKS)](https://azure.microsoft.com/services/kubernetes-service/) 管理托管的 Kubernetes 环境。 使用 AKS 可以快速轻松地部署和管理容器化应用程序，而无需具备容器业务流程方面的专业知识。 它还消除了正在进行的操作和维护的负担。 AKS 按需预配、升级和缩放资源，且无需使应用程序脱机就能进行。

### <a name="azure-kubernetes-service-key-features"></a>Azure Kubernetes 服务重要功能

- 轻松将现有应用程序迁移到容器中，并在 AKS 中运行。
- 简化基于微服务的应用程序的部署和管理。
- 保护 AKS 的 DevOps 的安全，实现速度与安全性的平衡，同时更快地交付大规模代码。
- 通过使用 AKS 和 Azure 容器实例在容器实例内预配 pod（可在几秒内启动），轻松进行缩放。
- 按需部署和管理 IoT 设备。
- 使用 TensorFlow 和 KubeFlow 等工具训练机器学习模型。

### <a name="azure-kubernetes-service-references"></a>Azure Kubernetes 服务参考

- [Azure 门户](https://portal.azure.com/)
- [Azure Kubernetes 服务文档](/azure/aks/)
- [快速入门](/azure/aks/kubernetes-walkthrough-portal)
- [教程](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure 容器实例

[Azure 容器实例](https://azure.microsoft.com/services/container-instances/)解决方案非常适合可在独立容器中操作的方案，例如简单应用程序、任务自动化、生成作业等。 可快速开发应用，且无需管理 VM。

### <a name="azure-container-instances-key-features"></a>Azure 容器实例重要功能

- 启动速度快：Azure 容器实例可在数秒内启动 Azure 中的容器，且无需预配和管理 VM。
- 公共 IP 连接和自定义 DNS 名称。
- 提供虚拟机监控程序级别的安全性，可保证容器中的应用程序像在 VM 中一样保持隔离状态。
- 自定义大小允许确切地指定 CPU 核心数和内存量，因此可提供最佳的利用方式。 费用取决于具体请求并按秒计收，因此可以根据实际需求来严格控制花费。
- 永久性存储可用于检索和保存状态。 容器实例提供直接装载 Azure 文件存储共享的功能。
- 用同一 API 安排 Linux 和 Windows 容器计划。

### <a name="azure-container-instances-references"></a>Azure 容器实例参考

- [Azure 门户](https://portal.azure.com/)
- [Azure 容器实例文档](/azure/container-instances/)
- [快速入门](/azure/container-instances/container-instances-quickstart-portal)
- [示例](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [教程](/azure/container-instances/container-instances-tutorial-prepare-app)