---
title: 演练，第 2 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 讨论示例方案中的不同身份验证需求和挑战，以及如何通过 Azure 集成身份验证来应对这些挑战。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: c6719e3c86b590edff551d98e5a961fd08f857c3
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379471"
---
# <a name="part-2-authentication-needs-in-the-scenario"></a>第 2 部分：方案中的身份验证需求

[上一部分：简介与背景](walkthrough-tutorial-authentication-01.md)

在此示例方案中，主应用具有以下身份验证要求：

- 使用 Azure Key Vault 进行身份验证以访问存储的第三方 API 密钥。
- 使用 API 密钥通过第三方 API 进行身份验证。
- 使用存储帐户所需的凭据通过 Azure 队列存储进行身份验证。

由于这三个不同要求，应用程序必须管理三组凭据：两组用于 Azure 资源（Key Vault 和队列存储），一组用于外部资源（第三方 API）。

如前文所述，你可以安全管理 Key Vault 中的所有凭据，但 Key Vault 本身所需的凭据除外。 通过 Key Vault 进行身份验证之后，应用程序可以在运行时检索任何其他密钥，以便通过队列存储等服务进行身份验证。

但是，此方法仍要求应用单独管理 Key Vault 的凭据。 那么，如何才能安全管理该凭据，并使其针对本地开发以及在云端的生产部署中正常工作？

部分解决方案是将密钥存储在服务器端环境变量中（即，通过 Azure 应用服务和 Azure Functions 的应用程序设置），这样至少可以使密钥远离源代码管理。 但是，要在开发者工作站上运行代码，必须在本地复制该环境变量，而这会产生暴露凭据和/或在源代码管理中意外包含的风险。 通过在代码的开发版本中实现特殊过程，可以在某种程度上解决此问题，但这样做会增加开发过程的复杂性。

幸运的是，使用 Azure Active Directory (AD) 进行集成身份验证可使应用完全避免处理任何 Azure 凭据。

## <a name="integrated-authentication-with-managed-identity"></a>使用托管标识进行集成身份验证

许多 Azure 服务（如存储和 Key Vault）都与 Azure Active Directory (Azure AD) 集成，这样，在使用[托管标识](/azure/active-directory/managed-identities-azure-resources/overview)通过 Azure AD 对应用程序进行身份验证时，该应用程序便会通过其他连接的资源自动进行身份验证。 标识的授权通过[基于角色的访问控制 (RBAC)](how-to-assign-role-permissions.md) 进行处理，有时也通过其他访问策略来处理。

此集成意味着你永远无需在应用代码中处理任何与 Azure 相关的凭据，并且这些凭据绝不会出现在开发者工作站或源代码管理中。 此外，对第三方 API 和服务的密钥的任何处理都完全在运行时完成，因此，这些密钥非常安全。

托管标识特别适用于部署到 Azure 的应用。 对于本地开发，你可以创建一个单独的服务主体，以便在本地运行时充当应用标识。 可以使用环境变量将此服务主体提供给 Azure 库，如[配置本地开发环境 - 配置身份验证](configure-local-development-environment.md#configure-authentication)中所述。 还可以将角色权限分配给此服务主体和云中使用的托管标识。

在针对本地服务主体执行了这些步骤之后，即可使用相同的代码在本地和云中通过 Azure 资源对应用进行身份验证。 [如何对应用进行身份验证和授权](azure-sdk-authenticate.md)中讨论了这些详细信息，下面对其进行了简短描述：

1. 在代码中，创建一个 `DefaultAzureCredential` 对象，该对象在 Azure 上运行时自动使用托管标识，而在本地运行时自动使用单独的服务主体。

1. 在为要访问的任意资源（Key Vault、队列存储等）创建适当的客户端对象时，请使用此凭据。

1. 然后，当你通过客户端对象调用操作方法时，将发生身份验证，这会生成对资源的 REST API 调用。

1. 如果应用标识有效，则 Azure 还会检查该标识是否也已获得特定操作的授权。

本教程的其余部分在示例方案和随附示例代码的上下文中演示了该过程的所有详细信息。

在示例的预配脚本中，所有资源都在名为 `auth-scenario-rg` 的资源组下创建。 此组使用 Azure CLI [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) 命令创建。

> [!div class="nextstepaction"]
> [第 3 部分 - 第三方 API 实现示例 >>>](walkthrough-tutorial-authentication-03.md)
