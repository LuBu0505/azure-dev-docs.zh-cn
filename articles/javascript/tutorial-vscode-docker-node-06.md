---
title: 在 Visual Studio Code 中将更改后的容器重新部署到 Azure 应用服务
description: Docker 教程步骤 6：用于重新生成和重新部署容器映像的简单步骤。
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: 29f07936d69d971c9ea139b019ac4ee272871342
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365100"
---
# <a name="make-changes-and-redeploy-a-container-using-visual-studio-code"></a>使用 Visual Studio Code 进行更改并重新部署容器

[上一步：部署应用映像](tutorial-vscode-docker-node-05.md)

由于你不可避免地需要对应用进行更改，因此最终会重新生成并重新部署容器多次。 好在此过程很简单：

1. 在本地更改并测试应用。

1. 在 Visual Studio Code 中打开命令面板  (**F1**)，然后运行 **Docker Images:Build Image** 以重建映像。 如果只更改应用代码，生成过程应该只需要数秒钟。

1. 若要将映像推送到注册表，请再次打开命令面板  (**F1**)，然后运行 **Docker Images:Push**，选择刚生成的映像。 与以前一样，由于对应用代码的更改很小，因此只需推送那一层，此过程通常在数秒内完成。

1. 在“Azure：函数”  应用服务”资源管理器中，右键单击相应的应用服务，然后选择“重启”。  重启应用服务会自动从注册表拉取最新的容器映像。

1. 大约 15-20 秒后，再次访问应用服务 URL，查看更新。

> [!div class="nextstepaction"]
> [我看到了更改](tutorial-vscode-docker-node-07.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=deploy-changes)
