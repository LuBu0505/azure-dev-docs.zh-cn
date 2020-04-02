---
title: 安装 Azure SDK for Python
description: 如何使用 pip 或 GitHub 安装 Azure SDK for Python。 Azure SDK 可以作为单个库安装，也可以作为完整包安装。
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: bf74567e5018c7d48141a6992cafc91e90045a25
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441762"
---
# <a name="install-the-azure-sdk-for-python"></a>安装 Azure SDK for Python

Azure SDK for Python 提供一个 API，你可以在 Python 代码中通过该 API 与 Azure 交互。 根据需要从 SDK 安装各个库。

Azure SDK for Python 经过测试，可以在 CPython versions 2.7 和 3.5.3+ 以及 PyPy 5.4+ 中使用。 开发人员也可将此 SDK 与其他解释器（例如 IronPython 和 Jython）配合使用，但可能会遇到孤立的问题和不兼容问题。 如果需要 Python 解释器，请从 [python.org/downloads](https://www.python.org/downloads) 安装最新版本。

## <a name="install-sdk-libraries-using-pip"></a>使用 pip 安装 SDK 库

Azure SDK for Python 包含许多单独的库，每个这样的库都可以预配或兼容特定的 Azure 服务。 可以使用 `pip install <library>`安装每个库。 有关每个库的特定说明和文档，请参阅 [SDK 版本页](https://azure.github.io/azure-sdk/releases/latest/python.html)。

例如，如果使用 Azure 存储，则可安装 `azure-storage-file`、`azure-storage-blob` 或 `azure-storage-queue` 库。 如果使用 Azure Cosmos DB 表，则请安装 `azure-cosmosdb-table`。 Azure Functions 可以与 `azure-functions` 库配合使用，依此类推。 以 `azure-mgmt-` 开头的那些库提供的 API 可以用来预配 Azure 资源。

### <a name="install-specific-library-versions"></a>安装特定的库版本

如需安装特定的库版本，请在命令行中指定该版本：

```bash
pip install azure-storage-blob==12.0.0
```

> [!NOTE]
> 在 Linux 系统上，SDK 不支持使用 `sudo pip install` 为所有用户安装库。 每个用户都必须单独使用 `pip install`。 

### <a name="install-preview-packages"></a>安装预览版包

Microsoft 定期发布支持即将推出的功能的预览版 SDK 库。 若要安装最新的预览版库，请在命令行中包括 `--pre` 标志。 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>使用 pip 验证 SDK 安装详细信息

使用 `pip show <library>` 命令验证是否已安装某个库。 如果库已安装，则该命令会显示版本和其他摘要信息。 如果库未安装，则该命令不显示任何内容。

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

也可使用 `pip freeze` 或 `pip list` 来查看安装在当前 Python 环境中的所有库。

## <a name="uninstall-azure-sdk-for-python-libraries"></a>卸载 Azure SDK for Python 库

若要卸载单个库，请使用 `pip uninstall <library>`。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用此 SDK](azure-sdk-get-started.yml)
