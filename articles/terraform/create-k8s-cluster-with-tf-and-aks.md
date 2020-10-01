---
title: 使用 Terraform 和 Azure Kubernetes 服务 (AKS) 创建 Kubernetes 群集
description: 了解如何使用 Azure Kubernetes 服务和 Terraform 创建 Kubernetes 群集。
keywords: azure devops terraform aks kubernetes
ms.topic: how-to
ms.date: 03/09/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: ccf5855f414b233f97642f60a4f52c99848b34cd
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401647"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-using-terraform"></a>使用 Terraform 和 Azure Kubernetes 服务 (AKS) 创建 Kubernetes 群集

[Azure Kubernetes 服务 (AKS)](/azure/aks/) 管理托管的 Kubernetes 环境。 使用 AKS 可以部署和管理容器化应用程序，而无需具备容器业务流程方面的专业知识。 使用 AKS 还可在应用不离线的情况下进行许多常见的维护操作。 这些操作包括按需预配、更新和缩放资源。

本文介绍如何执行以下任务：

> [!div class="checklist"]
> * 使用 HCL（HashiCorp 语言）定义 Kubernetes 群集
> * 使用 Terraform 和 AKS 创建 Kubernetes 群集
> * 使用 kubectl 工具测试 Kubernetes 群集的可用性

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

- **配置 Terraform**：遵循[安装 Terraform 并配置对 Azure 的访问权限](get-started-cloud-shell.md)一文中的指导

- **Azure 服务主体**：遵循[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)一文的“创建服务主体”部分中的指导  。 记下 `appId`、`displayName`、`password` 和 `tenant` 的值。

## <a name="create-the-directory-structure"></a>创建目录结构

首先，创建包含 Terraform 配置文件的目录用于练习。

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 打开 [Azure Cloud Shell](/azure/cloud-shell/overview)。 如果事先未选择环境，请选择“Bash”作为环境。

    ![Cloud Shell 提示符](./media/create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. 切换到 `clouddrive` 目录。

    ```bash
    cd clouddrive
    ```

1. 创建名为 `terraform-aks-k8s` 的目录。

    ```bash
    mkdir terraform-aks-k8s
    ```

1. 将目录切换到新目录：

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>声明 Azure 提供程序

创建声明 Azure 提供程序的 Terraform 配置文件。

1. 在 Cloud Shell 中，创建名为 `main.tf` 的文件。

    ```bash
    code main.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    provider "azurerm" {
        # The "feature" block is required for AzureRM provider 2.x. 
        # If you are using version 1.x, the "features" block is not allowed.
        version = "~>2.0"
        features {}
    }

    terraform {
        backend "azurerm" {}
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="define-a-kubernetes-cluster"></a>定义 Kubernetes 群集

创建 Terraform 配置文件，用于声明 Kubernetes 群集的资源。

1. 在 Cloud Shell 中，创建名为 `k8s.tf` 的文件。

    ```bash
    code k8s.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    resource "azurerm_resource_group" "k8s" {
        name     = var.resource_group_name
        location = var.location
    }
    
    resource "random_id" "log_analytics_workspace_name_suffix" {
        byte_length = 8
    }

    resource "azurerm_log_analytics_workspace" "test" {
        # The WorkSpace name has to be unique across the whole of azure, not just the current subscription/tenant.
        name                = "${var.log_analytics_workspace_name}-${random_id.log_analytics_workspace_name_suffix.dec}"
        location            = var.log_analytics_workspace_location
        resource_group_name = azurerm_resource_group.k8s.name
        sku                 = var.log_analytics_workspace_sku
    }

    resource "azurerm_log_analytics_solution" "test" {
        solution_name         = "ContainerInsights"
        location              = azurerm_log_analytics_workspace.test.location
        resource_group_name   = azurerm_resource_group.k8s.name
        workspace_resource_id = azurerm_log_analytics_workspace.test.id
        workspace_name        = azurerm_log_analytics_workspace.test.name

        plan {
            publisher = "Microsoft"
            product   = "OMSGallery/ContainerInsights"
        }
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = var.cluster_name
        location            = azurerm_resource_group.k8s.location
        resource_group_name = azurerm_resource_group.k8s.name
        dns_prefix          = var.dns_prefix

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
                key_data = file(var.ssh_public_key)
            }
        }

        default_node_pool {
            name            = "agentpool"
            node_count      = var.agent_count
            vm_size         = "Standard_D2_v2"
        }

        service_principal {
            client_id     = var.client_id
            client_secret = var.client_secret
        }

        addon_profile {
            oms_agent {
            enabled                    = true
            log_analytics_workspace_id = azurerm_log_analytics_workspace.test.id
            }
        }
        
        network_profile {
        load_balancer_sku = "Standard"
        network_plugin = "kubenet"
        }

        tags = {
            Environment = "Development"
        }
    }
    ```

    上面的代码设置群集的名称、位置和资源组名称。 此外，还设置了完全限定域名 (FQDN) 的前缀。 FQDN 用于访问群集。

    使用 `linux_profile` 记录可以配置用于通过 SSH 登录到工作器节点的设置。

    使用 AKS 时，只需支付工作节点的费用。 `default_node_pool` 记录配置这些工作器节点的详细信息。 `default_node_pool record` 包含要创建的工作器节点数，以及工作器节点的类型。 如果将来需要纵向扩展或纵向缩减群集，请修改此记录中的 `count` 值。

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="declare-the-variables"></a>声明变量

1. 在 Cloud Shell 中，创建名为 `variables.tf` 的文件。

    ```bash
    code variables.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }

    variable log_analytics_workspace_name {
        default = "testLogAnalyticsWorkspaceName"
    }

    # refer https://azure.microsoft.com/global-infrastructure/services/?products=monitor for log analytics available regions
    variable log_analytics_workspace_location {
        default = "eastus"
    }

   # refer https://azure.microsoft.com/pricing/details/monitor/ for log analytics pricing 
   variable log_analytics_workspace_sku {
        default = "PerGB2018"
   }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="create-a-terraform-output-file"></a>创建 Terraform 输出文件

使用 [Terraform 输出](https://www.terraform.io/docs/configuration/outputs.html)可以定义当 Terraform 应用计划时要向用户突出显示的、可以使用 `terraform output` 命令查询的值。 在本部分，我们将创建一个输出文件，以便使用 [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) 访问群集。

1. 在 Cloud Shell 中，创建名为 `output.tf` 的文件。

    ```bash
    code output.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    output "client_key" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_key
    }

    output "client_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate
    }

    output "cluster_ca_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate
    }

    output "cluster_username" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.username
    }

    output "cluster_password" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.password
    }

    output "kube_config" {
        value = azurerm_kubernetes_cluster.k8s.kube_config_raw
    }

    output "host" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.host
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="set-up-azure-storage-to-store-terraform-state"></a>将 Azure 存储设置为存储 Terraform 状态

Terraform 在本地通过 `terraform.tfstate` 文件跟踪状态。 在单用户环境中，此模式非常合适。 在多用户环境中，[Azure 存储](/azure/storage/)用于跟踪状态。

本部分介绍如何执行以下任务：
- 检索存储帐户信息（帐户名称和帐户密钥）
- 创建存储 Terraform 状态信息的存储容器。

1. 在 Azure 门户的左侧菜单中，选择“所有服务”。

1. 选择“存储帐户”。

1. 在“存储帐户”选项卡上，选择用于存储 Terraform 状态信息的存储帐户名称。 例如，可以使用首次打开 Cloud Shell 时创建的存储帐户。  Cloud Shell 创建的存储帐户名称通常以 `cs` 开头，后接由数字和字母组成的随机字符串。 记下所选的存储帐户。 稍后将需要使用此值。

1. 在存储帐户选项卡上，选择“访问密钥”。

    ![存储帐户菜单](./media/create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. 记下“密钥 1”密钥值。 （选择密钥右侧的图标将值复制到剪贴板。）

    ![存储帐户访问密钥](./media/create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. 在 Cloud Shell 的 Azure 存储帐户中创建容器。 将占位符替换为你的环境的相应值。

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>创建 Kubernetes 群集

本部分介绍如何使用 `terraform init` 命令来创建资源，这些资源在前面部分所创建的配置文件中定义。

1. 在 Cloud Shell 中，初始化 Terraform。 将占位符替换为你的环境的相应值。

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    `terraform init` 命令显示成功初始化后端和提供程序插件：

    ![“Terraform init”结果示例](./media/create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. 导出服务主体凭据。 将占位符替换为服务主体的相应值。

    ```bash
    export TF_VAR_client_id=<service-principal-appid>
    export TF_VAR_client_secret=<service-principal-password>
    ```

1. 运行 `terraform plan` 命令，以创建定义基础结构元素的 Terraform 计划。 

    ```bash
    terraform plan -out out.plan
    ```

    `terraform plan` 命令显示运行 `terraform apply` 命令时要创建的资源：

    ![“Terraform plan”结果示例](./media/create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. 运行 `terraform apply` 命令，以应用该计划来创建 Kubernetes 群集。 创建 Kubernetes 群集的过程可能需要花费几分钟时间，从而导致 Cloud Shell 会话超时。如果 Cloud Shell 会话超时，可按照“在 Cloud Shell 超时后进行恢复”部分中的步骤来完成此过程。

    ```bash
    terraform apply out.plan
    ```

    `terraform apply` 命令显示创建配置文件中定义的资源的结果：

    ![“Terraform apply”结果示例](./media/create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. 在 Azure 门户中，在左侧菜单中选择“所有资源”，查看为新 Kubernetes 群集创建的资源  。

    ![Azure 门户中的所有资源](./media/create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>在 Cloud Shell 超时后进行恢复

如果 Cloud Shell 会话超时，可执行以下步骤予以恢复：

1. 启动 Cloud Shell 会话。

1. 切换到包含 Terraform 配置文件的目录。

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. 运行以下命令：

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>测试 Kubernetes 群集

可以使用 Kubernetes 工具来验证新建的群集。

1. 从 Terraform 状态中获取 Kubernetes 配置，并将其存储在 kubectl 可以读取的文件中。

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. 设置环境变量，使 kubectl 拾取正确的配置。

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. 验证群集的运行状况。

    ```bash
    kubectl get nodes
    ```

    应会看到工作节点的详细信息，并且这些节点的状态为“就绪”，如下图所示：

    ![使用 kubectl 工具可以验证 Kubernetes 群集的运行状况](./media/create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="monitor-health-and-logs"></a>监视运行状况和日志

创建 AKS 群集以后，已启用监视功能来捕获群集节点和 Pod 的运行状况指标。 Azure 门户提供这些运行状况指标。 有关容器运行状况监视的详细信息，请参阅[监视 Azure Kubernetes 服务运行状况](/azure/azure-monitor/insights/container-insights-overview)。

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [详细了解如何在 Azure 中使用 Terraform](/azure/terraform)
