---
title: 安装 Azure SDK for Python 库
description: 如何使用 pip 安装、卸载和验证 Azure SDK for Python 库。 包含有关安装特定版本和预览包的详细信息。
ms.date: 05/13/2020
ms.topic: conceptual
ms.openlocfilehash: 302d17211480f9c8793d7be1de20a6ab5dec3c95
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83403665"
---
# <a name="install-azure-sdk-for-python-libraries"></a>安装 Azure SDK for Python 库

Azure SDK for Python 提供一个 API，你可以在 Python 代码中通过该 API 与 Azure 交互。 所有当前库的名称位于 [Azure SDK for Python 索引页](https://azure.github.io/azure-sdk/releases/latest/all/python.html)上。

名称以 `azure-mgmt` 开头的库是管理库，你可以使用这些库来预配和管理 Azure 资源，就像通过 [Azure 门户](https://portal.azure.com)或使用 [Azure CLI](/cli/azure/install-azure-cli) 来预配和管理 Azure 资源一样。 例如，若要预配和管理 Azure 存储资源，请使用 `azure-mgmt-storage` 库。

该 SDK 中的所有其他库都是从应用程序代码中使用的客户端库，用于使用已经预配的资源。 例如，若要从应用程序代码中使用 Azure 存储 Blob，请使用 `azure-storage-blob` 库。

## <a name="install-the-latest-version-of-a-library"></a>安装最新版本的库

```bash
pip install azure-storage-blob
```

`pip install` 在当前 Python 环境中安装最新版库。

在 Linux 系统上，SDK 不支持使用 `sudo pip install` 为所有用户安装库。 每个用户都必须单独使用 `pip install`。

## <a name="install-specific-library-versions"></a>安装特定的库版本

```bash
pip install azure-storage-blob==12.0.0
```

在命令行上使用 `pip install` 指定版本。

## <a name="install-preview-packages"></a>安装预览版包

```bash
pip install --pre azure-storage-blob
```

若要安装最新的预览版库，请在命令行中包括 `--pre` 标志。

Microsoft 定期发布预览版 SDK 库，用于支持即将推出的功能，但需要注意的是，该库可能会发生更改，不能在生产项目中使用。

## <a name="verify-a-library-installation"></a>验证库安装

```bash
pip show azure-storage-blob
```

使用 `pip show <library>` 验证是否已安装库。 如果库已安装，则该命令会显示版本和其他摘要信息，否则该命令不会显示任何内容。

也可使用 `pip freeze` 或 `pip list` 来查看安装在当前 Python 环境中的所有库。

## <a name="uninstall-a-library"></a>卸载库

```bash
pip uninstall azure-storage-blob
```

若要卸载库，请使用 `pip uninstall <library>`。

## <a name="next-steps"></a>后续步骤

现在，你已经完全准备好编写和运行一些代码了。可以使用下面的任何示例来实现这一点：

> [!div class="nextstepaction"]
> [示例：创建资源组 >>>](azure-sdk-example-resource-group.md)
