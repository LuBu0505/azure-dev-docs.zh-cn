---
title: 安装 Azure 经典 CLI
description: 安装适用于 Mac、Linux 和 Windows 的 Azure 经典 CLI 即可使用 Azure 服务
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 0cc1d7811223bf6f473c2c4516d0919306aa74c7
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030759"
---
# <a name="install-the-azure-classic-cli"></a>安装 Azure 经典 CLI

> [!IMPORTANT]
> 本主题介绍如何安装 Azure 经典 CLI。 此经典 CLI 已弃用，只能与经典部署模型配合使用。
> 进行所有其他的部署时，请使用 [Azure CLI](/cli/azure)。

快速安装 Azure 经典 CLI，以便使用一组基于 shell 的开源命令在 Microsoft Azure 中创建和管理资源。 可以通过多个选项在计算机上安装这些跨平台工具：

* **npm 包** - 运行 npm（JavaScript 的包管理器），在 Linux 发行版或 OS 中安装 Azure 经典 CLI 包。 需要 node.js 和 npm。
* **安装程序** - 下载安装程序，在 macOS 或 Windows 上轻松地进行安装。
* **Docker 容器** - 开始在做好运行准备的 Docker 容器中使用经典 CLI。 需要 Docker 主机。

有关更多选项和背景信息，请参阅 [GitHub](https://github.com/azure/azure-xplat-cli) 上的项目存储库。

安装 Azure 经典 CLI 之后，请使用 `azure login` 进行连接，然后从命令行界面（Bash、终端、命令提示符等）运行 `azure` 命令，以便使用 Azure 资源。

## <a name="option-1-install-an-npm-package"></a>选项 1：安装 npm 包

若要通过 npm 包安装经典 CLI，请确保已下载并安装[最新 Node.js 和 npm](https://nodejs.org/en/download/package-manager/)。 然后，运行 `npm install` 以安装 azure-cli 包：

```bash
npm install -g azure-cli
```

在 Linux 发行版中，可能需要使用 `sudo` 才能成功运行 `npm` 命令，如下所示：

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> 如果需要在 OS 中安装或更新 Node.js 和 npm，建议安装 Node.js LTS 4.x 或更高版本。 如果使用旧版本，可能会遇到安装错误。

如果你愿意，也可从 [GitHub 发行版](https://github.com/Azure/azure-xplat-cli/releases)下载 tar 文件。 然后，安装所下载的 npm 包，如下所示（在 Linux 发行版中可能需要使用 `sudo`）：

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>选项 2：使用安装程序

如果使用 Mac 或 Windows 计算机，可以从 [GitHub 发行版](https://github.com/Azure/azure-xplat-cli/releases)获取 DMG 和 MSI 安装程序。

> [!TIP]
> 在 Windows 上，还可以下载 [Web 平台安装程序](https://go.microsoft.com/?linkid=9828653)来安装经典 CLI。 此安装程序允许安装附加的 Azure SDK 和命令行工具。

## <a name="option-3-use-a-docker-container"></a>选项 3：使用 Docker 容器

如果已将计算机设置为 [Docker](https://docs.docker.com/engine/understanding-docker/) 主机，可以在 Docker 容器中运行 Azure 经典 CLI。 运行以下命令（在 Linux 发行版中，可能需要使用 `sudo`）：

```bash
docker run -it microsoft/azure-cli:0.10.17
```

## <a name="run-azure-classic-cli-commands"></a>运行 Azure 经典 CLI 命令

安装经典 CLI 之后，请从命令行用户界面（Bash、终端、命令提示符等）运行 `azure` 命令。 例如，若要运行帮助命令，请键入以下命令：

```azurecli-interactive
azure help
```

> [!NOTE]
> 在某些 Linux 分发版中，可能会收到类似于“`/usr/bin/env: ‘node’: No such file or directory`”的错误。 此错误消息来自最近安装在 /usr/bin/nodejs 中的 Node.js。 若要解决此错误，请运行以下命令创建 /usr/bin/node 的符号链接：

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

若要查看安装的 Azure 经典 CLI 版本，请键入以下命令：

```azurecli-interactive
azure --version
```

> [!NOTE]
> 首次使用 Azure 经典 CLI 时，会看到一条消息，询问是否允许 Microsoft 收集使用情况信息。 参与为自愿性质。 如果选择参与，通过运行 `azure telemetry --disable` 即可随时停止参与。 若要随时启用参与，请运行 `azure telemetry --enable`。

## <a name="update-the-classic-cli"></a>更新经典 CLI

Microsoft 可能会发布 Azure 经典 CLI 的更新版本。 使用适用于操作系统的安装程序来重新安装经典 CLI，或运行最新的 Docker 容器。 如果已安装最新的 Node.js 和 npm，请键入以下命令（在 Linux 发行版中可能需要使用 `sudo`）进行更新。

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>启用 tab 自动补全

Mac 和 Linux 支持 tab 自动补全经典 CLI 命令。

如要在 zsh 中启用，运行：

```bash
echo '. <(azure --completion)' >> .zshrc
```

若要在 bash 中启用，运行：

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```

## <a name="next-steps"></a>后续步骤

* 若要了解有关 Azure 经典CLI、下载源代码、报告问题或贡献项目的详细信息，请访问[适用于 Azure 经典CLI 的 GitHub 存储库](https://github.com/azure/azure-xplat-cli)。
