---
title: 使用 GitHub Actions 部署到 Azure
description: 在存储库中创建工作流，以生成、测试、打包、发布和部署到 Azure。
author: N-Usha
ms.author: ushan
ms.topic: conceptual
ms.service: azure
ms.date: 05/05/2020
ms.openlocfilehash: a7bcf09da47e9af41f404bfdd2454b25f94eb7b4
ms.sourcegitcommit: a9b9157bb3a802ecfe3699854788d010a3f08d7e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202846"
---
# <a name="deploy-to-azure-using-github-actions"></a>使用 GitHub Actions 部署到 Azure

使用 [GitHub Actions](https://help.github.com/articles/about-github-actions)，开发人员可以生成自动化软件开发生命周期工作流。  

借助适用于 Azure 的 GitHub Actions，可以创建工作流，从而在存储库中设置工作流来生成、测试、打包、发布和部署到 Azure。 [详细了解其他所有与 Azure 的集成](https://aka.ms/GitHubonAzure)。

立即开始使用[免费 Azure 帐户](https://azure.com/free/open-source)！

> [!NOTE]   
> 本文中提供的链接指向 GitHub 文章或 GitHub 存储库。 

## <a name="key-concepts"></a>关键概念

GitHub Actions 使你可以直接在 GitHub 存储库中创建自定义软件开发生命周期 (SDLC) 工作流。 有关 GitHub Actions 和核心概念的概述，请参阅以下文章： 

- [关于 GitHub Actions](https://help.github.com/actions/getting-started-with-github-actions/about-github-actions)
- [核心概念](https://help.github.com/actions/getting-started-with-github-actions/core-concepts-for-github-actions)
- [关于使用 GitHub Actions 打包](https://help.github.com/en/actions/publishing-packages-with-github-actions/about-packaging-with-github-actions)

## <a name="get-started"></a>入门 

GitHub Actions 包括预配置的模板和市场操作。 

- [使用预配置的模板](https://help.github.com/actions/getting-started-with-github-actions/starting-with-preconfigured-workflow-templates)  
- [使用 GitHub 市场中的操作](https://help.github.com/en/actions/getting-started-with-github-actions/using-actions-from-github-marketplace)  
- [GitHub 市场操作，部署到 Azure](https://github.com/marketplace?type=actions&query=Azure)  
  
有关适用于 Azure 的 GitHub Actions，请参阅以下页面： 
   
- [Azure 操作](https://github.com/marketplace?query=Azure&type=actions)  
- [要部署到 Azure 的起动器操作工作流](https://github.com/Azure/actions-workflow-samples)


## <a name="connect-to-azure"></a>连接到 Azure

有关连接到 Azure 并运行基于 Az CLI 或 Az PowerShell 的脚本的示例工作流，请参阅以下 GitHub 操作：  

- [Azure 登录](https://github.com/Azure/login)  
- [Azure CLI](https://github.com/Azure/CLI)
- [Azure PowerShell](https://github.com/Azure/powershell)


## <a name="sample-apps-with-cicd-workflow-samples"></a>包含 CI/CD 工作流示例的示例应用 

下面的几个示例展示了端到端工作流，用于生成任何语言、任何生态系统的 Web 应用，并将其部署到 Azure。 

- [使用 ASP.NET 支持部署 Web 应用](https://github.com/Azure-Samples/dotnet-sample)  
- [部署 ASP.NET Core 应用](https://github.com/Azure-Samples/dotnet_core_sample)  
- [部署 Node.js Web 应用](https://github.com/Azure-Samples/node_express_app)  
- [部署 Java Web 应用](https://github.com/Azure-Samples/java-spring-petclinic)  
- [部署 Java Spring 应用](https://github.com/Azure-Samples/Java-application-petstore-ee7)  
- [部署 Python Web 应用](https://github.com/Azure-Samples/pythonSample_thecatsaidno)  
- [使用 Docker 部署容器化 Web 应用](https://github.com/Azure-Samples/Node_express_container)


## <a name="deploy-a-web-app"></a>部署 Web 应用

部署到 Azure Web 应用和用于容器的 Azure Web 应用：

- [Azure/webapps-deploy 操作](https://github.com/Azure/webapps-deploy)

部署静态 Web 应用：
- [Azure/static-web-apps-deploy](https://docs.microsoft.com/azure/static-web-apps/getting-started?tabs=angular)


使用操作来配置应用设置和连接字符串：

- [Azure/appservice-settings](https://github.com/Azure/appservice-settings) 
- [Azure 应用服务设置](https://github.com/Azure/appservice-settings)  

## <a name="deploy-a-serverless-app"></a>部署无服务器应用

- [Azure Functions](https://github.com/Azure/functions-action)  
- [适用于容器的 Azure Functions](https://github.com/Azure/webapps-container-deploy)  
 
## <a name="build-and-deploy-containerized-apps"></a>生成和部署容器化应用

- [Docker 登录](https://github.com/Azure/docker-login)  
- [部署到 Azure 容器实例](https://github.com/Azure/aci-deploy)

## <a name="deploy-to-kubernetes"></a>部署到 Kubernetes

- [Kubectl 工具安装程序](https://github.com/Azure/setup-kubectl)  
- [Kubernetes 设置上下文](https://github.com/Azure/k8s-set-context)  
- [AKS 设置上下文](https://github.com/Azure/aks-set-context)  
- [Kubernetes 创建机密](https://github.com/Azure/k8s-create-secret)  
- [Kubernetes 部署](https://github.com/Azure/k8s-deploy)  
- [设置 Helm](https://github.com/Azure/setup-helm)  
- [Kubernetes 烘培](https://github.com/Azure/k8s-bake)  

## <a name="train-and-deploy-a-machine-learning-model"></a>训练和部署机器学习模型 

- [登录](https://github.com/Azure/aml-workspace) 
- [训练](https://github.com/Azure/aml-run)
- [部署模型](https://github.com/Azure/aml-deploy)

## <a name="deploy-to-databases"></a>部署到数据库

- [Azure SQL 数据库](https://github.com/Azure/sql-action)  
- [Azure MySQL 操作](https://github.com/Azure/mysql-action)  

## <a name="trigger-a-run-in-azure-pipelines"></a>触发 Azure Pipelines 中的运行

- [Azure Pipelines](https://github.com/Azure/pipelines)  
 
## <a name="utility-actions"></a>实用工具操作

- [变量替换](https://github.com/Microsoft/variable-substitution) 


## <a name="additional-resources"></a>其他资源

以下 GitHub 资源可用于支持使用 GitHub 将应用部署到 Azure。  

- [适用于 Azure 的 GitHub Actions 市场](https://github.com/marketplace?query=Azure&type=actions)
- [学习实验室，使用 Azure 进行持续交付](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
- [要部署到 Azure 的起动器操作工作流](https://github.com/Azure/actions-workflow-samples)
