---
title: 步骤 5：使用 VS Code 通过 Python 部署无服务器 Azure Functions
description: 教程步骤 5，将 Python 无服务器函数代码部署到 Azure，学习如何在本地项目和 Azure 之间流式传输日志并同步设置。
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperfq2
ms.openlocfilehash: bcbc7116c1c0bc0cf323d56e46471856ebc5e9ea
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96759294"
---
# <a name="5-deploy-azure-functions-in-python"></a>5：在 Python 中部署 Azure Functions

[上一步：本地调试](tutorial-vs-code-serverless-python-04.md)

将函数部署到 Azure 意味着在 Azure 中创建一个 Functions 应用以及其他必需的 Azure 资源。 函数应用可将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。

函数应用需要一个用于数据的 Azure 存储帐户，以及一项[托管计划](/azure/azure-functions/functions-scale#hosting-plan-support)。 所有这些资源都被组织到单个资源组中。

1. 在“Azure：函数”Functions”资源管理器中选择“部署到函数应用”命令，或者打开命令面板 (**F1**)，然后选择“Azure Functions:部署到函数应用”命令。 同样，函数应用是 Python 项目在 Azure 中运行时所在的位置。

    ![将 Python 函数部署到 Azure 函数应用](media/tutorial-vs-code-serverless-python/deploy-a-python-fuction-to-azure-function-app.png)

1. 系统提示时（“在 Azure 中选择函数应用”），选择“在 Azure 中创建新的函数应用”，提供在 Azure 中独一无二的名称（通常使用个人或公司名称以及其他唯一标识符；可以使用字母、数字和连字符） 。

    如果以前创建过函数应用，其名称会显示在此选项列表中。

1. 在接下来的两个提示出现时，选择 Python 版本和 Azure 位置。

1. 此扩展执行以下操作，这些操作可以在 Visual Studio Code 弹出消息和“输出”窗口中观察到（此过程需要数分钟）：

    - 在所选位置使用你已命名的名称（删除连字符）来创建资源组。
    - 在该资源组中，创建存储帐户、托管计划和函数应用。 默认情况下，Azure Functions 使用[消耗计划](/azure/azure-functions/functions-scale#consumption-plan)。 若要在专用计划中运行函数，需[启用通过高级创建选项进行发布的功能](/azure/azure-functions/functions-develop-vs-code)。
    - 将代码部署到函数应用。

    “Azure:Functions”资源管理器也显示进度：

    ![“Azure:Functions”资源管理器中的“新建函数”命令](media/tutorial-vs-code-serverless-python/deployment-progress-indicator-in-azure-function-explorer.png)

1. 部署完成后，Azure Functions 扩展会显示一条消息，其中包含用于三项其他操作的按钮：

    ![指示已成功完成部署并提供其他操作的消息](media/tutorial-vs-code-serverless-python/azure-functions-deployment-success-with-additional-actions.png)

    对于“流式传输日志”和“上传设置”，请参阅后续部分。 

1. 选择“查看输出”，以切换到“输出”窗口。 输出中显示了 Azure 上的公共终结点（特定终结点的 URL 与你为函数应用指定的名称匹配）：

    <pre>
    1:38:04 PM vscode-azure-functions: HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    </pre>

    使用此终结点运行已在本地运行的相同测试，将 URL 参数和/或请求与请求正文中的 JSON 数据配合使用。 公共终结点的结果应该与以前在[第 4 部分](tutorial-vs-code-serverless-python-04.md)测试的本地终结点的结果相符。

## <a name="examine-logs-live-metrics"></a>检查日志（实时指标）

使用部署消息弹出窗口中的“流日志”按钮，可在浏览器中打开 Azure 门户中函数的“实时指标”部分 。 还可以通过右键单击函数项目名称并选择“启动流日志”来连接到 Azure Functions 资源管理器中使用的指标 。

## <a name="sync-local-settings-to-azure"></a>将本地设置同步到 Azure

部署弹出消息中的“上传设置”按钮会将你对 *local.settings.json* 文件所做的任何更改应用到 Azure。 也可调用 **Azure Functions** 资源管理器上的命令，方法是：展开 Functions 项目节点，右键单击“应用程序设置”，然后选择“上传本地设置”。  还可以使用命令面板来选择“Azure Functions:上传本地设置”命令。

上传设置可以更新任何现有的设置并添加在 *local.settings.json* 中定义的任何新设置。 上传操作不会删除未在本地文件中列出的任何来自 Azure 的设置。 若要删除那些设置，请在 **Azure Functions** 资源管理器中展开“应用程序设置”节点，右键单击设置，然后选择“删除设置”。  也可直接在 Azure 门户中编辑设置。

若要通过门户或 **Azure 资源管理器** 应用对 *local.settings.json* 文件所做的更改，请右键单击“应用程序设置”节点，然后选择“下载远程设置”命令。  还可以使用命令面板来选择“Azure Functions:下载远程设置”命令。

> [!div class="nextstepaction"]
> [我部署了函数 - 转到步骤 6 >>>](tutorial-vs-code-serverless-python-06.md)

[存在问题？请告诉我们。](https://aka.ms/python-functions-qs-ms-survey)
