---
author: yevster
ms.author: yebronsh
ms.date: 5/26/2020
ms.openlocfilehash: 30d39530700806dc910c085c36a7834efa9c1414
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507592"
---
#### <a name="determine-whether-and-how-the-file-system-is-used"></a>确定是否使用以及如何使用文件系统

查找向本地文件系统写入和/或从中读取服务的任何实例。 确定写入和读取短期/临时文件的位置，以及写入和读取长期文件的位置。

> [!NOTE]
> Azure Spring Cloud 为每个 Azure Spring Cloud 实例提供了 5 GB 临时存储（在 `/tmp` 中装载）。 如果临时文件的写入超出了该限制或位于其他位置，则需要进行代码更改。

<!-- The following two "static content" sections should be identical to the contents of includes\static-content.md except that here we use H5 headings. -->

##### <a name="read-only-static-content"></a>只读静态内容

如果应用程序当前提供静态内容，则需为其提供一个备用位置。 可能需要考虑将静态内容移到 Azure Blob 存储，并添加 Azure CDN，方便用户在全球范围内快速下载。 有关详细信息，请参阅 [Azure 存储中的静态网站托管](/azure/storage/blobs/storage-blob-static-website)和[快速入门：将 Azure 存储帐户与 Azure CDN 集成](/azure/cdn/cdn-create-a-storage-account-with-cdn)。

##### <a name="dynamically-published-static-content"></a>动态发布的静态内容

如果应用程序允许那些通过应用程序上传/生成但在创建后不可变的静态内容，则可将上述 Azure Blob 存储和 Azure CDN 与 Azure 函数配合使用，以便处理上传和 CDN 刷新操作。 我们提供了一个示例实现，用于[通过 Azure Functions 进行静态内容的上传和 CDN 预加载操作](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)。
