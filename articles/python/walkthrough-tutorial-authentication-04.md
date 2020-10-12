---
title: 演练，第 4 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 概述主应用的实现，包括其所有代码。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e2a43f7e204ba3f077beea7cc878076111f71313
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764739"
---
# <a name="part-4-example-main-application-implementation"></a>第 4 部分：主应用程序实现示例

[上一部分：第三方 API 实现示例](walkthrough-tutorial-authentication-03.md)

我们方案中的主应用是部署到 Azure 应用服务的简单 Flask 应用。 该应用提供名为 /api/v1/getcode 的公共 API 终结点，该终结点在应用中生成用于某种其他目的的代码（例如，为人类用户提供双因素身份验证）。

要查看终结点的运行情况，请在浏览器中访问 [https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode](https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode) 或使用 curl 发出请求。

主应用还提供了一个简单的主页，其中显示了指向 API 终结点的链接。 可以在 [https://msdocs-main-app-example.azurewebsites.net](https://msdocs-main-app-example.azurewebsites.net) 上查看应用的这一部分。

示例的预配脚本将执行以下步骤：

1. 创建应用服务主机，并通过 Azure CLI 命令 [`az webapp up`](/cli/azure/webapp#az-webapp-up) 来部署代码。

1. 为主应用预配 Azure 存储帐户（使用 [`az storage account create`](/cli/azure/storage/account#az-storage-account-create)）。

1. 在存储帐户中创建一个名为“code-requests”的队列（使用 [`az storage queue create`](/cli/azure/storage/queue#az-storage-queue-create)）。

1. 为了确保允许将应用写入队列，请使用 [`az role assignment create`](/cli/azure/role/assignment#az-role-assignment-create) 向该应用分配“存储队列数据参与者”角色。 有关角色的详细信息，请参阅[如何使用 Azure CLI 分配角色权限](/azure/role-based-access-control/role-assignments-cli)。

主应用代码如下所示；本系列的后面部分提供了重要详细信息的说明。

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential
from azure.storage.queue import QueueClient

app = Flask(__name__)

# Retrieve the URL of the third-party API endpoint we invoke.
number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]

# Authenticate with Azure. First, obtain the credential object. To run locally,
# you must have the necessary environment variables set up to provide the
# local service principal information. When deployed to the cloud, the app must
# have managed identity enabled in Azure App Service.
credential = DefaultAzureCredential()

# Next, get a client object for the Key Vault. The client object is strictly
# a client-side construct provided by the Azure libraries as a layer on top
# of the Azure REST API.
key_vault_url = os.environ["KEY_VAULT_URL"]
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)

# Obtain the secret: for this step to work you must add the app's identity to
# the key vault's access policies for secret management. When deployed to the cloud
# the identity is the name of the app in App Service; when running locally, the
# identity is the local service principal.
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The access key
# we want for the third-party API is in the secret's value property.
access_key = vault_secret.value

#Set up the Storage queue client to which we write messages
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)


@app.route('/', methods=['GET'])
def home():
    return f'Home page of the main app. Make a request to <a href="./api/v1/getcode">/api/v1/getcode</a>.'


def random_char(num):
       return ''.join(random.choice(string.ascii_letters) for x in range(num))


@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code

    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }

    # Log a queue message with the code for, say, a process that invalidates
    # the code after a certain period of time.
    queue_client.send_message(code)

    return jsonify(code)


if __name__ == '__main__':
    app.run()
```

> [!div class="nextstepaction"]
> [第 5 部分 - 主应用依赖项、导入和环境变量 >>>](walkthrough-tutorial-authentication-05.md)
