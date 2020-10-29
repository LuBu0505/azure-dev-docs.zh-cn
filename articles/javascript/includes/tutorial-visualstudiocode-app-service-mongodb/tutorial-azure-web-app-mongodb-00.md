---
title: include 文件 tutorial-azure-web-app-mongodb-00.md
description: include 文件 tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9903ca1ae761cb2e16a7f72fcc4c0942f7fa5991
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437171"
---
在本教程中，通过 MongoDB 本机 API 将 Node.js 应用和 MongoDB 数据库配合使用。 将 Node.js 应用程序部署到 Azure 应用服务（在 Linux 上），然后验证基于云的应用是否正常运行。 

已为你完成编程工作，本教程重点介绍如何借助 Azure 扩展从 Visual Studio Code 内部成功使用本地和远程 Azure 环境。

## <a name="top-tasks"></a>主要任务

本教程包括几个面向 JavaScript 开发人员的主要 Azure 任务：

* 使用本地 MongoDB 数据库
* 将应用与容器配合使用
* 将应用部署到云
* 配置云托管的应用设置 
* 将本地应用连接到远程数据库

## <a name="sample-application"></a>示例应用程序

GitHub 上提供的[示例 Node.js 应用](https://github.com/Azure-Samples/js-e2e-express-mongo)由以下元素组成：

* 托管在端口 8080 上的 Express.js 服务器
* 简单的 React.js 服务器端视图引擎
* MongoDB 本机 API 函数，用于插入、删除和查找数据

:::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::

## <a name="the-mongodb-connection"></a>MongoDB 连接

如果无法建立数据库连接，该应用将显示消息 `No database found.`。 这将是应用的初始状态。

建立数据库连接后，该应用将在一个窗体中包含两个文本字段，并带有一个提交按钮，该窗体下显示 Mongo 集合的内容。

## <a name="limited-time-to-work-on-the-tutorial"></a>学习教程的时间有限？

在本教程中，为 Cosmos DB 创建一个 Azure 资源，这就是 Azure 托管 MongoDB 数据库的方式。 此资源创建过程可能需要长达 20 分钟的时间。 如果时间有限，可以[立即开始该过程](../../tutorial/web-app-mongodb.yml?tutorial-step=5)，以便资源在需要时可供使用。 

## <a name="want-to-know-more"></a>想要了解更多信息？ 

本教程的每个步骤都包含“想要了解详细信息？”部分。 这是可选信息，允许你深入探究。 你可以在学习本教程时阅读，或稍后返回本教程。 