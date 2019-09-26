---
title: 在 Visual Studio Code 中将更改后的容器重新部署到 Azure 应用服务
description: 教程步骤 3，用于重新生成和重新部署容器映像的简单步骤。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 30b0d0863900c36232b69c23db0eae1c70a34396
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019495"
---
# <a name="make-changes-and-redeploy"></a>进行更改并重新部署

[上一步：将映像部署到 Azure](tutorial-deploy-containers-02.md)

由于你不可避免地需要对应用进行更改，因此最终会重新生成并重新部署容器多次。 好在此过程很简单：

1. 在本地更改并测试应用。 （如需此步骤以及随后两个步骤的说明，请参阅教程：[在 VS Code 中创建 Python 容器](https://code.visualstudio.com/docs/python/tutorial-create-container)。）

1. 重新生成 Docker 映像。 如果只更改应用代码，生成过程应该只需要数秒钟。

1. 将映像推送到注册表。 同样，如果只更改应用代码，则只需推送那个很小的层，此过程通常在数秒内完成。

1. 在“Azure:  应用服务”资源管理器中，右键单击相应的应用服务，然后选择“重启”。  重启应用服务会自动从注册表拉取最新的容器映像。

1. 大约 15-20 秒后，再次访问应用服务 URL，查看更新。

> [!div class="nextstepaction"]
> [我进行了更改并已重新部署](tutorial-deploy-containers-04.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)