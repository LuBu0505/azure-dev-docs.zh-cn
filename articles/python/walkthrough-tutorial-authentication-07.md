---
title: 演练，第 7 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 检查主应用 API 终结点的代码，该终结点使用第三方 API 终结点并将消息写入 Azure 队列存储。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: b6a54f51c53889ba95f86ba194232262f31c2d99
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764702"
---
# <a name="part-7-main-application-api-endpoint"></a>第 7 部分：主应用程序 API 终结点

[上一部分：主应用启动代码](walkthrough-tutorial-authentication-06.md)

应用的 /api/v1/getcode API 会生成包含字母数字代码和时间戳的 JSON 响应。

首先，`@app.route` 修饰器告知 Flask，`get_code` 函数会处理对 /api/v1/getcode URL 的请求。

```python
@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
```

接下来，我们调用第三方 API，该 API 的 URL 位于 `number_url`中，提供我们从标头中的密钥保管库内检索到的访问密钥。

```python
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code
```

标头中的 `x-functions-key` 属性专门说明了 Azure Functions（其中部署了此示例第三方 API）期望如何在标头中显示访问密钥。 有关详细信息，请参阅 [Azure Functions HTTP 触发器 - 授权密钥](/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#authorization-keys)。 如果调用 API 由于任何原因而失败，我们将返回一条错误消息和状态代码。

假设 API 调用成功并且返回一个数值，我们将使用该数字加上一些随机字符（使用我们自己的 `random_char` 函数）来构造一个更加复杂的代码。

```python
    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }
```

此处的 `code` 变量包含应用 API 的完整 JSON 响应，其中包括代码值和时间戳。 响应示例包括 `{"code":"ojE-161-pTv","timestamp":"2020-04-15 16:54:48.816549"}`。

但是，在返回该响应之前，我们将使用队列客户端的 [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) 方法，在存储队列中写入一条包含该响应的消息：

```python
    queue_client.send_message(code)

    return jsonify(code)
```

## <a name="processing-queue-messages"></a>处理队列消息

可以通过 [Azure 门户](/azure/storage/queues/storage-quickstart-queues-portal#view-message-properties)或 Azure CLI 命令 [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) 来查看和管理队列中存储的消息。 示例存储库包含一个脚本（test.cmd 和 test.sh），用于从应用终结点请求代码，然后检查消息队列 。 还存在一个用于使用 [`az storage message clear`](/cli/azure/storage/message#az-storage-message-clear) 命令来清除队列的脚本。

通常，像本示例这样的应用会有另一个进程，该进程以异步方式从队列中拉取消息进行进一步处理。 如前所述，此 API 终结点生成的响应可以通过双因素用户身份验证在应用中的其他地方使用。 在这种情况下，应用应在一段时间（比如 10 分钟）后使代码无效。 执行此任务的一种简单方法是维护有效双因素身份验证代码表，用户登录过程将使用这些代码。 然后，应用将具有一个简单的队列监视进程，其逻辑如下（在伪代码中）：

<pre>
pull a message from the queue and retrieve the code.

if (code is already in the table):
    remove the code from the table, thereby invalidating it
else:
    add the code to the table, making it valid
    call queue_client.send_message(code, visibility_timeout=600)
</pre>

此伪代码采用了 [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) 方法的可选 `visibility_timeout` 参数，该参数指定消息在队列中可见之前经历的秒数。 由于默认超时值为零，因此，最初由 API 终结点写入的消息会立即对队列监视进程可见。 因此，该进程会立即将它们存储在有效代码表中。 通过使用此超时值再次对同一消息进行排队，进程可知晓自己将在 10 分钟之后再次收到代码，并在届时从表中删除该代码。

## <a name="implementing-the-main-app-api-endpoint-in-azure-functions"></a>在 Azure Functions 中实现主应用 API 终结点

本文前面所示的代码使用 Flask Web 框架来创建其 API 终结点。 由于 Flask 需要使用 Web 服务器运行，因此必须将此类代码部署到 Azure 应用服务或虚拟机。

备用部署选项为 Azure Functions 的无服务器环境。 在这种情况下，所有启动代码和 API 终结点代码都将包含在绑定到 HTTP 触发器的同一函数中。 与应用服务一样，可以使用[函数应用程序设置](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings)为代码创建环境变量。

较为容易的一个实现是使用队列存储进行身份验证。 可以为函数创建队列存储绑定，而不是使用队列的 URL 和凭据对象获取 `QueueClient` 对象。 该绑定在后台处理所有身份验证。 借助此类绑定，函数可获得一个随时可用的客户端对象作为参数。 有关详细信息和示例代码，请参阅[将 Azure Functions 连接到 Azure 队列存储](/azure/azure-functions/functions-add-output-binding-storage-queue-cli?tabs=bash%2Cbrowser&pivots=programming-language-python)。

## <a name="next-steps"></a>后续步骤

通过此示例，你已了解应用如何使用其他 Azure 服务进行身份验证，以及应用如何使用 Azure Key Vault 来存储第三方 API 的任何其他必需机密。

这里演示的有关 Azure Key Vault 和 Azure 存储的相同模式适用于所有其他 Azure 服务。 关键步骤是在 Azure 门户上相应服务的页面中或通过 Azure CLI 为应用设置正确的角色权限。 （请参阅[如何分配角色权限](/azure/role-based-access-control/role-assignments-steps)）。 请务必查看服务文档，了解是否需要配置任何其他访问策略。

请时刻记住，需要将相同的角色和访问策略分配给用于本地开发的任何服务主体。

简而言之，完成本演练之后，你可以将你的知识应用到任意数量的其他 Azure 服务和任意数量的其他外部服务。

此处未涉及的一个主题是用户身份验证。 要探索 Web 应用的此领域，请先学习[在 Azure 应用服务中对用户进行端到端身份验证和授权](/azure/app-service/tutorial-auth-aad?pivots=platform-linux)。

## <a name="see-also"></a>另请参阅

- [如何在 Azure 上对 Python 应用进行身份验证和授权](azure-sdk-authenticate.md)
- 演练示例：[github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication)
- [Azure Active Directory 文档](/azure/active-directory)
- [Azure Key Vault 文档](/azure/key-vault)
