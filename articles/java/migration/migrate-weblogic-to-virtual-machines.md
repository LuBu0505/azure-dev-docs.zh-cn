---
title: 将 WebLogic 应用程序迁移到 Azure 虚拟机
description: 本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure 虚拟机上运行时应注意的事项。
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 10edb96e4e0781945da85d5a872b14178db3122f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673513"
---
# <a name="migrate-weblogic-applications-to-azure-virtual-machines"></a>将 WebLogic 应用程序迁移到 Azure 虚拟机

本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure 虚拟机上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

### <a name="define-what-you-mean-by-migration-complete"></a>定义“迁移完成”的含义

可以从本指南和相应的 Azure 市场套餐着手，加快将 WebLogic Server 工作负载迁移到 Azure。 定义迁移工作的范围很重要。 例如，你是否要严格按要求从现有的基础结构“直接迁移”到 Azure 虚拟机？ 如果答案为是，则在迁移过程中，你可能想要尝试使用一些“迁移加改进”措施。

考虑到本指南中详述的必要更改，最好是尽可能进行单纯的“直接迁移”。 定义“迁移完成”的含义，这样就可以知道自己何时到达了该里程碑。 “迁移完成”以后，即可拍摄虚拟机的快照，如[创建快照](/azure/virtual-machines/windows/snapshot-copy-managed-disk)中所述。 验证是否可以成功地从快照还原后，即可更安全地进行改进，而不必担心会丢失迄今为止的迁移进度。

### <a name="determine-whether-the-pre-built-marketplace-offers-are-a-good-starting-point"></a>确定是否可以从预生成的市场套餐着手

Oracle 和 Microsoft 进行了合作，将一组 Azure 解决方案模板引入 Azure 市场，为迁移到 Azure 提供坚实的基础。 请参阅 [Oracle Fusion Middleware](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/) 文档以获取套餐列表，选择与现有部署最匹配的套餐。 可以[在 Oracle 文档中](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/select-required-oracle-weblogic-server-offer-azure-marketplace.html#GUID-187739C5-EE7A-47C6-B3BA-C0A0333DC398)看到套餐列表

如果不好从现有的套餐着手，则需使用 Azure 虚拟机资源以手动方式重新生成部署。 有关详细信息，请参阅[什么是 IaaS？](https://azure.microsoft.com/overview/what-is-iaas/)。

### <a name="determine-whether-the-weblogic-version-is-compatible"></a>确定 WebLogic 版本是否兼容

现有的 WebLogic 版本必须与 IaaS 套餐中的版本兼容。 此查询会显示 [WebLogic 版本 12.2.1.3](https://azuremarketplace.microsoft.com/marketplace/apps?search=oracle%20weblogic%2012.2.1.3&page=1) 的套餐。 如果现有的 WebLogic 版本与该版本不兼容，则需使用 Azure IaaS 资源以手动方式重新生成部署。 有关详细信息，请参阅 [Azure 文档](https://azure.microsoft.com/overview/what-is-iaas/)。

[!INCLUDE [inventory-server-capacity-virtual-machines](includes/inventory-server-capacity-virtual-machines.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly](includes/validate-that-the-supported-java-version-works-correctly.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

[!INCLUDE [domain-configuration](includes/domain-configuration.md)]

[!INCLUDE [determine-whether-session-replication-is-used](includes/determine-whether-session-replication-is-used.md)]

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-management-over-rest-is-used](includes/determine-whether-management-over-rest-is-used.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use-virtual-machines](includes/determine-whether-jms-queues-or-topics-are-in-use-virtual-machines.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [determine-whether-wlst-is-used](includes/determine-whether-wlst-is-used.md)]

[!INCLUDE [validate-whether-and-how-the-file-system-is-used](includes/validate-whether-and-how-the-file-system-is-used.md)]

[!INCLUDE [determine-the-network-topology](includes/determine-the-network-topology.md)]

[!INCLUDE [account-for-the-use-of-jca-adapters-and-resource-adapters](includes/account-for-the-use-of-jca-adapters-and-resource-adapters.md)]

[!INCLUDE [account-for-the-use-of-custom-security-providers-and-jaas](includes/account-for-the-use-of-custom-security-providers-and-jaas.md)]

[!INCLUDE [determine-whether-weblogic-clustering-is-used](includes/determine-whether-weblogic-clustering-is-used.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-used](includes/determine-whether-the-java-ee-application-client-feature-is-used.md)]

## <a name="migration"></a>迁移

### <a name="select-a-weblogic-on-azure-virtual-machines-offer"></a>选择基于 Azure 虚拟机套餐的 WebLogic

以下套餐适用于基于 Azure 虚拟机的 WebLogic。

在部署套餐的过程中，系统会要求你选择 WebLogic Server 节点的虚拟机大小。 在选择 VM 大小时，请务必考虑有关大小调整的所有方面（内存、处理器、磁盘）。 有关详细信息，请参阅[有关套餐的文档](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/deploy-oracle-weblogic-server-administration-server-single-node.html)和[有关虚拟机大小调整的 Azure 文档](/azure/cloud-services/cloud-services-sizes-specs)

#### <a name="weblogic-server-single-node-with-no-admin-server"></a>没有管理服务器的 WebLogic Server 单一节点

此套餐创建单个 VM，并在其上安装 WebLogic，但不配置任何域，这适用于你有高度自定义域配置的情况。

#### <a name="weblogic-server-single-node-with-admin-server"></a>具有管理服务器的 WebLogic Server 单一节点

此套餐预配单个 VM，并在其上安装 WebLogic Server 12.1.2.3。 它创建一个域并启动管理服务器。

#### <a name="weblogic-server-n-node-cluster"></a>WebLogic Server N 节点群集

此套餐创建 WebLogic Server VM 的高度可用群集。

#### <a name="weblogic-server-n-node-dynamic-cluster"></a>WebLogic Server N 节点动态群集

此套餐创建 WebLogic Server VM 的高度可用且可缩放的动态群集

### <a name="provision-the-offer"></a>预配套餐

选择了要开始使用的套餐后，请按[套餐文档](https://wls-eng.github.io/arm-oraclelinux-wls/)中的说明预配该套餐。 请确保选择与现有域名匹配的域名。 你甚至可以将域密码与现有域密码匹配。

### <a name="migrate-the-domains"></a>迁移域

预配套餐后，可检查域配置并通过[此指南](https://support.oracle.com/knowledge/Middleware/2336356_1.html)详细了解如何迁移域。

### <a name="connect-the-databases"></a>连接数据库

迁移域以后，可以按[套餐文档中](https://wls-eng.github.io/arm-oraclelinux-wls/#connecting-a-database-to-a-cluster)的说明连接数据库。 可以根据这些说明来考虑涉及的任何数据库机密和访问字符串。

### <a name="account-for-keystores"></a>考虑密钥存储

必须考虑应用程序所用的任何 SSL 密钥存储的迁移。 有关详细信息，请参阅 [Configuring Keystores](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/secmg/identity_trust.html#GUID-7F03EB9C-9755-430B-8B86-17199E0C01DC)（配置密钥存储）。

### <a name="connect-the-jms-sources"></a>连接 JMS 源

连接数据库后，可以按照 WebLogic 文档的 [Fusion Middleware Administering JMS Resources for Oracle WebLogic Server](https://docs.oracle.com/middleware/12213/wls/JMSAD/toc.htm)（Fusion Middleware：管理 Oracle WebLogic Server 的 JMS 资源）中的说明配置 JMS。

### <a name="account-for-logging"></a>考虑日志记录

在迁移域时，应一并迁移现有的日志记录配置。 有关详细信息，请参阅 [Configure java.util.logging logger levels](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlach/taskhelp/logging/ConfigureJavaLoggingLevels.html)（配置 java.util.logging 记录器级别）和 [Configuring Log Files and Filtering Log Messages for Oracle WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wllog/index.html)（为 Oracle WebLogic Server 配置日志文件和筛选日志消息）

### <a name="migrating-your-applications"></a>迁移应用程序

用于将开发团队的应用程序部署到测试、过渡和生产服务器的技术因情况的不同而差异很大。 在某些情况下，可以通过高度演化的 CI/CD 平台将应用程序部署到 WebLogic Server。 在其他情况下，该过程可能需要更多的手动操作。 使用 Azure 虚拟机将 WebLogic 应用程序迁移到云的一个优点是，现有进程可以继续使用。

必须配置该套餐预配的网络安全组，使之允许从 CI/CD 管道或手动部署系统进行访问。 有关详细信息，请参阅 Azure 文档中的[安全组](/azure/virtual-network/security-overview)。

### <a name="testing"></a>测试

针对应用程序的任何容器内测试都必须配置为访问 Azure 中运行的新服务器。 如果涉及 CI/CD，则必须确保所需的网络安全规则允许你的测试访问已部署到 Azure 的应用程序。 有关详细信息，请参阅 Azure 文档中的[安全组](/azure/virtual-network/security-overview)。

## <a name="post-migration"></a>迁移后

实现在[预迁移](#pre-migration)步骤中定义的迁移目标后，请执行某些端到端验收测试，验证一切是否按预期工作。 显然，迁移后增强功能的一些主题包括但不限于以下方面：

* 使用 Azure 存储提供装载到虚拟机的静态内容。 有关详细信息，请参阅[在虚拟机中附加或拆离数据磁盘](/azure/lab-services/devtest-lab-attach-detach-data-disk)。

* 通过 Azure DevOps 将应用程序部署到已迁移的 WebLogic 群集。 有关详细信息，请参阅 [Azure DevOps 入门文档](/azure/devops/get-started/?view=azure-devops)。

* 通过高级负载均衡服务增强网络拓扑。 有关详细信息，请参阅[在 Azure 中使用负载均衡服务](/azure/traffic-manager/traffic-manager-load-balancing-azure)。

* 利用 Azure 托管标识管理机密，并分配基于角色的访问权限以访问 Azure 资源。 有关详细信息，请参阅[什么是 Azure 资源的托管标识？](/azure/active-directory/managed-identities-azure-resources/overview)。

* 将 WebLogic Java EE 身份验证和授权与 Azure Active Directory 集成。 有关详细信息，请参阅[集成 Azure Active Directory 入门指南](/azure/active-directory/manage-apps/plan-an-application-integration)。

* 使用 Azure Key Vault 存储充当“机密”的任何信息。 有关详细信息，请参阅 [Azure Key Vault 基本概念](/azure/key-vault/basic-concepts)。
