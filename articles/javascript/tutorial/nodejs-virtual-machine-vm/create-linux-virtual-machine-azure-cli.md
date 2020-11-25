---
title: 创建 Linux 虚拟机
description: 使用 Azure CLI 创建和配置虚拟机。 在本教程的此阶段，应已打开一个终端窗口，并在要创建虚拟机的订阅上使用 Azure CLI 登录到了 Azure 云。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: d2da4cac2a93ad953ada9cd98de679abba034908
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625026"
---
# <a name="3-create-linux-virtual-machine-using-azure-cli"></a>3.使用 Azure CLI 创建 Linux 虚拟机

在本教程的此部分，使用 Azure CLI 创建和配置虚拟机。 在本教程的此阶段，应已打开一个终端窗口，并在要创建虚拟机的订阅上登录到了 Azure 云。 

所有 Azure CLI 步骤可集中通过一个 Azure CLI 实例完成。 如果关闭了在其中使用 Azure CLI 的窗口或进行了切换（如在 Cloud Shell 与本地终端之间切换），则需要重新登录。 

## <a name="create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation"></a>创建 cloud-init 文件以加速创建 Linux 虚拟机

本教程使用 cloud-init 配置文件创建 NGINX 反向代理服务器和 Express.js 服务器。 NGINX 用于将 Express.js 端口 (3000) 转发到公用端口 (80)。 

`runcmd` 具有多个任务：
* 下载并安装 Node.js
* 克隆 Express.js 示例存储库
* 安装 Express.js 依赖项
* 使用 PM2 启动 Express.js 应用

1. 创建一个名为 `cloud-init-github.txt` 的本地文件，并将以下内容保存到文件中，也可[将该存储库的文件保存](https://github.com/Azure-Samples/js-e2e-vm/blob/main/cloud-init-github.txt)到本地计算机。 [cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html#yaml-examples) 格式的文件需要与 Azure CLI 命令的终端路径位于同一文件夹中。

    :::code language="yaml" source="~/../js-e2e-vm/cloud-init-github.txt" :::

## <a name="create-a-virtual-machine-resource"></a>创建虚拟机资源 

1. 在终端输入 [Azure CLI 命令](/cli/azure/vm?view=azure-cli-latest#az_vm_create)以创建 Linux 虚拟机的 Azure 资源。 此命令将从 cloud-init 文件创建 VM，并为你生成 SSH 密钥。 正在运行的命令将显示密钥的存储位置。 

    ```azurecli
    az vm create \
      --resource-group rg-demo-vm-eastus \
      --name demo-vm \
      --location eastus \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data cloud-init-github.txt
    ```

    此过程可能需要几分钟时间。 完成此过程后，Azure CLI 将返回有关新资源的信息。 保留 `publicIpAddress` 值，在浏览器中查看 Web 应用和连接到 VM 时需要用到它。 
     

1. 第一次创建时，虚拟机未打开任何端口。 使用以下 [Azure CLI 命令](/cli/azure/vm?view=azure-cli-latest#az_vm_open_port)打开端口 80，以便 Web 应用可公开使用：

    ```azurecli
    az vm open-port \
      --port 80 \
      --resource-group rg-demo-vm-eastus \
      --name demo-vm
    ```

1. 在 Web 浏览器中使用公共 IP 地址，以确保虚拟机可用且正在运行。 更改 URL 以使用 `publicIpAddress` 中的值。

    ```HTTP
    http://YOUR-PUBLIC-IP-ADDRESS
    ```

    下图表示 Web 应用，但你的应用将使用不同的 IP 地址。 如果资源由于网关错误而失败，请稍后重试，Web 应用可能需要一分钟时间才能启动。 

    :::image type="content" source="../../media/tutorial-vm/basic-web-app.png" alt-text="Azure 上的 Linux 虚拟机提供的简单应用。":::

    Web 应用的初始代码文件具有单个路由，该路由显示客户端 IP 地址并通过 NGINX 代理传递。 

    :::code language="JavaScript" source="~/../js-e2e-vm/index.js" :::

1. 使终端保持打开状态，整个教程都会用到它。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 SSH 连接到 VM](connect-linux-virtual-machine-ssh.md) 