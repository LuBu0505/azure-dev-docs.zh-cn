---
title: 演练，第 5 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 讨论主应用的依赖项（主要是 Azure SDK 库）、必要的导入语句以及主应用期望设置的环境变量。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 9c6204afd17d86cd8677022a59641e5343c6a543
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764730"
---
# <a name="part-5-main-app-dependencies-import-statements-and-environment-variables"></a>第 5 部分：主应用依赖项、导入语句和环境变量

[上一部分：主应用实现示例](walkthrough-tutorial-authentication-04.md)

本部分检查引入主应用中的 Python 库和代码所需的环境变量。 在部署到 Azure 时，可以使用 Azure 应用服务中的应用程序设置来提供环境变量。

## <a name="dependencies-and-import-statements"></a>依赖项和导入语句

应用代码需要多个库：Flask、标准 HTTP 请求库以及 Active Directory ([azure.identity](/python/api/overview/azure/identity-readme))、Key Vault ([azure.keyvault.secrets](/python/api/overview/azure/keyvault-secrets-readme)) 和队列存储 ([azure.storage.queue](/python/api/overview/azure/storage-queue-readme)) 的 Azure 库。 这些库包含在应用的 requirements.txt 文件中：

```txt
flask
requests
azure.identity
azure.keyvault.secrets
azure.storage.queue
```

在将应用部署到 Azure 应用服务时，Azure 会自动在主机服务器上安装这些要求。 当在本地运行时，可以通过 `pip install -r requirements.txt` 在环境中安装它们。

代码的顶部是从库中使用的部分所需的导入语句：

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient
from azure.storage.queue import QueueClient
```

## <a name="environment-variables"></a>环境变量

应用代码依赖于四个环境变量：

| 变量 | 值 |
| --- | --- |
| THIRD_PARTY_API_ENDPOINT | 第三方 API 的 URL，如[第 3 部分](walkthrough-tutorial-authentication-03.md)中所述的 `https://msdocs-api-example.azurewebsites.net/api/RandomNumber`。 |
| KEY_VAULT_URL | 存储第三方 API 访问密钥的 Azure Key Vault 的 URL。 |
| THIRD_PARTY_API_SECRET_NAME | 包含第三方 API 访问密钥的 Key Vault 中机密的名称。 |
| STORAGE_QUEUE_URL | Azure 中已配置的 Azure 存储队列的 URL，例如 `https://msdocsmainappexample.queue.core.windows.net/code-requests`（请参阅[第 4 部分](walkthrough-tutorial-authentication-04.md)）。 由于队列名称包含在 URL 的末尾，因此在代码中的任何位置都看不到该名称。 |

当在本地运行时，可以在所使用的任何命令 shell 中创建这些变量。 如果将应用部署到虚拟机，则会创建类似的服务器端变量。

但是，在部署到 Azure 应用服务时，你将无法访问服务器本身。 在这种情况下，可以创建具有相同名称的应用程序设置，这些设置随后作为环境变量出现在应用中。 

预配脚本使用 Azure CLI 命令 [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) 来创建这些设置。 所有这四个变量均使用单个命令进行设置。

要通过 Azure 门户创建设置，请参阅[在 Azure 门户中配置应用服务应用](/azure/app-service/configure-common)。

> [!div class="nextstepaction"]
> [第 6 部分 - 主应用启动代码 >>>](walkthrough-tutorial-authentication-06.md)
