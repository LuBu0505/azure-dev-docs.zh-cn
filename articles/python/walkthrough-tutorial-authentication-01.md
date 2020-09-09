---
title: 演练：通过 Azure 服务对 Python 应用进行身份验证
description: 详细演练如何使用 Azure Python SDK Azure 标识库通过 Azure Active Directory、Azure Key Vault 和 Azure 队列存储对 Python 应用进行身份验证。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 7f716f3276c0b93ec37ba0941f4029b2ee817fa0
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379472"
---
# <a name="walkthrough-integrated-authentication-for-python-apps-with-azure-services"></a>演练：通过 Azure 服务对 Python 应用进行集成身份验证

Azure Active Directory (Azure AD) 和 Azure Key Vault 提供了一种全面便捷的方法，用于通过 Azure 服务和涉及访问密钥的第三方服务对应用程序进行身份验证。

在提供某些背景之后，本演练将在示例 [github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication) 的上下文中说明这些身份验证功能。

为方便起见，此示例已部署到 Azure，因此你可以在操作时看到该示例。 如果你想将此示例部署到自己的 Azure 订阅，存储库还提供了 Azure CLI 部署脚本。

## <a name="part-1-background"></a>第 1 部分：背景

虽然许多 Azure 服务仅依赖于基于角色的访问控制来进行授权，但某些服务却使用机密或密钥来控制对其各自资源的访问权限。 此类服务包括 Azure 存储、数据库、认知服务、Key Vault 和事件中心。

在创建使用这些服务内的资源的云应用时，可以使用 Azure 门户、Azure CLI 或 Azure PowerShell 来创建和配置与特定访问策略关联的应用的密钥。 这些密钥可以防止任何其他未经授权的代码访问这些特定于应用的资源。

在此常规设计中，云应用通常必须管理这些密钥，并分别对每项服务进行身份验证，这是一个繁琐且容易出错的过程。 直接在应用代码中管理密钥还会带来在源代码管理中暴露这些密钥的风险，并且密钥可能存储在不安全的开发者工作站上。

幸运的是，Azure 提供了两种特定服务来简化该过程并增强安全性：

- Azure Key Vault 为访问密钥（以及本文中未涵盖的密钥和证书）提供基于云的安全存储。 通过使用 Key Vault，应用仅在运行时访问此类密钥，因此这些密钥永远不会直接出现在源代码中。

- 借助 Azure Active Directory (Azure AD) 托管标识，应用只需对 Active Directory 进行一次身份验证。 然后，应用会自动通过其他 Azure 服务（包括 Key Vault）进行身份验证。 因此，你的代码永远不需要关心这些 Azure 服务的密钥或其他凭据。 更好的是，你可以在本地和云中以最低配置要求运行相同的代码。

通过同时使用 Azure AD 和 Key Vault，应用永远不需要通过单个 Azure 服务对自身进行身份验证，并且可以轻松安全地访问第三方服务所需的任何密钥。

> [!IMPORTANT]
> 本文使用通用术语“密钥”来表示作为“机密”存储在 Azure Key Vault 中的内容，例如 REST API 的访问密钥。 此用法不应与 Key Vault 的加密密钥管理混淆，后者是 Key Vault 机密的一项独立功能 。

## <a name="example-cloud-app-scenario"></a>云应用方案示例

要更深入地了解 Azure 的身份验证过程，请考虑以下方案：

- 主应用公开使用 JSON 数据响应 HTTP 请求的公共（未经过身份验证的）API 终结点。 本文中所示的示例终结点实现为部署到 Azure 应用服务的简单 Flask 应用。

- API 通过调用需要访问密钥的第三方 API 来生成其响应。 应用在运行时从 Azure Key Vault 检索该访问密钥。

- 在返回其响应之前，API 会将消息写入 Azure 存储队列以便稍后处理。 （这些消息的具体处理与主方案无关。）

![应用程序方案示意图](media/walkthrough-tutorial-authentication/scenario-diagram.png)

> [!NOTE]
> 虽然公共 API 终结点通常由其自己的访问密钥保护，但在本文中，我们假定终结点处于开放状态且未经过身份验证。 此假设可避免应用的身份验证需求与此终结点的外部调用方的身份验证需求之间产生任何混淆。 此方案不演示此类外部调用方。

> [!div class="nextstepaction"]
> [第 2 部分 - 身份验证需求 >>>](walkthrough-tutorial-authentication-02.md)
