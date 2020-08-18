---
title: 教程：使用 Azure 应用程序网关作为负载均衡器将 WebLogic Server 群集迁移到 Azure
description: 本教程介绍如何使用 Azure 应用程序网关作为负载均衡器将 WebLogic Server 部署到 Azure
author: edburns
ms.author: edburns
ms.topic: tutorial
ms.date: 08/05/2020
ms.openlocfilehash: 166e6f90218eb519242da0d89ae6146a2d589863
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057533"
---
# <a name="tutorial-migrate-a-weblogic-server-cluster-to-azure-with-azure-application-gateway-as-a-load-balancer"></a>教程：使用 Azure 应用程序网关作为负载均衡器将 WebLogic Server 群集迁移到 Azure

本教程介绍使用 Azure 应用程序网关部署 WebLogic Server (WLS) 的过程。  其中涉及特定步骤来创建 Key Vault、在 Key Vault 中存储 SSL 证书和使用该证书终止 SSL。  尽管所有这些元素均在各自的文档中得到很好的阐述，但本教程展示了一种特定的方式，将所有这些元素结合起来，以便为 Azure 上的 WLS 创建简单却又功能强大的负载均衡解决方案。

:::image type="content" border="false" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-key-vault.png" alt-text="显示 WLS、应用网关和 Key Vault 之间的关系的图形。":::

负载均衡是将 Oracle WebLogic Server 群集迁移到 Azure 时必不可少的一部分。  最简单的解决方案是使用 [Azure 应用程序网关](/azure/application-gateway/overview)的内置支持。  应用网关包含在 Azure 上的 WebLogic 群集支持中。  要简要了解 Azure 上的 WebLogic 群集支持，请参阅 [Azure 上的 Oracle WebLogic Server 是指什么？](/azure/virtual-machines/workloads/oracle/oracle-weblogic)。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建 Azure Key Vault
> * 创建 SSL 证书
> * 在 Key Vault 中存储 SSL 证书
> * 通过 Azure 应用程序网关将 WebLogic Server 部署到 Azure
> * 验证 WLS 和应用网关是否部署成功

## <a name="prerequisites"></a>必备知识

* 计算机上运行与 UNIX 类似的命令行环境的 [OpenSSL](https://www.openssl.org/)。

   本教程将使用 OpenSSL，不过，其他工具可能也可用于证书管理。 你可能会发现，许多 GNU/Linux 发行版（如 Ubuntu）中已捆绑 OpenSSL。
* 一个有效的 Azure 订阅。
  * 如果没有 Azure 订阅，可以[创建一个免费帐户](https://azure.microsoft.com/free/)。
* 能够部署 [Oracle WebLogic Server Azure 应用程序](/azure/virtual-machines/workloads/oracle/oracle-weblogic)中列出的 WLS Azure 应用程序之一。

## <a name="migration-context"></a>迁移上下文

下面是迁移本地 WLS 安装和 Azure 应用程序网关时需注意的一些事项。  虽然本教程的步骤是在 Azure 上的 WebLogic Server 群集签名建立负载均衡器的最简单的方法，但还有许多其他方法可实现此目的。  下表显示了其他注意事项。

* 如果有现有的负载均衡解决方案，请确保 Azure 应用程序网关可实现或超越其功能。  要查看摘要了解 Azure 应用程序网关的功能与其他 Azure 负载均衡解决方案的比较情况，请参阅 [Azure 中的负载均衡选项概述](/azure/architecture/guide/technology-choices/load-balancing-overview)。
* 如果现有的负载均衡解决方案可提供安全保护来抵御常见攻击和漏洞，则应用程序网关满足你的要求。 应用程序网关的内置 Web 应用程序防火墙 (WAF) 实施的是 [OWASP（开放式 Web 应用程序安全项目）核心规则集](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)。  要详细了解应用程序网关上的 WAF 支持，请参阅[应用程序网关功能](/azure/application-gateway/features#web-application-firewall)。
* 如果现有的负载均衡解决方案需要端到端 SSL 加密，则在执行本指南中的步骤后，还需要进行其他配置。  请查看[关于使用应用程序网关实现 TLS 终止和端到端 TLS 的概述](/azure/application-gateway/ssl-overview#end-to-end-tls-encryption)，以及关于[在 Oracle Fusion Middleware 中配置 SSL](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.3/asadm/configuring-ssl1.html#GUID-623906C0-B1FD-423F-AE51-061B5800E927) 的 Oracle 文档。
* 如果你正在针对云进行优化，本指南将介绍如何从头开始使用 Azure 应用网关和 WLS。
* 有关将 WebLogic Server 迁移到 Azure 虚拟机的全面调查，请参阅[将 WebLogic Server 应用程序迁移到 Azure 虚拟机](migrate-weblogic-to-virtual-machines.md)。

## <a name="create-an-azure-key-vault"></a>创建 Azure Key Vault

本教程说明如何使用 Azure 门户创建 Azure Key Vault。

1. 在 Azure 门户菜单或“主页”中，选择“创建资源” 。
1. 在“搜索”框中输入“Key Vault”。
1. 从结果列表中选择“Key Vault”。
1. 在“Key Vault”部分，选择“创建”。
1. 在“创建密钥保管库”部分，提供以下信息：
    * 订阅：选择订阅。
    * 在“资源组”下选择“新建”，然后输入资源组名称 。  记下 Key Vault 名称。  稍后在部署 WLS 时需要用到它。
    * **Key Vault 名称**：必须提供唯一的名称。  记下 Key Vault 名称。  稍后在部署 WLS 时需要用到它。
    > [!NOTE]
    > 可对资源组和 Key Vault 名称使用同一名称 。
    * 在“位置”下拉菜单中选择一个位置。
    * 让其他选项保留默认值。
1. 在完成时选择“下一步:访问策略”。
1. 在“启用访问”下，选择“用于模板部署的 Azure 资源管理器” 。
1. 选择“查看 + 创建”。
1. 选择“创建”。

Key Vault 创建是很轻量级的操作，通常两分钟内即可完成。  部署完成后，选择“转到资源”，然后继续下一部分。

## <a name="create-an-ssl-certificate"></a>创建 SSL 证书

本部分介绍如何以适用于由 Azure 上的 WebLogic 部署的应用程序网关的格式创建自签名 SSL 证书。  此证书必须具有非空密码。  如果你已具有采用 .pfx 格式的有效非空密码 SSL 证书，则可跳过此部分，转到下一部分。  如果现有的有效非空密码 SSL 证书不是 .pfx 格式，请先将其转换为 .pfx 文件，再跳到下一部分 。  否则，请打开命令行界面，输入以下命令。

> [!NOTE]
> 本部分介绍如何在将证书存储为 Key Vault 中的机密之前对证书进行 Base64 编码。  这是创建 WebLogic Server 和应用程序网关的基础 Azure 部署所必需的。

请按照以下步骤进行创建并对证书进行 Base64 编码：

1. 创建 `RSA PRIVATE KEY`

   ```bash
   openssl genrsa 2048 > private.pem
   ```
1. 创建对应的公钥。

   ```bash
   openssl req -x509 -new -key private.pem -out public.pem
   ```

   OpenSSL 工具进行提示时，你必须回答几个问题。  这些值将包含在证书中。  本教程使用自签名证书，因此这些值是不相关的。  以下文本值是正确的。
     1. 对于国家/地区名称，请输入由两个字母组成的代码。
     1. 对于省/自治区/直辖市的名称，请输入 WA。
     1. 对于组织名称，请输入 Contoso。  对于组织单位名，请输入“帐单”。
     1. 对于公用名，请输入 Contoso。
     1. 对于电子邮件地址，请输入 billing@contoso.com。

1. 将证书导出为 .pfx 文件

   ```bash
   openssl pkcs12 -export -in public.pem -inkey private.pem -out mycert.pfx
   ```

   输入两次密码。  记下密码。  稍后在部署 WLS 时需要用到它。

1. 对 mycert.pfx 文件进行 Base64 编码

   ```bash
   base64 mycert.pfx > mycert.txt
   ```

现在，你已创建了 Key Vault，并拥有具有非空密码的有效 SSL 证书，接下来可将该证书存储在 Key Vault 中。

## <a name="store-the-ssl-certificate-in-the-key-vault"></a>在 Key Vault 中存储 SSL 证书

本部分介绍如何将证书及其密码存储在上一部分创建的 Key Vault 中。

若要存储证书，请执行以下步骤：

1. 在 Azure 门户中，将光标放在页面顶部的搜索栏中，然后键入在本教程前面部分创建的 Key Vault 的名称。
1. Key Vault 应会出现在“资源”标题下。  选择该文件夹。
1. 在“设置”部分中，选择“机密” 。
1. 选择“生成/导入”。
1. 在“上传选项”下，保留默认值。
1. 在“名称”下，输入 `myCertSecretData` 或你喜欢的任意名称。
1. 在“值”下，输入 mycert.txt 文件的内容。  对于文本字段，值的长度和换行符的存在不会造成问题。
1. 将剩余值保留为默认值，并选择“创建”。

若要存储证书的密码，请执行以下步骤：

1. 你将回到“机密”页面。  选择“生成/导入”。
1. 在“上传选项”下，保留默认值。
1. 在“名称”下，输入 `myCertSecretPassword` 或你喜欢的任意名称。
1. 在“值”下，输入证书的密码。
1. 将剩余值保留为默认值，并选择“创建”。
1. 你将回到“机密”页面。

## <a name="deploy-weblogic-server-with-application-gateway-to-azure"></a>通过应用程序网关将 WebLogic Server 部署到 Azure

本部分将介绍如何使用在先前部分创建的 Key Vault、SSL 证书和密码。  你将预配一个 WLS 群集，其中 Azure 应用程序网关自动创建为群集节点的负载均衡器。  应用程序网关将使用所提供的 SSL 证书来终止 SSL。  有关使用应用程序网关终止 SSL 的高级详细信息，请参阅[关于使用应用程序网关实现 TLS 终止和端到端 TLS 的概述](/azure/application-gateway/ssl-overview)。

若要创建 WLS 群集和应用程序网关，请执行以下步骤：

1. 按照 [Oracle 文档](https://aka.ms/arm-oraclelinux-wls-cluster-oracle-docs)中所述，开始执行预配 WebLogic Server 群集的步骤，但到达“Azure 应用程序网关”边栏选项卡时请回到此页面，如此处所示。

   :::image type="content" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-blade.png" alt-text="显示 WLS、应用网关和 Key Vault 之间的关系的图形。":::

1. 在“Azure 应用程序网关”边栏选项卡中，选择“是” 。
1. 在“包含 KeyVault 的当前订阅的资源组名称”下，输入包含之前创建的 Key Vault 的资源组的名称。
1. 在“包含用于 SSL 终止的证书的机密的 Azure KeyVault 名称”下，输入 Key Vault 的名称。
1. 在“其值为 SSL 证书数据的指定 KeyVault 中的机密名称”下，输入 `myCertSecretData` 或之前输入的任何名称。
1. 在“其值为 SSL 证书密码的指定 KeyVault 中的机密名称”下，输入 `myCertSecretData` 或之前输入的任何名称。
1. 选择“查看 + 创建”。
1. 选择“创建”。  这将验证是否可从 Key Vault 获取证书，并且其密码是否与 Key Vault 中存储的密码值相匹配。  如果此验证步骤失败，请查看 Key Vault 的属性，确保已正确输入证书，并确保正确输入了密码。
1. 看到“验证通过”后，选择“创建” 。

这将开始创建 WLS 群集及其前端应用程序网关的过程，大约需要 15 分钟的时间。  部署完成后，请选择“转到资源组”。 从资源组的资源列表中，选择“myAppGateway”。

## <a name="validate-successful-deployment-of-wls-and-app-gateway"></a>验证 WLS 和应用网关是否部署成功

本部分介绍用于快速验证是否成功部署 WLS 群集和应用程序网关的技术。

如果已选择“转到资源组”，然后在上一部分末尾选择“myAppGateway”，则会看到应用程序网关的概述页面 。  如果没看到，可在 Azure 门户顶部的文本框中键入 `myAppGateway`，然后选择所显示的正确内容来找到该页面。  请确保在为 WLS 群集创建的资源组中选择一个。  然后，完成以下步骤。

1. 在“myAppGateway”的概述页面的左窗格中，向下滚动到“监视”部分，然后选择“后端运行状况”  。
1. “加载”消息消失后，屏幕中间会出现一张表，其中显示配置为后端池中节点的群集节点。
1. 验证每个节点的状态是否都是“正常运行”。

## <a name="clean-up-resources"></a>清理资源

如果不打算继续使用此 WLS 群集，请按照以下步骤删除 Key Vault 和 WLS 群集：

1. 请访问“myAppGateway”的概述页面，如上一部分中所示。
1. 在页面顶部的“资源组”文本下，选择资源组。
1. 选择“删除资源组”。
1. 输入焦点将置于标有“键入资源组名称”的字段上。  根据请求键入资源组名称。
1. 这将导致启用“删除”按钮。  选择“删除”按钮。  此操作需要一段时间，但可在删除过程中继续下一步操作。
1. 按照[在 Key Vault 中存储 SSL 证书]()部分的第一步操作来查找 Key Vault。
1. 选择“删除”。
1. 选择窗格中出现的“删除”。

## <a name="next-steps"></a>后续步骤

继续探索用于运行 Azure 上的 WLS 的选项。
> [!div class="nextstepaction"]
> [详细了解 Azure 上的 Oracle WebLogic](/azure/virtual-machines/workloads/oracle/oracle-weblogic)
