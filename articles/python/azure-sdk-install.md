---
title: 如何安装用于 Python 的 Azure SDK 库包
description: 如何使用 pip 安装、卸载和验证 Azure SDK for Python 库。 包含有关安装特定版本和预览包的详细信息。
ms.date: 05/26/2020
ms.topic: conceptual
ms.openlocfilehash: f50de734ab1d007c9e5efac8cd6559a2c03d83f5
ms.sourcegitcommit: efab6be74671ea4300162e0b30aa8ac134d3b0a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2020
ms.locfileid: "84256252"
---
# <a name="how-to-install-azure-library-packages-for-python"></a>如何安装用于 Python 的 Azure 库包

用于 Python 的 Azure SDK 完全由[用于 Python 的 Azure SDK 索引页](https://azure.github.io/azure-sdk/releases/latest/all/python.html)上列出的多个单独库组成。 请使用 `pip install` 来安装项目所需的特定库包。

利用这些库，可在 Azure 服务上预配和管理资源（使用其名称中有 `-mgmt` 的管理库），并从应用代码连接到这些资源（使用客户端库）。

## <a name="install-the-latest-version-of-a-library"></a>安装最新版本的库

```cmd
pip install azure-storage-blob
```

```cmd
pip install azure-mgmt-storage
```

`pip install` 在当前 Python 环境中检索库的最新版本。

在 Linux 系统上，必须分别为每个用户单独安装库。 不支持使用 `sudo pip install` 为所有用户安装库。

## <a name="install-specific-library-versions"></a>安装特定的库版本

```cmd
pip install azure-storage-blob==12.0.0
```

```cmd
pip install azure-mgmt-storage==10.0.0
```

使用 `pip install` 在命令行上指定所需版本。

## <a name="install-preview-packages"></a>安装预览版包

```cmd
pip install --pre azure-storage-blob
```

```cmd
pip install --pre azure-mgmt-storage
```

若要安装最新的预览版库，请在命令行中包括 `--pre` 标志。

Microsoft 会定期发布预览版库包，这些库包支持即将推出的功能，但要注意：该库可能会发生更改，不能在生产项目中使用。

## <a name="verify-a-library-installation"></a>验证库安装

```cmd
pip show azure-storage-blob
```

```cmd
pip show azure-mgmt-storage
```

使用 `pip show <library>` 验证是否已安装库。 如果库已安装，则该命令会显示版本和其他摘要信息，否则该命令不会显示任何内容。

也可使用 `pip freeze` 或 `pip list` 来查看安装在当前 Python 环境中的所有库。

## <a name="uninstall-a-library"></a>卸载库

```cmd
pip uninstall azure-storage-blob
```

若要卸载库，请使用 `pip uninstall <library>`。
