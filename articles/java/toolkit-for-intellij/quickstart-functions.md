---
title: 在 Azure 中使用 IntelliJ IDEA 创建你的第一个函数
description: 使用 Azure Toolkit for IntelliJ 创建一个简单的 HTTP 触发函数并将其发布到 Azure。
ms.topic: quickstart
ms.date: 03/26/2020
ms.openlocfilehash: ff0733e275f89ffa349f8455455df93587ff4fdf
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81674813"
---
# <a name="quickstart-create-an-azure-functions-project-using-intellij-idea"></a>快速入门：使用 IntelliJ IDEA 创建 Azure Functions 项目

本文介绍如何使用 IntelliJ IDEA 创建响应 HTTP 请求的函数。 在本地测试代码后，将代码部署到 Azure Functions 的无服务器环境。 完成本快速入门会从你的 Azure 帐户中扣取最多几美分的费用。

## <a name="configure-your-environment"></a>配置环境

在开始之前，请确保已满足下列要求：

+ 具有活动订阅的 Azure 帐户。 [免费创建帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
+ [Azure 支持的 Java 开发工具包 (JDK)](https://aka.ms/azure-jdks)（适用于 Java 8）
+ 已安装的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) 旗舰版或社区版
+ [Maven 3.5.0+](https://maven.apache.org/download.cgi)
+ 最新的 [Function Core Tools](https://github.com/Azure/azure-functions-core-tools)

## <a name="installation-and-sign-in"></a>安装和登录

1. 在 IntelliJ IDEA 的“设置/首选项”对话框中 (Ctrl+Alt+S) 中，选择“插件”。  然后，在“市场”中找到“Azure Toolkit for IntelliJ”并单击“安装”。    安装后，单击“重启”以激活该插件。  

   ![市场中的 Azure Toolkit for IntelliJ 插件][marketplace]

2. 若要登录到你的 Azure 帐户，请打开边栏中的“Azure 资源管理器”，然后单击顶部栏中的“Azure 登录”图标（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 登录”）。   

   ![“IntelliJ Azure 登录”命令][I01]

3. 在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](sign-in-instructions.md)）。   

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][I03]

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][I04]

6. 在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。  

   ![“选择订阅”对话框][I05]

## <a name="create-your-local-project"></a>创建本地项目

本节介绍如何使用 Azure Toolkit for IntelliJ 创建一个本地 Azure Functions 项目。 稍后在本文中，你要将函数代码发布到 Azure。 

1. 打开“欢迎使用 IntelliJ”对话框，选择“创建新项目”  以打开“新建项目”向导，然后选择“Azure Functions”  。

    ![创建 Functions 项目](media/quickstart-functions/create-functions-project.png)

1. 选择“HTTP 触发器”  ，然后单击“下一步”  并按照向导的指示完成以下页面中的所有配置；确认项目位置，然后单击“完成”  ；然后，Intellj IDEA 将打开新项目。

    ![创建 Functions 项目 - 完成](media/quickstart-functions/create-functions-project-finish.png)

## <a name="run-the-function-app-locally"></a>在本地运行函数应用

1. 导航到 `src/main/java/org/example/functions/HttpTriggerFunction.java`，查看生成的代码。 在第 17 行旁，你可看到一个绿色的“运行”按钮。单击它并选择“运行 azure-function-exam…”，你即可看到函数应用在本地运行，并带有一些日志    。

    ![本地运行 Functions 项目](media/quickstart-functions/local-run-functions-project.png)

    ![本地运行函数 - 输出](media/quickstart-functions/local-run-functions-output.png)

1. 可通过从浏览器访问打印的终结点（如 `http://localhost:7071/api/HttpTrigger-Java?name=Azure`）来试用该函数。

    ![本地运行函数 - 测试结果](media/quickstart-functions/local-run-functions-test.png)

1. 日志还会在 IDEA 中打印出来。现在，单击“停止”  按钮，停止该功函数。

    ![本地运行函数 - 测试日志](media/quickstart-functions/local-run-functions-log.png)

## <a name="debug-the-function-app-locally"></a>在本地调试函数应用

1. 现在，尝试在本地调试你的函数应用。单击工具栏中的“调试”  按钮（如果看不到该按钮，请单击“视图”->“外观”->“工具栏”  以启用工具栏）。

    ![本地调试函数 - 按钮](media/quickstart-functions/local-debug-functions-button.png)

1. 单击文件 `src/main/java/org/example/functions/HttpTriggerFunction.java` 的第 20 行，添加一个断点，然后再次访问终结点 `http://localhost:7071/api/HttpTrigger-Java?name=Azure`，你会发现断点已命中。可试用更多调试功能，如“单步执行”、“监视”和“评估”     。 单击“停止”按钮，停止调试会话。

    ![本地调试函数 - 中断](media/quickstart-functions/local-debug-functions-break.png)

## <a name="deploy-your-function-app-to-azure"></a>将函数应用部署到 Azure

1. 在 IntelliJ 项目资源管理器中右键单击你的项目，选择“Azure”->“部署到 Azure Functions” 

    ![将函数部署到 Azure](media/quickstart-functions/deploy-functions-to-azure.png)

1. 如果你还没有任何函数应用，单击“无可用函数，单击以创建一个新函数”  。

    ![将函数部署到 Azure - 创建应用](media/quickstart-functions/deploy-functions-create-app.png)

1. 键入函数应用名称并选择适当的订阅/平台/资源组/应用服务计划。你也可在此处创建资源组/应用服务计划。 然后，保持应用设置不变，然后单击“确定”，等待几分钟以创建新函数  。 待“正在创建新的函数应用…”  进度条消失。

    ![将函数部署到 Azure - 创建应用向导](media/quickstart-functions/deploy-functions-create-app-wizard.png)

1. 选择要部署的函数应用（自动选择你刚刚创建的新函数应用）。 单击“运行”  以部署函数。

    ![将函数部署到 Azure - 运行](media/quickstart-functions/deploy-functions-run.png)

    ![将函数部署到 Azure - 日志](media/quickstart-functions/deploy-functions-log.png)

## <a name="manage-azure-functions-from-idea"></a>在 IDEA 中管理 Azure Functions

1. 可在 IDEA 中通过“Azure 资源管理器”来管理函数。单击“函数应用”，即可在此处看到你的所有函数   。

    ![在资源管理器中查看函数](media/quickstart-functions/explorer-view-functions.png)

1. 单击以选择一个函数，然后右键单击它，选择“显示属性”  可打开详细信息页。 

    ![显示函数属性](media/quickstart-functions/explorer-functions-show-properties.png)

1. 右键单击 HttpTrigger-Java  函数，然后选择“触发器函数”  ，你可看到浏览器打开了触发器 URL。

    ![将函数部署到 Azure - 运行](media/quickstart-functions/explorer-trigger-functions.png)

## <a name="add-more-functions-to-the-project"></a>向项目添加更多函数

1. 右键单击 org.example.functions 包  ，然后选择“新建”->“Azure 函数类”  。 

    ![向项目添加函数 - 项](media/quickstart-functions/add-functions-entry.png)

1. 在“创建函数类”向导中填写类名 HttpTest  并选择 HttpTrigger  ，单击“确定”  即可创建。采用这种方式，你可根据需要创建新的函数。

    ![向项目中添加函数 - 选择触发器](media/quickstart-functions/add-functions-trigger.png)
    
    ![向项目添加函数 - 输出](media/quickstart-functions/add-functions-output.png)

## <a name="next-steps"></a>后续步骤

我们已使用 HTTP 触发的函数创建 Java 函数项目，在本地计算机上运行该项目，并将其部署到 Azure。 现在，通过以下方式扩展函数

> [!div class="nextstepaction"]
> [添加 Azure 存储队列输出绑定](/azure/azure-functions/functions-add-output-binding-storage-queue-java)


[marketplace]:./media/create-hello-world-web-app/marketplace.png
[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png
[I03]: media/sign-in-instructions/I03.png
[I04]: media/sign-in-instructions/I04.png
[I05]: media/sign-in-instructions/I05.png
