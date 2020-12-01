---
title: 步骤 5：使用 VS Code 将 Python Web 应用部署到 Linux 上的 Azure 应用服务
description: 教程步骤 5，部署 Web 应用代码
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 7b3743d417ed3455c59f5b9887ee54728fb7318a
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485691"
---
# <a name="5-deploy-your-python-web-app-to-azure-app-service-on-linux"></a>5：将 Python Web 应用部署到 Linux 上的 Azure 应用服务

[上一步：配置自定义启动文件](tutorial-deploy-app-service-on-linux-04.md)

按照此过程操作可将 Python 应用部署到 Azure 应用服务。

1. 在 Visual Studio Code 中打开“Azure:  应用服务”资源管理器，然后选择蓝色的向上箭头：

   ![在应用服务资源管理器中将 Web 应用部署到应用服务](media/deploy-azure/deploy-web-app-to-app-service-in-app-service-explorer.png)

    也可右键单击应用服务名称，然后选择“部署到 Web 应用”命令。 

1. 在随后的提示窗口中，提供以下详细信息：

    - 对于“选择要部署的文件夹”选项，请选择当前的应用文件夹。
    - 对于“选择 Web 应用”选项，请选择在上一步创建的应用服务。
    - 如果系统提示更新构建配置以运行构建命令，请回答“是”。
    - 如果系统提示覆盖现有部署，请回答“部署”。
    - 如果系统提示“始终部署工作区”，请回答“是”。

1. 部署过程正在进行时，可以在 VS Code 的“输出”窗口中查看进度。 

    ![VS Code 的“输出”窗口中的部署进度](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. 如果部署在数分钟（具体取决于多少依赖项需要安装）后完成，则会显示以下消息。 选择“浏览网站”按钮，查看正在运行的站点。 

    ![部署完成，出现“浏览网站”按钮](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![在应用服务上成功运行的应用](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. 如果仍看到默认应用，请等待一或两分钟，以便容器在部署后重启，然后重试。 如果使用的是自定义启动命令且已验证其正确性，请继续执行步骤 6 以检查日志。

1. 若要验证文件是否已部署，请在“Azure:  应用服务”资源管理器中展开应用服务，然后展开“文件”： 

    ![通过应用服务资源管理器检查部署文件](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    应用服务构建系统使用文件 .deployment、antenv.tar.gz 和 oryx-manifest.toml（供你了解）  。 hostingstart.html 是默认的应用页面。

> [!div class="nextstepaction"]
> [我部署了我的应用 - 转到步骤 6 >>>](tutorial-deploy-app-service-on-linux-06.md)

[存在问题？请告诉我们。](https://aka.ms/FlaskVSCQuickstartHelp)
