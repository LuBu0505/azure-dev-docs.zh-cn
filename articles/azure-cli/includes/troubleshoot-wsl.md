---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 11/26/2018
ms.topic: include
ms.openlocfilehash: ee0b2b0c8c557aa54255f28429bb3a174c0e21a9
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030889"
---
### <a name="cli-fails-to-install-or-run-on-windows-subsystem-for-linux"></a>CLI 未能在适用于 Linux 的 Windows 子系统上安装或运行

由于[适用于 Linux 的 Windows 子系统 (WSL)](/windows/wsl/about) 是基于 Windows 平台的一个系统调用转换层，因此，在尝试安装或运行 Azure CLI 时可能会发生错误。 CLI 依赖于在 WSL 中可能具有 bug 的某些功能。 如果无论你以何方式安装 CLI 都会发生错误，则很可能是因为 WSL 有问题而不是 CLI 安装过程有问题。

若要对 WSL 安装进行故障排除并尽可能解决问题，请执行以下操作：

* 如果可以，在 Linux 计算机或 VM 上运行相同的安装过程来看看它是否会成功。 如果成功，则几乎可以肯定问题与 WSL 有关。 若要启动 Azure 中的 Linux VM，请参阅[在 Azure 门户中创建 Linux VM](/azure/virtual-machines/linux/quick-create-portal) 文档。
* 确保运行的是最新版本的 WSL。 若要获取最新版本，请[更新 Windows 10 安装](https://support.microsoft.com/help/4027667/windows-10-update)。
* 检查 WSL 是否存在可能会解决你的问题的[待解决问题](https://github.com/Microsoft/WSL/issues)。
  那里通常会提供有关如何解决该问题的建议，或者提供有关将修复该问题的发行版的信息。
* 如果没有与你的问题对应的现有问题，请[提交一个新的 WSL 问题](https://github.com/Microsoft/WSL/issues/new)并确保提供尽可能多的信息。

如果在 WSL 上安装或运行时继续出现问题，请考虑[安装适用于 Windows 的 CLI](../install-azure-cli-windows.md)。
