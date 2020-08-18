---
title: Jenkins 和 Azure 概述
description: 在 Azure 中托管 Jenkins 生成和部署自动化服务器，并使用 Azure 计算和存储资源来扩展持续集成及部署 (CI/CD) 管道。
keywords: jenkins, azure, devops, 概述
ms.topic: overview
ms.date: 08/08/2020
ms.openlocfilehash: 2592ad806d58b3cbfcf930f180fa582945be3196
ms.sourcegitcommit: f65561589d22b9ba2d69b290daee82eb47b0b20f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2020
ms.locfileid: "88162036"
---
# <a name="azure-and-jenkins"></a>Azure 和 Jenkins

[Jenkins](https://jenkins.io/) 是一个受欢迎的开源自动化服务器，用于设置软件项目的持续集成和交付 (CI/CD)。 可以使用 Azure 资源在 Azure 中托管 Jenkins 部署或扩展现有的 Jenkins 配置。 此外，Jenkins 插件还可用来简化应用程序在 Azure 中的 CI/CD 过程。

本文将介绍如何将 Azure 用于 Jenkins，详细说明可供 Jenkins 用户使用的核心 Azure 功能。 若要详细了解如何在 Azure 中完成自己的 Jenkins 服务器的入门，请参阅[在 Azure 上创建 Jenkins 服务器](configure-on-linux-vm.md)。

## <a name="host-your-jenkins-servers-in-azure"></a>在 Azure 中托管 Jenkins 服务器

在 Azure 中托管 Jenkins，以集中执行生成自动化，并根据软件项目增长的需要来扩展部署。 请查看[快速入门 - Jenkins 入门](configure-on-linux-vm.md)，了解如何在 Linux VM 上安装和配置 Jenkins。 使用 [Azure Monitor 日志](/azure/log-analytics/log-analytics-overview)和 [Azure CLI](/cli/azure) 来监视和管理 Azure Jenkins 部署。

## <a name="scale-your-build-automation-on-demand"></a>按需扩展生成自动化

将生成代理添加到现有 Jenkins 部署来扩展 Jenkins 生成能力，因为作业和管道的生成数量及复杂性都在增加。 你可以通过使用 [Azure VM 代理插件](https://plugins.jenkins.io/azure-vm-agents)在 Azure 虚拟机上运行这些生成代理。 请参阅我们的[教程](/azure/jenkins/jenkins-azure-vm-agents)，了解详细信息。

配置 [Azure 服务主体](/azure/azure-resource-manager/resource-group-overview)后，Jenkins 作业和管道可以使用此凭据执行以下操作：

- 在使用 [Azure 存储插件](https://plugins.jenkins.io/windows-azure-storage)的 [Azure 存储](/azure/storage/common/storage-introduction)中，安全存储并存档生成项目。 查看 [Jenkins 存储操作方法](azure-storage-blobs-as-build-artifact-repository.md)了解详细信息。
- 使用 [Azure CLI](deploy-to-azure-app-service-using-azure-cli.md) 管理和配置 Azure 资源。

## <a name="deploy-your-code-into-azure-services"></a>将代码部署到 Azure 服务

使用 Jenkins 插件将应用程序作为 Jenkins CI/CD 管道的一部分部署到 Azure。 通过部署到 [Azure 应用服务](/azure/app-service/)和 [Azure 容器服务](/azure/container-service/kubernetes/)，可以暂存和测试更新，并将其发布到应用程序，而无需管理基础结构。

 插件可用于部署到以下服务和环境：

- [Linux 上的 Azure 应用服务](/azure/app-service/containers/app-service-linux-intro)。 请参阅[教程](deploy-from-github-to-azure-app-service.md)以开始使用。
- [Azure 应用服务](/azure/app-service/overview)。 请参阅[操作指南](deploy-to-azure-app-service-using-plugin.md)以开始使用。