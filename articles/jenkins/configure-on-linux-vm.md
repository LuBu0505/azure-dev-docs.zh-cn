---
title: 快速入门 - 使用 Azure CLI 配置 Jenkins
description: 了解如何在 Azure Linux 虚拟机上安装 Jenkins 以及如何构建示例 Java 应用程序。
keywords: jenkins, azure, devops, 门户, linux, 虚拟机
ms.topic: quickstart
ms.date: 08/21/2020
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 6fc5eafbec8917b517b38d7a02c3149512675ac9
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689132"
---
# <a name="quickstart-configure-jenkins-using-azure-cli"></a>快速入门：使用 Azure CLI 配置 Jenkins

本快速入门介绍如何使用经过配置的适用于 Azure 的工具和插件在 Ubuntu Linux VM 上安装 [Jenkins](https://jenkins.io)。

在本快速入门中，我们将完成以下任务：

> [!div class="checklist"]
> * 创建可下载和安装 Jenkins 的安装文件
> * 创建资源组
> * 使用安装文件创建虚拟机
> * 打开端口 8080 以访问虚拟机上的 Jenkins
> * 通过 SSH 连接到虚拟机
> * 基于 GitHub 中的示例 Java 应用配置示例 Jenkins 作业
> * 生成示例 Jenkins 作业

## <a name="prerequisites"></a>必备知识

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="troubleshooting"></a>疑难解答

如果在配置 Jenkins 时遇到任何问题，请参阅 [Cloudbees Jenkins 安装页面](https://www.jenkins.io/doc/book/installing/)来查看最新说明和已知问题。

## <a name="create-a-virtual-machine"></a>创建虚拟机

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 打开 [Azure Cloud Shell](/azure/cloud-shell/overview)，并切换到 Bash（如果尚未切换）。

1. 创建名为 `cloud-init-jenkins.txt` 的文件。

    ```bash
    code cloud-init-jenkins.txt
    ```

1. 将以下代码粘贴到新文件中：

    ```json
    #cloud-config
    package_upgrade: true
    runcmd:
      - apt install openjdk-8-jdk -y
      - wget -qO - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
      - sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
      - apt-get update && apt-get install jenkins -y
      - service jenkins restart
    ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 使用 [az group create](/cli/azure/group#az-group-create) 创建资源组。 可能需要将 `--location` 参数替换为你的环境的相应值。

    ```azurecli
    az group create \
    --name QuickstartJenkins-rg \
    --location eastus
    ```

1. 使用 [az vm create](/cli/azure/vm#az-vm-create) 创建虚拟机。

    ```azurecli
    az vm create \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm \
    --image UbuntuLTS \
    --admin-username "azureuser" \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
    ```

1. 使用 [az vm list](/cli/azure/vm#az-vm-list) 验证新虚拟机的创建（和状态）。

    ```azurecli
    az vm list -d -o table --query "[?name=='QuickstartJenkins-vm']"
    ```

1. Jenkins 默认在端口 8080 上运行。 因此，请使用 [az vm open](/cli/azure/vm#az-vm-open-port) 在新的虚拟机上打开端口 8080。

    ```azurecli
    az vm open-port \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm  \
    --port 8080 --priority 1010
    ```

## <a name="configure-jenkins"></a>配置 Jenkins

1. 使用 [az vm show](/cli/azure/vm#az-vm-show) 获取示例虚拟机的公共 IP 地址。

    ```azurecli
    az vm show \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm -d \
    --query [publicIps] \
    --output tsv
    ```

    **注释** ：

    - `--query` 参数将输出限制为虚拟机的公共 IP 地址。

1. 使用在上一步骤中检索到的 IP 地址，通过 SSH 连接到虚拟机。 需要确认连接请求。

    ```azurecli
    ssh azureuser@<ip_address>
    ```

    **注释** ：

    - 成功连接后，Cloud Shell 提示符会包含用户名和虚拟机名称：`azureuser@QuickstartJenkins-vm`。

1. 获取 Jenkins 服务的状态来验证 Jenkins 是否正在运行。

    ```bash
    service jenkins status
    ```

1. 获取自动生成的 Jenkins 密码。

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. 使用 IP 地址在浏览器中打开以下 URL：`http://<ip_address>:8080`

1. 输入之前检索到的密码，然后选择“继续”。

    ![用于解锁 Jenkins 的初始网页](./media/configure-on-linux-vm/unlock-jenkins.png)

1. 勾选“选择要安装的插件”。

    ![选择用于安装所选插件的选项](./media/configure-on-linux-vm/select-plugins.png)

1. 在页面顶部的筛选器框中，输入 `github`。 选择 GitHub 插件并选择“安装”。

    ![安装 GitHub 插件](./media/configure-on-linux-vm/install-github-plugin.png)

1. 输入第一名管理员用户的信息，然后选择“保存并继续”。

    ![输入第一名管理员用户的信息](./media/configure-on-linux-vm/create-first-user.png)

1. 在“实例配置”页面上，选择“保存并完成” 。

    ![实例配置的确认页面](./media/configure-on-linux-vm/instance-configuration.png)

1. 选择“开始使用 Jenkins”。

    ![Jenkins 已准备就绪！](./media/configure-on-linux-vm/start-using-jenkins.png)

## <a name="create-your-first-job"></a>创建第一个作业

1. 在 Jenkins 主页上，选择“创建作业”。

    ![Jenkins 控制台主页](./media/configure-on-linux-vm/jenkins-home-page.png)

1. 输入 `mySampleApp` 的作业名，选择“自由风格项目”，然后选择“确定” 。

    ![新作业创建](./media/configure-on-linux-vm/new-job.png)

1. 选择“源代码管理”选项卡。启用 Git，并为存储库 URL 输入以下 URL 值：`https://github.com/spring-guides/gs-spring-boot.git` 

    ![定义 Git 存储库](./media/configure-on-linux-vm/source-code-management.png)

1. 选择“生成”选项卡，然后选择“添加生成步骤” 

    ![添加新的生成步骤](./media/configure-on-linux-vm/add-build-step.png)

1. 从下拉列表中，选择“调用 Gradle 脚本”。

    ![选择 Gradle 脚本选项](./media/configure-on-linux-vm/invoke-gradle-script-option.png)

1. 选择“使用 Gradle 包装器”，然后在“包装器位置”中输入 `complete`，并输入 `build` 作为“任务”。 

    ![Gradle 脚本选项](./media/configure-on-linux-vm/gradle-script-options.png)

1. 选择“高级”，然后在“根生成脚本”字段中输入 `complete` 。

    ![高级 Gradle 脚本选项](./media/configure-on-linux-vm/root-build-script.png)

1. 滚动到页面底部，然后选择“保存”。

## <a name="build-the-sample-java-app"></a>生成示例 Java 应用

1. 显示项目的主页时，选择“立即生成”以编译代码并打包示例应用。

    ![项目主页](./media/configure-on-linux-vm/project-home-page.png)

1. “生成历史记录”标题下的图形指示正在生成作业。

    ![正在作业生成](./media/configure-on-linux-vm/job-currently-building.png)

1. 生成完成后，选择工作区链接。

    ![选择工作区链接](./media/configure-on-linux-vm/job-workspace.png)

1. 导航到 `complete/build/libs`，查看 `.jar` 文件是否已成功生成。

    ![目标库验证是否已成功生成](./media/configure-on-linux-vm/successful-build.png)

1. Jenkins 服务器现已就绪，可用于在 Azure 中生成你自己的项目了！

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Jenkins](./index.yml)
