---
title: include 文件 tutorial-azure-web-app-mongodb-00.md
description: include 文件 tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: b9ab409c845b7690474eb47117df4401ecaf59cd
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344102"
---
在本教程中，使用 React 应用将文件上传到 Azure 存储 Blob。 

已为你完成编程工作，本教程重点介绍如何借助 Azure 扩展从 Visual Studio Code 内部成功使用本地和远程 Azure 环境。

## <a name="top-tasks"></a>主要任务

本教程包括几个面向 JavaScript 开发人员的主要 Azure 任务：

* 使用 Visual Studio Code 在本地运行 React 应用
* 创建存储资源并配置文件上传
    * 配置 CORS
    * 创建共享访问签名 (SAS) 令牌
* 配置 Azure SDK 客户端库的代码，以使用 SAS 令牌向服务进行身份验证

## <a name="sample-application"></a>示例应用程序

[GitHub 上提供的](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)示例 React 应用由以下元素组成：

* 托管在端口 3000 上的 React 应用
* 要上传到存储 blob 的 Azure SDK 客户端库脚本

:::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-image-uploaded-displayed.png" alt-text="连接到 Azure 存储 Blob 的简单 React 应用。":::
