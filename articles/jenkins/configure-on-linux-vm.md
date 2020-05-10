---
title: 快速入门 - 在 Azure 上创建 Jenkins 服务器
description: 了解如何通过 Jenkins 解决方案模板在 Azure Linux 虚拟机上安装 Jenkins，然后生成示例 Java 应用程序。
keywords: jenkins, azure, devops, 门户, linux, 虚拟机, 解决方案模板
ms.topic: quickstart
ms.date: 03/03/2020
ms.openlocfilehash: 2ea038dad2294784804f45026ea26198a9b12d79
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82170333"
---
# <a name="quickstart-create-a-jenkins-server-on-an-azure-linux-vm"></a>快速入门：在 Azure Linux VM 上创建 Jenkins 服务器

本快速入门介绍如何使用经过配置的适用于 Azure 的工具和插件在 Ubuntu Linux VM 上安装 [Jenkins](https://jenkins.io)。 完成后，你会有一个在 Azure 中运行的 Jenkins 服务器，并可生成 [GitHub](https://github.com) 中的示例 Java 应用。

## <a name="prerequisites"></a>先决条件

* 可以在计算机的命令行（例如 Bash shell 或 [PuTTY](https://www.putty.org/)）上访问 SSH

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>从解决方案模板创建 Jenkins VM

Jenkins 支持这样一个模型：其中的 Jenkins 服务器将工作委托给一个或多个代理，使单个 Jenkins 安装即可托管大量的项目，或者为生成或测试活动提供不同的环境。 本部分中的步骤将引导你在 Azure 上安装和配置 Jenkins 服务器。

1. 在浏览器中，打开[用于 Jenkins 的 Azure 市场映像](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview)。

1. 选择“立即获取”  。

    ![选择“立即获取”可启动 Jenkins Marketplace 映像的安装过程。](./media/install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. 在查看定价详细信息和条款信息后，选择“继续”  。

    ![Jenkins Marketplace 映像的定价和条款信息。](./media/install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. 选择“创建”  可在 Azure 门户中配置 Jenkins 服务器。 

    ![安装 Jenkins Marketplace 映像。](./media/install-from-azure-marketplace-image/jenkins-install-create.png)

1. 在“基本”  选项卡上，指定以下值：

   - **名称** - 输入 `Jenkins`。
   - 用户名  - 输入登录到运行 Jenkins 的虚拟机时要使用的用户名。 用户名称必须满足[特定要求](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm)。
   - **身份验证类型** - 选择“SSH 公钥”  。
   - **SSH 公钥** - 以单行格式（以 `ssh-rsa` 开头）或多行 PEM 格式复制并粘贴 RSA 公钥。 可以在 Linux 和 macOS 上使用 ssh-keygen 生成 SSH 密钥，或在 Windows 上使用 PuTTYGen 生成这些密钥。 有关 SSH 密钥和 Azure 的详细信息，请参阅[如何在 Azure 上将 SSH 密钥与 Windows 配合使用](/azure/virtual-machines/linux/ssh-from-windows)一文。
   - **订阅** - 选择要用于安装 Jenkins 的 Azure 订阅。
   - **资源组** - 选择“新建”  ，并输入资源组的名称，该资源组用作构成 Jenkins 安装的资源集合的逻辑容器。
   - **位置** - 选择“美国东部”。 

     ![在“基本”选项卡中输入 Jenkins 的身份验证和资源组信息。](./media/install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. 选择“确定”  以进入“其他设置”  选项卡。 

1. 在“其他设置”  选项卡中，指定以下值：

   - **大小** - 选择适合于 Jenkins 虚拟机的调整大小选项。
   - **VM 磁盘类型** - 指定 HDD（硬盘驱动器）或 SSD（固态驱动器）以指明允许将哪种存储磁盘类型用于 Jenkins 虚拟机。
   - **虚拟网络** -（可选）选择要修改默认设置的**虚拟网络**。
   - **子网** - 选择“子网”  ，验证信息，然后选择“确定”  。
   - **公共 IP 地址** - IP 地址名称默认为在前一页中指定的带有 -IP 后缀的 Jenkins 名称。 可以选择相应选项更改该默认设置。
   - **域名标签** - 为 Jenkins 虚拟机的完全限定 URL 指定值。
   - **Jenkins 版本类型** - 从以下选项中选择所需的版本类型：`LTS`、`Weekly build` 或 `Azure Verified`。 `LTS` 和 `Weekly build` 选项在 [Jenkins LTS 版本行](https://jenkins.io/download/lts/)一文中进行说明。 `Azure Verified` 选项是指已经过验证可以在 Azure 上运行的 [Jenkins LTS 版本](https://jenkins.io/download/lts/)。 
   - **JDK 类型** - 要安装的 JDK。 默认为经测试的 Zulu，即 OpenJDK 的认证版本。

     ![在“设置”选项卡中输入适用于 Jenkins 的虚拟机设置。](./media/install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. 选择“确定”  以进入“集成设置”  选项卡。

1. 在“集成设置”  选项卡中，指定以下值：

    - **服务主体** - 服务主体已添加到 Jenkins 中，作为使用 Azure 进行身份验证的凭据。 `Auto` 意味着将由 MSI（托管服务标识）创建主体。 `Manual` 意味着应由你创建主体。 
        - **应用程序 ID** 和**机密** - 如果针对“服务主体”  选项选择 `Manual` 选项，则需要为服务主体指定 `Application ID` 和 `Secret`。 [创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)时，请注意，默认角色是“参与者”  ，该角色对于使用 Azure 资源已足够。
    - **启用云代理** - 为代理指定默认云模板，其中 `ACI` 是指 Azure 容器实例，`VM` 是指虚拟机。 如果不想启用云代理，也可以指定 `No`。

1. 选择“确定”  以进入“摘要”  选项卡。

1. 显示“摘要”  选项卡时，将验证输入的信息。 （在选项卡顶部）看到“验证通过”消息后，选择“确定”。   

     ![“摘要”选项卡显示并验证所选的选项。](./media/install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. 显示“创建”  选项卡时，选择“创建”  以创建 Jenkins 虚拟机。 服务器就绪时，将在 Azure 门户中显示一条通知。

     ![Jenkins 就绪通知。](./media/install-from-azure-marketplace-image/jenkins-install-notification.png)

## <a name="connect-to-jenkins"></a>连接到 Jenkins

1. 在 Web 浏览器中导航到虚拟机（例如 `http://jenkins2517454.eastus.cloudapp.azure.com/`。 Jenkins 控制台无法通过不安全的 HTTP 进行访问，因此在页面上提供了说明，介绍如何在计算机中使用 SSH 隧道安全地访问 Jenkins 控制台。

    ![解锁 Jenkins](./media/install-solution-template-steps/jenkins-ssh-instructions.png)

1. 在页面上通过命令行使用 `ssh` 命令设置该隧道，将 `username` 替换为此前从解决方案模板设置虚拟机时选择的虚拟机管理员用户的名称。

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
    ```
    
1. 启动隧道后，导航到本地计算机上的 `http://localhost:8080/`。 

1. 通过 SSH 连接到 Jenkins VM 时，在命令行中运行以下命令，以便获取初始密码。

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
    
1. 首次解锁 Jenkins 仪表板时，请使用此初始密码。

    ![解锁 Jenkins](./media/install-solution-template-steps/jenkins-unlock.png)

1. 在下一页选择“安装建议的插件”，然后创建用于访问 Jenkins 仪表板的 Jenkins 管理员用户。 

    ![Jenkins 已准备就绪！](./media/install-solution-template-steps/jenkins-welcome.png)

Jenkins 服务器现在已就绪，可以生成代码了。

## <a name="create-your-first-job"></a>创建第一个作业

1. 从 Jenkins 控制台选择“创建新作业”，将其命名为“mySampleApp”并选择“自由格式项目”，然后选择“确定”。    

    ![创建新作业](./media/install-solution-template-steps/jenkins-new-job.png) 

1. 选择“源代码管理”选项卡，启用“Git”，然后在“存储库 URL”字段中输入以下 URL：    `https://github.com/spring-guides/gs-spring-boot.git`

    ![定义 Git 存储库](./media/install-solution-template-steps/jenkins-job-git-configuration.png) 

1. 依次选择“生成”选项卡、“添加生成步骤”、“调用 Gradle 脚本”。    选择“使用 Gradle 包装器”，然后在“包装器位置”中输入 `complete`，并输入 `build` 作为“任务”。   

    ![使用要生成的 Gradle 包装器](./media/install-solution-template-steps/jenkins-job-gradle-config.png) 

1. 选择“高级”，然后在“根生成脚本”字段中输入 `complete`。   选择“保存”。 

    ![在 Gradle 包装器生成步骤中设置高级设置](./media/install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>生成代码

1. 选择“立即生成”  ，编译代码并打包示例应用。 生成完成后，选择项目的  Workspace 链接。

    ![浏览到要从生成中获取 JAR 文件的工作区](./media/install-solution-template-steps/jenkins-access-workspace.png) 

1. 导航到 `complete/build/libs`，确保能够验证生成是否成功的 `gs-spring-boot-0.1.0.jar` 位于其中。 Jenkins 服务器现已就绪，可以在 Azure 中生成你自己的项目了。

## <a name="troubleshooting-the-jenkins-solution-template"></a>排查 Jenkins 解决方案模板问题

如果 Jenkins 解决方案模板出现 Bug，请在 [Jenkins GitHub 存储库](https://github.com/azure/jenkins/issues)中提交问题。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Jenkins](/azure/developer/jenkins)