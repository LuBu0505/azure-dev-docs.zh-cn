---
title: 通过 Visual Studio App Center 和 Azure 服务自动部署和发布移动应用程序
description: 了解 App Center 等服务，这些服务可帮助你为移动应用程序设置持续交付管道。
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 96ca72ffb239b7baa48c06f09afe08a2653e3e0b
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632369"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>通过持续交付服务自动部署和发布移动应用程序

作为开发人员，你将编写代码并将其签入代码存储库，但签入存储库的提交内容可能并不总是一致的。 当多个开发人员处理同一个项目时，可能会出现问题。 团队可能会遇到这样的情况：操作不起作用、bug 堆积以及项目开发延迟。 开发人员必须等待，直到生成并测试整个软件代码后才能检查是否存在错误，这会降低处理速度并减少迭代。

通过持续交付，可以自动部署和发布移动应用程序。 无论你是要将应用程序分发到一组测试人员或公司员工（用于 beta 测试）还是应用商店（用于生产），这都不重要。 持续交付使部署风险降低，并鼓励快速迭代。 你还可持续不断地向客户发布新的更改。

## <a name="distribute-application-binaries-to-beta-testers"></a>将应用程序二进制文件分发给 beta 版测试人员

Beta 测试移动应用程序是应用程序开发过程中的关键步骤之一。 可帮助你及早查找应用程序中的 bug 和问题。 当正在准备供生产使用时，反馈会提高你的应用程序质量。

使用以下服务在移动应用中启用持续交付管道。

### <a name="visual-studio-app-center-distribute"></a>Visual Studio App Center Distribute

[App Center Distribute](/appcenter/distribution/) 是一种工具，供开发人员快速将版本发布到设备。 App Center Distribute 具有完整的安装门户体验，是用于 beta 应用测试人员分发的强大解决方案。 它也是通过公用应用商店分发的便利方法。 开发人员还可以通过 App Center 生成和公共应用程序商店集成，进一步自动化其分发工作流。

#### <a name="visual-studio-app-center-distribute-features"></a>Visual Studio App Center Distribute 功能

- 将你的应用分发给 beta 版测试人员和用户，并确保所有测试人员都使用最新版本的应用程序。
- 通知测试人员使用新版本，测试人员无需再次经历下载流。
- 管理应用程序不同版本的通讯组。
- 分发到商店： 
  - [Apple](/appcenter/distribution/stores/apple)
  - [Google Play](/appcenter/distribution/stores/googleplay)
  - [Intune](/appcenter/distribution/stores/intune)
- 获取对 iOS、Android、macOS、tvOS、Xamarin、React Native、Unity 和 Cordova 的平台支持。
- 自动将 iOS 设备注册到预配配置文件。

#### <a name="visual-studio-app-center-distribute-references"></a>Visual Studio App Center Distribute 参考

- [注册 Visual Studio App Center](https://appcenter.ms/signup)
- [App Center Distribute 入门](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) 是一项别具特色的持续集成 (CI) 和持续交付 (CD) 服务，可与 Git 提供程序配合使用。 Azure Pipelines 可以部署到最主要的云服务，如 Azure 服务。 你可以在 GitHub、GitHub Enterprise Server、GitLab、Bitbucket 云和 Azure Repos 上开始编写代码。 然后可以自动化构建和测试代码，并将其部署到 Microsoft Azure、Google Cloud Platform 或 Amazon Web Services (AWS) 中。

#### <a name="azure-pipelines-features"></a>Azure Pipelines 功能

- **简化了基于任务的 CI 服务器设置体验：** 为本机（Android、iOS 和 Windows）和跨平台（Xamarin、Cordova 和 React Native）移动应用程序设置 CI 服务器。
- **任何语言、平台和云：** 构建、测试和部署 Node.js、Python、Java、PHP、Ruby、Go、C/C++、C#、Android 和 iOS 应用。 在 Linux、macOS 和 Windows 上并行运行。 部署到 Azure、AWS 和 Google Cloud Platform 等云提供程序。 通过 beta 通道和应用商店分发移动应用程序。
- **本机容器支持：** 轻松创建新的容器，并将其推送到任何注册表。 将容器部署到独立的主机或 Kubernetes。
- **高级工作流和功能：** 轻松创建版本链和多阶段的版本内容。 获取对 YAML、测试集成、发布入口、报表等的支持。
- **可扩展：** 使用由社区生成的一系列生成、测试和部署任务，其中包括从 Slack 到 SonarCloud 的数百个扩展。 甚至可以从其他 CI 系统（如 Jenkins）进行部署。 可借助 Webhook 和 REST API 进行集成。
- **免费的云托管版本：** 这些版本适用于公共和专用存储库。
- **支持部署到其他云供应商：** 供应商包括 AWS 和 Google Cloud Platform。

#### <a name="azure-pipelines-references"></a>Azure Pipelines 参考

- [Azure Pipelines 入门指南](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps 入门](https://app.vsaex.visualstudio.com/signup/)
  
## <a name="distribute-your-application-directly-to-app-stores"></a>直接将应用程序分发到应用商店

在应用程序准备好供生产使用并且你想要将其公开使用后，需要将其提交到应用商店供客户下载。 可通过多种方式将应用程序直接分发到应用商店。 

### <a name="visual-studio-app-center-distribute-stores"></a>Visual Studio App Center Distribute 商店

利用 [App Center Distribute](/appcenter/distribution/stores/)，你可以将移动应用程序直接发布到应用商店。 应用程序准备好供用户下载后，你可以直接从 Visual Studio App Center 门户发布应用程序二进制文件。 

可直接分发到：

- [Apple App Store](/appcenter/distribution/stores/apple)
- [Google Play 商店](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)

### <a name="apple-app-store"></a>Apple App Store

在 Apple 开发和维护的应用商店中，用户可以浏览和下载为 iOS、MacOS、WatchOS 和 tvOS 设备开发的应用程序。 开发人员需要将其 iOS 应用提交到 Apple App Store 以供公开使用。

### <a name="google-play"></a>Google Play

Google Play 是适用于 Android 操作系统的官方应用商店，用户可在其中浏览和下载为 Android 设备（通过 Google 发布）开发的应用程序。

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) 是企业移动管理领域中基于云的服务，可帮助员工提高工作效率，同时保护企业数据。 通过 Intune，还可以：

- 管理员工用于访问公司数据的移动设备和电脑。
- 管理员工使用的移动应用程序。
- 通过控制员工访问和共享公司信息的方式来保护公司信息。
- 确保设备和应用程序符合公司安全要求。

## <a name="deploy-updates-directly-to-users-devices"></a>直接将更新部署到用户的设备

### <a name="codepush"></a>CodePush

利用 App Center 中的 [CodePush](/appcenter/distribution/codepush/)，Apache Cordova 和 React Native 开发人员可以直接将移动应用程序更新部署到用户的设备。 它充当中央存储库，开发人员可以将某些更新发布到该存储库，如 JavaScript、HTML、CSS 和图像更改。 然后，应用程序可以使用提供的客户端 SDK 从存储库查询更新。 通过这种方式，你可以在处理 bug 或添加小功能时，为你的用户提供一个更具确定性的直接参与模型。 无需重新生成二进制文件或通过任何公共应用商店重新分发。

#### <a name="codepush-key-features"></a>CodePush 主要功能

- Apache Cordova 和 React Native 开发人员可以直接将移动应用程序更新部署到用户的设备，无需在商店发布。
- 用于修复 bug 或添加和删除小功能，无需重新生成二进制文件并通过各自的商店重新分发。

#### <a name="codepush-references"></a>CodePush 参考

- [注册 Visual Studio App Center](https://appcenter.ms/signup)
- [App Center 中的 CodePush 入门](/appcenter/distribution/codepush/)
- [CodePush CLI](/appcenter/distribution/codepush/cli)
