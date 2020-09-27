---
title: 使用 Azure SDK 库预配和部署 Web 应用
description: 使用用于 Python 的 Azure SDK 库中的管理库来预配 Web 应用，然后从 GitHub 存储库部署应用代码。
ms.date: 05/29/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 03a2f8b8f8830916243db0778d16650da1892b04
ms.sourcegitcommit: b03cb337db8a35e6e62b063c347891e44a8a5a13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91110459"
---
# <a name="example-use-the-azure-libraries-to-provision-and-deploy-a-web-app"></a>示例：使用 Azure 库预配和部署 Web 应用

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库，以在 Azure 应用服务上预配 Web 应用，以及从 GitHub 存储库部署应用代码。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。）

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-needed-azure-library-packages"></a>2:安装所需的 Azure 库包

创建一个具有以下内容的名为 *requirements.txt* 的文件：

```text
azure-mgmt-resource
azure-mgmt-web
azure-cli-core
```

在激活了虚拟环境的终端或命令提示符下，安装下列要求：

```cmd
pip install -r requirements.txt
```

## <a name="3-fork-the-sample-repository"></a>3：创建示例存储库分支

访问 [https://github.com/Azure-Samples/python-docs-hello-world](https://github.com/Azure-Samples/python-docs-hello-world)，并创建存储库分支到自己的 GitHub 帐户。 你使用分支确保你有权将存储库部署到 Azure。

![在 GitHub 上创建示例存储库分支](media/azure-sdk-example-web-app/fork-github-repository.png)

然后创建一个名为 `REPO_URL` 且带有分支 URL 的环境变量。 下一部分中的代码示例取决于此环境变量：

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set REPO_URL=<url_of_your_fork>
```

# <a name="bash"></a>[bash](#tab/bash)

```bash
REPO_URL=<url_of_your_fork>
```

---

## <a name="4-write-code-to-provision-and-deploy-a-web-app"></a>4：将代码写入预配并部署 Web 应用

创建包含以下代码的名为 provision_deploy_webapp.py 的 Python 文件。 注释对详细信息进行了说明：

```python
import random, os
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.web import WebSiteManagementClient

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = 'PythonAzureExample-WebApp-rg'
LOCATION = "centralus"

# Step 1: Provision the resource group.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


#Step 2: Provision the App Service plan, which defines the underlying VM for the web app.

# Names for the App Service plan and App Service. We use a random number with the
# latter to create a reasonably unique name. If you've already provisioned a
# web app and need to re-run the script, set the WEB_APP_NAME environment 
# variable to that name instead.
SERVICE_PLAN_NAME = 'PythonAzureExample-WebApp-plan'
WEB_APP_NAME = os.environ.get("WEB_APP_NAME", f"PythonAzureExample-WebApp-{random.randint(1,100000):05}")

# Obtain the client object
app_service_client = get_client_from_cli_profile(WebSiteManagementClient)

# Provision the plan; Linux is the default
poller = app_service_client.app_service_plans.create_or_update(RESOURCE_GROUP_NAME,
    SERVICE_PLAN_NAME,
    {
        "location": LOCATION,
        "reserved": True,
        "sku" : {"name" : "B1"}
    }
)

plan_result = poller.result()

print(f"Provisioned App Service plan {plan_result.name}")


# Step 3: With the plan in place, provision the web app itself, which is the process that can host
# whatever code we want to deploy to it.

poller = app_service_client.web_apps.create_or_update(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": LOCATION,
        "server_farm_id": plan_result.id,
        "site_config": {
            "linux_fx_version": "python|3.8"
        }
    }
)

web_app_result = poller.result()

print(f"Provisioned web app {web_app_result.name} at {web_app_result.default_host_name}")

# Step 4: deploy code from a GitHub repository. For Python code, App Service on Linux runs
# the code inside a container that makes certain assumptions about the structure of the code.
# For more information, see How to configure Python apps,
# https://docs.microsoft.com/azure/app-service/configure-language-python.
#
# The create_or_update_source_control method doesn't provision a web app. It only sets the
# source control configuration for the app. In this case we're simply pointing to
# a GitHub repository.
#
# You can call this method again to change the repo.

REPO_URL = 'https://github.com/<your_fork>/python-docs-hello-world'

poller = app_service_client.web_apps.create_or_update_source_control(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": "GitHub",
        "repo_url": REPO_URL,
        "branch": "master"
    }
)

sc_result = poller.result()

print(f"Set source control on web app to {sc_result.branch} branch of {sc_result.repo_url}")
```

此代码使用基于 CLI 的身份验证方法 (`get_client_from_cli_profile`)，因为它演示了你可能会使用 Azure CLI 直接执行的操作。 在这两种情况下，使用相同的身份验证标识。

若要在生产脚本中使用此类代码，应改为使用 `DefaultAzureCredential`（推荐）或基于服务主体的方法，如[如何使用 Azure 服务对 Python 应用进行身份验证](azure-sdk-authenticate.md)中所述。

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)
- [WebSiteManagementClient (azure.mgmt.web import)](/python/api/azure-mgmt-web/azure.mgmt.web.websitemanagementclient?view=azure-python)

## <a name="5-run-the-script"></a>5：运行脚本

```cmd
python provision_deploy_web_app.py
```

## <a name="6-verify-the-web-app-deployment"></a>6：验证 Web 应用部署

1. 通过运行以下命令访问部署的网站：

    ```azurecli
    az webapp browse -n PythonAzureExample-WebApp-12345
    ```

    将“PythonAzureExample-WebApp-12345”替换为 Web 应用的具体名称。

    应会显示“Hello World!” 显示在浏览器中。

1. 访问 [Azure 门户](https://portal.azure.com)，选择“资源组”，并检查是否列出了“PythonAzureExample-WebApp-rg”。 然后导航到该列表以验证预期资源是否存在，即应用服务计划和应用服务。

## <a name="7-clean-up-resources"></a>7:清理资源

```azurecli
az group delete -n PythonAzureExample-WebApp-rg --no-wait
```

如果不需要保留预配在此示例中的资源，并想要避免订阅中的持续费用，则运行此命令。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤：

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 ^
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

rem You can use --deployment-source-url with the first create command. It's shown here
rem to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan ^
    --deployment-source-url https://github.com/<your_fork>/python-docs-hello-world

rem Replace <your_fork> with the specific URL of your forked repository.
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 \
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

# You can use --deployment-source-url with the first create command. It's shown here
# to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan \
    --deployment-source-url https://github.com/<your_fork>/python-docs-hello-world

# Replace <your_fork> with the specific URL of your forked repository.
```

---

## <a name="see-also"></a>另请参阅

- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配和使用 MySQL 数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
