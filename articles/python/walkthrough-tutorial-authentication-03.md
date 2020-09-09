---
title: 演练，第 3 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 检查使用 Azure Functions 实现第三方 API 的示例，以及如何使用访问密钥来保护终结点。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 84078a455843cb28f80a633bb5344bc5ab645ac7
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379470"
---
# <a name="part-3-example-third-party-api-implementation"></a>第 3 部分：第三方 API 实现示例

[上一部分：身份验证的挑战](walkthrough-tutorial-authentication-02.md)

在我们的示例方案中，主应用的公共终结点使用受访问密钥保护的第三方 API。 本部分演示如何使用 Azure Functions 实现第三方 API，但此 API 可通过其他方式实现，并且可部署到不同的云服务器或 Web 主机。 唯一重要的方面是受特定访问密钥保护的终结点，该密钥必须包含在任何客户端请求中。 任何调用此 API 的应用都必须安全管理该密钥。

出于演示目的，此 API 已部署到终结点 `https://msdocs-api-example.azurewebsites.net/api/RandomNumber`。 但是，要调用此 API，必须在 `?code=` URL 参数或 HTTP 标头的 `'x-functions-key'` 属性中提供访问密钥 `d0c5atM1cr0s0ft`。 例如，在浏览器或 curl 中尝试此 URL：[https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft](https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft)。

如果访问密钥有效，则终结点将返回一个 JSON 响应，其中包含值为介于 1 和 999 之间的某个数字的单个属性“value”，例如 `{"value": 959}`。

终结点通过 Python 实现并部署到 Azure Functions。 代码如下所示:

```python
import logging
import random
import json

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('RandomNumber invoked via HTTP trigger.')

    random_value = random.randint(1, 1000)
    dict = { "value" : random_value }
    return func.HttpResponse(json.dumps(dict))
```

在示例存储库中，此代码位于 third_party_api/RandomNumber/\_\_init\_\_.py 下。 文件夹 RandomNumber 提供函数的名称，并且 \_\_init\_\_.py 包含该代码 。 该文件夹中的另一个文件 function.json 描述了触发函数的时间。 third_party_api 父文件夹中的其他文件提供了托管函数本身的 Azure 函数“app”的详细信息。

为了部署代码，示例的预配脚本将执行以下步骤：

1. 使用 Azure CLI 命令 [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) 为 Azure Functions 创建后备存储帐户。

1. 使用 Azure CLI 命令 [`az function app create`](/cli/azure/functionapp?view=azure-cli-latest#az-functionapp-create) 创建 Azure Functions“app”。

1. 等待 60 秒以便完全预配主机，然后使用 [Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash) 命令 [`func azure functionapp publish`](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash#project-file-deployment) 部署代码

1. 将访问密钥 `d0c5atM1cr0s0ft` 分配给函数。 （请参阅[保护 Azure Functions](/azure/azure-functions/security-concepts)，了解函数密钥的背景。）

    在预配脚本中，此步骤通过对 [Functions 密钥管理 API](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) 的 REST API 调用来完成，因为 Azure CLI 目前尚不支持此特定功能。 要调用该 REST API，预配脚本必须首先使用另一个 REST API 调用来检索函数应用的主密钥。

    还可以通过 [Azure 门户](https://portal.azure.com)分配访问密钥。 在 Functions 应用页上，选择“Functions”，然后选择要保护的特定函数（在本示例中名为 `RandomNumber`）。 在函数页上，选择“函数密钥”打开可在其中创建和管理这些密钥的页面。

> [!div class="nextstepaction"]
> [第 4 部分 - 主应用实现示例 >>>](walkthrough-tutorial-authentication-04.md)
