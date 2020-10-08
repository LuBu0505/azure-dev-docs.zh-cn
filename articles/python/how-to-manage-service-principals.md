---
title: 管理 Azure 开发的本地服务主体
description: 如何使用 Azure 门户或 Azure CLI 管理为本地开发创建的服务主体。
ms.date: 08/18/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 9d090a4615621c60485b64fac22929472c0cd175
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764792"
---
# <a name="how-to-manage-service-principals"></a>如何管理服务主体

如[如何对应用进行身份验证](azure-sdk-authenticate.md)中所述，通常使用服务主体来标识 Azure 中的应用，但使用托管标识时除外。

随着时间的推移，通常需要删除、重命名或管理这些服务主体，你可以通过 Azure 门户或使用 Azure CLI 来执行这些操作。

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

- 创建、查看、更新和删除服务主体：[az ad sp](/cli/azure/ad/sp) 命令。 另请参阅[通过 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)。
- 管理角色分配：[az role assignment](/cli/azure/role/assignment) 命令。

另请参阅：

- [使用 Azure 库向 Azure 进行身份验证](azure-sdk-authenticate.md)
