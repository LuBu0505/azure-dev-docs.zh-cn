---
ms.openlocfilehash: f644d66895b7b8bcce345cd42c6efedfaddb4240
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764483"
---
此代码使用基于 CLI 的身份验证方法（使用 `AzureCliCredential`），因为它演示了你可能会使用 Azure CLI 直接执行的操作。 在这两种情况下，使用相同的身份验证标识。

若要在生产脚本中使用此类代码（例如，自动执行 VM 管理），请使用 `DefaultAzureCredential`（推荐）或基于服务主体的方法，如[如何使用 Azure 服务对 Python 应用进行身份验证](../azure-sdk-authenticate.md)中所述。
