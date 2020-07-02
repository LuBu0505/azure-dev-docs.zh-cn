---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: c95177d6b4cb101b764acd8f8dad54f937a495eb
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85791560"
---
1. 在终端命令提示符处，转到要创建应用文件夹的位置。

1. 运行以下命令，以通过使用 Express Generator 创建一个名为 `myexpressapp` 的新 Express 应用。 `--git --view pug` 参数告知生成器创建 .gitignore 文件并使用 [Pug](https://pugjs.org/api/getting-started.html) 模板引擎（以前称为 Jade）。

    ```bash
    npx express-generator myexpressapp --git --view pug
    ```

1. 转到应用文件夹：

    ```bash
    cd myexpressapp
    ```

1. 安装应用程序的依赖项：

    ```bash
    npm install
    ```

1. 启动服务器：

    ```bash
    npm start
    ```

1. 打开浏览器并访问 `http://localhost:3000`，对应用进行测试。 站点应如下所示：

    ![运行 Express 应用程序](../media/deploy-azure/express.png)

1. 在终端中选择“Ctrl+C”以停止服务器。  
 
