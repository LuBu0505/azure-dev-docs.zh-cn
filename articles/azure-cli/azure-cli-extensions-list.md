---
title: Azure CLI 的可用扩展
description: 官方支持的 Azure CLI 扩展的完整列表。
author: haroldrandom
ms.author: jianzen
manager: yonzhan,yungezz
ms.date: 04/16/2020
ms.topic: article
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azure-cli
ms.openlocfilehash: 531ea056b276cc12d5807ed62baa4d3c6044c26e
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030949"
---
# <a name="available-extensions-for-the-azure-cli"></a>Azure CLI 的可用扩展

本文是 Microsoft 支持的 Azure CLI 可用扩展的完整列表。

也可以通过 CLI 获得该扩展列表。 若要获取该列表，请运行 [az extension list-available](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)：

```azurecli-interactive
az extension list-available --output table
```

| 名称 | 版本 | 总结 | 预览 |
|------|---------|---------|---------|
| [aem](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | 管理适用于 SAP 的 Azure 增强型监视扩展 |  |
| [ai-examples](https://github.com/Azure/azure-cli-extensions/tree/master/src/ai-examples) | 0.2.0 | 向帮助内容添加 AI 支持的示例。 | 是 |
| [aks-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview) | 0.4.43 | 提供即将推出的 AKS 功能的预览版 | 是 |
| [alertsmanagement](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具警报扩展 |  |
| [alias](https://github.com/Azure/azure-cli-extensions) | 0.5.2 | 支持命令别名 | 是 |
| [application-insights](https://github.com/Azure/azure-cli-extensions/tree/master/src/application-insights) | 0.1.6 | 对管理 Application Insights 组件以及从这样的组件查询指标、事件和日志的支持。 | 是 |
| [azure-batch-cli-extensions](https://github.com/Azure/azure-batch-cli-extensions) | 5.0.1 | 用于操作 Azure Batch 服务的其他命令 |  |
| [azure-cli-iot-ext](https://github.com/azure/azure-iot-cli-extension) | 0.8.9 | 适用于 Azure CLI 的 Azure IoT 扩展。 |  |
| [azure-cli-ml](https://docs.microsoft.com/azure/machine-learning/service/) | 1.3.0 | Microsoft Azure 命令行工具 AzureML 命令模块 |  |
| [azure-devops](https://github.com/Microsoft/azure-devops-cli-extension) | 0.18.0 | 用于管理 Azure DevOps 的工具。 |  |
| [azure-firewall](https://github.com/Azure/azure-cli-extensions/tree/master/src/azure-firewall) | 0.3.1 | 管理 Azure 防火墙资源。 | 是 |
| [azure-iot](https://github.com/azure/azure-iot-cli-extension) | 0.9.1 | 适用于 Azure CLI 的 Azure IoT 扩展。 |  |
| [blueprint](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具蓝图扩展 |  |
| [connectedmachine](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 命令行工具 Connectedmachine 扩展 | 是 |
| [connection-monitor-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/connection-monitor-preview) | 0.1.0 | Microsoft Azure 命令行连接监视器 V2 扩展 | 是 |
| [csvmware](https://github.com/Azure/az-vmware-cli) | 0.3.0 | 管理 Azure VMware Solution by CloudSimple。 | 是 |
| [databox](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具 DataBox 扩展 |  |
| [databricks](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Microsoft Azure 命令行工具 DatabricksClient 扩展 |  |
| [db-up](https://github.com/Azure/azure-cli-extensions/tree/master/src/db-up) | 0.1.13 | 用于简化 Azure 数据库工作流的其他命令。 | 是 |
| [deploy-to-azure](https://github.com/Azure/deploy-to-azure-cli-extension) | 0.2.0 | 使用 Github Actions 部署到 Azure。 | 是 |
| [dev-spaces](https://github.com/Azure/azure-cli-extensions) | 1.0.5 | Dev Spaces 为团队提供快速、迭代的 Kubernetes 开发体验。 |  |
| [dev-spaces-preview](https://github.com/Azure/azure-cli-extensions) | 0.1.6 | Dev Spaces 为团队提供快速、迭代的 Kubernetes 开发体验。 | 是 |
| [dms-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/dms-preview) | 0.11.0 | 支持新的数据库迁移服务方案。 | 是 |
| [eventgrid](https://github.com/Azure/azure-cli-extensions) | 0.4.7 | Microsoft Azure 命令行工具 EventGrid 命令模块。 | 是 |
| [express-route](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route) | 0.1.3 | 使用预览功能管理 ExpressRoutes。 | 是 |
| [express-route-cross-connection](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route-cross-connection) | 0.1.1 | 使用 ExpressRoute 跨连接管理客户的 ExpressRoute 线路。 |  |
| [front-door](https://github.com/Azure/azure-cli-extensions/tree/master/src/front-door) | 1.0.6 | 管理网络 Front Door。 |  |
| [hack](https://github.com/Azure/azure-cli-extensions) | 0.4.2 | Microsoft Azure 命令行工具 Hack 扩展 | 是 |
| [healthcareapis](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | Microsoft Azure 命令行工具 HealthCareApis 扩展 |  |
| [hpc-cache](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具 StorageCache 扩展 | 是 |
| [image-copy-extension](https://github.com/Azure/azure-cli-extensions) | 0.2.3 | 对在区域之间复制托管 VM 映像的支持 |  |
| [交互](https://github.com/Azure/azure-cli) | 0.4.4 | Microsoft Azure 命令行交互 Shell | 是 |
| [internet-analyzer](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc5 | Microsoft Azure 命令行工具 Internet 分析器扩展 | 是 |
| [ip-group](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 命令行工具 IpGroup 扩展 |  |
| [keyvault-preview](https://github.com/Azure/azure-keyvault-cli-extension) | 0.1.3 | 预览 Azure Key Vault 命令。 | 是 |
| [log-analytics](https://github.com/Azure/azure-cli-extensions/tree/master/src/log-analytics) | 0.1.4 | 支持 Azure Log Analytics 查询功能。 | 是 |
| [维护](https://github.com/Azure/azure-cli-extensions) | 1.0.1 | 支持 Azure 维护管理。 |  |
| [managementpartner](https://github.com/Azure/azure-cli-extensions) | 0.1.2 | 支持“管理合作伙伴”预览版 |  |
| [mesh](https://github.com/Azure/azure-cli-extensions) | 0.10.6 | 支持 Microsoft Azure Service Fabric 网格 - 公共预览版 | 是 |
| [mixed-reality](https://github.com/Azure/azure-cli-extensions) | 0.0.1 | 混合现实 Azure CLI 扩展。 |  |
| [netappfiles-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/netappfiles-preview) | 0.3.2 | 提供即将推出的 Azure NetApp 文件 (ANF) 功能的预览版。 | 是 |
| [notification-hub](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Microsoft Azure 命令行工具通知中心扩展 |  |
| [peering](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc2 | Microsoft Azure 命令行工具 Peering 扩展 | 是 |
| [powerbidedicated](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Microsoft Azure 命令行工具 PowerBIDedicated 扩展 | 是 |
| [privatedns](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | 用于管理专用 DNS 区域的命令 | 是 |
| [resource-graph](https://github.com/Azure/azure-cli-extensions/tree/master/src/resource-graph) | 1.1.0 | 支持使用 Resource Graph 查询 Azure 资源。 | 是 |
| [sap-hana](https://github.com/Azure/azure-hanaonazure-cli-extension) | 0.5.9 | 适用于 SAP HanaOnAzure 实例的其他命令。 |  |
| [spring-cloud](https://github.com/Azure/azure-cli-extensions) | 0.2.2 | Microsoft Azure 命令行工具 spring-cloud 扩展 | 是 |
| [storage-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview) | 0.2.10 | 提供即将推出的存储功能的预览版。 | 是 |
| [storagesync](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具 MicrosoftStorageSync 扩展 |  |
| [stream-analytics](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具 stream-analytics 扩展 |  |
| [subscription](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | 支持订阅管理预览版。 |  |
| [support](https://github.com/azure/azure-cli-extensions/tree/master/src/support) | 1.0.1 | Microsoft Azure 命令行工具支持扩展 |  |
| [synapse](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Microsoft Azure 命令行工具 Synapse 扩展 | 是 |
| [virtual-network-tap](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-network-tap) | 0.1.0 | 管理虚拟网络分路器 (VTAP)。 | 是 |
| [virtual-wan](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-wan) | 0.1.3 | 管理虚拟 WAN、中心、VPN 网关和 VPN 站点。 | 是 |
| [vm-repair](https://github.com/Azure/azure-cli-extensions/tree/master/src/vm-repair) | 0.2.6 | 用于修复 VM 的自动修复命令。 |  |
| [vmware](https://github.com/virtustream/azure-vmware-virtustream-cli-extension) | 0.5.5 | 预览 Azure VMware Solution by Virtustream 命令。 | 是 |
| [webapp](https://github.com/Azure/azure-cli-extensions/tree/master/src/webapp) | 0.2.24 | 用于 Azure 应用服务的其他命令。 | 是 |