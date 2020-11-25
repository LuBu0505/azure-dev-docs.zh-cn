---
title: SSH 到虚拟机
description: 使用 SSH 连接到 Linux 虚拟机。  如果使用的是新式 Mac、Windows 或 Linux 操作系统，应已安装基于终端的客户端 SSH。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 7c496a907b6cbac894e92d0ecccdf32411a370ab
ms.sourcegitcommit: ed749b136f0d6b876fd5866ba4a151c73af5b71f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674717"
---
# <a name="4-connect-to-linux-virtual-machine-using-ssh"></a>4.使用 SSH 连接到 Linux 虚拟机

在本教程的此部分，请在终端中使用 SSH 连接到虚拟机。 [SSH](https://www.ssh.com/ssh/) 是随多种新式 shell（包括 Azure Cloud Shell）提供的常用工具。 

## <a name="connect-with-ssh-and-change-web-app"></a>使用 SSH 连接并更改 Web 应用

使用与前面步骤相同的终端或 shell 窗口。 

1. 使用以下命令连接到远程虚拟机。 此过程假设 SSH 客户端可以找到 SSH 密钥，这些密钥是在创建 VM 的过程中创建的并且放置在本地计算机上。 如果系统询问你是否要继续连接，请回答 `yes`。 连接完成后，终端提示符应发生更改以指示远程虚拟机。 

    将 `YOUR-PUBLIC-IP-ADDRESS` 替换为你自己的虚拟机的公共 IP。 

    ```console
    ssh azureuser@YOUR-PUBLIC-IP-ADDRESS
    ``` 

1. 使用以下命令了解你在虚拟机上的位置。 你应位于 azureuser 根目录：`/home/azureuser`。 

    ```bash
    pwd
    ```

1. Web 应用位于子目录 `myapp`。 切换到 `myapp` 目录并列出内容：

    ```bash
    cd myapp && ls -l
    ```

    你应会看到一些内容，例如显示 GitHub 存储库已克隆到虚拟机和 npm 包文件：
    
    ```console
    -rw-r--r--   1 root root   891 Nov 11 20:23 cloud-init-github.txt
    -rw-r--r--   1 root root  1347 Nov 11 20:23 index-logging.js
    -rw-r--r--   1 root root   282 Nov 11 20:23 index.js
    drwxr-xr-x 190 root root  4096 Nov 11 20:23 node_modules
    -rw-r--r--   1 root root 84115 Nov 11 20:23 package-lock.json
    -rw-r--r--   1 root root   329 Nov 11 20:23 package.json
    -rw-r--r--   1 root root   697 Nov 11 20:23 readme.md
    ```

1. 安装[适用于 Application Insights 的 Azure SDK 客户端库](https://www.npmjs.com/package/applicationinsights)。

    ```bash
    sudo npm install --save applicationinsights
    ```

1. 使用 [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) 编辑器更改 `package.json` 文件。

    ```bash
    sudo nano package.json
    ```

1. 编辑文件的启动脚本以包含环境变量。 将 `REPLACE-WITH-YOUR-KEY` 替换为检测密钥值。

    ```json
    "start": "APPINSIGHTS_INSTRUMENTATIONKEY=REPLACE-WITH-YOUR-KEY pm2 start index.js --watch --log /var/log/pm2.log"
    ```

1. 使用以下命令停止并重启 PM2：

    ```bash
    sudo npm run-script stop && sudo npm start
    ```

    Azure 客户端库现在位于 node_modules 目录中，密钥将作为环境变量传递到应用中。 下一步是向 `index.js` 添加所需代码。 

1. 使终端保持打开状态并连接到 VM，你将在下一步中使用它。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [添加 Azure SDK 客户端代码以将日志记录到 Azure 云](azure-monitor-application-insights-nodejs-expressjs-code.md) 