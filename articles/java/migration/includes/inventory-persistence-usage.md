---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: 478059f598b9c39609a85647945ebef3df3cb7ec
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671633"
---
### <a name="inventory-persistence-usage"></a>清点持久性使用情况

使用应用程序服务器上的文件系统需要重新配置，在极少数情况下需要体系结构更改。 可以识别下面的部分或所有情况。

#### <a name="read-only-static-content"></a>只读静态内容

如果应用程序当前通过某种方式（例如，通过 Apache 集成）提供静态内容，则需为该静态内容提供一个备用位置。 可能需要考虑将静态内容移到 Azure Blob 存储，并添加 Azure CDN，方便用户在全球范围内快速下载。 有关详细信息，请参阅 [Azure 存储中的静态网站托管](/azure/storage/blobs/storage-blob-static-website)和[快速入门：将 Azure 存储帐户与 Azure CDN 集成](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account)。

#### <a name="dynamically-published-static-content"></a>动态发布的静态内容

如果应用程序允许那些通过应用程序上传/生成但在创建后不可变的静态内容，则可将上述 Azure Blob 存储和 Azure CDN 与 Azure 函数配合使用，以便处理上传和 CDN 刷新操作。 我们提供了一个示例实现，用于[通过 Azure Functions 进行静态内容的上传和 CDN 预加载操作](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)。
