---
title: 用于处理 Azure CLI 的跨服务链接
description: 指向常用教程、快速入门、示例、概念、操作指南、Azure CLI、虚拟机、Azure Kubernetes 服务 (AKS)、Batch、Azure CLI (Core)、Azure 资源管理器、Key Vault、Azure Stack Hub、函数、数据库、事件中心、应用配置、德国、安全性、治理、见解、物联网 (IoT)、DevOps、虚拟网络、计算、网络、开发人员工具、数据库、分析、管理和治理、混合、存储、AI、AI + 计算机学习、Linux、Windows、Ubuntu、自动化、应用程序、Web 应用、脚本的链接
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 03/01/2020
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 3a0b2e315e0eaf6c352aa2737f33da043b5feb7d
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031249"
---
# <a name="popular-articles-using-the-azure-cli"></a>有关使用 Azure CLI 的热门文章

Azure CLI 在许多 Azure 服务中使用，导致文章分散到多个文档存储库中。  本页提供了指向所选热门文章的链接。  

## <a name="compute"></a>计算

| | | | |
|-|-|-|-|
|虚拟机 | 教程：Linux | [使用 Azure CLI 创建 Linux 虚拟机](azure-cli-vm-tutorial.yml) | 创建虚拟机。  了解输出查询以及环境变量的设置。
|虚拟机 | 快速入门：Linux | [使用 Azure CLI 创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-cli) | 创建并部署 Linux 虚拟机。  打开用于 Web 流量的端口，并安装 Web 服务器。
|虚拟机 | 操作指南：Linux |[创建虚拟机或 VHD 的 Linux 映像](/azure/virtual-machines/linux/capture-image) | 取消预配现有虚拟机、创建映像，然后根据捕获的映像创建新的虚拟机。
|虚拟机 | 操作指南：Linux | [使用 Azure CLI 将 VHD 上传到 Azure](/azure/virtual-machines/linux/disks-upload-vhd-to-managed-disk-cli) | 创建空的托管磁盘，上传本地 VHD 文件，然后复制托管磁盘。
|虚拟机 | 操作指南：Linux | [使用 Azure CLI 创建共享映像库](/azure/virtual-machines/linux/shared-images) | 在组织中、区域内（或跨区域）或 Azure Active Directory 租户中创建包含自定义 VM 映像和其他项的共享映像库。
|虚拟机 | 操作指南：Linux | [使用 Azure CLI（预览版）部署 Spot VM](/azure/virtual-machines/linux/spot-cli) | 部署不会根据价格逐出的 Linux Spot 虚拟机。
|虚拟机 | 快速入门：Windows | [使用 Azure CLI 创建 Windows 虚拟机](/azure/virtual-machines/windows/quick-create-cli) | 在 Azure 中部署运行 Windows Server 2016 的虚拟机。
|虚拟机 | Learn 模块 | [使用 Azure CLI 管理虚拟机](https://docs.microsoft.com/learn/modules/manage-virtual-machines-with-azure-cli/) | 创建、启动、停止和执行与虚拟机相关的其他管理任务。
|Azure Kubernetes 服务 (AKS)| 快速入门 | [使用 Azure CLI 部署 Azure Kubernetes 服务 (AKS) 群集](/azure/aks/kubernetes-walkthrough) | 部署和管理 AKS 群集。  了解如何监视运行应用程序的群集和 Pod 的运行状况。
|Azure 批处理|示例 | [配合使用 Azure CLI 和 Azure Batch 来运行作业和任务](/azure/batch/scripts/batch-cli-sample-run-job) | 创建一个 Batch 作业，并将一系列任务添加到该作业。 监视作业及其任务。
|Azure 批处理|示例 | [使用 Azure CLI 在 Azure Batch 中创建并管理 Windows 池](/azure/batch/scripts/batch-cli-sample-manage-windows-pool) | 创建并管理使用云服务配置的 Windows 计算节点池。
|Azure 容器实例|快速入门 | [使用 Azure CLI 部署容器实例](/azure/container-instances/container-instances-quickstart) | 部署一个独立的 Docker 容器，使其应用程序可通过完全限定的域名 (FQDN) 使用。 执行单个部署命令，然后浏览到正在容器中运行的应用程序。
|Azure 函数|快速入门 |  [使用 Azure CLI 在 Azure 中创建用于响应 HTTP 请求的函数](/azure/azure-functions/functions-create-first-azure-function-azure-cli) | 使用命令行工具创建响应 HTTP 请求的函数。 在本地测试代码后，将函数部署到 Azure Functions 的无服务器环境。

## <a name="networking"></a>网络

| | | | |
|-|-|-|-|
|虚拟网络|快速入门 | [使用 Azure CLI 创建虚拟网络](/azure/virtual-network/quick-create-cli) | 创建虚拟网络，将两个虚拟机部署到虚拟网络中，并从 Internet 连接到虚拟机。
|虚拟网络|操作指南 | [使用 Azure CLI 在 Linux 虚拟机上启用加速网络](/azure/virtual-network/create-vm-accelerated-networking-cli) | 创建 Linux 虚拟机、处理虚拟功能的动态绑定和吊销，以及启用加速网络。

## <a name="internet-of-things"></a>物联网

| | | | |
|-|-|-|-|
|IoT 中心|教程 | [使用 Azure CLI 配置 IoT 中心消息路由](/azure/iot-hub/tutorial-routing) | 通过 IoT 中心设置并使用自定义路由查询。

## <a name="developer-tools"></a>开发人员工具

| | | | |
|-|-|-|-|
|Azure 应用配置|示例 |[适用于 Azure 应用配置的 Azure CLI 示例](/azure/azure-app-configuration/cli-samples) | 获取 bash 脚本的链接，这些脚本使用与 Azure 应用配置相对应的 Azure CLI。
|Azure DevOps| 入门：DevOps 管道 |[使用 Azure CLI 创建第一个 Azure 管道](/azure/devops/pipelines/create-first-pipeline-cli) | 在克隆的 GitHub 目录中创建新管道，以及管理并运行管道。
|Azure DevOps| 操作指南：DevOps 管道 |[使用 Azure CLI 进行的 Azure 管道部署任务](/azure/devops/pipelines/tasks/deploy/azure-cli?view=azure-devops) | 在生成或发布管道中，运行包含 Azure CLI 的 shell 或批处理脚本。  命令在跨平台代理上运行，而这些代理在 Linux、macOS 或 Windows 操作系统上运行。
|Azure DevOps| 教程：Jenkins 管道 |[配合使用 Azure CLI 和 Jenkins 来部署到 Azure 应用服务](/azure/jenkins/execute-cli-jenkins-pipeline) | 创建并配置 Jenkins 虚拟机、在 Azure 中创建 Web 应用，以及准备 GitHub 存储库。  创建并运行 Jenkins 管道。

## <a name="databases"></a>数据库

| | | | |
|-|-|-|-|
|SQL 数据库| 示例 |[使用 Azure CLI 配置 Azure SQL 数据库](/azure/sql-database/sql-database-cli-samples?tabs=single-database) | 适用于 Azure SQL 数据库的 Azure CLI 示例。
|MySQL|快速入门 |[使用 Azure CLI 创建 Azure Database for MySQL 服务器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli) | 创建 Azure Database for MySQL 服务器。  配置防火墙规则和 SSL 设置。  获取并使用连接信息。
|Cosmos DB |操作指南 |[使用 Azure CLI 管理 Azure Cosmos 资源](/azure/cosmos-db/manage-with-cli) | 使用常用命令来自动管理 Azure Cosmos DB 帐户、数据库和容器。
|Cosmos DB |示例 |[适用于 Azure Cosmos DB SQL (Core) API 的 Azure CLI 示例](/azure/cosmos-db/cli-samples) | 获取适用于 Azure Cosmos DB SQL (Core) API 的示例 Azure CLI 脚本的链接。

## <a name="analytics"></a>Analytics

| | | | |
|-|-|-|-|
Azure 事件中心 |快速入门 |[使用 Azure CLI 创建事件中心](/azure/event-hubs/event-hubs-quickstart-cli) | 创建事件中心命名空间和事件中心。
HDInsight |操作指南 |[使用 Azure CLI 创建 HDInsight 群集](/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli) | 创建 HDInsight 3.6 群集。
HDInsight |教程 |[使用 Azure CLI 管理 Azure HDInsight 群集](/azure/hdinsight/hdinsight-administer-use-command-line) | 列出、显示、删除和缩放 HDInsight 群集。

## <a name="management-and-governance"></a>管理和监管

| | | | |
|-|-|-|-|
资源管理器模板 |操作指南 |[使用 Azure 资源管理器模板和 Azure CLI 部署资源](/azure/azure-resource-manager/templates/deploy-cli) | 使用模板将资源部署到 Azure。
资源管理器组 |操作指南 |[使用 Azure CLI 管理 Azure 资源管理器资源组](/azure/azure-resource-manager/management/manage-resource-groups-cli) | 使用 Azure 资源管理器管理 Azure 资源组。
Resource Graph |快速入门 |[使用 Azure CLI 运行第一个 Resource Graph 查询](/azure/governance/resource-graph/first-query-azurecli) | 将 Azure Resource Graph 添加到 Azure CLI 安装，并运行第一个 Resource Graph 查询。
策略分配 |快速入门 |[使用 Azure CLI 创建策略分配以识别不符合的资源](/azure/governance/policy/assign-policy-azurecli) | 创建策略分配，以识别未使用托管磁盘的虚拟机。

## <a name="hybrid"></a>混合

| | | | |
|-|-|-|-|
Azure Stack Hub| 快速入门：Linux VM |[使用 Azure CLI 在 Azure Stack Hub 中创建 Linux 服务器虚拟机](/azure-stack/user/azure-stack-quick-create-vm-linux-cli) | 创建 Ubuntu Server 16.04 LTS 虚拟机，使用远程客户端连接到该虚拟机，并安装 NGINX Web 服务器。
Azure Stack Hub| 快速入门：Windows VM |[使用 Azure CLI 在 Azure Stack Hub 中创建 Windows Server 虚拟机](/azure-stack/user/azure-stack-quick-create-vm-windows-cli) |创建 Windows Server 2016 虚拟机，使用远程客户端连接到该虚拟机，并安装 IIS Web 服务器。
Azure Stack Hub| 操作指南：ASDK 资源 |[使用 Azure CLI 管理资源并将其部署到 Azure Stack Hub](/azure-stack/user/azure-stack-version-profiles-azurecli2) | 设置 Azure CLI，以便管理 Linux、Mac 和 Windows 客户端平台中的 Azure Stack 开发工具包 (ASDK) 资源。

## <a name="storage"></a>存储

| | | | |
|-|-|-|-|
Blob 存储 |快速入门 |  [使用 Azure CLI 创建、下载和列出 Blob](/azure/storage/blobs/storage-quickstart-blobs-cli) | 将数据上传到 Azure Blob 存储，以及从其下载数据。
Blob 存储 |操作指南 |[使用 Azure CLI 授予对 Blob 或队列数据的访问权限](/azure/storage/common/authorize-data-operations-cli) | 指定数据操作的授权方式，并为参数设置环境变量。
Blob 存储 |操作指南 |[使用 Azure CLI 管理 Azure Data Lake Storage Gen2（预览版）中的目录、文件和 ACL](/azure/storage/blobs/data-lake-storage-directory-file-acl-cli) | 在具有分层命名空间的存储帐户中创建并管理目录、文件和权限。
文件存储 |快速入门 |[使用 Azure CLI 创建并管理 Azure 文件共享](/azure/storage/files/storage-how-to-use-files-cli) | 创建并使用 Azure 文件共享。  创建并管理共享快照。

## <a name="security"></a>安全性

| | | | |
|-|-|-|-|
服务主体 |操作指南 |[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli) | 使用 Azure CLI 创建和重置服务主体以及获取其信息。
RBAC |操作指南 |[使用 Azure RBAC 和 Azure CLI 添加或删除角色分配](/azure/role-based-access-control/role-assignments-cli) | 将角色分配给 Azure 的基于角色的访问控制。
Key Vault |操作指南 |[使用 Azure CLI 管理 Key Vault](/azure/key-vault/key-vault-manage-with-cli2) | 创建并管理 Azure Key Vault。  注册并授权应用程序、设置高级访问策略，以及学习跨平台命令行界面命令。
Key Vault |教程 |[使用 Key Vault 和 Azure CLI 管理存储帐户密钥](/azure/key-vault/key-vault-ovw-storage-keys) | 管理存储帐户密钥，以及生成共享访问签名令牌。

## <a name="ai--machine-learning"></a>AI + 机器学习

| | | | |
|-|-|-|-|
机器学习 |参考 |[使用 Azure 机器学习的 Azure CLI 扩展](/azure/machine-learning/reference-azure-machine-learning-cli) | 运行试验以创建机器学习模型，并注册供客户使用的机器学习模型。  打包、部署和跟踪机器学习模型的生命周期。
认知服务 |操作指南 |[使用 Azure CLI 创建认知服务资源](/azure/cognitive-services/cognitive-services-apis-create-account-cli) | 注册 Azure 认知服务，并创建具有单服务订阅或多服务订阅的帐户。  使用生成的密钥和终结点对应用程序进行身份验证。
Azure Monitor |操作指南 |[使用 Azure CLI 创建 Log Analytics 工作区](/azure/azure-monitor/learn/quick-create-workspace-cli) | 创建并部署 Log Analytics 工作区。

## <a name="geographies"></a>地理区域

| | | | |
|-|-|-|-|
Azure 德国 |入门 |[Connect to Azure Germany by using the Azure CLI](/azure/germany/germany-get-started-connect-with-cli)（使用 Azure CLI 连接到 Azure 德国） | 使用 Azure 德国，可以通过脚本管理大型订阅，以及访问当前在全球 Azure 门户中不可用的功能。
Azure Government|入门 |[连接到具有 Azure CLI 的 Azure 政府](/azure/azure-government/documentation-government-get-started-connect-with-cli)|访问并开始管理 Azure 政府中的资源。

## <a name="see-also"></a>另请参阅

* [Azure CLI 入门](get-started-with-azure-cli.md)
* [Azure CLI 的完整命令参考列表](/cli/azure/reference-index)
* [Azure CLI 可以管理的服务](azure-services-the-azure-cli-can-manage.md)
