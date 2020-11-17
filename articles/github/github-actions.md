---
title: 什么是适用于 Azure 的 GitHub Actions？
description: 在存储库中创建工作流，以在 Azure 中进行生成、测试、打包、发布和部署。
author: N-Usha
ms.author: ushan
ms.topic: conceptual
ms.service: azure
ms.date: 10/30/2020
ms.custom: github-actions-azure
ms.openlocfilehash: bbb87890f4db4b744ae4b2794c86e86a18c0e352
ms.sourcegitcommit: 12f80b1e0fe08db707c198271d0c399c3aba343a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515118"
---
# <a name="what-is-github-actions-for-azure"></a>什么是适用于 Azure 的 GitHub Actions

[GitHub Actions](https://help.github.com/articles/about-github-actions) 可帮助你从 GitHub 内部自动化软件开发工作流。 可以将工作流部署在存储代码的同一位置，并就拉取请求和问题进行协作。

在 GitHub Actions 中，[工作流](https://help.github.com/articles/about-github-actions#workflow)是在 GitHub 存储库中设置的自动化流程。 你可以使用工作流在 GitHub 上生成、测试、打包、发布或部署任何项目。

每个工作流均由在特定事件（例如拉取请求）发生后运行的各个[操作](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)组成。  各个操作均是打包的脚本，这些脚本可自动执行软件开发任务。

借助适用于 Azure 的 GitHub Actions，可以创建工作流，在存储库中设置该工作流可在 Azure 中进行生成、测试、打包、发布和部署。 适用于 Azure 的 GitHub Actions 支持 Azure 服务，包括 Azure 应用服务、Azure Functions 和 Azure Key Vault。

GitHub Actions 还包括对实用程序的支持，包括 Azure 资源管理器模板、Azure CLI 和 Azure Policy。

## <a name="why-should-i-use-github-actions-for-azure"></a>为什么我应该使用适用于 Azure 的 GitHub Actions

适用于 Azure 的 GitHub Actions 由 Microsoft 开发，旨在与 Azure 一起使用。 可以在 [GitHub 市场](https://github.com/marketplace?query=Azure&type=actions)中查看所有适用于 Azure 的 GitHub Actions。 请参阅[查找和自定义操作](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)，以详细了解如何将操作合并到工作流中。

## <a name="what-is-the-difference-between-github-actions-and-azure-pipelines"></a>GitHub Actions 和 Azure Pipelines 之间有什么区别

Azure Pipelines 和 GitHub Actions 均可帮助你自动化软件开发工作流。 [详细了解](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/migrating-from-azure-pipelines-to-github-actions)服务的区别以及如何从 Azure Pipelines 迁移到 GitHub Actions。

## <a name="what-do-i-need-to-use-github-actions-for-azure"></a>使用适用于 Azure 的 GitHub Actions 需具备什么内容

需要 Azure 和 GitHub 帐户：

* 具有活动订阅的 Azure 帐户。 [免费创建帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* 一个 GitHub 帐户。 如果没有该帐户，请注册[免费版](https://github.com/join)。  

## <a name="how-do-i-connect-github-actions-and-azure"></a>如何连接 GitHub Actions 和 Azure

使用服务主体或发布配置文件从 GitHub 连接到 Azure，具体取决于操作。 每次使用 [Azure 登录](https://github.com/marketplace/actions/azure-login)操作时，都将使用服务主体。 [Azure 应用服务操作](https://github.com/marketplace/actions/azure-webapp)支持使用发布配置文件或服务主体。 请参阅 [Azure Active Directory 中的应用程序和服务主体对象](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object)，了解关于服务主体的详细信息。  

可以将 Azure 登录操作与 [Azure CLI](https://github.com/marketplace/actions/azure-cli-action) 和 [Azure PowerShell](https://github.com/marketplace/actions/azure-powershell-action) 操作结合使用。 Azure 登录操作还可以与适用于 Azure 的其他大多数 GitHub 操作一起使用，包括[部署到 Web 应用](https://github.com/marketplace/actions/azure-webapp)和[访问密钥保管库机密](https://github.com/marketplace/actions/azure-key-vault-get-secrets)。

## <a name="what-is-included-in-a-github-actions-workflow"></a>GitHub Actions 工作流中包含什么内容

工作流由一个或多个作业组成。 在一个作业中，有一些由各个操作组成的步骤。 请参阅 [GitHub Actions 简介](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)，以了解有关 GitHub Actions 概念的详细信息。  

## <a name="where-can-i-see-complete-workflow-examples"></a>在哪里可以查看完整的工作流示例

[Azure 入门操作工作流存储库](https://github.com/Azure/actions-workflow-samples)包括端到端工作流，用于生成任何语言、任何生态系统的 Web 应用，并将其部署到 Azure。

## <a name="where-can-i-see-all-the-available-actions"></a>在哪里可以查看所有可用操作

访问[针对适用于 Azure 的 GitHub Actions 的市场](https://github.com/marketplace?query=Azure&type=actions)，以查看所有可用的适用于 Azure 的 GitHub Actions。

* [部署到静态 Web 应用](/azure/static-web-apps/getting-started?tabs=angular)
* [Azure 应用服务设置](https://github.com/Azure/appservice-settings)  
* [部署到 Azure Functions](https://github.com/Azure/functions-action)  
* [部署到适用于容器的 Azure Functions](https://github.com/Azure/webapps-container-deploy)  
* [Docker 登录](https://github.com/Azure/docker-login)  
* [部署到 Azure 容器实例](https://github.com/Azure/aci-deploy)
* [容器扫描操作](https://github.com/Azure/container-scan)
* [Kubectl 工具安装程序](https://github.com/Azure/setup-kubectl)  
* [Kubernetes 设置上下文](https://github.com/Azure/k8s-set-context)  
* [AKS 设置上下文](https://github.com/Azure/aks-set-context)  
* [Kubernetes 创建机密](https://github.com/Azure/k8s-create-secret)  
* [Kubernetes 部署](https://github.com/Azure/k8s-deploy)  
* [设置 Helm](https://github.com/Azure/setup-helm)  
* [Kubernetes 烘培](https://github.com/Azure/k8s-bake)  
* [构建 Azure 虚拟机映像](https://github.com/Azure/build-vm-image)
* [机器学习登录](https://github.com/Azure/aml-workspace)
* [机器学习训练](https://github.com/Azure/aml-run)
* [机器学习 - 部署模型](https://github.com/Azure/aml-deploy)
* [部署到 Azure SQL 数据库](https://github.com/Azure/sql-action)  
* [部署到 Azure MySQL 操作](https://github.com/Azure/mysql-action)  
* [Azure Policy 符合性扫描](https://github.com/Azure/policy-compliance-scan)
* [管理 Azure Policy](https://github.com/Azure/manage-azure-policy)
* [触发 Azure Pipelines 运行](https://github.com/Azure/pipelines)  
* [变量替换](https://github.com/Microsoft/variable-substitution)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [学习路径，使用 GitHub Actions 实现工作流自动化](https://docs.microsoft.com/learn/modules/github-actions-automate-tasks/)

> [!div class="nextstepaction"]
> [学习实验室，使用 Azure 进行持续交付](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
