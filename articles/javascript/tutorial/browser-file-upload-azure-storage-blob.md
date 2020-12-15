---
title: 使用 VSCode 将映像上传到 Blob 存储 - 应用服务/CosmosDB
description: 使用 React 应用将文件上传到 Azure 存储 Blob。 本文重点介绍如何将本地和远程环境与 Visual Studio Code 扩展配合使用。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript, azure-sdk-storage-blob-typescript-version-12.2.1
ms.openlocfilehash: 514c4cf3c5d54c30451759958b1cef5e465c5c8e
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772595"
---
# <a name="upload-an-image-to-an-azure-storage-blob"></a>将映像上传到 Azure 存储 Blob

使用客户端 React 应用，通过 Azure 存储 npm 包将图像文件上传到 Azure 存储 Blob。 

已为你完成编程工作，本教程重点介绍如何借助 Azure 扩展从 Visual Studio Code 内部成功使用本地和远程 Azure 环境。

* [源代码](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)

## <a name="application-architecture-and-functionality"></a>应用程序体系结构和功能

本教程包括几个面向 JavaScript 开发人员的主要 Azure 任务：

* 使用 Visual Studio Code 在本地运行 React 应用
* 创建存储资源并配置文件上传
    * 配置 CORS
    * 创建共享访问签名 (SAS) 令牌
* 配置 Azure SDK 客户端库的代码，以使用 SAS 令牌向服务进行身份验证

[GitHub 上提供的](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)示例 React 应用由以下元素组成：

* 托管在端口 3000 上的 React 应用
* 要上传到存储 blob 的 Azure SDK 客户端库脚本

:::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-image-uploaded-displayed.png" alt-text="连接到 Azure 存储 Blob 的简单 React 应用。":::

## <a name="1-set-up-development-environment"></a>1.设置开发环境

- 具有活动订阅的 Azure 用户帐户。 [免费创建一个](https://azure.microsoft.com/free/)。
- [Node.js 和 npm](https://nodejs.org/en/download)，即安装到本地计算机的 Node.js 包管理器。
- 安装到本地计算机中的 [Visual Studio Code](https://code.visualstudio.com/)。 
- Visual Studio Code 扩展：
    - [Azure 存储](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) - 用于查看存储资源

## <a name="2-clone-and-run-the-initial-react-app"></a>2.克隆并运行初始 React 应用

克隆应用、安装依赖项并运行应用。 如果在代码中配置了 Azure 存储，则初始应用会尝试连接到 Azure 存储，如果不可用，则会显示消息 `Storage is not configured`。 

1. 打开 Visual Studio Code。
1. 通过选择 Git 图标并选择“克隆存储库”来克隆 GitHub 存储库。 此过程要求你登录到 GitHub。 使用 `https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob` 的存储库 URL，然后在本地计算机上选择一个文件夹将示例克隆到其中。 出现提示时，打开克隆的存储库。 

    :::image type="content" source="../media/tutorial-browser-file-upload/vscode-git-clone-repository.png" alt-text="通过选择 Git 图标，然后选择“克隆存储库”来克隆 GitHub 存储库。":::

1. 在 Visual Studio Code 中，打开终端窗口，然后运行以下命令以安装示例的依赖项。

    ```javascript
    npm install
    ```

1. 在同一终端窗口中，运行命令以运行 Web 应用。

    ```javascript
    npm start
    ```

1. 打开 Web 浏览器并使用以下 url 查看本地计算机上的 Web 应用。

    ```url
    http://localhost:3000/
    ```

    如果在浏览器中看到这一简单的 Web 应用，其中文本显示“未配置存储”，则本教程的此部分已成功完成。

    :::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-no-azure-storage-resource-configured.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::

1. 在 Visual Studio Code 终端中使用 Control+C 停止代码。

## <a name="3-create-storage-resource-with-visual-studio-extension"></a>3.用 Visual Studio 扩展创建存储资源

1. 导航到 Azure 存储扩展。 右键单击该订阅，然后选择 `Create Storage Account...`。

    :::image type="content" source="../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource.png" alt-text="导航到 Azure 存储扩展。右键单击该订阅，然后选择“创建存储帐户…”。":::

1. 使用下表按照提示进行操作，理解如何使用你的值。

    |属性|“值”|
    |--|--|
    |为新 Web 应用输入全局唯一名称。| 为存储资源名称输入一个值，如 `fileuploadyourname`。 将 `yourname` 替换为小写名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。 只使用字符和数字，长度不超过 24。 稍后将用到此帐户名称。|

1. 应用创建过程完成后，将显示一条通知，其中包含有关新资源的信息。 

    :::image type="content" source="../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource-complete.png" alt-text="应用创建过程完成后，将显示一条通知，其中包含有关新资源的信息。":::

## <a name="4-set-storage-account-name-in-code-file"></a>4.在代码文件中设置存储帐户名称

通过将存储密钥名称添加到空字符串中，在 `src/uploadToBlob.ts` 中为 `storageAccountName` 值设置资源名称。 将其余代码保留原样。 

```typescript
const storageAccountName = process.env.storageresourcename || ""; 
```

## <a name="5-generate-your-shared-access-signature-sas-token"></a>5.生成共享访问签名 (SAS) 令牌 

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

    :::image type="content" source="../media/tutorial-browser-file-upload/azure-portal-storage-blob-generate-sas-token.png" alt-text="如图所示配置 SAS 令牌。图像下方对设置进行了说明。":::

1. 选择“生成 SAS 和连接字符串”。 立即复制 SAS 令牌。 你将无法列出此令牌，因此如果你没有复制它，则需要生成一个新的 SAS 令牌。 

## <a name="set-sas-token-in-code-file"></a>在代码文件中设置 SAS 令牌

SAS 令牌值为部分查询字符串，并在对基于云的资源进行查询时在 URL 中使用。

令牌格式取决于你用来创建它的工具： 
* **Azure 门户**：如果在门户中创建 SAS 令牌，该令牌将包含 `?` 作为字符串的第一个字符。
* **Azure CLI**：如果使用 Azure CLI 创建 SAS 令牌，则返回的值不包括 `?` 作为字符串的第一个字符。 

1. 如果 `?` 是令牌的第一个字符，请将其删除。 该代码文件为你提供了 `?`，因此令牌中不需要它。

1. 通过将 SAS 令牌添加到空字符串中，将 SAS 令牌的 sasToken 值设置为 `src/uploadToBlob.ts`。 将其余代码保留原样。 

```typescript
// remove `?` if it is first character of token
const sasToken = process.env.storagesastoken || "";
```

## <a name="6-configure-cors-for-azure-storage-resource"></a>6.为 Azure 存储资源配置 CORS

为资源配置 CORS，以便客户端 React 代码可以访问你的存储帐户。 

1. 仍处于 Azure 门户中时，请在“设置”部分选择“CORS”。 
1. 如图所示配置 CORS。 图像下方对设置进行了说明。 

    | 属性|“值”|
    |--|--|
    |允许的域|`*`|
    |允许的方法|全部（补丁除外）。|
    |允许的标题|`*`|
    |公开的标题|`*`|
    |最长时间|86400|

    :::image type="content" source="../media/tutorial-browser-file-upload/azure-portal-storage-blob-cors.png" alt-text="如图所示配置 CORS。图像下方对设置进行了说明。":::

1. 选择设置上面的“保存”，将其保存到资源中。 此代码无需任何更改即可使用这些 CORS 设置。 

## <a name="7-run-project-locally-to-verify-connection-to-storage-account"></a>7.在本地运行项目以验证是否连接到存储帐户

SAS 令牌和存储帐户名称已在 `src/uploadToBlob.ts` 文件中设置，因此可以开始运行该应用程序。

1. 在 Visual Studio Code 终端中输入以下命令：

    ```javascript
    npm start
    ```

1. 当终端显示 URL（如 `http://localhost:3000`）时，应用已就绪。 打开浏览器并输入该 URL。 连接到 Azure 存储 Blob 的网站应显示一个文件选择按钮和一个文件上传按钮。 

    :::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-configured-upload-button-displayed.png" alt-text="连接到 Azure 存储 Blob 的 React 网站应显示一个文件选择按钮和一个文件上传按钮。":::

1. 从 `images` 文件夹中选择要上传的映像。 对于此测试，`spring-flowers.jpg` 是一个不错的视觉对象。 然后选择“上传”！ 按钮。 

    React 前端客户端代码调用 `src/uploadToBlob.ts` 以向 Azure 进行身份验证，然后创建一个存储容器（如果尚不存在），然后将文件上传到该容器。 

## <a name="troubleshoot-local-connection-to-storage-account"></a>排查存储帐户的本地连接的故障

如果收到错误或文件未上传到容器，请检查以下各项：

* 重新创建 SAS 令牌，确保令牌是在存储资源级别创建而不是在容器级别创建的。 将新令牌复制到代码中的正确位置。
* 检查复制到代码中的令牌字符串是否在字符串开头不包含 `?`（问号）。
* 验证存储资源的 CORS 设置。

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

## <a name="clean-up-resources"></a>清理资源

在 Visual Studio Code 中，使用用于存储的 Azure 资源管理器，右键单击资源并选择“删除存储帐户…”。

## <a name="next-steps"></a>后续步骤

如果想继续探索此应用，可了解如何通过以下选择之一将应用部署到 Azure 进行托管：

* [作为静态 Web 应用上传](/azure/static-web-apps/getting-started?tabs=vanilla-javascript)
* 使用[应用服务的 Visual Studio Code 扩展上传到 Web 应用资源](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
* [将应用上传到 Azure VM](nodejs-virtual-machine-vm/introduction.md)