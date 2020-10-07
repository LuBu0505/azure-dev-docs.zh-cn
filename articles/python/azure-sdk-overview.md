---
title: 使用用于 Python 的 Azure 库 (SDK)
description: 概述了用于 Python 的 Azure 库的特性和功能，这些特性和功能可提高开发人员预配、使用和管理 Azure 资源时的工作效率。
ms.date: 09/19/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 81bda5dbe4c39341c2c799ecdbac10c2f0347bcc
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364430"
---
# <a name="use-the-azure-libraries-sdk-for-python"></a>使用用于 Python 的 Azure 库 (SDK)

用于 Python 的开放源代码 Azure 库简化了通过 Python 应用程序代码预配、管理和使用 Azure 资源的过程。

## <a name="the-details-you-really-want-to-know"></a>你真正想要了解的详细信息

- Azure 库是用于从本地或云中运行的 Python 代码与 Azure 服务进行通信的方式。 （是否可以在特定服务的作用域内运行 Python 代码取决于该服务当前是否支持 Python。）

- 这些库支持 Python 2.7 和 Python 3.5.3 或更高版本，还针对 PyPy 5.4 以上的版本进行了测试。

- 用于 Python 的 Azure SDK 完全由 180 多个与特定 Azure 服务相关的 Python 库组成。 该“SDK”中没有其他工具。

- 在本地运行代码时，Azure 身份验证依赖于环境变量，如[配置本地开发环境](configure-local-development-environment.md)中所述。 

- 利用 `pip install <library_name>`，并使用 [Python SDK 包索引](azure-sdk-library-package-index.md)上的库名称，安装所需的库包。 有关更多详细信息，请参阅[安装 Azure 库](azure-sdk-install.md)。

- 有不同的“管理”库和“客户端”库（有时称为“管理平面”库和“数据平面”库）。 每一组的库都有不同的用途，由不同类型的代码使用。 有关详细信息，请参阅本文后面的以下部分：
  - [使用管理库预配和管理 Azure 资源](#provision-and-manage-azure-resources-with-management-libraries)
  - [通过客户端库连接并使用 Azure 资源](#connect-to-and-use-azure-resources-with-client-libraries)

- 这些库的文档可在 [Azure for Python 参考](/python/api/overview/azure/?view=azure-python)（按 Azure 服务进行组织）中找到，也可在 [Python API 浏览器](/python/api/?view=azure-python)（按包名称进行组织）中找到。 目前，经常需要单击多个层，才能找到你想要了解的类和方法。 对此造成的不便，我们深表歉意。 我们正在努力改进！

- 若要亲自试用这些库，首先建议你[设置本地开发环境](configure-local-development-environment.md)。 然后，可以尝试以下任何独立的示例（按任意顺序）：[示例：预配资源组](azure-sdk-example-resource-group.md)，[示例：预配和使用 Azure 存储](azure-sdk-example-storage.md)，[示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)，[示例：预配和使用 MySQL 数据库](azure-sdk-example-database.md)，以及[示例：预配虚拟机](azure-sdk-example-virtual-machines.md)。

- 有关演示视频，请观看虚拟 PyCon 2020 中的<a href="https://www.youtube.com/watch?v=M1pVxItg2Mg&feature=youtu.be&ocid=AID3006292" target="_blank">使用 Azure SDK 与 Azure 资源进行交互</a> (youtube.com)。

### <a name="non-essential-but-still-interesting-details"></a>不重要但仍很有趣的详细信息

- 由于 Azure CLI 是使用管理库通过 Python 编写的，因此，使用 Azure CLI 命令可执行的任何操作也可以通过 Python 脚本来执行。 也就是说，CLI 命令提供了许多有用的功能，如同时执行多项任务，自动处理异步操作、格式化输出（如连接字符串）等。 因此，使用 CLI（或其等效的 Azure PowerShell）进行自动预配和管理脚本比编写等效的 Python 代码要方便得多，除非你希望对过程进行更为严格的控制。

- 用于 Python 的 Azure 库是基于底层 Azure REST API 构建的，使你可以通过熟悉的 Python 范式使用这些 API。 不过，在需要时，始终可以直接通过 Python 代码使用 REST API。

- 可在 [https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) 上找到这些 Azure 库的源代码。 作为一个开源项目，你的贡献会受到欢迎！

- 尽管可将这些库与我们未针对其进测试的解释器（例如 IronPython 和 Jython）配合使用，但可能会遇到孤立的问题和不兼容问题。

- 库 API 参考文档的源存储库位于 [https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/) 上。

- 我们目前正在更新用于 Python 的 Azure 库，以共享常见的云模式，如身份验证协议、日志记录、跟踪、传输协议、缓冲的响应和重试。

  - 该共享功能包含在 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 库中。

  - [Azure SDK for Python 最新版本](azure-sdk-library-package-index.md#libraries-using-azurecore)上列出了当前与 Core 库兼容的库。 这些库（主要是客户机库）有时称为“跟踪 2”。

  - 管理库和任何其他尚未更新的库有时称为“跟踪 1”。

- 若要详细了解我们应用于这些库的指导原则，请参阅 [Python Guidelines:Introduction](https://azure.github.io/azure-sdk/python_introduction.html)（Python 指南：简介）。

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>使用管理库预配和管理 Azure 资源

SDK 的管理（或“管理平面”）库（其名称都以 `azure-mgmt-` 开头）可帮助你通过 Python 脚本创建、预配和管理 Azure 资源。 所有 Azure 服务都有相应的管理库。

借助管理库，可以编写配置和部署脚本，以执行可通过 [Azure 门户](https://portal.azure.com)或 [Azure CLI](/cli/azure/install-azure-cli) 执行的相同任务。 （如前文所述，Azure CLI 是用 Python 编写的，并使用管理库来实现其各种命令。）

若要详细了解如何使用每个管理库，请参阅 *README.md* 或 *README.rst* 文件（位于 [SDK GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中）。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](/samples/browse/?languages=python&products=azure)中找到其他代码片段。

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>通过客户端库连接并使用 Azure 资源

SDK 的客户端（或“数据平面”）库可帮助你编写 Python 应用程序代码，以与已预配的服务进行交互。 只有那些支持客户端 API 的服务才存在客户端库。

若要详细了解如何使用每个客户端库，请参阅 *README.md* 或 *README.rst* 文件（位于 [SDK 的 GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中）。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](/samples/browse/?languages=python&products=azure)中找到其他代码片段。

## <a name="get-help-and-connect-with-the-sdk-team"></a>获取帮助并与 SDK 团队联系

- 访问[用于 Python 的 Azure 库文档](https://aka.ms/python-docs)
- 在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问
- 在 [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) 上提出针对此 SDK 的问题
- 在 Twitter 上提及 [@AzureSDK](https://twitter.com/AzureSdk/)

## <a name="next-step"></a>后续步骤

我们强烈建议执行本地开发环境的一次性设置，以便你可以轻松使用任何用于 Python 的 Azure 库。

> [!div class="nextstepaction"]
> [设置本地开发环境 >>>](configure-local-development-environment.md)
