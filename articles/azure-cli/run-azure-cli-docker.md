---
title: 在 Docker 容器中运行 Azure CLI
description: 如何运行托管 Azure CLI 的 Docker 容器
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/29/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 96a2bcf311cb504b08d44da3ea6033dbad9034b8
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031289"
---
# <a name="run-azure-cli-in-a-docker-container"></a>在 Docker 容器中运行 Azure CLI

可以使用 Docker 运行已预装 Azure CLI 的独立 Linux 容器。 Docker 可让你快速开始创建一个用于运行 CLI 的隔离环境。 映像也可以用作你自己的部署的基础。

## <a name="run-in-a-docker-container"></a>在 Docker 容器中运行

> [!NOTE]
> Azure CLI 已迁移到 [Microsoft 容器注册表](https://azure.microsoft.com/services/container-registry)。 Docker 中心上的现有标记仍然受支持，但新版本将仅作为 mcr.microsoft.com/azure-cli 提供。

请使用 `docker run` 安装 CLI。

   ```bash
   docker run -it mcr.microsoft.com/azure-cli
   ```

> [!NOTE]
> 若要从用户环境中选取 SSH 密钥，请使用 `-v ${HOME}/.ssh:/root/.ssh` 在环境中装载 SSH 密钥。
>
> ```bash
> docker run -it -v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli
> ```

CLI 作为 `/usr/local/bin` 中的 `az` 命令安装在映像中。 若要登录，请运行 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="update-docker-image"></a>更新 Docker 映像

使用 Docker 进行更新需要拉取新映像和重新创建任何现有的容器。 因此，应先行尝试，避免将托管 CLI 的容器用作数据存储。

使用 `docker pull` 更新本地映像。

```bash
docker pull mcr.microsoft.com/azure-cli
```

## <a name="uninstall-docker-image"></a>卸载 Docker 映像

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

停止运行 CLI 映像的任何容器后，请删除该映像。

```bash
docker rmi mcr.microsoft.com/azure-cli
```

## <a name="next-steps"></a>后续步骤

现在你已准备好使用 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
