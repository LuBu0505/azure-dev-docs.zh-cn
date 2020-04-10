---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: 59eea52aab3f2b1941a3accb5848bc4ad92d2992
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740512"
---
1. 在终端中或命令提示符处，导航到要创建应用文件夹的位置。

1. 运行以下命令，使用 Express Generator 创建一个名为 *myexpressapp* 的新 Express 应用。 `--git --view pug` 参数告知生成器创建 *.gitignore* 文件 并使用 [pug](https://pugjs.org/api/getting-started.html) 模板引擎（以前称为 Jade）。

    ```bash
    npx express-generator myexpressapp --git --view pug
    ```

1. 导航到应用文件夹：

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

1. 通过打开浏览器访问 [http://localhost:3000](http://localhost:3000) 来测试应用。 站点应如下所示：

    ![运行 Express 应用程序](../media/deploy-azure/express.png)

1. 在终端中按 **Ctrl**+**C** 停止服务器。
