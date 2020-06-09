---
title: 管理 Azure 开发的本地服务主体
description: 如何使用 Azure 门户或 Azure CLI 管理为本地开发创建的服务主体。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: ffb526a0073667f5ea53631925f2565215f60787
ms.sourcegitcommit: 79890367158a9931909f11da1c894daa11188cba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84146185"
---
# <a name="how-to-manage-service-principals"></a>如何管理服务主体

出于安全考虑，应始终细心地管理授权应用代码访问和修改任何 Azure 资源的方式。 在本地测试代码时，应始终使用本地服务主体，而不是以具有完全特权的用户身份运行，如[配置本地 Python 开发环境 - 身份验证](configure-local-development-environment.md#configure-authentication)中所述。

随着时间的推移，可能需要删除、重命名或管理这些服务主体，可以通过 Azure 门户或使用 Azure CLI 来执行这些操作。

## <a name="basics-of-azure-authorization"></a>Azure 授权的基础知识

每当代码尝试对 Azure 资源执行任何操作（通过 Azure 库中的类执行）时，Azure 都会确保该应用程序已被授权执行该操作。 可以使用 [Azure 门户](https://portal.azure.com)或 Azure CLI 向应用程序标识授予特定的基于角色或基于资源的权限。 （当应用程序的安全受到威胁时，此过程可避免向该应用程序授予多余的权限，以防止权限被利用。）

当部署到 Azure 时，应用程序的标识通常与在托管该应用程序的服务（如启用托管标识时的 Azure 应用服务、Azure Functions、虚拟机等）中为其提供的名称相同。 但在本地运行代码时，不会涉及此类托管服务，因此需要使用其他合适的名称来表示 Azure。

为此，请使用本地服务主体，它是应用标识的另一名称，而不是用户标识。 服务主体具有名称、“租户”标识符（实质上是组织 ID）、应用或“客户端”标识符以及机密/密码。 这些凭据足以使用 Azure 对标识进行身份验证，然后可以检查该标识是否被授权访问任何给定的资源。

每个开发人员都应具有自己的服务主体，这些服务主体可以安全地存储在他们的工作站上的用户帐户中，且永远不要将其存储在源代码管理存储库中。 如有任一个服务主体被窃取或泄露，可以在 Azure 门户上轻松删除它以撤销其所有权限，然后为该开发人员重新创建服务主体。

## <a name="manage-service-principals-using-the-azure-portal"></a>使用 Azure 门户管理服务主体

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 使用门户主页上的图标或在门户搜索栏中搜索“Azure Active Directory”，以导航到“Azure Active Directory”页。

    ![在 Azure 门户上搜索 Azure Active Directory](media/how-to-manage-service-principals/azure-ad-portal-search.png)

1. 选择左侧导航菜单中的“管理” > “应用注册”。 本地开发服务主体将显示在列表中：

    ![Azure Active Directory 中的应用注册](media/how-to-manage-service-principals/azure-ad-app-registrations.png)

1. 选择任一个服务主体以导航到其属性页，可以在其中查看 ID 值、重命名或删除服务主体以及获取各种终结点 URL。

1. 授权服务主体访问特定资源的过程通常取决于所涉及的服务。 有关详细信息，请参阅该服务的文档。 例如，[授权访问 Blob 存储](/azure/storage/common/storage-auth-aad-rbac-portal)和[授权访问队列存储](/azure/storage/common/storage-auth-aad-rbac-portal)，这两篇文章介绍了 Azure 存储中的过程。

## <a name="manage-service-principals-using-the-azure-cli"></a>使用 Azure CLI 管理服务主体

使用 Azure CLI，可以对服务主体执行许多与通过 Azure 门户可以执行的操作相同的操作：

- 创建、查看、更新和删除服务主体：[az ad sp](/cli/azure/ad/sp?view=azure-cli-latest) 命令。 另请参阅[通过 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。
- 管理角色分配：[az role assignment](/cli/azure/role/assignment?view=azure-cli-latest) 命令。

另请参阅：

- [使用 Azure 库向 Azure 进行身份验证](azure-sdk-authenticate.md)
