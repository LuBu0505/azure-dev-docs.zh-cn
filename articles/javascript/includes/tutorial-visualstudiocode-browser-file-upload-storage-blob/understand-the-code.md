---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: include 文件 understand-the-code.md
description: include 文件 understand-the-code.md
ms.openlocfilehash: fd9d2eb2c6c8dd3a39d737cdcdf609b9cf04cf7e
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344104"
---
在本教程的这一部分中，了解示例中使 Web 应用上传文件的代码。

## <a name="sample-created-with-create-react-app"></a>使用 create-react-app 创建的示例

[示例](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)是一种基本 React 应用，通过 TypeScript 模板使用 [create-react-app](https://create-react-app.dev/docs/adding-typescript/) 创建。

```typescript
npx create-react-app my-app --template typescript
```

create-react-app 框架可用于：
* 自动提供文件捆绑，这是在浏览器中使用 Azure 客户端库的必要条件
* 提供转译和生成脚本 

## <a name="upload-button-functionality"></a>上传按钮功能

`src/app` 文件是在使用 create-react-app 创建应用的过程中提供的。 已对文件进行了修改，以提供文件选择按钮和上载按钮，以及提供该功能的支持代码。 

已突出显示连接到 Azure Blob 存储代码的代码。 对 `uploadFileToBlob` 的调用以平面列表的形式返回容器中的所有 blob（文件）。 该列表通过 `DisplayImagesFromContainer` 函数显示。

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/App.tsx" highlight="3,28":::

## <a name="upload-file-to-azure-storage-blob-with-azure-sdk-client-library"></a>使用 Azure SDK 客户端库将文件上传到 Azure 存储 Blob

用于将文件上传到 Azure 存储的代码与框架无关。 由于代码是为教程构建的，因此代码选择的依据是简单易懂。 这些选择已进行了说明；你应该检查自己项目的意向性使用、安全性和效率。 

该示例创建并使用可公开访问的容器和文件。 如果希望保护自己项目中的文件，可以在许多层中进行控制，从要求对资源进行总体身份验证到细化到每个 Blob 对象的特定权限。 

### <a name="dependencies-and-variables"></a>依赖项和变量

`uploadToBlob.ts` 文件加载依赖项，并通过环境变量或硬编码字符串拉取所需的变量。

| 变量 | 描述 |
|--|--|
|`sasToken`|使用 Azure 门户创建的 SAS 令牌前面带有一个 `?`。 在 `sasToken` 变量中设置之前将其删除。| 
|`container`|Blob 存储中的容器的名称。 可以将其视为等同于文件系统的文件夹或目录。|
|`storageAccountName`|你的资源名称。|

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="2,5,16" id="snippet_package":::

### <a name="security-for-azure-credentials"></a>Azure 凭据的安全性

在你自己的项目中，考虑存储机密（如 SAS 令牌）的位置。 如果应用程序要求你保护 Azure 信息，请考虑将此存储代码托管在 [Azure 函数](/azure/azure-functions/)中而不是客户端上，然后从 React 应用中调用该 Azure 函数。  

### <a name="create-storage-client-and-manage-steps"></a>创建存储客户端并管理步骤

`uploadFileToBlob` 函数是文件的主要函数。 它为存储服务创建客户端对象，为容器对象创建客户端，上传文件，然后获取容器中所有 Blob 的列表。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="5,6,7" id="snippet_uploadFileToBlob":::

### <a name="upload-file-to-blob"></a>将文件上传到 Blob

`createBlobInContainer` 函数使用 `uploadBrowserData` 客户端库方法将文件上传到容器。 如果要使用浏览器功能，则内容类型必须与请求一起发送，这取决于文件类型，例如显示图片。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_createBlobInContainer":::

### <a name="get-list-of-blobs"></a>获取 Blob 列表

`getBlobsInContainer` 函数获取容器中 Blob 的 URL 列表。 构造这些 URL 以用作 HTML 中图像显示的 `src`：`<img src={item} alt={item} height="200" />`。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_getBlobsInContainer":::

