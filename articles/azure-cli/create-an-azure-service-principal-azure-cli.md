---
title: 通过 Azure CLI 使用 Azure 服务主体
description: 了解如何通过 Azure CLI 创建和使用服务主体。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/15/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c18adbee84fd3e5c73367b07bbd0b03ac61008cd
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030729"
---
# <a name="create-an-azure-service-principal-with-the-azure-cli"></a>使用 Azure CLI 创建 Azure 服务主体

使用 Azure 服务的自动化工具应始终具有受限权限。 Azure 提供了服务主体，而不是让应用程序以具有完全特权的用户身份登录。

Azure 服务主体是为与应用程序、托管服务和自动化工具配合使用以访问 Azure 资源而创建的标识。 此访问权限受分配给服务主体的角色限制，可用于控制哪些资源可以访问以及在哪个级别进行访问。 出于安全原因，始终建议将服务主体与自动化工具配合使用，而不是允许它们使用用户标识进行登录。

本文介绍了用于使用 Azure CLI 创建服务主体、获取服务主体相关信息以及重置服务主体的步骤。

## <a name="create-a-service-principal"></a>创建服务主体

使用 [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) 命令创建服务主体。 在创建服务主体时，需选择其使用的登录身份验证的类型。

> [!NOTE]
>
> 如果帐户没有足够的权限创建服务主体，`az ad sp create-for-rbac` 将返回一条错误消息，其中包含“权限不足，无法完成该操作”。 请与 Azure Active Directory 管理员联系以创建服务主体。

有两种类型的身份验证可用于服务主体：基于密码的身份验证和基于证书的身份验证。

### <a name="password-based-authentication"></a>基于密码的身份验证

没有任何身份验证参数，基于密码的身份验证与为你创建的随机密码配合使用。

  ```azurecli-interactive
  az ad sp create-for-rbac --name ServicePrincipalName
  ```

> [!IMPORTANT]
> 从 Azure CLI 2.0.68 开始，用于创建具有用户定义密码的服务主体的 `--password` 参数__不再受支持__，以防止意外使用弱密码。

与密码身份验证配合使用的服务主体的输出包括 `password` 密钥。 __请确保__复制此值 - 它不可检索。 如果忘记了密码，请[重置服务主体凭据](#reset-credentials)。

`appId` 和 `tenant` 密钥出现在 `az ad sp create-for-rbac` 的输出中，并且在服务主体身份验证中使用。
请记录其值，但它们随时可以通过 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) 检索。

### <a name="certificate-based-authentication"></a>基于证书的身份验证

对于基于证书的身份验证，请使用 `--cert` 参数。 此参数需要你持有现有的证书。 请确保任何使用此服务主体的工具都有权访问该证书的私钥。 证书应采用 PEM、CER 或 DER 等 ASCII 格式。 请将证书作为字符串传递，或使用 `@path` 格式从文件加载证书。

> [!NOTE]
> 使用 PEM 文件时，必须将证书  追加到文件中的私钥  之后。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert "-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----"
```

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert @/path/to/cert.pem
```

可以添加 `--keyvault` 参数以使用 Azure Key Vault 中的证书。 在这种情况下，`--cert` 值是证书的名称。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert CertName --keyvault VaultName
```

若要创建自签名  证书以用于身份验证，请使用 `--create-cert` 参数：

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert
```

控制台输出：

```
Creating a role assignment under the scope of "/subscriptions/myId"
Please copy C:\myPath\myNewFile.pem to a safe place.
When you run 'az login', provide the file path in the --password argument
{
  "appId": "myAppId",
  "displayName": "myDisplayName",
  "fileWithCertAndPrivateKey": "C:\\myPath\\myNewFile.pem",
  "name": "http://myName",
  "password": null,
  "tenant": "myTenantId"
}
```

新 PEM 文件的内容：
```
-----BEGIN PRIVATE KEY-----
myPrivateKeyValue
-----END PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
myCertificateValue
-----END CERTIFICATE-----
```

> [!NOTE]
> `az ad sp create-for-rbac --create-cert` 命令将创建服务主体和 PEM 文件。 PEM 文件包含格式正确的私钥  和证书  。

可以添加 `--keyvault` 参数以将证书存储在 Azure Key Vault 中。 使用 `--keyvault` 时，`--cert` 参数是__必需__的。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert --cert CertName --keyvault VaultName
```

除非将证书存储在 Key Vault 中，否则输出将包括 `fileWithCertAndPrivateKey` 密钥。 此密钥的值指示所生成的证书的存储位置。
__请确保__将证书复制到安全位置，否则将无法使用此服务主体登录。

对于 Key Vault 中存储的证书，请使用 [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show) 检索该证书的私钥。 在 Key Vault 中，证书机密的名称与证书名称相同。 如果无法访问证书的私钥，请[重置服务主体凭据](#reset-credentials)。

`appId` 和 `tenant` 密钥出现在 `az ad sp create-for-rbac` 的输出中，并且在服务主体身份验证中使用。
请记录其值，但它们随时可以通过 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) 检索。

## <a name="get-an-existing-service-principal"></a>获取现有服务主体

可使用 [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) 检索租户中的服务主体的列表。 默认情况下，此命令返回租户的前 100 个服务主体。 若要获取某个租户的所有服务主体，请使用 `--all` 参数。 获取此列表可能需要很长时间，因此建议使用以下参数之一筛选该列表：

* `--display-name` 用于请求具有与所提供名称匹配的前缀  的服务主体。 服务主体的显示名称是在创建期间使用 `--name` 参数设置的值。 如果在创建服务主体期间未设置 `--name`，则名称前缀为 `azure-cli-`。
* `--spn` 基于完全服务主体名称匹配进行筛选。 服务主体名称始终以 `https://` 开头。
  如果用于 `--name` 的值不是一个 URI，则此值是后跟显示名称的 `https://`。
* `--show-mine` 仅请求由登录用户创建的服务主体。
* `--filter` 伴随一个 OData 筛选器，并执行“服务器端”  筛选。 建议使用此方法通过 CLI 的 `--query` 参数筛选客户端。 若要了解 OData 筛选器，请参阅[筛选器的 OData 表达式语法](/rest/api/searchservice/odata-expression-syntax-for-azure-search)。

为服务主体对象返回的信息十分详细。 若要仅获取执行登录所需的信息，请使用查询字符串 `[].{id:appId, tenant:appOwnerTenantId}`。 例如，若要获取由当前登录用户创建的所有服务主体的登录信息，请执行以下命令：

```azurecli-interactive
az ad sp list --show-mine --query "[].{id:appId, tenant:appOwnerTenantId}"
```

> [!IMPORTANT]
>
> `az ad sp list` 或 [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show) 获取用户和租户，但不获取任何身份验证机密或  身份验证方法。
> 可使用 [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show) 检索 Key Vault 中的证书机密，但默认情况下任何其他机密都不会存储。
> 如果忘记了身份验证方法或机密，请[重置服务主体凭据](#reset-credentials)。

## <a name="manage-service-principal-roles"></a>管理服务主体角色

Azure CLI 提供以下命令来管理角色分配：

* [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list)
* [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)
* [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete)

服务主体的默认角色是“参与者”。  此角色具有读取和写入到 Azure 帐户的完全权限。 “读者”角色限制性更强，具有只读访问权限。   有关基于角色的访问控制 (RBAC) 和角色的详细信息，请参阅 [RBAC：内置角色](/azure/active-directory/role-based-access-built-in-roles)。

此示例将添加“读者”  角色并删除“参与者”  角色：

```azurecli-interactive
az role assignment create --assignee APP_ID --role Reader
az role assignment delete --assignee APP_ID --role Contributor
```

> [!NOTE]
> 如果帐户无权分配角色，则将显示错误消息“你的帐户无权执行操作 'Microsoft.Authorization/roleAssignments/write'”。请与 Azure Active Directory 管理员联系以管理角色。

添加角色并不  会限制以前分配的权限。 要限制服务主体的权限时，应删除“参与者”  角色。

可以通过列出分配的角色来验证所做的更改：

```azurecli-interactive
az role assignment list --assignee APP_ID
```

## <a name="sign-in-using-a-service-principal"></a>使用服务主体登录

可以通过登录来测试新服务主体的凭据和权限。 若要使用服务主体登录，需要`appId`、`tenant` 和凭据。

若要将服务主体与密码配合使用进行登录，请使用以下命令：

```azurecli-interactive
az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
```

若要使用证书登录，则证书必须在本地以 PEM 或 DER 文件的形式存在（采用 ASCII 格式）。 使用 PEM 文件时，必须将私钥和证书一起追加到该文件中   。

```azurecli-interactive
az login --service-principal --username APP_ID --tenant TENANT_ID --password /path/to/cert
```

若要了解有关使用服务主体登录的详细信息，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="reset-credentials"></a>重置凭据

如果忘记了服务主体凭据，请使用 [az ad sp credential reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset)。 该重置命令带有与 `az ad sp create-for-rbac` 相同的参数。

```azurecli-interactive
az ad sp credential reset --name APP_ID
```
