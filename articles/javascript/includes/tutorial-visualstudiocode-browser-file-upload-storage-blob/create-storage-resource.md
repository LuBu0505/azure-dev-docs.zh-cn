---
title: include 文件 create-storage-resource.md
description: include 文件 create-storage-resource.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: a4eb48afd578de7ddc3426a3907b8b4192387cbb
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344122"
---
在本教程的这一部分中，使用 Visual Studio 扩展创建 Azure 存储资源，然后在 Azure 门户中配置该资源。 

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](azure-sign-in.md)]

## <a name="create-storage-resource"></a>创建存储资源 

使用 Visual Studio Code 扩展创建存储资源。 

1. 导航到 Azure 存储扩展。 右键单击该订阅，然后选择 `Create Storage Account...`。

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

1. 使用下表按照提示进行操作，理解如何使用你的值。

    |属性|“值”|
    |--|--|
    |为新 Web 应用输入全局唯一名称。| 为存储资源名称输入一个值，如 `fileuploadyourname`。 将 `yourname` 替换为小写名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。 只使用字符和数字，长度不超过 24。 稍后将用到此帐户名称。|

1. 应用创建过程完成后，将显示一条通知，其中包含有关新资源的信息。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource-complete.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

## <a name="set-storage-account-name-in-code-file"></a>在代码文件中设置存储帐户名称

通过将存储密钥名称添加到空字符串中，在 `src/uploadToBlob.ts` 中为 `storageAccountName` 值设置资源名称。 将其余代码保留原样。 

```typescript
const storageAccountName = process.env.storageresourcename || ""; 
```

## <a name="generate-your-shared-access-signature-sas-token"></a>生成共享访问签名 (SAS) 令牌 

在配置 CORS 之前生成 SAS 令牌。 

1. 在用于存储的 Visual Studio Code 扩展中，右键单击资源，然后选择“在门户中打开”。 这将打开指向确切的存储资源的 Azure 门户。
1. 在“设置”部分选择“共享访问签名” 。 
1. 使用以下设置配置 SAS 令牌。 

    | 属性|值|
    |--|--|
    |允许的服务|Blob|
    |允许的资源类型|服务、容器、对象|
    |允许的权限|读取、写入、删除、列出、添加、创建|
    |启用版本删除|已选中|
    |开始和到期日期/时间|接受开始日期/时间，并将结束日期时间设置为将来的 24 小时。 SAS 令牌只在 24 小时内有效。|
    |仅 HTTPS|选定|
    |首选路由层|Basic|
    |签名密钥|已选择 key1|

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-generate-sas-token.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

1.  选择“生成 SAS 和连接字符串”。 立即复制 SAS 令牌。 你将无法列出此令牌，因此如果你没有复制它，则需要生成一个新的 SAS 令牌。 

## <a name="set-sas-token-in-code-file"></a>在代码文件中设置 SAS 令牌

SAS 令牌值为部分查询字符串，并在对基于云的资源进行查询时在 URL 中使用。

令牌格式取决于你用来创建它的工具： 
* **Azure 门户** ：如果在门户中创建 SAS 令牌，该令牌将包含 `?` 作为字符串的第一个字符。
* **Azure CLI** ：如果使用 Azure CLI 创建 SAS 令牌，则返回的值不包括 `?` 作为字符串的第一个字符。 

1. 如果 `?` 是令牌的第一个字符，请将其删除。 该代码文件为你提供了 `?`，因此令牌中不需要它。

1. 通过将 SAS 令牌添加到空字符串中，将 SAS 令牌的 sasToken 值设置为 `src/uploadToBlob.ts`。 将其余代码保留原样。 

```typescript
// remove `?` if it is first character of token
const sasToken = process.env.storagesastoken || "";
```

## <a name="configure-your-azure-storage-resource-for-cors-with-azure-cli"></a>使用 Azure CLI 为 CORS 配置 Azure 存储资源

为资源配置 CORS，以便客户端 React 代码可以访问你的存储帐户。 

1. 仍处于 Azure 门户中时，请在“设置”部分选择“CORS” 。 
1. 如图所示配置 CORS。 图像下方对设置进行了说明。 

    | 属性|“值”|
    |--|--|
    |允许的域|`*`|
    |允许的方法|全部（补丁除外）。|
    |允许的标题|`*`|
    |公开的标题|`*`|
    |最长时间|86400|

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-cors.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

1. 选择设置上面的“保存”，将其保存到资源中。 此代码无需任何更改即可使用这些 CORS 设置。 

## <a name="run-project-locally-to-verify-connection-to-storage-account"></a>在本地运行项目以验证是否连接到存储帐户

SAS 令牌和存储帐户名称已在 `src/uploadToBlob.ts` 文件中设置，因此可以开始运行该应用程序。

1. 在 Visual Studio Code 终端中输入以下命令：

    ```javascript
    npm start
    ```

1. 当终端显示 URL（如 `http://localhost:3000`）时，应用已就绪。 打开浏览器并输入该 URL。 连接到 Azure 存储 Blob 的网站应显示一个文件选择按钮和一个文件上传按钮。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-configured-upload-button-displayed.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

1. 从 `images` 文件夹中选择要上传的映像。 对于此测试，`spring-flowers.jpg` 是一个不错的视觉对象。 然后选择“上传”！ 按钮。 

    React 前端客户端代码调用 `src/uploadToBlob.ts` 以向 Azure 进行身份验证，然后创建一个存储容器（如果尚不存在），并将 blob 上传到该容器。 

## <a name="want-to-know-more"></a>想要了解更多信息？ 

其他配置存储帐户的方法包括：
* 使用 [PowerShell](/azure/powershell/module/azure.storage/new-azurestorageblobsastoken) 的 SAS 令牌
* 使用门户的 SAS 令牌
* 使用 [PowerShell](/azure/powershell/module/azure.storage/set-azurestoragecorsrule) 的 CORS
* 使用门户的 CORS

详细了解[共享访问签名](/azure/storage/common/storage-sas-overview.md)。