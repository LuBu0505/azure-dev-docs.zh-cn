---
title: 连接 GitHub 和 Azure
description: 有关从 Azure 和其他服务连接到 GitHub 的资源
author: N-Usha
ms.author: ushan
ms.topic: reference
ms.service: azure
ms.date: 08/31/2020
ms.custom: github-actions-azure
ms.openlocfilehash: d7c791aec426e139592e8a32e7779b2f02832d5f
ms.sourcegitcommit: 5205d15c697bbfd4ecb3f45b5de093f709d11979
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90026408"
---
# <a name="use-github-actions-to-connect-to-azure"></a>使用 GitHub Actions 连接到 Azure

了解如何将 [Azure 登录](https://github.com/Azure/login)与 [Azure PowerShell](https://github.com/Azure/PowerShell) 或 [Azure CLI](https://github.com/Azure/CLI) 搭配使用，以便与你的 Azure 资源进行交互。

若要使用 Azure PowerShell 或 Azure CLI，首先需要使用 [Azure 登录名](https://github.com/marketplace/actions/azure-login)登录。 Azure 登录操作使用服务主体将你的 Azure 订阅连接到 GitHub。

设置登录操作后，便可以使用 Azure CLI 或 Azure PowerShell。  
Azure CLI 为 Azure CLI 设置 GitHub 操作运行程序环境。 Azure PowerShell 使用 Azure PowerShell 模块设置 GitHub 操作运行程序环境。


## <a name="create-a-service-principal-and-add-it-to-github-secret"></a>创建一个服务主体，并将其添加到 GitHub 机密

若要使用 [Azure 登录](https://github.com/marketplace/actions/azure-login)，首先需要将 Azure 服务主体作为机密添加到 GitHub 存储库。

在本例中，你将创建一个可用于向 Azure 进行身份验证的机密，名为 `AZURE_CREDENTIALS`。  

1. 如果没有现有的应用程序，请注册[新 Active Directory 应用程序](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal&preserve-view=true)，以便用于服务主体。

    ```azurecli-interactive
        appName="myApp"

        az ad app create \
        --display-name $appName \
        --homepage "http://localhost/$appName" \
        --identifier-uris http://localhost/$appName
    ```

1. 在 Azure 门户中为你的应用[创建新服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。 

    ```azurecli-interactive
        az ad sp create-for-rbac --name "myApp" --role contributor \
                                    --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                    --sdk-auth
    ```

1. 复制你的服务主体的 JSON 对象。

    ```json
    {
        "clientId": "<GUID>",
        "clientSecret": "<GUID>",
        "subscriptionId": "<GUID>",
        "tenantId": "<GUID>",
        (...)
    }
    ```

1. 打开 GitHub 存储库并转到“设置”。

    :::image type="content" source="media/github-repo-settings.png" alt-text="在导航中选择“设置”":::

1. 依次选择“机密”和“新建机密” 。

    :::image type="content" source="media/select-secrets.png" alt-text="选择以添加机密":::

1. 粘贴名为 `AZURE_CREDENTIALS` 的服务主体的 JSON 对象。 

    :::image type="content" source="media/azure-secret-add.png" alt-text="在 GitHub 中添加机密":::

1. 通过选择“添加机密”保存。

## <a name="use-the-azure-login-action"></a>使用 Azure 登录操作

将服务主体机密与 [Azure 登录操作](https://github.com/Azure/login)配合使用，以向 Azure 进行身份验证。

在此工作流中，你需要使用 `secrets.AZURE_CREDENTIALS` 进行身份验证，然后运行 Azure CLI 操作。

拥有可用的 Azure 登录名后，便可以使用 Azure PowerShell 或 Azure CLI 操作。 还可以使用其他 Azure 操作，如 [Azure webapp 部署](https://github.com/Azure/webapps-deploy)和 [Azure Functions](https://github.com/Azure/functions-action)。

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
```

## <a name="use-the-azure-powershell-action"></a>使用 Azure PowerShell 操作

在本例中，你需要使用 [Azure 登录操作](https://github.com/Azure/login)登录，然后使用 [Azure CLI 操作](https://github.com/azure/powershell)检索资源组。

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
          enable-AzPSSession: true
      - name: Azure PowerShell Action
        uses: Azure/powershell@v1
        with:
          inlineScript: Get-AzVM -ResourceGroupName "< YOUR RESOURCE GROUP >"
          azPSVersion: 3.1.0
```

## <a name="use-the-azure-cli-action"></a>使用 Azure CLI 操作

在本例中，你需要使用 [Azure 登录操作](https://github.com/Azure/login)登录，然后使用 [Azure CLI 操作](https://github.com/Azure/CLI)检索资源组。


```yaml
on: [push]

name: AzureLoginSample

jobs:
build-and-deploy:
    runs-on: ubuntu-latest
    steps:

    - name: Log in with Azure
        uses: azure/login@v1
        with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Azure CLI script
        uses: azure/CLI@v1
        with:
        azcliversion: 2.0.72
        inlineScript: |
            az account show
            az storage -h
```

## <a name="connect-with-other-azure-services"></a>连接其他 Azure 服务

以下文章详细介绍了如何从 Azure 与其他服务连接到 GitHub。  

### <a name="azure-active-directory"></a>Azure Active Directory 

- [通过 Azure AD（单一登录）登录到 GitHub Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/github-tutorial)   

### <a name="power-bi"></a>Power BI

- [将 Power BI 与 GitHub 连接](https://docs.microsoft.com/power-bi/service-connect-to-github)   

### <a name="connectors"></a>连接器

- [适用于 Azure 逻辑应用、Power Automate 和 Power Apps 的 GitHub 连接器](https://docs.microsoft.com/connectors/github/)   

### <a name="azure-databricks"></a>Azure Databricks

- [使用 GitHub 作为笔记本的版本控制](https://docs.microsoft.com/azure/databricks/notebooks/github-version-control) 

> [!div class="nextstepaction"]
> [将应用从 GitHub 部署到 Azure](deploy-to-azure.md)
