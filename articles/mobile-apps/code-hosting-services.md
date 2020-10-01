---
title: 利用 GitHub 和 Azure DevOps 在云中托管移动应用程序源代码
description: 了解如何通过 Microsoft 服务在云中托管移动应用程序代码。
author: codemillmatt
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 0ab44652d4a635d5ff04928b76883a76fe3a62fa
ms.sourcegitcommit: e97cb81a245ce7dcabeac3260abc3db7c30edd79
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91493108"
---
# <a name="cloud-hosted-source-code-management-services"></a>云托管的源代码管理服务

如果你的开发团队有多个使用同一基本代码的团队成员，则需要找到合适的位置来托管你的代码。 将数据存储在云中，以及使用集中式存储库，使每个人都可以上传、编辑和管理代码文件。 他们还可以与开发人员进行有关项目的交互。 只要有 Internet 连接，就可以随时访问该代码，无论你身处何处都能实现。

云托管比本地托管要容易得多。 它所需的硬件配置更少，并使组织能够以更灵活的方式完成实现过程。

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>在云中托管源代码的优点

- **集中式云存储：** 随时随地查看和管理数据。
- **更好的协作和更简洁的代码：** 跟踪团队中的代码和管理项目，以确保实现优秀软件的持续交付。
- **更易于参与：** 轻松参与项目。
- **更快的发布周期：** 在团队中更快地工作，轻松地参与项目。
- **降低成本：** 不必担心如何维护自己的硬件、服务器、VPN 等。

使用以下服务在云中托管你的应用程序数据。

## <a name="github"></a>GitHub

[GitHub](https://github.com/) 是一种开放源代码存储库托管服务，可托管各种不同编程语言的源代码项目。 GitHub 跟踪对每个迭代进行的各种更改。

### <a name="github-key-features"></a>GitHub 关键功能

- 使用代码托管可将所有代码保存在一个位置。 专用、公共和开放源代码存储库都具备可帮助你托管、进行版本控制和发布代码的工具。
- 查看代码并使用内置的评审工具使代码评审成为任何团队进程的基本组成部分：
  - 保护分支、建议更改和请求评审。
  - 发现差异，在上下文中注释，并获得明确的反馈。
- 提前协调、保持一致，并通过 GitHub 的项目管理工具完成更多工作：
  - 查看项目的大图。
  - 使用 GitHub 中你的代码旁边的任务板。
  - 拖动卡片，将问题或拉取请求分配给团队成员。
  - 设置里程碑以组织和跟踪进度。
  - 编写注释来捕获可能有用但不属于特定问题或拉取请求的想法。
- 通过购买 [GitHub 市场](https://github.com/marketplace)中的应用程序，可以轻松发现和选择合适的工具以实现更好的通信和自动化工作。
- 使用以下内容管理和壮大团队： 
  - 用于帮助组织团队和访问权限的用户角色。
  - 用于保持会话不偏题和团队专注性的讨论线程工具。
  - 使用帐户快速设置新团队成员的社区指导原则。
- 浏览并对常用项目标注星号以关注这些项目。
- 与行业内的其他人员建立联系并向其学习。

### <a name="github-references"></a>GitHub 参考

- [GitHub](https://github.com/)
- [GitHub 指南](https://guides.github.com/)
- [GitHub 社区论坛](https://github.community/)
- [GitHub 市场](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps

[Azure DevOps](https://azure.microsoft.com/services/devops/) 支持 [Azure Repos](https://azure.microsoft.com/services/devops/repos/) 和 [Team Foundation 版本控制 (TFVC)](/azure/devops/repos/tfvc/index?view=azure-devops) 作为源代码管理选项。 它拥有无限制的免费专用存储库，并提供协作代码评审、高级文件管理、代码搜索和分支策略，以确保高质量代码。 对于需要本机 Azure Active Directory 支持和高级策略的小型项目和大型组织而言，Azure Repos 非常有用。

### <a name="azure-devops-features"></a>Azure DevOps 功能

- 为公共和专用存储库使用无限制的云托管 Git 源代码存储库：
  - 获取对任何 Git 客户端的支持。
  - 使用 Webhook 和 API 集成。
- 从存储库拉取请求开始下一次生成：
  - 通过对每个更改使用线程化讨论和持续集成，协作生成更优的代码。
  - 设置持续集成/持续交付 (CI/CD)，以通过每个已完成的拉取请求自动触发生成、测试和部署。 你可以使用 Azure Pipelines 或自己的工具。
  - 使用分支策略保护代码质量。
- 使用 Team Foundation 版本控制维护集中式版本控制，其中包括代码评审。
- 使用 Xcode、Eclipse、IntelliJ、Android Studio、Visual Studio、Visual Studio Code 等连接到你的代码。
- 使用功能强大的语义代码搜索。
- 企业就绪功能可带来诸多好处。 Azure DevOps 与 Azure Active Directory 本机集成，这简化了管理对代码存储库的访问过程。
- 使用分支策略确保代码质量，例如最小数量的代码评审、成功生成的要求以及 Git 合并策略的强制执行。
- 连接你最喜爱的开发环境以访问存储库并管理工作。

### <a name="azure-devops-references"></a>Azure DevOps 参考

- [Azure Repos 入门](https://azure.microsoft.com/services/devops/repos/) 
- [Azure Repos 文档](/azure/devops/repos/?view=azure-devops)