---
title: 通过 Visual Studio App Center 和 Azure 服务自动实现应用的生命周期
description: 了解 App Center 等服务，这些服务可帮助设置移动应用程序的持续生成和集成。
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 19b83e0dcc161c87e5a70eb6f8d4bbac29858c7b
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632459"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>通过持续生成和集成自动完成应用的生命周期

作为开发人员，你将编写代码并将其签入代码存储库，但签入存储库的提交内容可能并不总是一致的。 当多个开发人员处理同一个项目时，可能会出现问题。 团队可能会遇到这样的情况：操作不起作用、bug 堆积以及项目开发延迟。 开发人员必须等待，直到生成并测试整个软件代码后才能检查是否存在错误，这会降低处理速度并减少迭代。 

通过持续生成和集成，开发人员可以通过将所做更改提交到源代码存储库，并在生成环境中进行测试和验证，来简化代码生成和测试。 通过这种方式，他们能够持续对其代码运行测试。 每当对存储库进行提交时，都会连续生成对源代码所做的所有更改。 对于每次签入，持续集成 (CI) 服务器都会验证并执行开发人员创建的任何测试。 如果未通过测试，会将代码返回以进行进一步更改。 通过这种方式，开发人员不会中断已创建的版本。 他们也不必在自己的计算机上本地运行所有测试，从而提高了开发人员的工作效率。 

## <a name="key-benefits"></a>主要优点

- 自动执行管道的生成、测试和部署。
- 尽早检测 bug 和解决问题，以确保更快的发布速度。
- 更频繁地提交代码，快速生成应用程序。
- 可灵活、快速地更改代码，且不会引发任何问题。
- 加快上市速度，以便最后留存下来的都是优质代码。
- 更高效地进行小规模代码更改，因为小段代码是一次性集成的。
- 增加团队透明性、强化问责机制，以便能够从客户和团队获得持续反馈。

使用以下服务在移动应用中启用持续集成管道。

## <a name="visual-studio-app-center"></a>Visual Studio App Center

[App Center Build](/appcenter/build/) 使你能够通过使用安全的云基础结构，构建团队使用的本机和跨平台应用程序。 你可以轻松将存储库连接到 Visual Studio App Center，并在每次提交后轻松开始在云中构建应用程序。 无需考虑在本地配置生成服务器、完成复杂配置以及编写在同事计算机上而非你自己的计算机上进行构建的代码等问题。

借助 Visual Studio App Center 服务的增强功能，你可以进一步实现工作流自动化。 可以通过 App Center Distribute，自动将生成内容发布给测试人员和公共应用商店。 还可以通过 App Center Test，在云中针对数以千计的真实设备和 OS 配置运行自动 UI 测试。

### <a name="visual-studio-app-center-features"></a>Visual Studio App Center 功能

- 在几分钟内设置完持续集成，以更高的频率更快地生成应用程序。
- 与 GitHub、BitBucket、Azure DevOps 和 GitLab 进行集成。
- 在实施了管理措施的云托管的计算机上快速、安全地创建生成内容。
- 为生成内容启用测试，验证应用是否能在真实的 iOS 和 Android 设备中生成。
- 为 iOS、Android、macOS、Windows、Xamarin 和 React Native 获取本机支持和跨平台支持。
- 通过添加后期克隆、生成前和生成后脚本来自定义生成。

### <a name="visual-studio-app-center-references"></a>Visual Studio App Center 参考

- [注册 Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Build 入门](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines

 [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) 是 Azure DevOps 中的一项功能丰富的持续集成 (CI) 和持续交付 (CD) 服务，可与 Git 提供程序配合使用。 可将其部署到主流云服务中，包括 Azure。 你可以在 GitHub、GitHub Enterprise Server、GitLab、Bitbucket 云和 Azure Repos 上开始编写代码。 然后可以自动化构建和测试代码，并将其部署到 Microsoft Azure、Google Cloud Platform 或 Amazon Web Services (AWS) 中。

### <a name="azure-pipelines-features"></a>Azure Pipelines 功能

- **简化了基于任务的 CI 服务器设置体验：** 除了 Microsoft 和非基于 Microsoft（基于 Node.js、Java）的服务器技术外，还可以同时为本机（Android、iOS 和 Windows）和跨平台（Xamarin、Cordova 和 React Native）移动应用程序设置 CI 服务器。
- **任何语言、平台和云：** 构建、测试和部署 Node.js、Python、Java、PHP、Ruby、Go、C/C++、C#、Android 和 iOS 应用程序。 在 Linux、macOS 和 Windows 上并行运行。 部署到 Azure、AWS 和 Google Cloud Platform 等云提供程序。 通过 beta 通道和应用商店分发移动应用程序。
- **本机容器支持：** 轻松创建新的容器，并将其推送到任何注册表。 将容器部署到独立的主机或 Kubernetes。
- **高级工作流：** 轻松创建版本链和多阶段的版本内容。 获取对 YAML、测试集成、发布入口、报表等的支持。
- **可扩展：** 使用由社区生成的一系列生成、测试和部署任务，其中包括从 Slack 到 SonarCloud 的数百个扩展。 甚至可以从其他 CI 系统（如 Jenkins）进行部署。 可借助 Webhook 和 REST API 进行集成。
- **免费的云托管版本：** 这些版本适用于公共和专用存储库。
- **支持部署到其他云供应商：** 供应商包括 AWS 和 Google Cloud Platform。

### <a name="azure-pipelines-references"></a>Azure Pipelines 参考

- [Azure Pipelines 入门指南](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps 入门](https://app.vsaex.visualstudio.com/signup/)
- [快速入门](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

若要为应用程序版本选择正确的服务，请参阅 [App Center Build 与Azure Pipelines](/appcenter/build/choose-between-services) 一文，该问对二者进行了比较。
