---
title: 步骤 3：在 Visual Studio Code 中将更改后的容器重新部署到 Azure 应用服务
description: 教程步骤 3，用于重新生成和重新部署容器映像的简单步骤。
ms.topic: conceptual
ms.date: 12/09/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: f760720755a170e6ff47c5971384f3f83b2b07be
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522093"
---
# <a name="2-redeploy-a-container-to-azure-app-service-after-making-changes"></a>2:在更改后将容器重新部署到 Azure 应用服务

[上一步：将映像部署到 Azure](tutorial-deploy-containers-02.md)

本文说明如何在 Visual Studio Code 中进行更改后将容器重新部署到 Azure 应用服务。

由于你不可避免地需要对应用进行更改，因此最终会重新生成并重新部署容器多次。 好在此过程很简单：

1. 在本地更改并测试应用。 （如需此步骤以及随后两个步骤的说明，请参阅教程：[在 VS Code 中创建 Python 容器](https://code.visualstudio.com/docs/python/tutorial-create-containers)。）

1. 重新生成 Docker 映像。 如果只更改应用代码，生成过程应该只需要数秒钟。

1. 将映像推送到注册表。 同样，如果只更改应用代码，则只需推送那个很小的层，此过程通常在数秒内完成。

1. 在“Azure：函数”  应用服务”资源管理器中，右键单击相应的应用服务，然后选择“重启”。  重启应用服务会自动从注册表拉取最新的容器映像。

1. 大约 15-20 秒后，再次访问应用服务 URL，查看更新。

> [!div class="nextstepaction"]
> [我进行了更改并已重新部署 - 转到步骤 4 >>>](tutorial-deploy-containers-04.md)
