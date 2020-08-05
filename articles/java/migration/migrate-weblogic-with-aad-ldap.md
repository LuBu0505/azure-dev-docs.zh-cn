---
title: 使用 Azure Active Directory 进行最终用户授权和身份验证，用于将 WebLogic Server 上的 Java 应用迁移到 Azure
description: 本指南介绍如何配置 Oracle WebLogic Server 以通过 LDAP 与 Azure Active Directory 域服务连接
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: 0f3b8f7e2535bc91629f056cf59bc0d2658aba19
ms.sourcegitcommit: 1f78e54deb85c6063b887286a13a967d1d186b50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118437"
---
# <a name="end-user-authorization-and-authentication-for-migrating-java-apps-on-weblogic-server-to-azure"></a>用于将 WebLogic Server 上的 Java 应用迁移到 Azure 的最终用户授权和身份验证

本指南将帮助你使用 Azure Active Directory 为 WebLogic Server 上的 Java 应用启用企业级最终用户身份验证和授权。

Java EE 开发人员希望[标准平台安全机制](https://javaee.github.io/tutorial/security-intro.html#BNBWJ)能够“正常工作”，即使在将工作负载移至 Azure 时也是如此。  通过 [Oracle WebLogic Server (WLS) Azure 应用程序](/azure/virtual-machines/workloads/oracle/oracle-weblogic)，可使用 Azure Active Directory 域服务 (Azure AD DS) 中的用户填充内置安全领域。  在 Azure 应用程序上的 Java EE 中使用标准的 `<security-role>` 元素；用户信息从 Azure AD DS 通过轻型目录访问协议 (LDAP) 流出。

本指南分为两部分。  如果你公开了使用安全 LDAP 的 Azure AD DS，则可以直接跳到第二部分。

* [Azure Active Directory 配置](#azure-active-directory-configuration)
* [WLS 配置](#wls-configuration)

本指南介绍如何：

> [!div class="checklist"]
> * 创建和配置 Azure Active Directory 域服务托管域
> * 为 Azure AD DS 托管域配置安全轻型目录访问协议 (LDAP)
> * 使 WebLogic Server 能够访问作为其默认安全领域的 LDAP

本指南不会帮助你重新配置现有的 Azure AD 部署，但你可以按照本指南进行操作，并查看可以跳过哪些步骤。

## <a name="prerequisites"></a>先决条件

* 一个有效的 Azure 订阅。
  * 如果你没有 Azure 订阅，请[创建一个帐户](https://azure.microsoft.com/free/)。
* 能够部署 [Oracle WebLogic Server Azure 应用程序](/azure/virtual-machines/workloads/oracle/oracle-weblogic)中列出的 WLS Azure 应用程序之一。

## <a name="migration-context"></a>迁移上下文

以下是一些关于迁移本地 WLS 安装和 Azure AD 需要考虑的事项。

* 如果你通过 LDAP 公开了一个不具有域服务的 Azure AD 租户，本指南将说明如何公开 LDAP 功能并将其与 WLS 集成。
* 如果你的场景涉及本地 Active Directory 林，请考虑使用 Azure AD 实现混合标识解决方案。  有关详细信息，请参阅[混合标识文档](/azure/active-directory/hybrid/)
* 如果已部署本地 Active Directory 域服务 (AD DS)，请访问[自我管理型 Azure Active Directory 域服务、Azure Active Directory 和托管型 Azure Active Directory 域服务的比较](/azure/active-directory-domain-services/compare-identity-solutions)，以探索迁移路径。
* 如果你正在针对云进行优化，本指南将介绍如何从头开始使用 Azure AD DS LDAP 和 WLS。

## <a name="azure-active-directory-configuration"></a>Azure Active Directory 配置

本部分将引导你完成设置与 WLS 集成的 Azure AD DS 实例的所有步骤。  Azure Active Directory 不直接支持轻型目录访问协议 (LDAP) 或安全 LDAP。 支持而是通过 Azure AD 租户内的 Azure AD 域服务 (Azure AD DS) 实例启用的。

>[!NOTE]
> 本指南使用 Azure AD DS 的“仅限云”用户帐户功能。  支持其他用户帐户类型，但本指南中不作介绍。

### <a name="create-and-configure-an-azure-active-directory-domain-services-managed-domain"></a>创建和配置 Azure Active Directory 域服务托管域

本部分将指导你通过单独的教程来设置 Azure AD DS 托管域。

完成[创建和配置 Azure Active Directory 域服务托管域](/azure/active-directory-domain-services/tutorial-create-instance)教程，直至但不包括[为 Azure AD DS 启用用户帐户](/azure/active-directory-domain-services/tutorial-create-instance#enable-user-accounts-for-azure-ad-ds)一节。  这一节需要在本教程的上下文中进行特殊处理，如下一部分所述。  请确保完全正确地完成 DNS 操作。

在完成步骤“输入托管域的 DNS 域名”时，记下指定的值。  在本文后面部分将使用它。

### <a name="create-users-and-reset-passwords"></a>创建用户和重置密码

本节包括创建用户和更改其密码的步骤，这是使用户通过 LDAP 成功传播所必需的。  如果你有现有的 Azure AD DS 安装，则可能不需要执行此步骤。

1. 在 Azure 门户中，确保与 Azure AD 租户对应的目录是当前活动目录。  若要了解如何选择正确的目录，请参阅[将 Azure 订阅关联或添加到 Azure Active Directory 租户](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)。  如果选择了不正确的目录，你将无法创建用户，或者将在错误的目录中创建用户。
1. 在 Azure 门户顶部的搜索框中，输入“用户”。
1. 选择“新建用户”。
1. 确保已选中“创建用户”。
1. 填写用户名、名称、名字和姓氏的值。  让其余字段保留其默认值。
1. 选择“创建”。
1. 在表中选择新创建的用户。
1. 选择“重置密码”。
1. 在出现的面板中，选择“重置密码”。
1. 记下临时密码。
1. 在“incognito”浏览器窗口中，访问 [Azure 门户](https://portal.azure.com/)，并使用用户的凭据和密码登录。
1. 出现提示时，更改密码。  记下新密码。  稍后将使用它。
1. 注销并关闭“incognito”窗口。

为每个要启用的用户重复从“选择新用户”到“注销并关闭”的步骤。

### <a name="allow-ldap-in-azure-ad-ds"></a>允许在 Azure AD DS 中使用 LDAP

本节将指导你完成一个单独的教程，以提取用于配置 WLS 的值。

首先，在单独的浏览器窗口中打开教程[为 Azure Active Directory 域服务托管域配置安全 LDAP](/azure/active-directory-domain-services/tutorial-configure-ldaps)，这样你可以在学习该教程时查看以下变化。  

当你到达[为客户端计算机导出证书](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers)一节时，请记下你保存以 .cer 结尾的证书文件的位置。  我们将使用该证书作为 WLS 配置的输入。

当你到达[锁定通过 Internet 进行的安全 LDAP 访问](/azure/active-directory-domain-services/tutorial-configure-ldaps#lock-down-secure-ldap-access-over-the-internet)一节时，指定“任意”作为源。  稍后在本指南中，我们将使用特定的 IP 地址来加强安全规则。

在执行[测试对托管域的查询](/azure/active-directory-domain-services/tutorial-configure-ldaps#test-queries-to-the-managed-domain)中的步骤之前，请执行以下步骤以使测试成功。

   1. 在门户中，访问 Azure AD 域服务实例的概述页面。
   1. 在“设置”区域中选择“属性”。
   1. 在页面的右侧，向下滚动直到看到“管理组”。  此标题下应该有一个“AAD DC 管理员”的链接。  选择该链接。
   1. 在“管理部分”中选择“成员” 。
   1. 选择“添加成员”。
   1. 在“搜索”文本字段中，输入一些字符以查找在前面步骤中创建的一个用户。
   1. 选择该用户，然后激活“选择”按钮。
   1. 在执行“测试对托管域的查询”一节中的步骤时，必须使用此用户。
   >[!NOTE]
   >
   > 这里有一些关于查询 LDAP 数据的提示，你需要执行这些操作以收集 WLS 配置所必需的一些值。
   >
   > * 本教程建议使用 Windows 程序 LDP.exe。  也可以使用 [Apache Directory Studio](https://directory.apache.org/studio/downloads.html) 来实现同样的目的。
   > * 当使用 LDP.exe 登录到 LDAP 时，用户名只是 @ 之前的部分。  例如，如果用户是 `alice@contoso.onmicrosoft.com`，则 LDP.exe 绑定操作的用户名是 `alice`。  此外，让 LDP.exe 保持运行并登录，以便在后续步骤中使用。
   >
在[为外部访问配置 DNS 区域](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access)一节中，记下“安全 LDAP 外部 IP 地址”的值。  稍后将使用它。

请不要执行[清理资源](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources)中的步骤，除非本指南中有指示。

考虑到上述变化，请完成[为 Azure Active Directory 域服务托管域配置安全 LDAP](/azure/active-directory-domain-services/tutorial-configure-ldaps)。  我们现在可以收集必要的值以提供给 WLS 配置。

## <a name="wls-configuration"></a>WLS 配置

本节帮助你从先前部署的 Azure AD DS 收集参数值。

部署 [Oracle WebLogic Server Azure 应用程序](/azure/virtual-machines/workloads/oracle/oracle-weblogic)中列出的任何 Azure 应用程序时，可选择让部署自动连接到预先存在的 LDAP 服务器。  或者，你可以稍后通过调用 Active Directory 集成子模板来配置 LDAP 连接。  此方法在[官方文档](https://wls-eng.github.io/arm-oraclelinux-wls/)的附录 A 中有所介绍。  无论采用哪种方式，你都必须有必要的参数值以传递给 ARM 模板。

| 参数名称 | 说明   | 详细信息 | 
|----------------|---------------|---------|
| `aadsServerHost` | 服务器主机 | 此值是你在完成[创建和配置 Azure Active Directory 域服务托管域](/azure/active-directory-domain-services/tutorial-create-instance)时保存的公用 DNS 名称。 |
| `aadsPublicIP` | 安全 LDAP 外部 IP 地址 | 此值是你在[为外部访问配置 DNS 区域](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access)一节中保存的“安全 LDAP 外部 IP 地址”。|
| `wlsLDAPPrincipal` | 主体   | 返回到 LDP.exe。  执行以下步骤以获取 `wlsLDAPPrincipal` 的附加值。 <ol><li>在“视图”菜单中，选择“树” 。</li><li>在“树状视图”对话框中，将“BaseDN”保留为空，然后选择“确定”  。</li><li>在右侧窗格中单击右键并选择“清除输出”。</li><li>展开左侧的树状视图并选择以“OU=AADDC Users”开头的条目。</li><li>在“浏览”菜单中，选择“搜索” 。</li><li>在出现的对话框中，接受默认值并选择“运行”。</li><li>输出显示在右侧窗格后，选择“运行”旁边的“关闭” 。</li><li>扫描输出中与你添加到“AAD DC 管理员”组的用户对应的 Dn 条目。  它将以“Dn:CN=&lt;用户名&gt;OU=AADDC Users”开头。</li></ol> |
| `wlsLDAPGroupBaseDN` 和 `wlsLDAPUserBaseDN` | 用户基 DN 和组基 DN | 为实现本教程的目的，这两个属性的值都是相同的：第一个逗号后的“wlsLDAPPrincipal”部分。|
| `wlsLDAPPrincipalPassword` | 主体密码 | 此值是已添加到“AAD DC 管理员”组的用户的密码。 |
| `wlsLDAPProviderName` | Provider Name | 此值可以保留为默认值。  它用作 WLS 中身份验证提供程序的名称。 |
| `wlsLDAPSSLCertificate` | SSL 配置的信任密钥存储 | 此值是你在完成[为客户端计算机导出证书](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers)步骤时系统要求你保存的 Base64 编码的 .cer 文件。  可以使用以下 UNIX 或 PowerShell 命令获取此值。 <br /> Bash： <br /> `base64 your-certificate.cer -w 0 >temp.txt` <br /> PowerShell： <br /> `$Content = Get-Content -Path .\your-certificate.cer -Encoding Byte`<br /> `$Base64 = [System.Convert]::ToBase64String($Content)` <br /> `$Base64 | Out-File .\temp.txt`

### <a name="integrating-azure-ad-ds-ldap-with-wls"></a>将 Azure AD DS LDAP 与 WLS 集成

有了上述配置值，并且部署了 Azure AD DS LDAP，现在就可以启动配置了。  可使用两种方法完成此过程。

#### <a name="during-wls-deployment"></a>在 WLS 部署过程中

访问 [Oracle WebLogic Server Azure 应用程序](/azure/virtual-machines/workloads/oracle/oracle-weblogic)并选择管理产品/服务或任一群集产品/服务。  在部署该产品/服务时，部署过程中的一个选项卡将是“Azure Active Directory”。  将“连接到 Azure Active Directory”切换到“是” 。  使用上一节中收集的信息填写这些值。  对于证书，必须直接上传 `.cer` 文件。

#### <a name="after-wls-deployment"></a>在 WLS 部署后

如果在部署时没有将“连接到 Azure Active Directory”切换到“是”，则稍后可以使用在上一节中收集的值来进行配置 。  有关更多详细信息，请参阅[正式文档](https://wls-eng.github.io/arm-oraclelinux-wls/)。

### <a name="validate-the-deployment"></a>验证部署

在使用上述两种方法之一部署 WLS 和配置 LDAP 之后，按照以下步骤验证集成是否成功。

1. 访问 WLS 管理控制台。
1. 在左侧导航器中，展开树以选择“安全领域” -> “myrealm” -> “提供程序”  。
1. 如果集成成功，你将发现 AAD 提供程序，例如 `AzureActiveDirectoryProvider`。
1. 在左侧导航器中，展开树以选择“安全领域” -> “myrealm” -> “用户和组”  。
1. 如果集成成功，你将从 AAD 提供程序发现用户。

### <a name="lock-down-and-secure-ldap-access-over-the-internet"></a>锁定通过 Internet 进行的安全 LDAP 访问

在前面的步骤中设置安全 LDAP 时，对于网络安全组中的 `AllowLDAPS` 规则，我们将源设置为“任意”。  WLS 管理服务器现已部署并连接到 LDAP，可使用 Azure 门户获取其公共 IP 地址。  重新访问[锁定通过 Internet 进行的安全 LDAP 访问](/azure/active-directory-domain-services/tutorial-configure-ldaps?branch=pr-en-us-778#lock-down-secure-ldap-access-over-the-internet)，并将“任意”更改为 WLS 管理服务器的特定 IP 地址。

## <a name="clean-up-resources"></a>清理资源

现在，可以按照[为 Azure Active Directory 域服务托管域配置安全 LDAP](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources) 中[清理资源](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources)一节的步骤操作。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 WebLogic 应用程序迁移到 Azure 虚拟机](/azure/developer/java/migration/migrate-weblogic-to-virtual-machines)
