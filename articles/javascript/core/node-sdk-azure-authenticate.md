---
title: 使用用于 Node.js 的 Azure 管理模块进行身份验证
description: 在用于 Node.js 的 Azure 管理模块中使用服务主体进行身份验证
ms.topic: how-to
ms.date: 10/19/2020
ms.custom: devx-track-js
ms.openlocfilehash: a3f51cd40f1f1029fbdd6e8e806cb1f7c3cd02c7
ms.sourcegitcommit: 3c904d8d89d0cb4f13209cde3425c5307b83237c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "93024061"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>对适用于 JavaScript 的 Azure 管理模块进行身份验证

在被实例化时，所有 [SDK 客户端库](../azure-sdk-library-package-index.md)都需要通过 `credentials` 对象进行身份验证。 可使用多种方法来验证和创建所需凭据。

像所有软件和服务一样，多年来，身份验证一直在改进。 了解你的服务或服务使用的身份验证库十分重要。 

身份验证库包括以下内容：

* @azure/identity - 最新身份验证包
* @azure/ms-rest-nodeauth
* @azure/ms-rest-browserauth

旧式身份验证包正在使用中。 如果你使用的是这些包，则应考虑迁移旧式身份验证方法，以获得更安全和更可靠的体验。 

## <a name="best-practices-with-azure-sdk-client-library-authentication"></a>Azure SDK 客户端库身份验证的最佳做法

每个 npm 包都将显示该确切客户端库的身份验证。 除非所有包都使用 npm 包页面上的相同身份验证包，否则请勿混用和匹配身份验证包和代码。 

## <a name="azure-identity-library"></a>Azure 标识库

Azure 标识库是 Azure 的最新身份验证包。 使用 Azure 标识查看[受支持库的列表](https://www.npmjs.com/package/@azure/identity#client-libraries-supporting-authentication-with-azure-identity)。

[@azure/identity](https://www.npmjs.com/package/@azure/identity) 库简化了对 Azure Active Directory for Azure SDK 库的身份验证。 它提供了一组 TokenCredential 实现，可以将这些实现传递到 SDK 库中以验证 API 请求。 它支持使用 Azure Active Directory 服务主体或托管标识进行令牌身份验证。

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { BlobServiceClient } = require("@azure/storage-blob");
 
// Enter your storage account name
const account = "<account>";
const defaultAzureCredential = new DefaultAzureCredential();
 
const blobServiceClient = new BlobServiceClient(
  `https://${account}.blob.core.windows.net`,
  defaultAzureCredential
);
```

## <a name="azure-ms-rest--libraries"></a>Azure ms-rest-* 库
对于 `@azure` 范围内的[客户端库](../azure-sdk-library-package-index.md#modern-javascripttypescript-libraries)，你需要一个令牌来使用服务。 可使用 Azure SDK 客户端身份验证方法来获取令牌，该方法会返回一个凭据。 

```javascript
const msRestNodeAuth = require("@azure/ms-rest-nodeauth");
msRestNodeAuth.interactiveLogin().then((credential) => {
}).catch((err) => {
    // service code goes here
    console.error(err);
});
```

将该凭据传递给特定的 Azure 服务客户端库，例如下一个代码示例中使用的存储服务。 客户端库会接受凭证，并为你生成令牌。 服务会使用令牌验证你的请求。 

```javascript
// service code - this is an example only and not best practices for code flow
const { BlobServiceClient } = require('@azure/storage-blob');
const billingManagementClient = new billing.BillingManagementClient(credential, subscriptionId);
billingManagementClient.enrollmentAccounts.list().then((enrollmentList) => {
    console.log("The result is:");
    console.log(result);
})
```

客户端库管理令牌，并知道何时刷新令牌。 作为拥有代码库的开发者，你不必管理这些。

## <a name="older-azure-sdk-client-authentication"></a>旧式 Azure SDK 客户端身份验证 

旧式 Azure SDK 客户端最终将迁移到上面使用的新式身份验证。 在此迁移之前，旧式客户端库使用不同的身份验证客户端，或者可以使用完全独立的机制（例如资源密钥）进行身份验证。 

为了获得旧式客户端库的最佳结果： 
* 每个 npm 包都将显示该确切客户端库的身份验证。 
* 如果当前代码使用 `@azure/ms

## <a name="authentication-with-azure-services-while-developing"></a>开发时使用 Azure 服务进行身份验证

开发时创建所需凭据的常见方法包括：

| Azure 身份验证类型|用途|
|--|--|
|**服务主体**|这是推荐的身份验证方法。 了解[如何创建 Azure 服务主体](node-sdk-azure-authenticate-principal.md)。 通过服务主体，你可以连接到独立于 Azure 个人帐户的 Azure。 它可以是临时帐户，也可以是用来代替你的个人帐户的寿命更长的帐户。|
| **交互式**| 这是你尝试 Azure 服务时进行身份验证的最简单方法。 它需要使用你的个人帐户通过浏览器登录。 |
|**基本**|此身份验证要求你输入个人用户名和密码。 这种方法最不安全，因此不建议使用。| 

## <a name="authentication-with-azure-services-and-production-code"></a>使用 Azure 服务和产品代码进行身份验证

使用产品代码创建所需凭证的常见方法：

|Azure 身份验证类型|用途|
|--|--|
|**托管的服务标识 (MSI)**|[MSI 身份验证](/azure/active-directory/managed-identities-azure-resources/overview)最适合于生产场景。 请勿在本地开发环境中使用它。 [支持 MSI 的服务](/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)。|
|**Certificates**|[证书](/azure/cloud-services/cloud-services-certs-create)需要使用[门户](/azure/cloud-services/cloud-services-configure-ssl-certificate-portal)上传到 Azure。|

## <a name="javascript-authentication-samples-for-azure"></a>Azure 的 JavaScript 身份验证示例

|身份验证包|示例身份验证脚本|
|--|--|
|[@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) <br>（建议）|[使用证书的服务主体](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpCert.ts)<br>[来自文件的服务主体](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpSecret.ts)<br>[交互式](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/interactivePersonalAccount.ts)<br>[基本](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/usernamePassword.ts)|
|[@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)<br>（建议）|[带有弹出窗口的身份验证 (create-react-app)](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/authentication-with-popup)<br>[无弹出窗口回应](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/react-app)<br>[带登录按钮的 HTML](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/vanilla)|
|[ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|[服务主体](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#service-principal-authentication)<br>[交互式](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#interactive-login)<br>[基本](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#basic-authentication)|

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="next-steps"></a>后续步骤   

* [Azure npm 包](../azure-sdk-library-package-index.md)
* [Azure npm 包文档](/javascript/api/overview/azure/?view=azure-node-latest)
