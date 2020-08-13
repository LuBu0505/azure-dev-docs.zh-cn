---
title: 向应用标识或服务主体分配角色权限
description: 如何使用 Azure CLI 向服务主体或应用标识授予权限
ms.date: 05/12/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 3eb81eac5ee9a7f2f85e50494efa2e04bbcbe439
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983129"
---
# <a name="how-to-assign-role-permissions-to-an-app-identity-or-service-principal"></a>如何向应用标识或服务主体分配角色权限

Azure 的基于角色的访问控制 (RBAC) 系统管理各种资源的特定权限。 角色本质上是通常需要一起使用的相关权限的集合。 若要启用权限，请将角色分配到具有此角色适用的特定范围的安全主体（用户、组、服务主体或应用标识）。

在实践中，始终仅分配安全主体在最特定范围内真正需要的角色。 即使最初看起来更方便操作，也应避免在更广泛的范围内分配更广泛的角色。 如果安全主体曾被泄露（即，如果该主体的凭据在数据泄露或其他安全事件中公开），通过限制角色和范围，你可以限制哪些资源面临风险。

由于在开发和生产中使用不同的安全主体，因此需要在每个环境中重复角色分配。 也就是说，在开发过程中，通常会将角色分配给工作站上创建的本地服务主体（请参阅[配置本地 Python 开发环境 - 身份验证](configure-local-development-environment.md#configure-authentication)）。 在生产中，你可以在部署之前将角色分配给应用程序标识或服务主体，以确保应用程序在启动时具有访问权限。

有关 RBAC 的一般详细信息，请参阅 [Azure 基于角色的访问控制是什么？](/azure/role-based-access-control/overview)。

## <a name="role-assignment-process"></a>角色分配进程

分配角色包含三个步骤：

1. 为所涉及的资源类型和想要授权的操作[找到合适的角色](#find-the-roles-you-need)。

1. 确定相关角色所需的范围，该范围介绍了授权操作的资源范围。

1. 向安全主体分配角色。

步骤 1 对于 Azure 门户和 Azure CLI 来说都是一样的。 步骤 2 和步骤 3 在门户和 CLI 之间有所不同，因此以下各节中进行了合并：[确定范围并在 Azure 门户上分配角色](#azure-portal)和[确定范围并通过 Azure CLI 分配角色](#azure-cli)。

> [!NOTE]
> 如果用户帐户无权在订阅内分配角色，则将显示错误消息“你的帐户无权执行操作 'Microsoft.Authorization/roleAssignments/write'”。在这种情况下，请与你的订阅管理员联系，因为他们可以代表你分配权限。

## <a name="find-the-roles-you-need"></a>查找所需的角色

1. 请从综合性文章 [Azure 内置角色](/azure/role-based-access-control/built-in-roles)开始。 本文顶部的表是对本文后面的详细信息的索引。

1. 在此文章中，导航到要向其授予权限的资源的服务类别（计算、存储、数据库等）。 查找所需内容的最简单方法通常是在页面上搜索相关的关键字，如“blob”、“虚拟机”等。

1. 查看服务类别列出的角色，并确定所需的特定操作。 同样，始终以最严格的角色开始。

    例如，如果安全主体需要读取 Azure 存储帐户中的 blob，但不需要写入访问权限，请选择“存储 Blob 数据读取器”而不是“存储 Blob 数据参与者”（绝不是管理员级别的“存储 Blob 数据所有者”角色）。 稍后，你可以根据需要随时更新角色分配。

    如果安全主体还需要访问队列存储，则可以使用诸如“存储队列数据读取器”和“存储队列数据参与者”之类的角色。 还是那句话，请尽可能地具体化，而不是分配广泛的角色（如“读取器和数据访问”），从而提供对帐户密钥的访问权限，主体可以通过此方法访问整个存储帐户中的任何内容。

1. 如果找不到合适的角色，可以创建[自定义角色](/azure/role-based-access-control/custom-roles)。

## <a name="identify-scope-and-assign-a-role-on-the-azure-portal"></a><a name="azure-portal"></a>确定范围并在 Azure 门户上分配角色

1. 在 [Azure 门户](https://portal.azure.com)上，导航到要为其分配角色的资源。 资源的范围确定分配的范围。

    例如，如果导航到某个存储帐户，则任何角色分配都适用于整个存储帐户。 如果导航到该存储帐户中的特定 blob 容器，则角色分配仅适用于该容器。

1. 选择“访问控制 (IAM)”边栏选项卡（IAM 代表“标识和访问管理”）。

1. 在该边栏选项卡中有一个角色分配部分，你可以在其中添加和删除分配给任何安全主体的角色。

有关完整的详细信息和 UI 演练，请参阅 Azure RBAC 文档中的[添加或删除 Azure 角色分配](/azure/role-based-access-control/role-assignments-portal)。

## <a name="identify-scope-and-assign-a-role-through-the-azure-cli"></a><a name="azure-cli"></a>确定范围并通过 Azure CLI 分配角色

具有 Azure CLI 的角色分配使用 [`az role assignment`](/cli/azure/role/assignment?view=azure-cli-latest) 命令。 使用 `az role assignment create` 添加分配，使用 `az role assignment delete` 删除分配。 

尽管[使用 Azure CLI 添加或删除 Azure 角色分配](/azure/role-based-access-control/role-assignments-cli)中介绍了完整的进程，但以下摘要提供了与此开发人员中心上的其他文章相关的特定示例。

`az role assignment create` 命令的语法如下：

```azurecli
az role assignment create --assignee <assignee> --role <role> --scope <scope>
```

- `<assignee>` 标识安全主体；对于服务主体（如在本地开发期间使用的服务主体），被分派人是该主体的客户端 ID。 对于部署到云的应用程序，被分派人是应用程序的名称。
- `<role>` 是要分配的角色名称，如“存储 Blob 数据参与者”或其 GUID，例如“ba92f5b4-2d11-453d-a403-e96b0029c9fe”。
- `<scope>` 是一个潜在的长字符串，用于标识分配的确切范围。

范围由 / 字符分隔的一系列标识符组成。 可以将此字符串视为表示以下层次结构，其中没有占位符的文本 (`<>`) 是固定标识符：

<pre>
/subscriptions
  /&lt;subscription_id&gt;
    /resourcegroups
      /&lt;resource_group_name&gt;
        /providers
          /&lt;provider_name&gt;
            /&lt;resource_type&gt;
              /&lt;resource_sub_type_1&gt;
                /&lt;resource_sub_type_2&gt;
                  /&lt;resource_name&gt;
</pre>

- `<subscription_id>` 是要使用的订阅的 ID (GUID)。
- `<resources_group_name>` 是包含资源组的名称。
- `<provider_name>` 是处理资源的服务名称，然后 `<resource_type>` 和 `<resource_sub_type_*>` 标识该服务内的更多级别。
  
    通过参考 [Azure 内置角色](/azure/role-based-access-control/built-in-roles)引用查找这些名称和类型。 找到并选择上表中的角色，以跳转到该角色的特定说明。 你可以在这里找到包含提供程序名称、资源类型和资源子类型的字符串。 例如，对于“存储 Blob 数据参与者”，你会看到“Microsoft.Storage/storageAccounts/blobServices/containers/”。 对于“Cosmos DB 帐户读取器角色”，你会看到“Microsoft.DocumentDB/databaseAccounts/readonlykeys”，其中只有一个子类型。

- `<resource_name>` 是标识特定资源的字符串的最后一部分。

最广泛（最不具体）的范围是 `/subscriptions/<subscription_id>`，它在整个订阅中应用分配。 层次结构的每个附加级别都会使范围更具针对性。

### <a name="examples"></a>示例

下面的示例假定满足以下条件（请参阅[示例：预配和使用 Azure 存储](azure-sdk-example-storage.md)）：

- 你的 Azure 订阅 ID 包含在名为 `AZURE_SUBSCRIPTION_ID` 的环境变量中。
- 要向其分配角色的服务主体的客户端 ID 包含在名为 `AZURE_CLIENT_ID` 的环境变量中。
- 在订阅中，你有一个名为“PythonAzureExample-Storage-rg”的资源组。
- 该资源组包含名为“pythonazurestorage-12345”的 Azure 存储帐户。
- 在该存储帐户中有一个名为“blob-容器-01”的 blob 容器。
- 需要向服务主体分配“存储 Blob 数据参与者”。

> [!TIP]
> 可能需要一分钟时间才能在 Azure 内传播角色分配更改。 因此，你可能会发现，在你删除权限后，代码仍可以在短时间内正常运行。 如果出现意外行为，请等待一两分钟，然后重试。

#### <a name="grant-permissions-for-the-specific-container-only"></a>仅授予特定容器的权限

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-storage-account"></a>为存储帐户中所有 blob 容器授予权限

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-resource-group"></a>为资源组中所有 blob 容器授予权限

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg"
```

或者，也可以使用 `--resource-group` 参数来指定资源组：

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --resource-group "PythonAzureExample-Storage-rg"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg"
```

或者，也可以使用 `--resource-group` 参数来指定资源组：

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --resource-group "PythonAzureExample-Storage-rg"
```

---

#### <a name="grant-permissions-to-all-blob-containers-in-the-subscription"></a>向订阅中所有 blob 容器授予权限

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID"
```

---
