---
title: Azure CLI 发行说明
description: 了解 Azure CLI 的最新更新
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 04/21/2020
ms.topic: article
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 4dcc1bcfb6e42089e221a1ebd7eb5abc0d37a4e3
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89374548"
---
# <a name="azure-cli-release-notes"></a>Azure CLI 发行说明

## <a name="april-21-2020"></a>2020 年 4 月 21 日

版本 2.4.0

### <a name="acr"></a>ACR

* `az acr run --cmd`：禁用工作目录替代
* 支持专用数据终结点

### <a name="aks"></a>AKS

* `az aks list -o table` 应显示 privateFqdn 作为专用群集的 FQDN
* 添加了 --uptime-sla
* 更新了 containerservice 包
* 添加了节点公共 IP 支持
* 修复了 help 命令中的拼写错误

### <a name="appconfig"></a>AppConfig

* 解决了有关 kv list 和 export 命令的密钥保管库引用问题
* 修复了 Bug 以便列出键值

### <a name="appservice"></a>应用服务

* `az functionapp create`：更改了为 .NET Linux 函数应用设置 linuxFxVersion 的方式。 这应会修复一个阻止创建 .NET Linux 消耗应用的 Bug
* [中断性变更] `az webapp create`：修复了在使用 az webapp create 时保留现有 AppSettings 的问题
* [中断性变更] `az webapp up`：修复了结合 -g 标志使用 az webapp up 命令创建资源组时的问题
* [中断性变更] `az webapp config`：修复了使用 az webapp config connection-string list 显示非 JSON 输出值时的问题

### <a name="arm"></a>ARM

* `az deployment create/validate`：添加了参数 `--no-prompt`，支持跳过有关 ARM 模板缺少参数的提示
* `az deployment group/mg/sub/tenant validate`：支持部署参数文件中的注释
* `az deployment`：为参数 `--handle-extended-json-format` 删除了 `is_preview`
* `az deployment group/mg/sub/tenant cancel`：支持 ARM 模板的取消部署
* `az deployment group/mg/sub/tenant validate`：改进了部署验证失败时显示的错误消息
* `az deployment-scripts`：为 DeploymentScripts 添加了新命令
* `az resource tag`：添加了参数 `--is-incremental`，支持以增量方式将标记添加到资源

### <a name="aro"></a>ARO

* `az aro`：添加了 Azure RedHat OpenShift V4 aro 命令模块

### <a name="batch"></a>Batch

* 更新了 Batch API

### <a name="compute"></a>计算

* `az sig image-version create`：添加了存储帐户类型 Premium_LRS
* `az vmss update`：修复了终止通知更新问题
* `az vm/vmss create`：添加了对专用映像版本的支持
* SIG API 版本 2019-12-01
* `az sig image-version create`：添加了 --target-region-encryption
* 修复了在连续运行时由于 keyvault 名称在全局内存中缓存中重复而出现的测试失败

### <a name="cosmosdb"></a>CosmosDB

* 支持 `az cosmosdb private-link-resource/private-endpoint-connection`

### <a name="iot-central"></a>IoT Central

* 弃用了 `az iotcentral`
* 添加了 `az iot central` 命令模块

### <a name="monitor"></a>监视

* 支持监视器的专用链接方案
* 修复了 test_monitor_general_operations.py 中的错误模拟方式

### <a name="network"></a>网络

* 弃用了 public ip update 命令的 sku
* `az network private-endpoint`：支持专用 DNS 区域组
* 为 vnet/subnet 参数启用了本地上下文功能
* 修复了 test_nw_flow_log_delete 中错误的用法示例

### <a name="packaging"></a>打包

* 删除了对 Ubuntu/Disco 包的支持

### <a name="rbac"></a>RBAC

* `az ad app create/update`：支持 --optional-claims 作为参数

### <a name="rdbms"></a>RDBMS

* 添加了适用于 PostgreSQL 和 MySQL 的 Azure Active Directory 管理员命令

### <a name="service-fabric"></a>Service Fabric

* 修复 #12891：`az sf application update --application-parameters` 删除了不在请求中的旧参数
* 修复 #12470 az sf create cluster，修复了更新持久性和可靠性中的 bug，在给定节点类型名称的情况下可以通过代码正确查找 VMSS

### <a name="sql"></a>SQL

* 添加了 `az sql mi op list`、`az sql mi op get` 和 `az sql mi op cancel`
* `az sql midb`：更新/显示长期保留策略、显示/删除长期保留备份、还原长期保留备份

### <a name="storage"></a>存储

* 将 azure-mgmt-storage 升级到 9.0.0
* `az storage logging off`：支持对存储帐户关闭日志记录功能
* `az storage account update`：为 CMK 启用了密钥自动轮换
* `az storage account encryption-scope create/update/list/show`：添加了对自定义加密范围的支持
* `az storage container create`：添加了 --default-encryption-scope 和 --deny-encryption-scope-override 以设置容器级别的加密范围

### <a name="survey"></a>调查

* 添加了用于关闭调查链接的开关

## <a name="april-01-2020"></a>2020 年 4 月 1 日

版本 2.3.1

### <a name="acr"></a>ACR

* 修复用于 Linux 的 azure-mgmt-containerregistry 的错误版本

### <a name="profile"></a>配置文件

* az login：修复在使用除 `latest` 之外的云配置文件时发生登录失败的问题

## <a name="march-31-2020"></a>2020 年 3 月 31 日

版本 2.3.0

### <a name="acr"></a>ACR

* 'az acr task update'：null 指针异常
* `az acr import`：修改帮助和错误消息以阐明 --source 和 --registry 的用法
* 为参数 'registry_name' 添加了验证程序
* `az acr login`：删除了 '--expose-token' 上的预览标志
* [中断性变更E] 'az acr task create/update' 分支参数已删除
* 'az acr task update'：客户现在可以单独更新上下文、git-token 和/或触发器
* 'az acr agentpool'：新功能

### <a name="aks"></a>AKS

* 更新 --api-server-authorized-ip-ranges 时修复了 apiServerAccessProfile
* aks 更新：更新时用输入值替代出站 IP
* 不要为 MSI 群集创建 SPN，支持将 acr 附加到 MSI 群集

### <a name="ams"></a>AMS

* 修复了 #12469：添加 Fairplay content-key-policy 时由于 'ask' 参数问题而失败

### <a name="appconfig"></a>AppConfig

* 为 kv export 添加了 --skip-keyvault

### <a name="appservice"></a>应用服务

* 修复了 #12509：默认情况下删除 az webapp up 的标记
* az functionapp create：已更新 --runtime-version 帮助菜单，并在用户为 .net 指定 --runtime-version 时添加了警告
* az functionapp create：更新了为 Windows 函数应用设置 javaVersion 的方式

### <a name="arm"></a>ARM

* az deployment create/validate：默认情况下使用 --handle-extended-json-format
* az lock create：在帮助文档中添加了用于创建子资源的示例
* az deployment {group/mg/sub/tenant} list：支持 provisioningState 筛选
* az deployment：修复了最后一个参数下注释的分析 bug

### <a name="backup"></a>备份

* 添加了多个文件还原功能
* 添加了对仅备份 OS 磁盘的支持
* 添加了 restore-as-unmanaged-disk 参数以指定非托管还原

### <a name="compute"></a>计算

* az vm create：为 --nsg-rule 添加了 NONE 选项
* az vmss create/update：删除了 vmss 自动修复预览标记
* az vm update：支持 --workspace
* 修复了 VirtualMachineScaleSetExtension 初始化代码中的 bug
* 将 VMAccessAgent 版本升级到了 2.4
* az vmss set-orchestration-service-state：支持设置 vmss 中的业务流程服务的状态
* 将磁盘 API 版本升级到了 2019-11-01
* az disk create：添加了 --disk-iops-read-only、--disk-mbps-read-only、--max-shares、--image-reference、--image-reference-lun、--gallery-image-reference、--gallery-image-reference-lun

### <a name="cosmos-db"></a>Cosmos DB

* 修复了进行弃用重定向时缺少的 --type 选项

### <a name="docker"></a>Docker

* 更新到了 Alpine 3.11 和 Python 3.6.10

### <a name="extension"></a>分机

* 允许通过包在系统路径中加载扩展

### <a name="hdinsight"></a>HDInsight

* （az hdinsight create：）支持客户通过使用参数 `--minimal-tls-version` 指定受支持的最低 tls 版本。 允许的值为 1.0,1.1,1.2

### <a name="iot"></a>IoT

* 添加了 codeowner
* az iot hub create：将默认 sku 从 F1 更改为 S1
* iot hub：支持 2019-03-01-hybrid 的配置文件中的 IotHub

### <a name="iotcentral"></a>IoTCentral

* 更新了错误详细信息，更新了默认应用程序模板和提示消息

### <a name="keyvault"></a>KeyVault

* 支持证书备份/还原
* keyvault create/update：支持 --retention-days
* 列出时不再显示托管密钥/机密
* az keyvault create：支持 `--network-acls`、`--network-acls-ips` 和 `--network-acls-vnets`，以便可以在创建保管库时指定网络规则

### <a name="lock"></a>Lock

* 修复了 az lock delete 的 bug：az lock delete 对 Microsoft.DocumentDB 不起作用

### <a name="monitor"></a>监视

* az monitor clone：支持将指标规则从一个资源克隆到另一个资源
* 修复了 IcM179210086：无法为 Application Insights 指标创建自定义指标警报

### <a name="netappfiles"></a>NetAppFiles

* az volume create：允许数据保护卷添加复制操作：批准、挂起、继续、状态、删除

### <a name="network"></a>网络

* az network application-gateway waf-policy managed-rule rule-set add：支持 Microsoft_BotManagerRuleSet
* network watcher flow-log show：修复了错误的弃用信息
* 支持应用程序网关侦听器中的主机名
* az network nat gateway：支持创建空资源，无需公共 IP 或公共 IP 前缀
* 支持 VPN 网关生成
* 支持 `az network dns record-set {} add-record` 中的 `--if-none-match`

### <a name="packaging"></a>打包

* 取消了对 Python 3.5 的支持

### <a name="profile"></a>配置文件

* az login：为 MFA 错误显示警告

### <a name="rdbms"></a>RDBMS

* 为 PostgreSQL 和 MySQL 添加服务器数据加密密钥管理命令

## <a name="march-10-2020"></a>2020 年 3 月 10 日

版本 2.2.0

### <a name="acr"></a>ACR

* 修复：`az acr login` 错误地引发错误
* 添加新命令 `az acr helm install-cli`
* 添加专用链接和 CMK 支持
* 添加“private-link-resource list”命令

### <a name="aks"></a>AKS

* 修复 Cloud Shell 中的 aks browse
* az aks：修复监视加载项和 agentpool NoneType 错误
* 在创建 Azure Kubernetes 群集时向节点池添加 --nodepool-tags
* 将 nodepool 添加到群集或将其更新时添加 --tags
* aks create：添加 `--enable-private-cluster`
* 创建 Azure Kubernetes 群集时添加 --nodepool-labels
* 向 Azure Kubernetes 群集添加新 nodepool 时添加 --labels
* 在仪表板 URL 中添加缺失的 /
* 支持创建 aks 群集来启用托管标识
* az aks：验证网络插件是“azure”还是“kubenet”
* az aks：添加 aad 会话密钥支持
* [中断性变更E] az aks：支持针对 omsagent 的 GF 和 BF 的 msi 更改（容器监视）（#1）
* az aks use-dev-spaces：将终结点类型选项添加到 use-dev-spaces 命令，自定义在 Azure Dev Spaces 控制器上创建的终结点

### <a name="appconfig"></a>AppConfig

* 取消阻止使用“kv set”来添加 keyvault 引用和功能 …

### <a name="appservice"></a>应用服务

* az webapp create：修复使用 --runtime 运行此命令时的问题
* az functionapp deployment source config-zip：在资源组或函数名无效/不存在的情况下添加错误消息
* functionapp create：修复了目前在使用 `functionapp create` 时会显示的警告消息，该消息引用了一个 `--functions_version` 标志，但在标志名称中错误地使用了 `_`（应该是 `-`）
* az functionapp create：更新了为 linux 函数应用设置 linuxFxVersion 和容器映像名称的方式
* az functionapp deployment source config-zip：修复在 zip 部署过程中应用设置更改争用条件导致的问题，在部署过程中给出 5xx 错误
* 修复 #5720946：az webapp backup 无法设置名称

### <a name="arm"></a>ARM

* az resource：改进资源模块的示例
* az policy assignment list：支持列出管理组范围的策略分配
* 添加 `az deployment group` 和 `az deployment operation group`，用于在资源组中部署模板。 这是 `az group deployment` 和 `az group deployment operation` 的副本
* 添加 `az deployment sub` 和 `az deployment operation sub`，用于在订阅范围部署模板。 这是 `az deployment` 和 `az deployment operation` 的副本
* 添加 `az deployment mg` 和 `az deployment operation mg`，用于在管理组中部署模板
* 添加 `az deployment tenant` 和 `az deployment operation tenant`，用于在租户范围部署模板
* az policy assignment create：为 `--location` 参数添加说明
* az group deployment create：添加参数 `--aux-tenants`，用于提供跨租户支持

### <a name="cdn"></a>CDN

* 添加 CDN WAF 命令

### <a name="compute"></a>计算

* az sig image-version：添加 --data-snapshot-luns
* az ppg show：添加 --colocation-status，允许获取邻近放置组中所有资源的归置状态
* az vmss create/update：支持自动修复
* [中断性变更E] az image template：将模板重命名为生成器
* az image builder create：添加 --image-template

### <a name="cosmos-db"></a>Cosmos DB

* 添加 Sql 存储过程、udf 和触发器 cmdlet
* az cosmosdb create：添加 --key-uri，支持添加密钥保管库加密信息

### <a name="keyvault"></a>KeyVault

* keyvault create：默认启用软删除

### <a name="monitor"></a>监视

* az monitor metrics alert create：在 `--condition` 中支持 `~`

### <a name="network"></a>网络

* az network application-gateway rewrite-rule create：支持 url 配置
* az network dns zone import：--zone-name 在将来不区分大小写
* az network private-endpoint/private-link-service：删除预览标签
* az network bastion：支持堡垒
* az network vnet list-available-ips：支持列出 VNet 中的可用 IP
* az network watcher flow-log create/list/delete/update：添加新命令来管理观察程序流日志，并公开--location 以显式标识观察程序
* az network watcher flow-log configure：已弃用
* az network watcher flow-log show：支持使用 --location 和 --name 来获取 ARM 格式的结果，弃用了旧格式的输出

### <a name="policy"></a>策略

* az policy assignment create：修复了自动生成的策略分配名称超出限制的 Bug

### <a name="rbac"></a>RBAC

* az ad group show: 修复了将 --group 值视为正则表达式的问题

### <a name="rdbms"></a>RDBMS

* 将 azure-mgmt-rdbms SDK 版本升级到 2.0.0
* az postgres private-endpoint-connection：管理 postgres 专用终结点连接
* az postgres private-link-resource：管理 postgres 专用链接资源
* az mysql private-endpoint-connection：管理 mysql 专用终结点连接
* az mysql private-link-resource：管理 mysql 专用链接资源
* az mariadb private-endpoint-connection：管理 mariadb 专用终结点连接
* az mariadb private-link-resource：管理 mariadb 专用链接资源
* 更新 RDBMS 专用终结点测试

### <a name="sql"></a>SQL

* Sql midb 添加：list-deleted、show-deleted、update-retention、show-retention
* （sql server create：）为 sql server create 添加可选的 public-network-access 'Enable'/'Disable' 标志
* （sql server update：）进行了某些面向客户的更改
* 为 MI 和 SQL DB 添加 minimal_tls_version 属性

### <a name="storage"></a>存储

* az storage blob delete-batch：`--dryrun` 标志行为异常
* az storage account network-rule add（Bug 修复）：添加操作应该幂等
* az storage account create/update：添加路由首选项支持
* 将 azure-mgmt-storage 版本升级到 8.0.0
* az storage container immutability create：添加 --allow-protected-append-write 参数
* az storage account private-link-resource list：添加了相关支持，允许列出存储帐户的专用链接资源
* az storage account private-endpoint-connection approve/reject/show/delete：支持管理专用终结点连接
* az storage account blob-service-properties update：添加 --enable-restore-policy 和 --restore-days
* az storage blob restore：添加了相关支持，允许还原 blob 范围

## <a name="february-18-2020"></a>2020 年 2 月 18 日

版本 2.1.0

### <a name="acr"></a>ACR

* 为 `az acr login` 添加了新参数 `--expose-token`
* 修复了 `az acr task identity show -n Name -r Registry -o table` 的错误输出
* az acr login：如果 docker 命令返回了错误，则引发 CLIError

### <a name="acs"></a>ACS

* aks create/update：添加 `--vnet-subnet-id` 验证

### <a name="aladdin"></a>Aladdin

* 将生成的示例分析为命令的 _help.py

### <a name="ams"></a>AMS

* az ams 现为正式版

### <a name="appconfig"></a>AppConfig

* 修订帮助消息，排除不受支持的键/标签筛选器
* 删除大多数命令的预览标记，不包括托管标识和功能标志
* 添加了更新存储时使用的客户托管密钥

### <a name="appservice"></a>应用服务

* az webapp list-runtimes：修复了 list-runtimes 的 Bug
* 添加了 az webapp|functionapp config ssl create
* 添加了对 v3 函数应用和 Node 12 的支持

### <a name="arm"></a>ARM

* az policy assignment create：修复了在 `--policy` 参数无效时出现的错误消息
* az group deployment create：修复了在使用大型 parameters.json 文件时出现的“stat: Windows 的路径太长”错误

### <a name="backup"></a>备份

* 针对 OLR 中的项级恢复流进行了修复
* 为 SQL 和 SAP 数据库添加“还原为文件”支持

### <a name="compute"></a>计算

* vm/vmss/availability-set update：添加了 --ppg，允许更新 ProximityPlacementGroup
* vmss create：添加了 --data-disk-iops 和 --data-disk-mbps
* az vm host：删除了 `vm host` 和 `vm host group` 的预览标记
* [中断性变更E] 修复 10728：`az vm create`：如果指定了 vnet 但子网不存在，则自动创建子网
* 提高了 vm image list 的可靠性

### <a name="eventhub"></a>Eventhub

* 针对 2019-03-01-hybrid 配置文件的 Azure Stack 支持

### <a name="keyvault"></a>KeyVault

* az keyvault key create：添加了适合参数 `--ops` 的新值 `import`
* az keyvault key list-versions：支持使用参数 `--id` 来指定密钥
* 支持专用终结点连接

### <a name="network"></a>网络

* 升级到 azure-mgmt-network 9.0.0
* az network private-link-service update/create：支持 --enable-proxy-protocol
* 添加连接监视器 V2 功能

### <a name="packaging"></a>打包

* [中断性变更E] 删除了对 Python 2.7 的支持

### <a name="profile"></a>配置文件

* 预览版：为订阅帐户添加了新属性 `homeTenantId` 和 `managedByTenants`。 请重新运行 `az login`，使更改生效
* az login：当列出的某个订阅来自多个租户时，显示警告，并将其默认设置为第一个租户的。 若要在访问此订阅时选择特定租户，请确保 `az login` 中包括 `--tenant`

### <a name="role"></a>角色

* az role assignment create：修复了按显示名称将角色分配给服务主体时会生成 HTTP 400 的错误

### <a name="sql"></a>SQL

* 用这两个新参数更新了 SQL 托管实例 cmdlet `az sql mi update`：tier 和 family

### <a name="storage"></a>存储

* [重大更改] `az storage account create`：将默认存储帐户类型更改为 StorageV2

## <a name="february-04-2020"></a>2020 年 2 月 4 日

版本 2.0.81

### <a name="acs"></a>ACS

* 增加了对在标准负载均衡器上设置出站分配端口和空闲超时的支持
* 更新了 API 版本 2019-11-01

### <a name="acr"></a>ACR

* [中断性变更] `az acr delete` 将进行提示
* [中断性变更E]“az acr task delete”将进行提示
* 添加了用于管理任务运行的一个新的命令组“az acr taskrun show/list/delete”

### <a name="aks"></a>AKS

* 每个群集都获得一个单独的服务主体，从而改进了隔离

### <a name="appconfig"></a>AppConfig

* 支持从/向应用服务导入/导出 keyvault 引用
* 支持将所有标签从 appconfig 导入/导出到 appconfig
* 在设置和导入之前验证密钥和功能名称
* 公开配置存储的 SKU 修改。
* 添加了适用于托管标识的命令组。

### <a name="appservice"></a>应用服务

* Azure Stack：2019-03-01-hybrid 的配置文件下的 surface 命令
* functionapp：增加了在 Linux 中创建 Java 函数应用的能力

### <a name="arm"></a>ARM

* 修复了问题 #10246：当传入的参数 `--ids` 是资源组 ID 时，`az resource tag` 崩溃
* 修复了问题 #11658：`az group export` 命令不支持 `--query` 和 `--output` 参数
* 修复了问题 #10279：当验证失败时，`az group deployment validate` 的退出代码为 0
* 修复了问题 #9916：改进了当 `az resource list` 命令的标记与其他筛选器条件存在冲突时显示的错误消息
* 为命令 `az group create` 增加了新参数 `--managed-by`，用以支持添加 managedBy 信息

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* 添加了 `monitor` 子组来管理 Azure Red Hat OpensShift 群集中的 Log Analytics 监视

### <a name="botservice"></a>BotService

* 修复了问题 #11697：`az bot create` 不是幂等的
* 将名称更正测试更改成了仅在实时模式下运行

### <a name="cdn"></a>CDN

* 增加了对 rulesEngine 功能的支持
* 添加了新的命令组“cdn endpoint rule”，用以管理规则
* 将 azure-mgmt-cdn 版本更新到了 4.0.0，以便使用 api 版本 2019-04-15

### <a name="deployment-manager"></a>部署管理器

* 增加了针对所有资源的列表操作。
* 增强了用于新建步骤类型的步骤资源。
* 更新了 azure-mgmt-deploymentmanager 程序包以使用版本 0.2.0。

### <a name="iot"></a>IoT

* 启用了“IoT hub Job”命令。

### <a name="iot-central"></a>IoT Central

* 支持使用新的 sku 名称 ST0、ST1、ST2 来创建/更新应用。

### <a name="key-vault"></a>Key Vault

* 增加了一个用于下载密钥的新命令 `az keyvault key download`。

### <a name="misc"></a>杂项

* 修复了 #6371：支持在 Bash 中完成文件名和环境变量

### <a name="network"></a>网络

* 修复了 #2092：az network dns record-set add/remove：添加了找不到记录集时要显示的警告。 将来，将支持使用一个额外的参数来确认此自动创建。

### <a name="policy"></a>策略

* 增加了新命令 `az policy metadata`，用以检索丰富的策略元数据资源
* `az policy remediation create`：通过 `--resource-discovery-mode` 参数指定在修正之前是否应当重新评估符合性

### <a name="profile"></a>配置文件

* `az account get-access-token`：增加了 `--tenant` 参数，用以直接为租户获取令牌，不需要指定订阅

### <a name="rbac"></a>RBAC

* [中断性变更E] 修复了 #11883：`az role assignment create`：作用域为空时会提示错误

### <a name="security"></a>安全性

* 增加了新命令 `az atp show` 和 `az atp update`，用以查看和管理存储帐户的高级威胁防护设置。

### <a name="sql"></a>SQL

* `sql dw create`：弃用了 `--zone-redundant` 和 `--read-replica-count` 参数。 这些参数不适用于数据仓库。
* [重大更改] `az sql db create`：删除了记录为“az sql db create --sample-name”的允许值的“WideWorldImportersStd”和“WideWorldImportersFull”。 这些示例数据库总是会导致创建失败。
* 增加了新命令 `sql db classification show/list/update/delete` 和 `sql db classification recommendation list/enable/disable`，用以管理 SQL 数据库的敏感度分类。
* `az sql db audit-policy`：针对空的审核操作和组进行了修复

### <a name="storage"></a>存储

* 增加了新命令组 `az storage share-rm`，用以使用 Microsoft.Storage 资源提供程序执行 Azure 文件共享管理操作。
* 修复了问题 #11415：`az storage blob update` 的权限错误
* 集成了 Azcopy 10.3.3 并支持 Win32。
* `az storage copy`：增加了 `--include-path`、`--include-pattern`、`--exclude-path` 和 `--exclude-pattern` 参数
* `az storage remove`：将 `--inlcude` 和 `--exclude` 参数更改成了 `--include-path`、`--include-pattern`、`--exclude-path` 和 `--exclude-pattern` 参数
* `az storage sync`：增加了 `--include-pattern`、`--exclude-path` 和 `--exclude-pattern` 参数

### <a name="servicefabric"></a>ServiceFabric

* 增加了用于管理应用程序和服务的新命令。

## <a name="january-13-2020"></a>2020 年 1 月 13 日

版本 2.0.80

### <a name="compute"></a>计算

* 磁盘更新：添加了 --disk-encryption-set 和 --encryption-type
* 快照创建/更新：添加了 --disk-encryption-set 和 --encryption-type

### <a name="storage"></a>存储

* 将 azure-mgmt-storage 版本升级到 7.1.0
* `az storage account create`：添加了 `--encryption-key-type-for-table` 和 `--encryption-key-type-for-queue` 以支持表和队列加密服务

## <a name="january-07-2020"></a>2020 年 1 月 7 日

版本 2.0.79

### <a name="acr"></a>ACR

* [中断性变更E] 删除了 'acr build'、'acr task create/update'、'acr run' 和 'acr pack' 的 '--os' 参数。 改用 '--platform'。

### <a name="appconfig"></a>AppConfig

* 添加了对导入/导出功能标志的支持
* 添加了新命令 'az appconfig kv set-keyvault'，用于创建 keyvault 引用
* 支持将功能标志导出到文件时使用的各种命名约定

### <a name="appservice"></a>应用服务

* 修复问题 7154：更新了命令 <> 的文档，使用反引号来代替单引号
* 修复了问题 11287：webapp up：默认情况下，'make the app created using up' 应该是 'SSL enabled'
* 修复问题 11592：添加适用于 Html 静态站点的 az webapp up flag

### <a name="arm"></a>ARM

* 修复 `az resource tag`：恢复服务保管库标记不能更新

### <a name="backup"></a>备份

* 添加了新命令 'backup protection undelete'，可以为 IaasVM 工作负荷启用软删除功能
* 添加了新参数 '--soft-delete-feature-state'，用于设置 backup-properties 命令
* 添加了对 IaasVM 工作负荷的磁盘排除支持

### <a name="compute"></a>计算

* 修复了 Azure Stack 配置文件中的 `vm create` 故障。
* vm monitor metrics tail/list-definitions：支持用于 VM 的 query metric 和 list definitions。
* 添加了用于 az vm 的新 reapply 命令操作

### <a name="hdinsight"></a>HDInsight

* 支持创建带有 Kafka Rest 代理的 Kafka 群集
* 将 azure-mgmt-hdinsight 升级到 1.3.0

### <a name="misc"></a>杂项

* 添加了预览版命令 `az version show`，用于显示默认 JSON 格式或通过 --output 配置的格式的 Azure CLI 模块和扩展的版本

### <a name="event-hubs"></a>事件中心

* [中断性变更E] 从命令 'az eventhubs eventhub update' 和 'az eventhubs eventhub create' 中删除了 'ReceiveDisabled' 状态选项。 此选项对事件中心条目无效。

### <a name="service-bus"></a>服务总线

* [中断性变更E] 从命令 'az servicebus topic create'、'az servicebus topic update'、'az servicebus queue create' 和 'az servicebus queue update' 中删除了 'ReceiveDisabled' 状态选项。 此选项对服务总线主题和队列无效。

### <a name="rbac"></a>RBAC

* 修复了 11712：当应用程序或服务主体不存在时，`az ad app/sp show` 不返回退出代码 3

### <a name="storage"></a>存储

* `az storage account create`：删除了 --enable-hierarchical-namespace 参数的 preview 标志
* 将 azure-mgmt-storage 版本更新为 7.0.0，以便使用 api 版本 2019-06-01
* 添加了新参数 `--enable-delete-retention` 和 `--delete-retention-days`，支持管理存储帐户 blob-service-properties 的删除保留策略。

## <a name="december-17-2019"></a>2019 年 12 月 17 日

2.0.78

### <a name="acr"></a>ACR

* 添加了在 acr task run 中对本地上下文的支持

### <a name="acs"></a>ACS

* [中断性变更E]az openshift create：将 `--workspace-resource-id` 重命名为 `--workspace-id`。

### <a name="ams"></a>AMS

* 更新了 show 命令，在找不到资源时返回 3

### <a name="appconfig"></a>AppConfig

* 修复了通过追加 api-version 的方式请求 url 时出现的 Bug 现有解决方案不适用于分页。
* 添加了相关支持，可以显示英语之外的语言作为我们的后端服务支持 unicode 以实现全球化。

### <a name="appservice"></a>应用服务

* 修复了问题 11217：webapp：az webapp config ssl upload 应支持 slot 参数
* 修复了问题 10965：错误：名称不能为空。 允许按 ip_address 和子网进行删除
* 添加了相关支持，可以通过 `az webapp config ssl import` 从 Key Vault 导入证书

### <a name="arm"></a>ARM

* 已将 azure-mgmt-resource 包更新为使用 6.0.0
* 添加了新参数 `--aux-subs`，提供对 `az group deployment create` 命令的跨租户支持
* 添加了新参数 `--metadata`，支持为策略集定义添加元数据信息。

### <a name="backup"></a>备份

* 为 SQL 和 SAP Hana 工作负荷添加了备份支持。

### <a name="botservice"></a>BotService

* [中断性变更E] 从预览版命令 'az bot create' 中删除了 '--version' 标志。 仅支持 v4 SDK 机器人。
* 添加了针对 'az bot create' 的名称可用性检查。
* 添加了相关支持，可以通过 'az bot update' 更新机器人的图标 URL。
* 添加了相关支持，可以通过 'az bot directline update' 更新 Direct Line 通道。
* 为 'az bot directline create' 添加了 '--enable-enhanced-auth' 标志支持。
* 以下命令组为 GA 版而不是预览版：'az bot authsetting'。
* 'az bot' 中的以下命令为 GA 版而不是预览版：'create'、'prepare-deploy'、'show'、'delete'、'update'。
* 修复了 'az bot prepare-deploy' 问题，将 '--proj-file-path' 值更改为小写（例如，将“Test.csproj”更改为“test.csproj”）。

### <a name="compute"></a>计算

* vmss create/update：添加了 --scale-in-policy，此项决定了在对 VMSS 进行横向缩减时选择哪些虚拟机进行删除。
* vm/vmss update：添加了 --priority。
* vm/vmss update：添加了 --max-price。
* 添加了 disk-encryption-set 命令组（create、show、update、delete、list）。
* disk create：添加了 --encryption-type 和 --disk-encryption-set。
* vm/vmss create：添加了 --os-disk-encryption-set 和 --data-disk-encryption-sets。

### <a name="core"></a>核心

* 取消了对 Python 3.4 的支持
* 在多个命令中插入了 HaTS 调查

### <a name="dls"></a>DLS

* 更新了 ADLS sdk 版本 (0.0.48)。

### <a name="install"></a>安装

* 安装脚本支持 python 3.8

### <a name="iot"></a>IOT

* [中断性变更E] 从 manual-failover 中删除了 --failover-region 参数。 现在，它会故障转移到已分配的、异地配对的次要区域。

### <a name="key-vault"></a>密钥保管库

* 修复了 8095：`az keyvault storage remove`：改进帮助消息
* 修复了 8921：`az keyvault key/secret/certificate list/list-deleted/list-versions`：修复了参数 `--maxresults` 上出现的验证 Bug
* 修复了 10512：`az keyvault set-policy`：改进了未指定 `--object-id`、`--spn` 或 `--upn` 中的任何一项时出现的错误消息
* 修复了 10846：`az keyvault secret show-deleted`：在指定 `--id` 的情况下，不需要 `--name/-n`
* 修复了 11084：`az keyvault secret download`：改进了参数 `--encoding` 的帮助消息

### <a name="network"></a>网络

* az network application-gateway probe：添加了对 --port 选项的支持，可以在执行创建和更新操作时指定用于探测后端服务器的端口
* az network application-gateway url-path-map create/update：针对 `--waf-policy` 进行了 Bug 修复
* az network application-gateway：添加了对 `--rewrite-rule-set` 的支持
* az network list-service-aliases：添加了可以用于服务终结点策略的列表服务别名支持
* az network dns zone import：添加了在记录名称中使用 .@ 的支持

### <a name="packaging"></a>打包

* 添加了用于 pip 安装的后缘版本
* 添加了 Ubuntu eoan 包

### <a name="policy"></a>策略

* 添加了对策略 API 版本 2019-09-01 的支持。
* az policy set-definition：添加了相关支持，可以使用 `--definition-groups` 参数在策略集定义中分组

### <a name="redis"></a>Redis

* 为 `az redis create` 命令添加了预览版参数 `--replicas-per-master`
* 已将 azure-mgmt-redis 从 6.0.0 更新到 7.0.0rc1

### <a name="servicefabric"></a>ServiceFabric

* 修复了 node-type add logic 中的问题（包括 10963）：在持久性级别为“黄金”时添加新的节点类型总是会引发 CLI 错误
* 已在创建模板中将 ServiceFabricNodeVmExt 版本更新为 1.1

### <a name="sql"></a>SQL

* 为 sql db 的 create 和 update 命令添加了 "--read-scale" 和 "--read-replicas" 参数，目的是支持读取缩放管理。

### <a name="storage"></a>存储

* GA 版大型文件共享属性，适用于存储帐户的 create 和 update 命令
* GA 版用户委托 SAS 令牌支持
* 添加了新命令 `az storage account blob-service-properties show` 和 `az storage account blob-service-properties update --enable-change-feed`，用于管理存储帐户的 blob 服务属性。
* [即将推出的中断性变更] `az storage copy`：`*` 字符再也不能在 URL 中作为通配符使用，但我们会添加提供 `*` 通配符支持的新参数 --include-pattern 和 --exclude-pattern。
* 修复了问题 11043：添加了相关支持，允许在 `az storage remove` 命令中删除整个容器/共享

## <a name="november-26-2019"></a>2019 年 11 月 26 日

版本 2.0.77

### <a name="acr"></a>ACR

* 已在 acr task create/update 中弃用了参数 `--branch`

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* 添加了 `--workspace-resource-id` 标志，以允许创建具有监视功能的 Azure Red Hat Openshift 群集
* 添加了 `monitor_profile` 以创建具有监视功能的 Azure Red Hat OpenShift 群集

### <a name="aks"></a>AKS

* 添加了对使用“az aks rotate-certs”执行群集证书轮换操作的支持。

### <a name="appconfig"></a>AppConfig

* 添加了对将“:”用作 `as az appconfig kv import` 分隔符的支持
* 修复了列出具有多个标签（包括 null 标签）的键值的问题。 
* 已将管理平面 sdk azure-mgmt-appconfiguration 更新到 0.3.0 版本。 

### <a name="appservice"></a>应用服务

* 修复了问题 #11100：创建服务计划时 az webapp up 的 AttributeError
* az webapp up：强制为支持的语言创建或部署到站点，不使用默认值。
* 添加了对应用服务环境的支持：az appservice ase show | list | list-addresses | list-plans | create | update | delete

### <a name="backup"></a>备份

* 修复了 az backup policy list-associated-items 中的问题。 添加了可选的 BackupManagementType 参数。

### <a name="compute"></a>计算

* 已将计算、磁盘和快照的 API 版本升级到 2019-07-01
* vmss create：针对 --orchestration-mode 的改进
* sig image-definition create：添加了--os-state，以允许指定在此映像下创建的虚拟机是“通用”还是“专用”
* sig image-definition create：添加了 -hyper-v-generation，以允许指定虚拟机监控程序代系
* sig image-version create：添加了对 --os-snapshot 和 --data-snapshots 的支持
* image create：添加了 --data-disk-caching，以允许指定数据磁盘的缓存设置
* 已将 Python 计算 SDK 升级到 10.0.0
* vm/vmss create：已将“Spot”添加到“Priority”枚举属性
* [中断性变更E] 对于 VM 和 VMSS，已将“--max-billing”参数重命名为“--max-price”，以便与 Swagger 和 Powershell cmdlet 保持一致
* vm monitor log show：添加了对在链接的 Log Analytics 工作区上查询日志的支持。

### <a name="iot"></a>IOT

* 修复 #2531：为中心更新添加了方便的参数。
* 修复 #8323：添加了缺少的参数以创建存储自定义终结点。
* 修复回归 bug：还原了替代默认存储终结点的更改。

### <a name="key-vault"></a>密钥保管库

* 已修复 #11121：使用 `az keyvault certificate list` 时，传递 `--include-pending` 现在不需要值 `true` 或 `false`

### <a name="netappfiles"></a>NetAppFiles

* 已将 azure-mgmt-netapp 升级到 0.7.0，其中包括其他一些与即将推出的复制操作关联的卷属性

### <a name="network"></a>网络

* application-gateway waf-config：已弃用
* application-gateway waf-policy：添加了 subgroup managed-rules，用于管理托管规则集和排除规则
* application-gateway waf-policy：添加了 subgroup policy-setting，用于管理 waf-policy 的全局配置
* [中断性变更E] application-gateway waf-policy：已将 subgroup rule 重命名为 custom-rule
* application-gateway http-listener：已添加在创建时使用的 --firewall-policy
* application-gateway url-path-map rule：已添加在创建时使用的 --firewall-policy

### <a name="packaging"></a>打包

* 使用 Python 重写了 az wrapper
* 添加了对 Python 3.8 的支持
* 已为 RPM 包更改到 Python 3

### <a name="profile"></a>配置文件

* 修改了使用 Microsoft 帐户运行 `az login -u {} -p {}` 时出现的错误
* 修改了在带有自签名根证书的代理后面运行 `az login` 时出现的 `SSLError`
* 修复了 #10578：在 Windows 或 WSL 上同时启动多个实例时，`az login` 会挂起
* 修复了 #11059：如果租户中有订阅，则 `az login --allow-no-subscriptions` 将失败
* 修复了 #11238：重命名订阅后，使用 MSI 登录将导致同一订阅出现两次

### <a name="rbac"></a>RBAC

* 修复了 #10996：修改了在未指定 `--password` 时 `az ad user update` 中 `--force-change-password-next-login` 的错误

### <a name="redis"></a>Redis

* 修复了 #2902：避免在更新基本 SKU 缓存时设置内存配置

### <a name="reservations"></a>预留

* 已将 SDK 版本升级到 0.6.0
* 添加了在调用 Get-Gatalogs 后提供的 billingplan 详细信息
* 添加了新的命令 `az reservations reservation-order calculate` 以计算预留项的价格
* 添加了新的命令 `az reservations reservation-order purchase` 以购买新的预留项

### <a name="rest"></a>Rest
* 已将 `az rest` 更改为 GA

### <a name="sql"></a>SQL

* 已将 azure-mgmt-sql 更新到版本 0.15.0。

### <a name="storage"></a>存储

* storage account create：添加了 --enable-hierarchical-namespace，以支持 Blob 服务中的文件系统语义。
* 已从错误消息中删除不相关的异常
* 修复了在通过网络规则或 AuthenticationFailed 阻止时不正确地显示错误消息“你没有执行此操作所需的权限。” 的问题。

## <a name="november-4-2019"></a>2019 年 11 月 4 日

版本 2.0.76

### <a name="acr"></a>ACR

* 已将预览版参数 `--pack-image-tag` 添加到命令 `az acr pack build`。
* 添加了对创建注册表时启用审核的支持
* 添加了对存储库范围内的 RBAC 的支持

### <a name="aks"></a>AKS

* 已将 `--enable-cluster-autoscaler`、`--min-count` 和 `--max-count` 添加到 `az aks create` 命令，这将为节点池启用群集自动缩放程序。
* 已将上述标志以及 `--update-cluster-autoscaler` 和 `--disable-cluster-autoscaler` 添加到 `az aks update` 命令，从而允许更新群集自动缩放程序。

### <a name="appconfig"></a>AppConfig

* 添加了 appconfig 功能命令组来管理存储在应用配置中的功能标志。
* 修复了“appconfig kv 导出到文件”命令的小 bug。 在导出过程中停止读取目标文件内容。

### <a name="appservice"></a>应用服务

* `az appservice plan create`：添加了对在 appservice plan create 上设置“persitescaling”的支持。
* 修复了 webapp config ssl bind 操作从资源中删除现有标记的问题
* 为 `az functionapp deployment source config-zip` 添加了 `--build-remote` 标志，以支持在函数应用部署过程中执行远程生成操作。
* 已将函数应用上的默认节点版本更改为 ~10（适用于 Windows）
* 已将 `--runtime-version` 属性添加到 `az functionapp create`

### <a name="arm"></a>ARM

* `az deployment/group deployment validate`：添加了 `--handle-extended-json-format` 参数，以便在部署时支持 json 模板中的多行和注释。
* 已将 azure-mgmt-resource 升级到 2019-07-01

### <a name="backup"></a>备份

* 添加了 AzureFiles 备份支持

### <a name="compute"></a>计算

* `az vm create`：添加了在将加速网络和现有 NIC 一起指定时的警告。
* `az vm create`：添加了 `--vmss` 以指定虚拟机应分配到的现有虚拟机规模集。
* `az vm/vmss create`：添加了映像别名文件的本地副本，以便可以在受限的网络环境中对其进行访问。
* `az vmss create`：添加了 `--orchestration-mode` 以指定规模集如何管理虚拟机。
* `az vm/vmss update`：添加了 `--ultra-ssd-enabled` 以允许更新超级 SSD 设置。
* [重大更改] `az vm extension set`：修复了用户无法使用 `--ids` 在 VM 上设置扩展的 bug。
* 添加了新命令 `az vm image terms accept/cancel/show` 以管理 Azure 市场映像条款。
* 已将 VMAccessForLinux 更新为版本 1.5

### <a name="cosmosdb"></a>CosmosDB

* [重大更改] `az sql container create`：已将 `--partition-key-path` 更改为必需的参数
* [重大更改] `az gremlin graph create`：已将 `--partition-key-path` 更改为必需的参数
* `az sql container create`：添加了 `--unique-key-policy` 和 `--conflict-resolution-policy`
* `az sql container create/update`：已更新 `--idx` 默认架构
* `gremlin graph create`：添加了 `--conflict-resolution-policy`
* `gremlin graph create/update`：已更新 `--idx` 默认架构
* 修复了帮助消息中的拼写错误
* 数据库：添加了弃用信息
* 集合：添加了弃用信息

### <a name="iot"></a>IoT

* 添加了新的路由源类型：DigitalTwinChangeEvents
* 解决了 `az iot hub create` 中缺少功能的问题

### <a name="key-vault"></a>密钥保管库

* 修复了证书文件不存在时出现的意外错误
* 解决了 `az keyvault recover/purge` 不起作用的问题

### <a name="netappfiles"></a>NetAppFiles

* 已将 azure-mgmt-netapp 升级到 0.6.0 以使用 API 版本 2019-07-01。 这个新的 API 版本包括：

    - “卷创建”命令的 `--protocol-types` 现在接受“NFSv 4.1”而不是“NFSv4”
    - 卷导出策略属性现在名为“nfsv41”而不是“nfsv4”
    - 卷 `--creation-token` 已重命名为 `--file-path`
    - 快照创建日期现在仅命名为“created”

### <a name="network"></a>网络

* `az network private-dns link vnet create/update`：支持跨租户虚拟网络链接。
* [重大更改] `az network vnet subnet list`：`--resource-group` 和 `--vnet-name` 现在已更改为必需。
* `az network public-ip prefix create`：添加了对在创建时指定 IP 地址版本（IPv4、IPv6）的支持
* 已将 azure-mgmt-network 升级到 7.0.0，并将 api-version 升级到 2019-09-01
* `az network vrouter`：添加了对新服务虚拟路由器和虚拟路由器对等互连的支持
* `az network express-route gateway connection`：添加了对 `--internet-security` 的支持

### <a name="profile"></a>配置文件

* 解决了 `az account get-access-token --resource-type ms-graph` 不起作用的问题
* 已从 `az login` 中删除警告

### <a name="rbac"></a>RBAC

* 解决了 `az ad app update --id {} --display-name {}` 不起作用的问题

### <a name="servicefabric"></a>ServiceFabric

* `az sf cluster create`：通过将 service fabric linux 和 windows template.json 计算 vmss 从标准修改为托管磁盘解决了问题

### <a name="sql"></a>SQL

* 添加了 `--compute-model`、`--auto-pause-delay` 和 `--min-capacity` 参数，以便对新的 SQL 数据库产品/服务支持 CRUD 操作：无服务器计算模型。

### <a name="storage"></a>存储

* `az storage account create/update`：添加了 --enable-files-adds 参数和 Azure Active Directory 属性参数组，以支持 Azure 文件存储 Active Directory 域服务身份验证
* 扩展了 `az storage account keys list/renew` 以支持列出或重新生成存储帐户的 Kerberos 密钥。

## <a name="october-15-2019"></a>2019 年 10 月 15 日

版本 2.0.75

### <a name="aks"></a>AKS

* 将 `--load-balancer-sku` 默认值更改为 `standard`（如果受 Kubernetes 版本支持）
* 将 `--vm-set-type` 默认值更改为 `virtualmachinescalesets`（如果受 Kubernetes 版本支持）

### <a name="ams"></a>AMS

* [中断性变更E] 已将 `job start` 的名称更改为 `job create`
* [中断性变更E] 已更改 `content-key-policy create` 的 `--ask` 参数，使用 32 字符的十六进制字符串而不是 UTF8

### <a name="appservice"></a>应用服务

* 添加了 `webapp config access-restriction show|set|add|remove` 命令
* 为 `webapp up` 添加了更好的错误处理
* 为 `appservice plan update` 添加了对 `Isolated` SKU 的支持

### <a name="arm"></a>ARM

* 为 `deployment create` 添加了 `--handle-extended-json-format` 参数，以便在 json 模板中支持多行和注释

### <a name="compute"></a>计算

* 为 `vm create` 添加了 `--enable-agent` 参数
* 更改了 `vm create`，可以在使用区域时自动使用标准的公共 IP SKU
* 更改了 `vm create`，可以在未提供任何计算机名称的情况下自动为 VM 创建有效的计算机名称
* 为 `vmss create` 添加了 `--computer-name-prefix` 参数，支持对 VMSS 中的虚拟机使用自定义计算机名称前缀
* 为 `vm create` 添加 `--workspace` 参数，可以自动启用 Log Analytics 工作区
* 已将库 API 版本更新为 2019-07-01

### <a name="core"></a>核心

* 在常规更新命令中为 `--set` 参数添加了语法检查

### <a name="iot"></a>IoT

* 修复了 `iot hub show` 会不正确地产生“找不到资源”错误的问题

### <a name="monitor"></a>监视

* 为 `monitor log-analytics workspace` 添加了对 CRUD 的支持

### <a name="network"></a>网络

* 为 `network private-dns link vnet [create|update]` 添加了对跨租户虚拟链接的支持
* [中断性变更E] 更改了 `network vnet subnet list`，使之需要 `--resource-group` 和 `--vnet-name` 参数

### <a name="sql"></a>SQL

* 为 `sql mi ad-admin` 添加了多个命令，这些命令支持在托管实例上设置 AAD 管理员

### <a name="storage"></a>存储

* 为 `storage copy` 添加了 `--preserve-s2s-access-tier` 参数，在进行服务到服务复制时保留访问层
* 为 `storage account [create|update]` 添加了 `--enable-large-file-share` 参数，支持针对存储帐户的大型文件共享

## <a name="september-24-2019"></a>2019 年 9 月 24 日

版本 2.0.74

### <a name="acr"></a>ACR

* 向 `acr config retention update` 添加了必需的 `--type` 参数
* [中断性变更E] 已将 `acr config` 命令组的重命名参数 `--name -n` 更改为 `--registry -r `

### <a name="aks"></a>AKS

* 向 `aks create` 命令添加了 `--load-balancer-sku` 参数，以便可以创建具有 SLB 的 AKS 群集
* 向 `aks [create|update]` 命令添加了 `--load-balancer-managed-outbound-ip-count`、`--load-balancer-outbound-ips` 和 `--load-balancer-outbound-ip-prefixes` 参数，以便可以使用 SLB 更新 AKS 群集的负载均衡器配置文件
* 向 `aks create` 命令添加了 `--vm-set-type` 参数，以便可以指定 AKS 群集的 VM 类型（vmas 或 vmss）

### <a name="arm"></a>ARM

* 向 `group deployment create` 命令添加了 `--handle-extended-json-format` 参数，以便在 json 模板中支持多行和注释

### <a name="compute"></a>计算

* 向 `vmss [create|update]` 命令添加了 `--terminate-notification-time` 参数，以便支持终止计划事件可配置性
* 向 `vmss update` 命令添加了 `--enable-terminate-notification` 参数，以便支持终止计划事件可配置性
* 向 `[vm|vmss] create` 命令添加了 `--priority,` `--eviction-policy,` `--max-billing` 参数
* 更改了 `disk create` 以允许指定磁盘上传的确切大小
* 向 `snapshot create` 添加了对托管磁盘增量快照的支持

### <a name="cosmos-db"></a>Cosmos DB

* 向 `cosmosdb keys list` 命令添加了 `--type <key-type>` 参数以显示密钥、只读密钥或连接字符串
* 添加了 `cosmosdb keys regenerate` 命令
* [已弃用] 弃用了 `cosmosdb list-connection-strings`、`cosmosdb regenerate-key` 和 `cosmosdb list-read-only-keys` 命令

### <a name="eventgrid"></a>EventGrid

* 修复了终结点帮助文本以引用正确的参数

### <a name="key-vault"></a>密钥保管库

* 修复了使用租户登录 (`login -t`) 可能导致 `keyvault create` 失败的问题

### <a name="monitor"></a>监视

* 修复了 `monitor metrics alert create` 的 `--condition` 参数中不允许使用 `:` 字符的问题

### <a name="policy"></a>策略

* 添加了对策略 API 版本 2019-06-01 的支持
* 向 `policy assignment create` 命令添加了参数 `--enforcement-mode`

### <a name="storage"></a>存储

* 向 `az storage copy` 命令添加了参数 `--blob-type`

## <a name="september-10-2019"></a>2019 年 9 月 10 日

### <a name="acr"></a>ACR

* 添加了命令组 `acr config retention` 以配置保留策略

### <a name="aks"></a>AKS

* 使用以下命令添加了对 ACR 集成的支持：
  * 向 `aks [create|update]` 添加了 `--attach-acr` 参数，以便将 ACR 附加到 AKS 群集
  * 向 `aks update` 添加了 `--detach-acr` 参数，以便将 ACR 与 AKS 群集分离

### <a name="arm"></a>ARM

* 已更新为使用 API 版本 2019-05-10

### <a name="batch"></a>Batch

* 为 `batch pool create` 的 `--json-file` 添加了新的 JSON 配置设置：
  * 为文件系统装载添加了 `MountConfigurations`（有关详细信息，请参阅 https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body ）
  * 为池上公共 IP 的 `NetworkConfiguration` 添加了可选属性 `publicIPs`（有关详细信息，请参阅 https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body ）
* 为 `--image` 添加了对共享映像库的支持
* [中断性变更E] 已将 `batch pool create` 上 `--start-task-wait-for-success` 的默认值更改为 `true`
* [中断性变更E] 已将 `AutoUserSpecification` 上 `Scope` 的默认值更改为始终为“Pool”（在 Windows 节点上为 `Task`，在 Linux 节点上为 `Pool`）
  * 只能使用 `--json-file` 通过 JSON 配置设置此参数

### <a name="hdinsight"></a>HDInsight

* 正式版
* [中断性变更E] 已将 `az hdinsight resize` 的参数 `--workernode-count/-c` 更改为必需。

### <a name="key-vault"></a>密钥保管库

* 修复了无法从网络规则中删除子网的问题
* 修复了可将重复的子网和 IP 地址添加到网络规则的问题

### <a name="network"></a>网络

* 向 `network watcher flow-log` 添加了 `--interval` 参数以设置流量分析间隔值
* 添加了 `network application-gateway identity` 以管理网关标识
* 为 `network application-gateway ssl-cert` 添加了对设置 Key Vault ID 的支持
* 添加了 `network express-route peering peer-connection [show|list]`

### <a name="policy"></a>策略

* 已更新为使用 API 版本 2019-01-01

## <a name="august-27-2019"></a>2019 年 8 月 27 日

版本 2.0.72

### <a name="acr"></a>ACR

* [中断性变更E] 删除了对 `classic` SKU 的支持

### <a name="api-management"></a>API 管理

* [预览版] 添加了 `apim` 命令组

### <a name="appservice"></a>应用服务

* 修复了指定槽时 `webapp webjob continuous start` 命令的问题
* 更改了 `webapp up` 以检测 `env` 文件夹并将其从用于部署的文件中删除

### <a name="keyvault"></a>KeyVault

* 修复了 `keyvault secret set` 中忽略 `--expires` 参数的 bug

### <a name="network"></a>网络

* 为 `--private-ip-address-version` 参数添加了对 IPv6 地址的支持
* 添加了新命令 `network private-endpoint [create|update|list-types]` 以用于专用终结点管理
* 添加了 `network private-link-service` 命令组
* 为 `network vnet subnet update` 添加了 `--private-endpoint-network-policies` 和 `--private-link-service-network-policies` 参数

### <a name="rbac"></a>RBAC

* 修复了`ad app update --homepage` 的以下问题：无法更新主页

### <a name="servicefabric"></a>ServiceFabric

* 添加了对混合大小写 Key Vault 名称的支持
* 修复了在 Key Vault 中使用证书时的问题
* 修复了使用 PFX 证书文件时的问题
* 修复了未指定 Key Vault 资源组时 `sf cluster certificate add` 的问题
* 修复了 `sf cluster set` 不起作用的问题

### <a name="signalr"></a>SignalR

* 添加了新命令：
  * `signalr cors`：管理 SignalR CORS
  * `signalr restart`：重新启动 SignalR 服务
  * `signalr update`：更新 SignalR 服务
* 为 `signalr create` 添加了 `--service-mode` 参数

### <a name="storage"></a>存储

* 添加了 `storage account revoke-delegation-keys` 命令

## <a name="august-13-2019"></a>2019 年 8 月 13 日

版本 2.0.71

### <a name="appservice"></a>应用服务

* 修复了 `webapp webjob continuous` 命令因插槽失败的问题

### <a name="botservice"></a>BotService

* [中断性变更E] 删除了对创建 v3 SDK 机器人的支持

### <a name="cognitiveservices"></a>认知服务

* 添加了 `cognitiveservices account network-rule` 命令

### <a name="cosmos-db"></a>Cosmos DB

* 删除了更新多个写入位置时的警告
* 为 CosmosDB SQL、MongoDB、Cassandra、Gremlin 和表资源以及资源的吞吐量添加了 CRUD 命令

### <a name="hdinsight"></a>HDInsight

此版本包含大量中断性变更。

* [中断性变更E] 重命名了 `hdinsight create` 的参数：
  * 已将 `--storage-default-container` 重命名为 `--storage-container`
  * 已将 `--storage-default-filesystem` 重命名为 `--storage-filesystem`
* [中断性变更E] 更改了 `application create` 的 `--name` 参数，以表示应用程序名称而不是群集名称
* 向 `application create` 添加了 `--cluster-name` 参数以替换旧的 `--name` 功能
* [中断性变更E] 重命名了 `application create` 的参数：
  * 已将 `--application-type` 重命名为 `--type`
  * 已将 `--marketplace-identifier` 重命名为 `--marketplace-id`
  * 已将 `--https-endpoint-access-mode` 重命名为 `--access-mode`
  * 已将 `--https-endpoint-destination-port` 重命名为 `--destination-port`
* [中断性变更E] 删除了 `application create` 的参数：
  * `--https-endpoint-location`
  * `--https-endpoint-public-port`
  * `--ssh-endpoint-destination-port`
  * `--ssh-endpoint-location`
  * `--ssh-endpoint-public-port`
* [中断性变更] 已将 `hdinsight resize` 的 `--target-instance-count` 重命名为 `--workernode-count`
* [中断性变更E] 更改了 `hdinsight script-action` 组中的所有命令，以使用 `--name` 参数作为脚本操作的名称。
* 为所有 `hdinsight script-action` 命令添加了 `--cluster-name` 参数以替换旧的 `--name` 功能
* [中断性变更E] 已将所有 `hdinsight script-action` 命令的 `--script-execution-id` 重命名为 `--execution-id`
* [中断性变更E] 已将 `hdinsight script-action show` 重命名为 `hdinsight script-action show-execution-details`
* [中断性变更] 已将参数更改为 `hdinsight script-action execute --roles`（以空格分隔，而不是以逗号分隔）
* [中断性变更E] 删除了 `hdinsight script-action list` 的 `--persisted` 参数
* 更改了 `hdinsight create --cluster-configurations` 参数以接受本地 JSON 文件或 JSON 字符串的路径
* 添加了命令 `hdinsight script-action list-execution-history`
* 更改了 `hdinsight monitor enable --workspace` 以接受 Log Analytics 工作区 ID 或工作区名称
* 添加了 `hdinsight monitor enable --primary-key` 参数，方便以参数的形式提供工作区 ID
* 添加了更多示例并更新了帮助消息的说明

### <a name="interactive"></a>交互

* 修复了加载错误

### <a name="kubernetes"></a>Kubernetes

* 更改为使用 `https`（如果仪表板容器端口正在使用 `https`）

### <a name="network"></a>网络

* 为 `network dns record-set cname delete` 添加了 `--yes` 参数

### <a name="profile"></a>配置文件

* 向 `account get-access-token` 添加了 `--resource-type` 参数以获取资源访问令牌

### <a name="servicefabric"></a>ServiceFabric

* 为 sf cluster create 添加了所有支持的 os 版本
* 修复了主要证书验证 bug

### <a name="storage"></a>存储

* 添加了命令 `storage copy`

## <a name="july-30-2019"></a>2019 年 7 月 30 日

版本 2.0.70

### <a name="acr"></a>ACR

* 修复了问题 #9952（`acr pack build` 命令中的回归）
* 删除了 `acr pack build` 中的默认生成器映像名称

### <a name="appservice"></a>应用服务

* 更改了 `webapp config ssl` 以在找不到资源时显示一条消息
* 修复了 `functionapp create` 不接受 `Standard_RAGRS` 存储帐户类型的问题
* 修复了使用较旧版本的 python 运行时 `webapp up` 会失败的问题

### <a name="network"></a>网络

* 从 `network nic ip-config add` 中删除了无效参数 `--ids`（修复 #9861）
* 修复 #9604。 向 `network application-gateway http-settings [create|update]` 添加了 `--root-certs` 参数以支持用户关联受信任的根证书。
* 修复了 `network dns record-set ns create` 的 `--subscription` 参数 (#9965)

### <a name="rbac"></a>RBAC

* 添加了 `user update` 命令
* [已弃用] 已在用户相关命令中弃用了 `--upn-or-object-id`
    * 使用替换参数 `--id`
* 向用户相关命令中添加了 `--id` 参数

### <a name="sql"></a>SQL

* 为托管实例密钥和 TDE 保护程序添加了管理命令

### <a name="storage"></a>存储

* 添加了 `storage remove` 命令
* 修复了 `storage blob update` 的问题

### <a name="vm"></a>VM

* 已将 `list-skus` 更改为使用较新的 api 版本来输出区域详细信息
* 已为 `vmss create` 将 `--single-placement-group` 的默认值更改为 `false`
* 为 `[snapshot|disk] create` 添加了选择 ZRS 存储 SKU 的功能
* 添加了新的命令组 `vm host` 以支持专用主机
* 在 `vm create` 上添加了参数 `--host` 和 `--host-group` 以设置 VM 专用主机

## <a name="july-16-2019"></a>2019 年 7 月 16 日

版本 2.0.69

### <a name="appservice"></a>应用服务

* 更改了 `webapp identity` 命令，在 ResourceGroupName 或应用名称无效的情况下，现在可以返回适当的错误消息
* 修复了 `webapp list`，在不提供 ResourceGroup 的情况下，现在可以返回 numberOfSites 的正确值
* 纠正了 `appservice plan create` 和 `webapp create` 的副作用

### <a name="core"></a>核心

* 修复了 `--subscription` 不管是否适用都会出现的问题

### <a name="batch"></a>Batch

* [中断性变更E] 已将 `batch pool node-agent-skus list` 替换为 `batch pool supported-images list`
* 添加了对安全规则的支持。在使用 `batch pool create network` 的 `--json-file` 选项时，这些规则会根据流量的源端口阻止对池的网络访问
* 添加了对执行任务的支持。使用 `batch task create` 的 `--json-file` 选项时，可以在容器工作目录或 Batch 任务工作目录中执行任务
* 修复了 `batch pool create` 的 `--application-package-references` 选项中存在的只对默认设置有效的错误

### <a name="eventhubs"></a>Eventhubs

* 添加了对 `authorizationrule` 命令的参数 `--rights` 的验证

### <a name="rdbms"></a>RDBMS

* 添加了 create replica 命令的可选参数，用于指定副本 SKU
* 修复了创建 MySQL 副本时 CI 测试失败的问题

### <a name="relay"></a>中继

* 修复了在禁用客户端授权时混合连接出现的问题 [#8775](https://github.com/azure/azure-cli/issues/8775)
* 为 `relay wcfrelay create` 增加了参数 `--requires-transport-security`

### <a name="servicebus"></a>Servicebus

* 添加了对 `authorizationrule` 命令的参数 `--rights` 的验证

### <a name="storage"></a>存储

* 支持 Files AADDS 进行存储帐户更新
* 修复了问题 `storage blob service-properties update --set`

## <a name="july-2-2019"></a>2019 年 7 月 2日

版本 2.0.68

### <a name="core"></a>核心

* 命令模块现已合并到单个 Python 可发行组件包中。 此版本不建议在 PyPI 上直接使用许多 `azure-cli-` 包。
  这应该会减少安装大小，并且只影响通过 `pip` 直接安装的用户。

### <a name="acr"></a>ACR

* 为任务添加了对计时器触发器的支持

### <a name="appservice"></a>应用服务

* 更改了 `functionapp create` 以默认启用 application insights
* [中断性变更E] 删除了已弃用的 `functionapp devops-build` 命令。
  *  改用新命令 `az functionapp devops-pipeline`
* 为 `functionapp deployment config-zip` 添加了 Linux 消耗计划函数应用计划支持

### <a name="cosmos-db"></a>Cosmos DB

* 添加了对禁用 TTL 的支持

### <a name="dls"></a>DLS

* 更新了 ADLS 版本 (0.0.45)

### <a name="feedback-command"></a>反馈命令

* 报告失败的扩展命令时，`az feedback` 现在尝试从索引打开浏览器到扩展的项目/存储库 url

### <a name="hdinsight"></a>HDInsight

* [中断性变更E] 将 `oms` 命令组名称更改为 `monitor`
* [中断性变更E] 使 `--http-password/-p` 成为必需参数 
* 为 `--cluster-admin-account` 和 `cluster-users-group-dns` 参数添加了补全选项 
* 将 `cluster-users-group-dns` 参数更改为在 `—esp` 存在时为必需参数
* 为所有现有参数自动补全选项添加了超时
* 为将资源名称转换为资源 ID 添加了超时
* 更改了自动补全选项以从任何资源组中选择资源。 它可以是与使用 `-g` 指定的资源组不同的资源组
* 在 `hdinsight application create` 命令中添加了对 `--sub-domain-suffix` 和 `--disable_gateway_auth` 参数的支持

### <a name="managed-services"></a>托管服务

* 在预览版中引入托管服务命令模块

### <a name="profile"></a>配置文件
* 取消注销命令的 `--subscription` 参数

### <a name="rbac"></a>RBAC

* [中断性变更E] 删除了 `create-for-rbac` 的 `--password` 参数
* 向 `create` 命令添加了 `--assignee-principal-type` 参数，以避免由 AAD 图形服务器复制延迟导致的间歇性故障
* 修复了列出拥有的对象时 `ad signed-in-user` 中的崩溃
* 修复了 `ad sp` 无法从服务主体中找到正确的应用程序的问题

### <a name="rdbms"></a>RDBMS

* 添加了对 MariaDB 复制的支持

### <a name="sql"></a>SQL

* 记录了 `sql db create --sample-name` 的允许值

### <a name="storage"></a>存储

* 使用 `--as-user` 向 `storage blob generate-sas` 添加了用户委托 SAS 令牌支持 
* 使用 `--as-user` 向 `storage container generate-sas` 添加了用户委托 SAS 令牌支持 

### <a name="vm"></a>VM

* 修复了 `vmss create` 在使用 `--no-wait` 运行时返回错误消息的 bug
* 删除了 `vmss create --single-placement-group` 的客户端验证。 如果 `--single-placement-group` 设置为 `true` 且 `--instance-count` 大于 100 或指定了可用性区域，则不会失败，但会将此验证留给计算服务
* 修复了与 `--latest` 一起使用时 `[vm|vmss] extension image list` 失败的 bug


## <a name="june-18-2019"></a>2019 年 6 月 18 日

版本 2.0.67

### <a name="core"></a>核心

此版本引入了新的 [Preview] 标记，以便在命令组、命令或参数处于预览状态时将相关信息更清楚地传达给客户。 以前，这是在帮助文本中说明的，或通过命令模块版本号隐式传达。
CLI 将在未来删除各个包的版本号。 如果命令处于预览状态，它的所有参数也是如此。 如果命令组标记为处于预览状态，那么所有命令和参数也都被视为处于预览状态。

由于此更改，多个命令组可能似乎“突然”在此版本中显示为处于预览状态。 实际情况是大多数包以前处于预览状态，但在此版本中被认为已正式发布

### <a name="acr"></a>ACR
* 添加了 'acr check-health' 命令
* 改进了针对 AAD 令牌以及用于检索外部命令的错误处理

### <a name="acs"></a>ACS
* 弃用的 ACS 命令现已在帮助视图中隐藏

### <a name="ams"></a>AMS
* [中断性变更E] 对于 archive-window-length 和 key-frame-interval-duration，已更改为返回 ISO 8601 时间字符串

### <a name="appservice"></a>应用服务
* 为 `webapp deleted list` 和 `webapp deleted restore` 添加了基于位置的路由功能
* 修复了 webapp up 记录的目标 URL（“可以启动应用...”）在 Azure Cloud Shell 中不可单击的问题
* 修复了使用某些 SKU 创建应用时失败且出现 AlwaysOn 错误的问题
* 为 `[appservice|webapp] create` 添加了预验证
* 修复了 `[webapp|functionapp] traffic-routing` 以使用正确的 actionHostName
* 为 `functionapp` 命令添加了槽支持

### <a name="batch"></a>Batch
* 修复了由共享密钥身份验证功能过度报告错误引起的 AAD 身份验证回归问题

### <a name="batchai"></a>BatchAI
* BatchAI 命令现在已弃用并隐藏

### <a name="botservice"></a>BotService
* 为支持 v3 SDK 的命令添加了“已停止的支持”/“维护模式”警告消息

### <a name="cosmosdb"></a>CosmosDB
* [已弃用] 已弃用 `cosmosdb list-keys` 命令
* 添加了 `cosmosdb keys list` 命令 - 替代 `cosmosdb list-keys`
* `cosmsodb create/update`：为 --location 添加了新格式，以允许设置 "isZoneRedundant" 属性。 已弃用旧格式

### <a name="eventgrid"></a>EventGrid
* 为域 CRUD 操作添加了 `eventgrid domain` 命令
* 为域主题 CRUD 操作添加了 `eventgrid domain topic` 命令
* 为 `eventgrid [topic|event-subscription] list` 添加了 `--odata-query` 参数以便使用 OData 语法筛选结果
* `event-subscription create/update`：添加了 servicebusqueue 作为 `--endpoint-type` 参数的新值
* [中断性变更E] 删除了对带有 `eventgrid event-subscription [create|update]` 的 `--included-event-types All` 的支持

### <a name="hdinsight"></a>HDInsight
* 在 `hdinsight create` 命令中添加了对 `--ssh-public-key` 参数的支持

### <a name="iot"></a>IoT
* 添加了支持以重新生成授权策略密钥
* 添加了对 DigitalTwin 存储库预配服务的支持以及相应的 SDK

### <a name="network"></a>网络
* 添加了对 Nat 网关使用区域的支持
* 添加了命令 `network list-service-tags`
* 已修复 `dns zone import` 的以下问题：用户无法导入通配符 A 记录
* 已修复 `watcher flow-log configure` 的以下问题：流日志记录无法在特定区域中启用

### <a name="resource"></a>资源
* 添加了 `az rest` 命令以用于进行 REST 调用
* 修复了将 `policy assignment list` 与资源组或订阅级别 `--scope` 配合使用时的错误

### <a name="servicebus"></a>ServiceBus
* 修复了 `servicebus topic create --max-size` 的问题 [9319](https://github.com/azure/azure-cli/issues/9319)

### <a name="sql"></a>SQL
* 已为 `sql [server|mi] create` 将 `--location` 更改为可选 - 如果未指定，则使用资源组位置
* 为 `sql db list-editions --available` 修复了“'NoneType' 对象不可迭代”错误

### <a name="sqlvm"></a>SQLVm
* [中断性变更] 已将 `sql vm create` 更改为需要 `--license-type` 参数
* 已更改为允许在创建或更新 sql vm 时设置 SQL 映像 SKU

### <a name="storage"></a>存储
* 为 `storage container generate-sas` 修复了缺少帐户密钥的问题
* 修复了 Linux 上 `storage blob sync` 的问题

### <a name="vm"></a>VM
* [预览] 添加了 `vm image template` 命令以生成 VM 映像

## <a name="june-4-2019"></a>2019 年 6 月 4 日

版本 2.0.66

### <a name="core"></a>核心
* 修复了在 `--output yaml` 与 `--query` 一起使用时命令失败的 bug

### <a name="acr"></a>ACR
* 添加了用于使用 Buildpack 创建快速生成任务的 'acr pack' 命令组。

### <a name="acs"></a>ACS
* 允许启用/禁用 AKS kube-dashboard 加载项
* 在订阅未列入可使用 Azure Red Hat OpenShift 的允许列表时输出友好消息

### <a name="batch"></a>Batch
* 改进了在未登录到帐户时的错误处理 \[[#9165](https://github.com/Azure/azure-cli/issues/9165)\]\[[#8978](https://github.com/Azure/azure-cli/issues/8978)\]

### <a name="iot"></a>IoT
* 添加了对手动故障转移的支持

### <a name="network"></a>网络
* 添加了 `network application-gateway waf-policy` 命令以支持自定义 WAF 规则。
* 为 `network application-gateway [create|update]` 添加了 `--waf-policy` 和 `--max-capacity` 参数 

### <a name="resource"></a>资源
* 改进了在没有任何 TTY 可用时来自 `deployment create` 的错误消息

### <a name="role"></a>角色
* 更新了帮助文本。

### <a name="compute"></a>计算
* 对于来自数据磁盘 LUN 未从 0 开始或跳过编号的托管映像的 VM，为 `vm create` 添加了对它们的支持

## <a name="may-21-2019"></a>2019 年 5 月 21 日

版本 2.0.65

### <a name="core"></a>核心
* 为身份验证错误添加了更好的反馈
* 修复了 CLI 会加载与其核心版本不兼容的扩展的问题
* 修复了在 `clouds.config` 已损坏时的启动问题

### <a name="acr"></a>ACR
* 为任务添加了对托管标识的支持

### <a name="acs"></a>ACS
* 修复了与客户 AAD 客户端一起使用时 `openshift create` 命令存在的问题

### <a name="appservice"></a>应用服务
* [已弃用] 已弃用 `functionapp devops-build` 命令 - 将在下一版本中删除
* 已将 `functionapp devops-pipeline` 更改为以详细模式从 Azure DevOps 提取生成日志
* [中断性变更E] 从 `functionapp devops-pipeline` 命令中删除了 `--use_local_settings` 标记 - 是 no-op
* 已将 `webapp up` 更改为在不使用 `--logs` 时返回 JSON 输出
* 为 `webapp up` 添加了对将默认资源写入到本地配置的支持
* 为 `webapp up` 添加了对在不使用 `--location` 参数的情况下重新部署应用的支持
* 修复了以下问题：创建 Linux Free SKU ASP 时，使用 Free 作为 SKU 值不起作用

### <a name="botservice"></a>BotService
* 已更改为允许命令参数 `--lang` 的所有大小写
* 已更新命令模块的说明

### <a name="consumption"></a>消耗
* 添加了运行 `consumption usage list --billing-period-name` 时缺少的必需参数

### <a name="iot"></a>IoT
* 添加了对列出所有密钥的支持

### <a name="network"></a>网络
* [中断性变更E]: Removed `network interface-endpoints` command group - use `network private-endpoints` 
* 为 `network vnet subnet [create|update]` 添加了 `--nat-gateway` 参数以用于连接到 NAT 网关
* 已修复 `dns zone import` 的以下问题：记录名称无法匹配记录类型

### <a name="rdbms"></a>RDBMS
* 为异地复制添加了 postgres 和 mysql 支持

### <a name="rbac"></a>RBAC
* 为 `role assignment` 添加了对管理组范围的支持

### <a name="storage"></a>存储
* `storage blob sync`：添加用于存储 blob 的同步命令

### <a name="compute"></a>计算
* 为 `vm create` 添加了 `--computer-name` 以用于设置 VM 的计算机名称
* 已为 `[vm|vmss] create` 将 `--ssh-key-value` 重命名为 `--ssh-key-values` - 现在可以接受多个 ssh 公钥值或路径
  * __注意__：这**不**是一项中断性变更 - `--ssh-key-value` 将被正确分析，因为它仅匹配 `--ssh-key-values`
* 已将 `ppg create` 的 `--type` 参数更改为可选

## <a name="may-6-2019"></a>2019 年 5 月 6 日

版本 2.0.64

### <a name="acs"></a>ACS
* [中断性变更E] 已从 `openshift` 命令中删除了 `--fqdn` 标记
* 已更改为使用 Azure Red Hat Openshift GA API 版本
* 为 `openshift create` 添加了 `customer-admin-group-id` 标志
* [GA] 已从 `aks create` 选项 `--network-policy` 中删除了 `(PREVIEW)`

### <a name="appservice"></a>应用服务
* [已弃用] 已弃用 `functionapp devops-build` 命令
  * 已重命名为 `functionapp devops-pipeline`
* 修复了获取 cloudshell 的正确用户名时导致 `webapp up` 失败的问题
* 更新了 `appservice plan --sku` 文档，以反映支持的 appserviceplans
* 为 `webapp up` 添加了资源组和计划的可选参数
* 添加了 `webapp ssh` 的支持，以遵循 `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION` 环境变量
* 添加了对 Linux 免费 SKU 的 `appserviceplan create` 支持
* 已更改 `webapp up`，以便在设置 `SCM_DO_BUILD_DURING_DEPLOYMENT=true` appsetting 来处理 kudu 冷启动后休眠 30 秒
* 为 Windows 上的 `functionapp create` 添加了 `powershell` 运行时支持
* 添加了 `create-remote-connection` 命令

### <a name="batch"></a>Batch
* 为 `--application-package-references` 选项修复了验证程序中的 bug

### <a name="botservice"></a>Botservice
* [中断性变更E] 已将 `bot create -v v4 -k webapp` 更改为在默认情况下创建空的 Web 应用机器人（即没有机器人部署到应用服务）
* 为 `bot create` 添加了 `--echo` 标记，以便将旧有行为与 `-v v4` 配合使用
* [中断性变更E] 已将 `--version` 的默认值更改为 `v4`
  * __注意：__ `bot prepare-publish` 仍会使用其旧的默认值
* [中断性变更E] 已将 `--lang` 更改为不再默认为 `Csharp`。 如果该命令需要 `--lang` 但它未被提供，该命令现在会引发错误并退出执行
* [中断性变更E] 已将 `bot create` 的参数 `--appid` 和`--password` 更改为必需，并且现在可通过 `ad app create` 创建
* 添加了 `--appid` 和 `--password` 验证
* [中断性变更E] 已将 `bot create -v v4` 更改为不创建或使用存储帐户或 Application Insights
* [中断性变更E] 已将 `bot create -v v3` 更改为要求一个可以使用 Application Insights 的区域
* [中断性变更E] 已将 `bot update` 更改为现在仅影响机器人的特定属性
* [中断性变更E] 更改了 `--lang` 标志以接受 `Javascript` 而不是 `Node`
* [中断性变更E] 已不再将 `Node` 作为允许的 `--lang` 值
* [中断性变更E] 已将 `bot create -v v4 -k webapp` 更改为不再将 `SCM_DO_BUILD_DURING_DEPLOYMENT` 设置为 true。 通过 Kudu 进行的所有部署将都根据其默认行为执行
* 对于不带 `.bot` 文件的机器人，已将 `bot download` 更改为使用机器人的“应用程序设置”中的值创建特定于语言的配置文件
* 为 `bot prepare-deploy` 添加了 `Typescript` 支持
* 对于 `--code-dir` 不包含 `package.json` 的情况，已为 `Javascript` 和 `Typescript` 机器人将警告消息添加到 `bot prepare-deploy`
* 已将 `bot prepare-deploy` 更改为在成功时返回 `true`
* 为 `bot prepare-deploy` 添加了详细日志记录
* 为 `az bot create -v v3` 添加了更多的可用 Application Insights 区域

### <a name="configure"></a>配置
* 添加了对基于文件夹的参数默认值配置的支持

### <a name="eventhubs"></a>Eventhubs
* 添加了 `namespace network-rule` 命令
* 为 `namespace [create|update]` 添加了网络规则的参数 `--default-action`

### <a name="network"></a>网络
* [中断性变更E] 已将 `vnet [create|update]` 的参数 `--cache` 替换为 `--defer` 

### <a name="policy-insights"></a>策略见解
* 添加了对 `--expand PolicyEvaluationDetails` 的支持，以便查询有关资源的策略评估详细信息

### <a name="role"></a>角色
* [已弃用] 已更改 `create-for-rbac` 以隐藏“--password”参数 - 2019 年 5 中将终止支持

### <a name="service-bus"></a>服务总线
* 添加了 `namespace network-rule` 命令
* 为 `namespace [create|update]` 添加了网络规则的参数 `--default-action`
* 修复了 `topic [create|update]` 以允许对 `--max-size` 的支持，从而可以将 10、20、40 和 80GB 值用于高级 SKU

### <a name="sql"></a>SQL
* 添加了 `sql virtual-cluster [list|show|delete]` 命令

### <a name="vm"></a>VM
* 为 `vmss update` 添加了 `--protect-from-scale-in` 和 `--protect-from-scale-set-actions`，以启用对 VMSS VM 实例保护策略的更新
* 为 `vmss update` 添加了 `--instance-id` 以启用 VMSS VM 实例的通用更新
* 为 `vmss wait` 添加了 `--instance-id`
* 添加了新的 `ppg` 命令组以用于管理邻近放置组
* 为 `[vm|vmss] create` 和 `vm availability-set create` 添加了 `--ppg` 以用于管理 PPG
* 为 `image create` 添加了 `--hyper-v-generation` 参数

## <a name="april-23-2019"></a>2019 年 4 月 23 日

版本 2.0.63

### <a name="acs"></a>ACS
* 更改了 `aks get-credentials` 以提示是否覆盖复制的值
* 从 Dev Spaces 命令“aks use-dev-spaces”和“aks remove-dev-spaces”中删除了 `(PREVIEW)`

### <a name="ams"></a>AMS
* 修复了有关资产和帐户筛选器更新的 bug

### <a name="appservice"></a>应用服务
* 为 `webapp ssh` 添加了 ASE 和超时支持
* 添加了在 Azure DevOps 管道中建立从 Github 存储库到函数应用的 CI CD 的支持
* 为 `functionapp devops-build create` 添加了 `--github-pat` 参数，以接受 Github 个人访问令牌
* 为 `functionapp devops-build create` 添加了 `--github-repository` 参数，以接受包含 functionapp 源代码的 Github 存储库
* 修复了 `az webapp up --logs` 失败并出现错误的问题；默认的 .NETCORE 版本将更新为 2.1
* 删除了使用消耗计划创建函数应用时不必要的 functionapp 设置
* 更改了 `webapp up`，以便在默认 asp 字符串的末尾追加数字，以基于 SKU 选项创建新的 ASP
* 添加了 `-b` 作为 `webapp up` 的一个选项，用于在浏览器中启动应用
* 更改了 `webapp deployment source config zip` 以处理 `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION` 环境变量

### <a name="deployment-manager"></a>部署管理器
* [预览] 创建和管理支持实施的项目

### <a name="lab"></a>实验室
* 修复了导致提前退出的 bug

### <a name="network"></a>网络
* 为 `dns zone create` 添加了自动名称服务器委托，以在创建子区域期间在父级中使用

### <a name="resource"></a>资源
* [已弃用] 已弃用 `resource link` 的 `--link-id`、`--target-id` 和 `--filter-string` 参数
  * 改用 `--link`、`--target` 和 `--filter` 参数
* 修复了 `resource link [create|update]` 命令无法正常运行的问题
* 修复了使用资源 ID 删除出错时可能导致崩溃的问题

### <a name="sql"></a>SQL
* 在托管实例上添加了对自定义时区的支持
* 已更改为允许对 `sql db update` 使用弹性池名称
* 为 `sql server [create|update]` 添加了 `--no-wait` 支持
* 添加了命令 `sql server wait`

### <a name="storage"></a>存储
* 修复了 `storage blob generate-sas` 中双重编码 SAS 令牌的问题

### <a name="vm"></a>VM
* 已将 `--skip-shutdown` 标志添加到 `vm|vmss stop`，以便在不关机的情况下关闭 VM
* 已将 `--storage-account-type` 参数添加到 `sig image-version create`，用于设置发布配置文件的帐户类型
* 已将 `--target-regions` 参数添加到 `sig image-version create`，以允许设置特定于区域的存储帐户类型

## <a name="april-9-2019"></a>2019 年 4 月 9 日

### <a name="core"></a>核心
* 修复了某些扩展显示了版本 `Unknown` 并且无法更新的问题

### <a name="acr"></a>ACR
* 增加了对无休止运行某个映像的支持

### <a name="ams"></a>AMS
* [已弃用]: Deprecated the `--bitrate` parameter of `account-filter` and `asset-filter`
* [重大更改]: Renamed the `--bitrate` parameter to `--first-quality`
* 在 `ams streaming-policy create` 中添加了新的加密参数支持
* 已将新参数 `--filters` 添加到 `ams streaming-locator create`

### <a name="appservice"></a>应用服务
* 为 `webapp up` 添加了 `--logs` 支持
* 修复了 `functionapp devops-build create` 命令 `azure-pipelines.yml` 生成问题
* 改进了 `unctionapp devops-build create` 错误处理和指示
* [中断性变更E] 删除了 `devops-build` 命令的 `--local-git` 标志，创建 Azure DevOps 管道时，必须进行本地 Git 检测和处理
* 添加了对创建 Linux 函数计划的支持
* 添加了使用 `functionapp update --plan` 在函数应用下切换计划的功能
* 添加了对 Azure Functions 高级计划横向扩展设置的支持

### <a name="cdn"></a>CDN
* 添加了对 `Microsoft_Standard` 和 `Standard_ChinaCdn` 的支持

### <a name="feedback-command"></a>反馈命令
* 更改了 `feedback` 以显示最近运行的命令的元数据
* 更改了 `feedback` 以提示用户通过打开浏览器并使用问题模板来帮助创建问题
* 更改了 `feedback` 以在使用“--verbose”运行时输出问题正文

### <a name="monitor"></a>监视
* 修复了 `metrics alert [create|update]` 中“count”不是允许的值的问题 

### <a name="network"></a>网络
* 修复了使用 `vnet-gateway list-bgp-peer-status` 时不显示表格式的问题
* 已将 `list-request-headers` 和 `list-response-headers` 命令添加到 `application-gateway rewrite-rule`
* 已将 `list-server-variables` 命令添加到 `application-gateway rewrite-rule condition`
* 修复了更新 express-route 端口上的链路状态将引发未知属性异常 `express-route port update` 的问题

### <a name="privatedns"></a>PrivateDNS
* 针对专用 DNS 区域添加了 `network private-dns`

### <a name="resource"></a>资源
* 修复了 `deployment create` 和 `group deployment create` 中具有空的参数集的参数文件不工作的问题

### <a name="role"></a>角色
* 修复了 `create-for-rbac` 以正确处理 `--years`
* [中断性变更E] 更改了 `role assignment delete` 以在无条件地删除订阅下的所有分配时进行提示

### <a name="sql"></a>SQL
* 更新了 `sql mi [create|update]` 的属性 proxyOverride 和 publicDataEndpointEnabled

### <a name="storage"></a>存储
* [中断性变更E] 删除了 `storage blob delete` 的结果
* 向 `storage blob generate-sas` 添加了 `--full-uri` 来通过 sas 创建 blob 的完整 RUI
* 向 `storage file copy start` 添加了 `--file-snapshot` 来从快照复制文件
* 更改了 `storage blob copy cancel` 以仅显示 NoPendingCopyOperation 的错误而不显示异常

## <a name="march-26-2019"></a>2019 年 3 月 26 日


### <a name="core"></a>核心
* 修复了 dev 扩展不兼容的问题
* 错误处理现在会将客户引导到问题页

### <a name="cloud"></a>云
* 修复了 `cloud set` 中的“找不到订阅”错误

### <a name="acr"></a>ACR
* 修复了映像导入中的冗余源
* 向 `acr build`、`acr run`、`acr task create` 和 `acr task update` 命令添加了 `--auth-mode`
* 添加了“acr task credential”命令组，用于管理任务的凭据
* 向 `acr build` 命令添加了“--no-wait”

### <a name="appservice"></a>应用服务
* 修复了 Bug：`webapp up` 无法正确处理空目录或未知代码方案的运行
* 修复了槽不适用于 `[webapp|functionapp] config ssl bind` 的 Bug

### <a name="bot-service"></a>机器人服务
* 添加了 `bot prepare-deploy`，为通过 `webapp` 部署机器人做准备
* 更改了 `bot create --kind registration`，可以在未提供密码的情况下显示密码
* [中断性变更E] 更改了 `bot create --kind registration` 中的 `--endpoint`，使之默认设置为空字符串而不是必需项
* 为 v4 Web 应用机器人的 ARM 模板的应用程序设置添加了 `SCM_DO_BUILD_DURING_DEPLOYMENT`

### <a name="cdn"></a>CDN
* 在 `cdn endpoint [create|update|start|stop|delete|load|purge]` 中添加了对 `--no-wait` 的支持  
* [中断性变更E]：更改了 `cdn endpoint create` 的默认查询字符串缓存行为。 不再默认设置为“IgnoreQueryString”。 它现在由服务设置

### <a name="cosmosdb"></a>Cosmosdb
* 在帐户更新时添加了对 `--enable-multiple-write-locations` 的支持
* 为 `add`、`remove` 和 `list` 命令添加了 `network-rule` 子组以用于管理 Cosmos DB 帐户的 VNET 规则

### <a name="interactive"></a>交互
* 修复了与通过 azdev 安装的交互式扩展不兼容的问题

### <a name="monitor"></a>监视
* 更改后允许对 `monitor metrics alert [create|update]` 使用维度值 `*`

### <a name="network"></a>网络
* 向 `application-gateway` 添加了 `rewrite-rule` 命令组

### <a name="profile"></a>配置文件
* 向 `login` 添加了针对托管服务标识的租户级别帐户支持

### <a name="postgres"></a>Postgres 
* 添加了 postgresql `replica` 命令和 `restart server` 命令
* 更改后，可以从资源组获取默认位置（如果没有为创建服务器而提供此项），并为保留天数添加验证

### <a name="resource"></a>资源
* 改进了 `deployment [create|list|show]` 的表输出
* 修复了 `deployment [create|validate]` 的无法识别类型 secureObject 的问题

### <a name="graph"></a>图形
* 在 `ad [app|sp] credential reset` 中添加了对 `--end-date` 的支持
* 增加了相关支持，可以通过 `ad app permission add` 添加权限
* 修复了没有权限时 `ad app permission list` 出现的 Bug
* 更改了 `ad sp delete`，可以在当前帐户没有订阅的情况下跳过角色分配删除操作
* 更改了 `ad app create`，可以在未提供值的情况下将 `--identifier-uris` 默认设置为空列表

### <a name="storage"></a>存储
* 向 `storage file download-batch` 添加了 `--snapshot`，可以从共享快照下载内容
* 更改了 `storage blob [download-batch|upload-batch]` 进度栏，使之更简洁并可指示当前 Blob 的情况
* 修复了更新加密参数时 `storage account update` 出现的问题
* 修复了使用 oauth (`--auth-mode=login`) 时 `storage blob show` 会失败的问题

### <a name="vm"></a>VM
* 添加了 `image update` 命令

## <a name="march-12-2019"></a>2019 年 3 月 12 日

版本 2.0.60

### <a name="core"></a>核心

* 修复了 `cloud set` 中有关找不到订阅的不正确错误

### <a name="acr"></a>ACR

* 修复了映像导入中的冗余源

### <a name="acs"></a>ACS

* 已更改为当 kubectl 不支持 `aks browse` 的 `--listen-address` 参数时忽略该参数 

### <a name="appservice"></a>应用服务

* 添加了 `[webapp|functionapp] deployment list-publishing-credentials` 以获取 Kudu 发布 url 及其凭据
* 删除了 `webapp auth update` 的错误打印语句
* 修复了 `functionapp` 以便为 Linux 应用服务计划中的运行时设置正确映像
* 删除了 `webapp up` 的预览标记并为该命令添加了改进

### <a name="botservice"></a>Botservice

* 为 v4 Web 应用机器人的 ARM 模板的应用程序设置添加了 `SCM_DO_BUILD_DURING_DEPLOYMENT`
* 为 v4 Web 应用机器人的 ARM 模板的应用程序设置添加了 `Microsoft-BotFramework-AppId` 和 `Microsoft-BotFramework-AppPassword`
* 从 `bot create` 末尾的 `bot publish` 命令输出删除了单引号
* 将 `bot publish` 更改为异步

### <a name="container"></a>容器

* 为 `container [start|restart]` 添加了 `--no-wait` 参数

### <a name="eventhub"></a>EventHub

* 为 `eventhub create|update` 添加了 `--skip-empty-archives` 标记以在捕获中支持空存档

### <a name="find"></a>查找

* 主要功能更新

### <a name="hdinsight"></a>HDInsight

* 为 `hdinsight create` 添加了 `--storage-account-managed-identity` 参数以支持 ADLS Gen2 MSI

### <a name="network"></a>网络

* 已修复 `vpn-connection update` 的以下问题：更新不同订阅中的网关之间的 VPN 连接时会失败

### <a name="rdbms"></a>Rdbms

* 次要修补程序，用于从资源组获取默认位置（如果没有为创建服务器而提供），并为保留天数添加验证

### <a name="role"></a>角色

* 修复了 `role definition update` 以使用 ID 正确解析定义
* 更改了 `ad app credential reset` 以删除应用的服务主体始终存在的假设

### <a name="service-fabric"></a>Service Fabric

* 已修复 `sf cluster list` 不可迭代的问题

## <a name="february-26-2019"></a>2019 年 2 月 26 日

版本 2.0.59

### <a name="core"></a>核心

* 修复了在某些情况下使用 `--subscription NAME` 会引发异常的问题

### <a name="acr"></a>ACR

* 为 `acr build`、`acr task create` 和 `acr task update` 命令添加了 `--target` 参数
* 改进了在未登录到 Azure 时运行时命令的错误处理

### <a name="acs"></a>ACS

* 为 `aks port-forward` 添加了 `--listen-address` 选项

### <a name="appservice"></a>应用服务

* 添加了 `functionapp devops-build` 命令

### <a name="batch"></a>Batch
* [中断性变更E] 已删除 `batch pool upgrade os` 命令
* [中断性变更E] 已从 `Application` 响应删除 `Pacakges` 属性
* 添加了 `batch application package list` 命令以列出应用程序的程序包
* [中断性变更E] 已在所有 `batch application` 命令中将 `--application-id` 更改为 `--application-name` 
* 为命令添加了 `--json-file` 参数以请求原始 API 响应
* 更新了验证以便将 `https://` 自动包含在所有终结点中（如果缺少）

### <a name="cosmosdb"></a>CosmosDB

* 为 `add`、`remove` 和 `list` 命令添加了 `network-rule` 子组以用于管理 Cosmos DB 帐户的 VNET 规则

### <a name="kusto"></a>Kusto

* [中断性变更E] 已将数据库的 `hot_cache_period` 和 `soft_delete_period` 类型更改为 ISO8601 持续时间格式

### <a name="network"></a>网络

* 为 `vpn-connection [create|update]` 添加了 `--express-route-gateway-bypass` 参数
* 已从 `express-route` 扩展中添加了命令组
* 添加了 `express-route gateway` 和 `express-route port` 命令组
* 为 `express-route peering [create|update]` 添加了 `--legacy-mode` 参数 
* 为 `express-route [create|update]` 添加了参数 `--allow-classic-operations` 和 `--express-route-port`
* 为 `vnet-gateway [create|update]` 添加了 `--gateway-default-site` 参数
* 为 `vnet-gateway` 添加了 `ipsec-policy` 命令

### <a name="resource"></a>资源

* 已修复 `deployment create` 的以下问题：类型字段区分大小写
* 为 `policy assignment create` 添加了对基于 URI 的参数文件的支持
* 为 `policy set-definition update` 添加了对基于 URI 的参数和定义的支持
* 修复了 `policy definition update` 的参数和规则处理
* 已修复 `resource show/update/delete/tag/invoke-action` 的以下问题：跨订阅 ID 未正确采用订阅 ID

### <a name="role"></a>角色

* 为 `ad app [create|update]` 添加了对应用角色的支持

### <a name="vm"></a>VM

* 已修复 `vm create where ` 的以下问题：未在默认情况下为 Ubuntu 18.0 启用加速网络

## <a name="february-12-2019"></a>2019 年 2 月 12 日

版本 2.0.58

### <a name="core"></a>核心

* 如果有可更新的包，`az --version` 现在会显示一条通知
* 修复了以下退化问题：`--ids` 不再可用于 JSON 输出

### <a name="acr"></a>ACR
* [中断性变更E] 删除了 `acr build-task` 命令组
* [中断性变更E] 从 `acr repository delete` 中删除了 `--tag` 和 `--manifest` 选项

### <a name="acs"></a>ACS
* 为 `aks [enable-addons|disable-addons]` 添加了对不区分大小写的名称的支持
* 为 Azure Active Directory 添加了使用 `aks update-credentials --reset-aad` 更新操作的支持
* 添加了说明，指出 `aks get-credentials` 的 `--output` 已被忽略

### <a name="ams"></a>AMS
* 添加了 `ams streaming-endpoint [start | stop | create | update] wait` 命令
* 添加了 `ams live-event [create | start | stop | reset] wait` 命令

### <a name="appservice"></a>应用服务
* 添加了使用 ACR 容器创建和配置函数的功能
* 添加了对通过 json 更新 webapp 配置的支持
* 改进了有关 `appservice-plan-update` 的帮助
* 在 functionapp create 上添加了对 App Insights 的支持
* 修复了 webapp SSH 的问题

### <a name="botservice"></a>Botservice
* 改进了 `bot publish` 的用户体验
* 添加了在执行 `az bot publish` 期间运行 `npm install` 时的超时警告
* 从 `az bot create` 中的 `--name` 删除了无效字符 `.`
* 已更改为停止在创建 Azure 存储、应用服务计划、Function App/Web 应用和 Application Insights 时将资源名称随机化
* [已弃用] 已弃用 `--proj-file-path` 风格的 `--proj-name` 参数
* 更改了 `az bot publish` 以删除提取的 IIS Node.js 部署文件（如果它们尚不存在）
* 为 `az bot publish` 添加了 `--keep-node-modules` 参数以便不删除应用服务的 `node_modules` 文件夹
* 为在创建 Azure 函数或 Web 应用机器人时 `az bot create` 的输出添加了 `"publishCommand"` 键值对
  * `"publishCommand"` 的值是一条 `az bot publish` 命令，其中预填充了发布新创建的机器人所需的参数
* 更新了 ARM 模板中的 `"WEBSITE_NODE_DEFAULT_VERSION"`，以便 v4 SDK 机器人能够使用 10.14.1 而不是 8.9.4

### <a name="key-vault"></a>Key Vault
* 已修复了 `keyvault secret backup` 存在的以下问题：一些用户在使用 `--id` 时收到了 `unexpected_keyword` 错误

### <a name="monitor"></a>监视
* 更改了 `monitor metrics alert [create|update]` 以允许维度值 `*`

### <a name="network"></a>网络
* 更改了 `dns zone export` 以确保导出的 CNAME 是 FQDN
* 为 `nic ip-config address-pool [add|remove]` 添加了 `--gateway-name` 参数以支持应用程序网关后端地址池
* 为 `network watcher flow-log configure` 添加了 `--traffic-analytics` 和 `--workspace` 参数以支持通过 Log Analytics 工作区执行流量分析
* 为 `lb inbound-nat-pool [create|update]` 添加了 `--idle-timeout` 和 `--floating-ip`

### <a name="policy-insights"></a>策略见解
* 添加了 `policy remediation` 命令以支持资源策略修正功能

### <a name="rdbms"></a>RDBMS
* 改进了帮助消息和命令参数

### <a name="redis"></a>Redis
* 添加了用于管理防火墙规则的命令（创建、更新、删除、显示、列出）
* 添加了用于管理服务器链接的命令（创建、删除、显示、列出）
* 添加了用于管理修补计划的命令（创建、更新、删除、显示）
* 为 redis create 添加了对可用性区域和最低 TLS 版本的支持
* [中断性变更E] 删除了 `redis update-settings` 和 `redis list-all` 命令
* [中断性变更E] 如果 `redis create` 的参数 'tenant settings' 采用 key[=value] 格式，则它不会被接受
* [已弃用] 添加了有关弃用 `redis import-method` 命令的警告消息

### <a name="role"></a>角色
* [中断性变更E] 将 `az identity` 命令从 `vm` 命令变动到此处

### <a name="sql-vm"></a>SQL VM
* [已弃用] 由于存在拼写错误，弃用了 `--boostrap-acc-pwd` 参数

### <a name="vm"></a>VM
* 更改了 `vm list-skus` 以允许使用 `--all` 来代替 `--all true`
* 添加了 `vmss run-command [invoke | list | show]`
* 修复了以下 bug：如果以前运行过 `vmss encryption enable`，那么它会失败
* [中断性变更E] 已将 `az identity` 命令变动为 `role` 命令

## <a name="january-31-2019"></a>2019 年 1 月 31 日

版本 2.0.57

### <a name="core"></a>核心

* [问题 8399](https://github.com/Azure/azure-cli/issues/8399) 的修补程序。

## <a name="january-28-2019"></a>2019 年 1 月 28 日

版本 2.0.56

### <a name="acr"></a>ACR
* 添加了对 VNet/IP 规则的支持

### <a name="acs"></a>ACS
* 添加了虚拟节点预览版
* 添加了托管 OpenShift 命令
* 为 `aks update-credentials -reset-service-principal` 添加了对服务主体更新操作的支持

### <a name="ams"></a>AMS
* [中断性变更E] 已将 `ams asset get-streaming-locators` 重命名为 `ams asset list-streaming-locators`
* [中断性变更E] 已将 `ams streaming-locator get-content-keys` 重命名为 `ams streaming-locator list-content-keys`

### <a name="appservice"></a>应用服务
* 在 `functionapp create` 上添加了对 App Insights 的支持
* 为 Function App 添加了对创建应用服务计划（包括“弹性高级”计划）的支持
* 修复了“弹性高级”计划的应用设置问题

### <a name="container"></a>容器
* 添加了 `container start` 命令
* 已更改为允许在容器创建期间对 CPU 使用十进制值

### <a name="eventgrid"></a>EventGrid
* 为 `event-subscription [create|update]` 添加了 `--deadletter-endpoint` 参数
* 已将 storagequeue 和 hybridconnection 添加为 'event-subscription [create|update] --endpoint-type' 的新值
* 为 `event-subscription create` 添加了 `--max-delivery-attempts` 和 `--event-ttl` 参数以指定事件的重试策略
* 为 `event-subscription [create|update]` 添加了一条警告消息，在作为目标的 webhook 用于事件订阅时会显示该消息
* 为所有与事件订阅相关的命令添加了 source-resource-id 参数，并且将所有其他与源资源相关的参数标记为已弃用

### <a name="hdinsight"></a>HDInsight
* [中断性变更E] 从 `hdinsight [application] create` 中删除了 `--virtual-network` 和 `--subnet-name` 参数
* [中断性变更E] 已将 `hdinsight create --storage-account` 更改为接受存储帐户（而不是 blob 终结点）的名称或 ID
* 为 `hdinsight create` 添加了 `--vnet-name` 和 `--subnet-name`
* 为 `hdinsight create` 添加了对企业安全性套餐和磁盘加密的支持 
* 添加了 `hdinsight rotate-disk-encryption-key` 命令
* 添加了 `hdinsight update` 命令

### <a name="iot"></a>IoT
* 为 routing-endpoint 命令添加了编码格式

### <a name="kusto"></a>Kusto
* 预览版

### <a name="monitor"></a>监视
* 已将 ID 比较更改为不区分大小写

### <a name="profile"></a>配置文件
* 为 `login` 的托管服务标识启用了租户级别帐户

### <a name="network"></a>网络
* 已修复 `express-route update` 的以下问题：`--bandwidth` 参数被忽略
* 已修复 `ddos-protection update` 的以下问题：集合推导导致了堆栈跟踪

### <a name="resource"></a>资源
* 为 `group deployment create` 添加了对 URI 参数文件的支持
* 为 `policy assignment [create|list|show]` 添加了对托管标识的支持

### <a name="sql-virtual-machine"></a>SQL 虚拟机
* 预览版

### <a name="storage"></a>存储
* 更改了修补程序以仅更新同一对象上更改的属性
* 已修复 #8021，二进制数据在返回时采用 base 64 编码

### <a name="vm"></a>VM
* 更改了 `vm encryption enable` 以验证磁盘加密 keyvault 和密钥加密 keyvault 是否存在
* 为 `vm encryption enable` 添加了 `--force` 标志

## <a name="january-15-2019"></a>2019 年 1 月 15 日

版本 2.0.55

### <a name="acr"></a>ACR
* 已更改为允许强制推送不存在的 helm chart
* 已更改为在没有 ARM 请求的情况下允许运行时操作
* [已弃用] 在以下命令中已弃用 `--resource-group` 参数：
  * `acr login`
  * `acr repository`
  * `acr helm`

### <a name="acs"></a>ACS
* 添加了对新 ACI 区域的支持

### <a name="appservice"></a>应用服务
* 修复了为 ASE 上托管的应用上传证书时的问题（其中 ASE RG 与 App RG 不同）
* 已将 `webapp up` 更改为使用 SKU P1V1 作为 Linux 的默认值
* 修复了 `[webapp|functionapp] deployment source config-zip` 以在部署失败时显示适当的错误消息 
* 添加了 `webapp ssh` 命令

### <a name="botservice"></a>Botservice
* 为 `bot create` 添加了部署状态更新

### <a name="configure"></a>配置
* 添加了 `none` 作为可配置的输出格式

### <a name="cosmosdb"></a>CosmosDB
* 添加了对使用共享吞吐量创建数据库的支持

### <a name="hdinsight"></a>HDInsight
* 添加了用于管理应用程序的命令
* 添加了用于管理脚本操作的命令
* 添加了用于管理 Operations Management Suite (OMS) 的命令
* 为 `hdinsight list-usage` 添加了对列出区域使用情况的支持
* [中断性变更E] 从 `hdinsight create` 中删除了默认群集类型

### <a name="network"></a>网络
* 为 `traffic-manager profile [create|update]` 添加了 `--custom-headers` 和 `--status-code-ranges` 参数
* 添加了新的路由类型：子网和多值
* 为 `traffic-manager endpoint [create|update]` 添加了 `--custom-headers` 和 `--subnets` 参数  
* 修复了为 `ddos-protection update` 提供 `--vnets ""` 导致出现错误的问题

### <a name="role"></a>角色
* [已弃用] 弃用了 `create-for-rbac` 的 `--password` 参数。 改为使用 CLI 生成的安全密码

### <a name="security"></a>安全性
* 初始版本

### <a name="storage"></a>存储
* [中断性变更E] 已将 `storage [blob|file|container|share] list` 的默认结果数更改为 5000。 将 `--num-results *` 用于返回所有结果的原始行为
* 为 `storage [blob|file|container|share] list` 添加了 `--marker` 参数
* 已为 `storage [blob|file|container|share] list` 将用于下一页的日志标记添加到 STDERR 
* 添加了带有对静态网站的支持的 `storage blob service-properties update` 命令

### <a name="vm"></a>VM
* 更改了 `vm [disk|unmanaged-disk]` 和 `vmss disk` 以具有更一致的参数
* 为 `[vm|vmss] create` 添加了对跨租户映像引用的支持
* 已修复了 `vm diagnostics get-default-config --windows-os` 中默认配置的 bug
* 为 `vmss extension set` 添加了参数 `--provision-after-extensions` 以定义在设置扩展之前必须预配哪些扩展
* 为 `sig image-version update` 添加了参数 `--replica-count` 以用于设置默认复制计数
* 修复了 `image create --source` 的以下 bug：源 os 磁盘被误解为同名的 VM，即使提供了完整资源 ID 也是如此

## <a name="december-20-2018"></a>2018 年 12 月 20 日

版本 2.0.54
### <a name="appservice"></a>应用服务
* 修复了 `webapp up` 无法重新部署的问题
* 添加了对列出和还原 Web 应用快照的支持
* 为 Windows 函数应用添加了对 `--runtime` 标志的支持

### <a name="iotcentral"></a>IoTCentral
* 修复了更新命令 API 调用

### <a name="role"></a>角色
* [中断性变更E] 将 `ad [app|sp] list` 更改成了默认情况下仅列出前 100 个对象

### <a name="sql"></a>SQL
* 在托管实例上添加了对自定义排序规则的支持

### <a name="vm"></a>VM
* 为 `disk create` 添加了 `---os-type` 参数

## <a name="december-18-2018"></a>2018 年 12 月 18 日

版本 2.0.53
### <a name="acr"></a>ACR
* 添加了对从外部容器注册表导入映像的支持
* 精简了任务列表的表布局
* 添加了对 Azure DevOps URL 的支持

### <a name="acs"></a>ACS
* 添加了虚拟节点预览版
* 从 `aks create` 的 AAD 参数中删除了“(PREVIEW)”
* [已弃用] 弃用了 `az acs` 命令。 ACS 服务将于 2020 年 1 月 31 日停用
* 创建新的 AKS 群集时添加了对网络策略的支持
* 当只有一个节点池时不再需要 `aks scale` 的 `--nodepool-name` 参数

### <a name="appservice"></a>应用服务
* 修复了 `webapp config container` 不遵守 `--slot` 参数的问题

### <a name="botservice"></a>Botservice
* 添加了调用 `bot show` 时对 `.bot` 文件分析的支持
* 修复了 AppInsights 预配 bug
* 修复了处理文件路径时的空格 bug
* 减少了 Kudu 网络调用
* 常规命令用户体验改进

### <a name="consumption"></a>消耗
* 修复了预算 API 的 bug 以显示通知

### <a name="cosmosdb"></a>CosmosDB
* 添加了对将帐户从多主更新为单主的支持

### <a name="maps"></a>地图
* 在 `maps account [create|update]` 中添加了对 S1 SKU 的支持

### <a name="network"></a>网络
* 在 `watcher flow-log configure` 中添加了对 `--format` 和 `--log-version` 的支持
* 修复了使用 "" 清除解决方案和注册 VNet 不起作用的 `dns zone update` 问题

### <a name="resource"></a>资源
* 修复了 `policy assignment [create|list|delete|show|update]` 中的管理组的作用域参数的处理 
* 添加了新命令 `resource wait`

### <a name="storage"></a>存储
*  在 `storage logging update` 中添加了为存储服务更新日志架构版本的能力

### <a name="vm"></a>VM
* 修复了当指定的 VM 没有已分配的托管服务标识时 `vm identity remove` 中的故障

## <a name="december-4-2018"></a>2018 年 12 月 4 日

版本 2.0.52
### <a name="core"></a>核心
* 添加了对多租户服务主体的跨租户资源预配的支持
* 修复了无法正确分析从带有 tsv 输出的命令通过管道传递的 ID 的 bug

### <a name="appservice"></a>应用服务
* [预览] 添加了 `webapp up` 命令以帮助创建内容并将内容部署到应用
* 修复了由于后端更改而产生的基于容器的 Windows 应用 bug

### <a name="network"></a>网络
* 为 `application-gateway waf-config set` 添加了 `--exclusion` 参数以支持 WAF 排除

### <a name="role"></a>角色
* 添加了对密码凭据的自定义标识符的支持 

### <a name="vm"></a>VM
* [已弃用] 已弃用 `vm extension [show|wait] --expand` 参数
* 为 `vm restart` 添加了 `--force` 参数以重新部署无响应的 VM
* 更改了 `[vm|vmss] create --authentication-type` 以接受 "all"，以便创建带密码和 ssh 身份验证的 VM
* 添加了 `image create --os-disk-caching` 参数以便为映像设置 os 磁盘缓存

## <a name="november-20-2018"></a>2018 年 11 月 20 日

版本 2.0.51
### <a name="core"></a>核心
* 更改了 MSI 登录，目的是在标识中不重复使用订阅名称

### <a name="acr"></a>ACR
* 向任务步骤添加了上下文标记
* 增加了对在 acr 运行中设置机密以镜像 acr 任务的支持
* 对于 `show-tags` 和 `show-manifests` 命令，改进了对 `--top` 和 `--orderby` 的支持。

### <a name="appservice"></a>应用服务
* 更改了 zip 部署的默认超时，轮询状态的时间增加到了 5 分钟。另外还增加了一个超时属性，用于自定义该值
* 更新了默认的 `node_version`。 在一个两阶段交换过程中执行的槽交换重置操作会保留所有的 appsetting 和连接字符串
* 去除了针对 Linux 应用服务计划创建操作的客户端 SKU 检查
* 修复了尝试获取 zipdeploy 状态时出现的错误

### <a name="iotcentral"></a>IotCentral
* 增加了创建 IoT Central 应用程序时进行的子域可用性检查

### <a name="keyvault"></a>KeyVault
* 修复了可能忽略了错误的 Bug

### <a name="network"></a>网络
* 为 `application-gateway` 添加了 `root-cert` 子命令，用于处理受信任的根证书
* 为 `application-gateway [create|update]` 添加了 `--min-capacity` 和 `--custom-error-pages` 选项：
* 为 `application-gateway create` 添加了适用于可用性区域支持的 `--zones` 
* 为 `application-gateway waf-config set` 添加了 `--file-upload-limit`、`--max-request-body-size` 和 `--request-body-check` 参数

### <a name="rdbms"></a>Rdbms
* 增加了 mariadb vnet 命令

### <a name="rbac"></a>Rbac
* 修复了尝试在 `ad app update` 中更新不可变凭据时出现的问题
* 增加了输出警告，目的是告知用户在不远的将来会对 `ad [app|sp] list` 进行重大更改 

### <a name="storage"></a>存储
* 改进了对使用存储复制命令时出现的极端情况的处理
* 修复了在目标和源帐户相同的情况下 `storage blob copy start-batch` 不使用登录凭据的问题
* 修复了在 `sas_token` 未合并到 URL 中的情况下 `storage [blob|file] url` 出现的 Bug
* 为 `[blob|container] list` 添加了重大更改警告：默认情况下，很快会输出前 5000 个结果

### <a name="vm"></a>VM
* 为 `[vm|vmss] create --storage-sku` 添加了支持，允许为托管的 OS 磁盘和数据磁盘分别指定存储帐户 SKU
* 已将 `sig image-version` 的版本名称参数更改为 `--image-version -e`
* 已弃用 `sig image-version` 参数 `--image-version-name`，代之以 `--image-version`
* 为 `[vm|vmss] create --ephemeral-os-disk` 添加了使用本地 OS 磁盘的支持
* 在 `snapshot create/update` 中添加了对 `--no-wait` 的支持
* 添加了 `snapshot wait` 命令
* 增加了将实例名称与 `[vm|vmss] extension set --extension-instance-name` 配合使用的支持

## <a name="november-6-2018"></a>2018 年 11 月 6 日

版本 2.0.50

### <a name="core"></a>核心
* 添加了对服务主体 sn + 颁发者身份验证的支持

### <a name="acr"></a>ACR
* 为任务源触发器添加了对提交和拉取请求 git 事件的支持
* 已更改为使用默认 Dockerfile（如果未在 build 命令中指定它）

### <a name="acs"></a>ACS
* [中断性变更E] 已删除 `enable_cloud_console_aks_browse` 以在默认情况下启用 'az aks browse'

### <a name="advisor"></a>顾问
* 正式版

### <a name="ams"></a>AMS
* 添加了新命令组：
  *  `ams account-filter`
  *  `ams asset-filter`
  *  `ams content-key-policy`
  *  `ams live-event`
  *  `ams live-output`
  *  `ams streaming-endpoint`
  *  `ams mru`
* 添加了新命令：
  * `ams account check-name`
  * `ams job update`
  * `ams asset get-encryption-key`
  * `ams asset get-streaming-locators`
  * `ams streaming-locator get-content-keys`
* 为 `ams streaming-policy create` 添加了加密参数支持
* 为 `ams transform output remove` 添加了支持，它现在可以通过传递要删除的输出索引来执行
* 为 `ams job` 命令组添加了 `--correlation-data` 和 `--label` 参数
* 为 `ams asset` 命令组添加了 `--storage-account` 和 `--container` 参数
* 在 `ams asset get-sas-url` 命令中添加了到期时间默认值（现在 + 23 小时）和权限默认值（读取） 
* [中断性变更E] 已将 `ams streaming locator` 命令替换为 `ams streaming-locator`
* [中断性变更E] 已更新 `ams streaming locator` 的 `--content-keys` 参数
* [中断性变更E] 已在 `ams streaming locator` 命令中将 `--content-policy-name` 重命名为 `--content-key-policy-name`
* [中断性变更E] 已将 `ams streaming policy` 命令替换为 `ams streaming-policy`
* [中断性变更E] 已在 `ams transform` 命令组中将 `--preset-names` 参数替换为 `--preset`。 现在只能一次设置 1 个输出/预设（若要添加更多，必须运行 `ams transform output add`）。 此外，还可以通过将路径传递到自定义 JSON 来设置自定义 StandardEncoderPreset
* [中断性变更E] 已在 `ams job start` 命令中将 `--output-asset-names ` 重命名为 `--output-assets`。 现在，它接受 'assetName=label' 格式的资产列表（以空格分隔）。 没有标签的资产可以采用以下格式发送：'assetName='

### <a name="appservice"></a>应用服务
* 修复了 `az webapp config backup update` 中的 bug，在尚未设置备份计划的情况下，该 bug 会阻止设置备份计划

### <a name="configure"></a>配置
* 已将 YAML 添加到输出格式选项

### <a name="container"></a>容器
* 已更改为在将容器组导出到 yaml 时显示标识

### <a name="eventhub"></a>EventHub
* 在 `eventhub namespace [create|update]` 中添加了 `--enable-kafka` 标志以支持 Kafka

### <a name="interactive"></a>交互
* Interactive 现在会安装 `interactive` 扩展，从而实现更快速的更新和支持

### <a name="monitor"></a>监视
* 为 `monitor metrics alert [create|update]` 中的 `--condition` 添加了对包括字符正斜杠 (/) 和句点 (.) 的指标名称的支持

### <a name="network"></a>网络
* 已弃用 `network interface-endpoint` 命令名称以支持 `network private-endpoint`
* 修复了 `express-route peering connection create` 中的参数 `--peer-circuit` 不接受某个 ID 的问题
* 修复了 `--ip-tags` 无法正确地与 `public-ip create` 配合工作的问题 

### <a name="profile"></a>配置文件
* 将 `--use-cert-sn-issuer` 添加到了 `az login`，以便服务主体可以在证书自动滚动更新的情况下登录

### <a name="rdbms"></a>RDBMS
* 添加了 mysql 副本命令

### <a name="resource"></a>资源
* 为 `policy definition|set-definition` 命令添加了对管理组和订阅的支持

### <a name="role"></a>角色
* 添加了对 API 权限管理、登录用户以及应用程序密码和证书凭据管理的支持
* 更改了 `ad sp create-for-rbac` 以避免 displayName 和服务主体名称之间的混淆
* 添加了支持以向 AAD 应用授予权限

### <a name="storage"></a>存储
* 添加了支持以仅使用 SAS 和终结点连接到存储服务（不使用帐户名或密钥），如 `Configure Azure Storage connection strings <https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string>` 中所述

### <a name="vm"></a>VM
* 为 `image create` 添加了 `storage-sku` 参数以用于设置映像的默认存储帐户类型
* 修复了 `vm resize` 的 bug，其中 `--no-wait` 选项会导致命令崩溃
* 更改了 `vm encryption show` 表输出格式以显示状态
* 更改了 `vm secret format` 以要求 json/jsonc 输出。 如果选择了不需要的输出格式，则会警告用户并将输出格式默认为 json 输出
* 已改进了 `vm create --image` 的参数验证

## <a name="october-23-2018"></a>2018 年 10 月 23 日

版本 2.0.49

### <a name="core"></a>核心
* 修复了 `--ids` 的问题，即 `--subscription` 优先于 `--ids` 中的订阅的问题
* 添加了使用 `--ids` 来忽略参数时会发出的显式警告

### <a name="acr"></a>ACR
* 修复了 Python2 中的 ACR 生成编码问题

### <a name="cdn"></a>CDN
* [中断性变更E] 更改了 `cdn endpoint create` 的默认查询字符串缓存行为，不再默认设置为“IgnoreQueryString”。 它现在由服务设置

### <a name="container"></a>容器
* 增加了 `Private`，作为一种可传递给“--ip-address”的有效类型
* 更改后，可以只使用子网 ID 为容器组设置虚拟网络
* 更改后，可以使用 VNet 名称或资源 ID 在不同资源组中使用 VNet
* 增加了 `--assign-identity`，用于向容器组添加 MSI 标识
* 增加了 `--scope`，用于为系统分配的 MSI 标识创建角色分配
* 添加了一条警告，该警告会在使用不需长时间运行的映像创建容器组时发出
* 修复了 `list` 和 `show` 命令的表输出问题

### <a name="cosmosdb"></a>CosmosDB
* 为 `cosmosdb create` 添加了 `--enable-multiple-write-locations` 支持

### <a name="interactive"></a>交互
* 更改后，可确保全局订阅参数显示在参数中

### <a name="iot-central"></a>IoT Central
* 增加了模板和显示名称选项，用于创建 IoT Central 应用程序
* [中断性变更E] 去除了对 F1 SKU 的支持；改用 S1 SKU

### <a name="monitor"></a>监视
* 对 `monitor activity-log list` 的更改：
  * 增加了相关支持，可以在订阅级别列出所有事件
  * 增加了 `--offset` 参数，可以更容易地创建时间查询
  * 改进了 `--start-time` 和 `--end-time` 的验证，可以使用更大范围的 ISO8601 格式以及对用户更友好的日期时间格式
  * 增加了 `--namespace`，作为已弃用选项 `--resource-provider` 的别名
  * 弃用了 `--filters`，因为除了那些带有强类型选项的值，服务不支持其他值
* 对 `monitor metrics list` 的更改：
  * 增加了 `--offset` 参数，可以更容易地创建时间查询
  * 改进了 `--start-time` 和 `--end-time` 的验证，可以使用更大范围的 ISO8601 格式以及对用户更友好的日期时间格式
* 改进了 `monitor diagnostic-settings create` 的 `--event-hub` 和 `--event-hub-rule` 参数的验证

### <a name="network"></a>网络
* 增加了 `nic create` 的 `--app-gateway-address-pools` 和 `--gateway-name` 参数，支持向 NIC 添加应用程序网关后端地址池
* 增加了 `nic ip-config create/update` 的 `--app-gateway-address-pools` 和 `--gateway-name` 参数，支持向 NIC 添加应用程序网关后端地址池

### <a name="servicebus"></a>ServiceBus
* 为 MigrationConfigProperties 增加了只读的 `migration_state`，可以显示将命名空间从服务总线标准版迁移到高级版时的最新状态

### <a name="sql"></a>SQL
* 修复了 `sql failover-group create` 和 `sql failover-group update`，使之适用于手动故障转移策略

### <a name="storage"></a>存储
* 修复了 `az storage cors list` 输出格式设置，所有项都显示正确的“服务”密钥
* 增加了 `--bypass-immutability-policy` 参数，适用于不可变性策略阻止的容器删除操作

### <a name="vm"></a>VM
* 在 `[vm|vmss] create` 中将 Lv/Lv2 系列计算机的磁盘缓存模式强制设置为 `None`
* 更新了支持的大小列表，支持适用于 `vm create` 的网络加速器
* 为 `disk create` 的 ultrassd iops 和 mbps 配置增加了强类型参数

## <a name="october-16-2018"></a>2018 年 10 月 16 日

版本 2.0.48

### <a name="vm"></a>VM
* 修复了导致 Homebrew 安装失败的 SDK 问题

## <a name="october-9-2018"></a>2018 年 10 月 9 日

版本 2.0.47

### <a name="core"></a>核心
* 改进了“错误请求”错误的错误处理

### <a name="acr"></a>ACR
* 添加了对与 helm 客户端类似的表格格式的支持

### <a name="acs"></a>ACS
* 添加了 `aks [create|scale] --nodepool-name` 以配置节点池名称，该名称截断为 12 个字符，默认值为 nodepool1 
* 已修复以在 Parimiko 失败时回退到“scp”
* 已将 `aks create` 更改为不再需要 `--aad-tenant-id`
* 改进了存在重复条目时 Kubernetes 凭据的合并

### <a name="container"></a>容器
* 更改了 `functionapp create` 以支持使用特定运行时创建 Linux 消耗计划类型
* [预览] 添加了对在 Windows 容器上托管 Web 应用的支持

### <a name="event-hub"></a>事件中心
* 修复了 `eventhub update` 命令
* [中断性变更E] 更改了 `list` 命令，以便以典型方式处理资源 NotFound(404) 错误，而不是显示空列表

### <a name="extensions"></a>扩展
* 修复了尝试添加已安装的扩展的问题

### <a name="hdinsight"></a>HDInsight
* 初始版本

### <a name="iot"></a>IoT
* 添加了扩展安装命令，以便首次运行横幅

### <a name="keyvault"></a>KeyVault
* 已更改为将 keyvault storage 命令限制为使用最新 API 配置文件

### <a name="network"></a>网络
* 修复了 `network dns zone create`：即使用户已配置了默认位置，命令也会成功。 请参阅 #6052
* 弃用了适用于 `network vnet peering create` 的 `--remote-vnet-id`
* 向 `network vnet peering create` 添加了 `--remote-vnet` 以接受名称或 ID
* 使用 `--subnet-prefixes` 向 `network vnet create` 添加了对多个子网前缀的支持
* 使用 `--address-prefixes` 向 `network vnet subnet [create|update]` 添加了对多个子网前缀的支持
* 修复了 `network application-gateway create` 存在的阻止使用 `WAF_v2` 或 `Standard_v2` SKU 创建网关的问题
* 向 `network vnet subnet update` 添加了 `--service-endpoint-policy` 便利参数

### <a name="role"></a>角色
* 添加了对将 Azure AD 应用所有者列为 `ad app owner` 的支持
* 添加了对将 Azure AD 服务主体所有者列入 `ad sp owner` 的支持
* 已更改为确保角色定义创建和更新命令接受多个权限配置
* 已更改 `ad sp create-for-rbac`，以确保主页 URI 始终为“https”

### <a name="service-bus"></a>服务总线
* [中断性变更E] 更改了 `list` 命令，以便以典型方式处理资源 NotFound(404) 错误，而不是显示空列表

### <a name="vm"></a>VM
* 修复了 `disk grant-access` 中的空 `accessSas` 字段
* 已更改 `vmss create`，以保留足够大的前端端口范围来处理过度预配
* 修复了 `sig` 的更新命令
* 在 `sig` 中添加了 `--no-wait` 支持以便管理映像版本
* 已更改 `vm list-ip-addresses`，以显示公共 IP 地址的可用性区域
* 已更改 `[vm|vmss] disk attach`，以将磁盘的默认 lun 设置为第一个可用位置

## <a name="september-21-2018"></a>2018 年 9 月 21 日

版本 2.0.46

### <a name="acr"></a>ACR
* 添加了 ACR 任务命令
* 添加了快速运行命令
* 已弃用 `build-task` 命令组
* 添加了 `helm` 命令组，以支持使用 ACR 管理 helm 图表
* 添加了幂等创建托管注册表的支持
* 添加了无格式标志以用于显示生成日志

### <a name="acs"></a>ACS
* 更改了 `install-connector` 命令以设置 AKS 主 FQDN
* 修复了未指定服务主体和 skip-role-assignemnt 时为 vnet-subnet-id 创建角色分配的问题

### <a name="appservice"></a>应用服务

* 添加了 Webjobs（连续和触发）操作管理的支持
* az webapp config set 支持 --fts-state 属性。另外，添加了 az functionapp config set 和 show 的支持
* 添加了为 Web 应用自带存储的支持
* 添加了列出和还原已删除 Web 应用的支持

### <a name="batch"></a>Batch
* 更改了通过 `--json-file` 添加任务的方法，以支持 AddTaskCollectionParameter 语法
* 更新了接受 `--json-file` 格式的文档
* 为 `batch pool create` 添加了 `--max-tasks-per-node-option`
* 更改了 `batch account` 的行为，以便在未指定选项时显示当前已登录的帐户

### <a name="batch-ai"></a>Batch AI 
* 修复了在 `batchai cluster create` 命令中自动创建存储帐户失败的问题

### <a name="cognitive-services"></a>认知服务
* 为 `--sku`、`--kind` 和 `--location` 参数添加了补全选项
* 添加了命令 `cognitiveservices account list-usage`
* 添加了命令 `cognitiveservices account list-kinds`
* 添加了命令 `cognitiveservices account list`
* 弃用了 `cognitiveservices list`
* 已将 `--name` 更改为 `cognitiveservices account list-skus` 的可选参数

### <a name="container"></a>容器
* 添加了重启和停止正在运行的容器组的功能
* 添加了 `--network-profile` 用于传入网络配置文件
* 添加了 `--subnet` 和 `--vnet_name`，以便在 VNET 中创建容器组
* 更改了表输出，以显示容器组的状态

### <a name="datalake"></a>Datalake
* 添加了针对虚拟网络规则的命令

### <a name="interactive-shell"></a>交互式 Shell
* 在 Windows 中修复了当命令无法正常运行时发生的错误
* 修复了交互模式下已弃用对象导致的命令加载问题

### <a name="iot"></a>IoT
* 添加了路由 IoT 中心的支持

### <a name="key-vault"></a>密钥保管库
* 修复了 RSA 密钥的 Key Vault 密钥导入问题

### <a name="network"></a>网络
* 添加了 `network public-ip prefix` 命令以支持公共 IP 前缀功能
* 添加了 `network service-endpoint` 命令以支持服务终结点策略功能
* 添加了 `network lb outbound-rule` 命令以支持创建标准负载均衡器出站规则
* 为 `network lb frontend-ip create/update` 添加了 `--public-ip-prefix`，以支持使用公共 IP 前缀的前端 IP 配置
* 为 `network lb rule/inbound-nat-rule/inbound-nat-pool create/update` 添加了 `--enable-tcp-reset`
* 为 `network lb rule create/update` 添加了 `--disable-outbound-snat`
* 允许对经典 NSG 使用 `network watcher flow-log show/configure`
* 添加 `network watcher run-configuration-diagnostic` 命令
* 修复了`network watcher test-connectivity` 命令，并添加了 `--method`、`--valid-status-codes` 和 `--headers` 属性
* `network express-route create/update`：添加了 `--allow-global-reach` 标志
* `network vnet subnet create/update`：添加了对 `--delegation` 的支持
* 添加了 `network vnet subnet list-available-delegations` 命令
* `network traffic-manager profile create/update`：为 Monitor 配置添加了对 `--interval`、`--timeout` 和 `--max-failures` 的支持。已弃用了选项 `--monitor-path`、`--monitor-port` 和 `--monitor-protocol`，并改用 `--path`、`--port` 和 `--protocol`
* `network lb frontend-ip create/update`：修复了用于设置专用 IP 分配方法的逻辑。如果提供了专用 IP 地址，则分配是静态的。如果未提供专用 IP 地址，或者为专用 IP 地址提供了空字符串，则分配是动态的。
* `dns record-set * create/update`：添加了对 `--target-resource` 的支持
* 添加了 `network interface-endpoint` 命令用于查询接口终结点对象
* 添加了 `network profile show/list/delete` 用于对网络配置文件进行部分管理
* 添加了 `network express-route peering connection` 命令用于管理 ExpressRoute 之间的对等连接

### <a name="rdbms"></a>RDBMS
* 添加了 MariaDB 服务的支持

### <a name="reservation"></a>预留
* 在保留的资源枚举类型中添加了 CosmosDb
* 在修补模型中添加了名称属性

### <a name="manage-app"></a>管理应用
* 修复了 `managedapp create --kind MarketPlace` 中导致创建市场托管应用的实例时发生崩溃的 bug
* 更改了 `feature` 命令，使其限制为受支持的配置文件

### <a name="role"></a>角色
* 添加了列出用户组成员身份的支持

### <a name="signalr"></a>SignalR
* 首次发布

### <a name="storage"></a>存储
* 添加了 `--auth-mode login` 参数，以及使用用户的登录凭据进行 Blob 和队列授权
* 添加了 `storage container immutability-policy/legal-hold` 用于管理不可变存储

### <a name="vm"></a>VM
* 修复了当公钥文件缺失时，`vm create --generate-ssh-keys` 覆盖私钥文件的问题（#4725、#6780）
* 通过 `az sig` 添加了对共享映像库的支持

## <a name="august-28-2018"></a>2018 年 8 月 28 日

版本 2.0.45

### <a name="core"></a>核心

* 修复了加载空配置文件的问题
* 增加了对 Azure Stack 的 `2018-03-01-hybrid` 配置文件的支持

### <a name="acr"></a>ACR

* 增加了在没有 ARM 请求的情况下针对运行时操作的解决方法
* 从默认情况下 `build` 命令中上传的 tar 更改为 exclude 版本控制文件（例如，.git、.gitignore）

### <a name="acs"></a>ACS

* 将 `aks create` 更改为默认的 `Standard_DS2_v2` VM
* 更改了 `aks get-credentials`，现在可以通过调用新 API 来获取群集凭据

### <a name="appservice"></a>应用服务

* 在 functionapp 和 webapp 中增加了对 CORS 的支持
* 在 create 命令中增加了 ARM 标记支持
* 更改了 `[webapp|functionapp] identity show`，将会在资源缺失的情况下退出并显示代码 3

### <a name="backup"></a>备份

* 更改了 `backup vault backup-properties show`，将会在资源缺失的情况下退出并显示代码 3

### <a name="bot-service"></a>Bot 服务

* 初始机器人服务 CLI 版本

### <a name="cognitive-services"></a>认知服务

* 添加了新参数 `--api-properties,`，此参数是创建某些服务所需的

### <a name="iot"></a>IoT

* 修复了关联已链接中心的问题

### <a name="monitor"></a>监视

* 增加了适用于近实时指标警报的 `monitor metrics alert` 命令
* 弃用了 `monitor alert` 命令

### <a name="network"></a>网络

* 更改了 `network application-gateway ssl-policy predefined show`，将会在资源缺失的情况下退出并显示代码 3

### <a name="resource"></a>资源

* 更改了 `provider operation show`，将会在资源缺失的情况下退出并显示代码 3

### <a name="storage"></a>存储

* 更改了 `storage share policy show`，将会在资源缺失的情况下退出并显示代码 3

### <a name="vm"></a>VM

* 更改了 `vm/vmss identity show`，将会在资源缺失的情况下退出并显示代码 3 
* 弃用了适用于 `vm create` 的 `--storage-caching`

## <a name="auguest-14-2018"></a>2018 年 8 月 14 日

版本 2.0.44

### <a name="core"></a>核心

* 修复了 `table` 输出中的数字显示问题
* 增加了 YAML 输出格式

### <a name="telemetry"></a>遥测

* 改进了遥测报告

### <a name="acr"></a>ACR

* 添加了 `content-trust policy` 命令
* 修复了 `.dockerignore` 未正确处置的问题

### <a name="acs"></a>ACS

* 更改了 `az acs/aks install-cli`，可以在 Windows 的 `%USERPROFILE%\.azure-kubectl` 下安装
* 更改了 `az aks install-connector`，可检测群集是否有 RBAC 并可正确配置 ACI 连接器
* 更改了角色分配方式，可以将提供的角色分配到子网
* 增加了新选项，对于已提供角色的子网，可以“跳过角色分配”                                 
* 更改了角色分配，对于已存在分配的子网，可以跳过角色分配                

### <a name="appservice"></a>应用服务

* 修复了妨碍在外部资源组中使用存储帐户创建 function-app 的 Bug
* 修复了在进行 zip 部署时发生崩溃的问题

### <a name="batchai"></a>BatchAI

* 更改了创建自动存储帐户时的记录器输出，现在会指定“资源组”。        

### <a name="container"></a>容器

* 增加了 `--secure-environment-variables`，用于将安全的环境变量传递到容器      

### <a name="iot"></a>IoT

* [中断性变更E] 删除了弃用的命令，这些命令已移至 IoT 扩展
* 更新了元素，现在不采用 `azure-devices.net` 域

### <a name="iot-central"></a>IoT 中心

* IoT Central 模块的初始版本

### <a name="keyvault"></a>KeyVault


* 增加了用于管理存储帐户和 SAS 定义的命令
* 增加了用于网络规则的命令                                                           
* 增加了针对机密、密钥和证书操作的 `--id` 参数
* 增加了对 KV 管理多 API 版本的支持
* 增加了对 KV 数据平面多 API 版本的支持

### <a name="relay"></a>中继

* 初始版本

### <a name="sql"></a>Sql

* 添加了 `sql failover-group` 命令

### <a name="storage"></a>存储

* [中断性变更E] 更改了 `storage account show-usage`，现在需要 `--location` 参数并且会按区域列出
* 更改了 `--resource-group` 参数，现在此参数为 `storage account` 命令的可选参数
* 对于适用于单个聚合消息的批处理命令中的单个失败，删除了“前提条件失败”警告
* 更改了 `[blob|file] delete-batch` 命令，不再输出 null 数组
* 更改了 `blob [download|upload|delete-batch]` 命令，现在可以读取容器 URL 中的 SAS 令牌

### <a name="vm"></a>VM

* 为 `vm list-skus` 添加了常用筛选器，方便用户使用

## <a name="july-31-2018"></a>2018 年 7 月 31 日

版本 2.0.43

### <a name="acr"></a>ACR

* 在 `acr build-task show` 命令中添加了 `--with-secure-properties` 标志
* 添加了 `acr build-task update-build` 命令

### <a name="acs"></a>ACS

* 更改为按 [Ctrl+C] 结束 `az aks browse` 时返回“0 (成功)”

### <a name="batch"></a>Batch

* 修复了在 cloudshell 中显示 AAD 令牌时的 bug

### <a name="container"></a>容器

* 删除了在设置订阅时 `--log-analytics-workspace-key` 对名称或 ID 的要求

### <a name="network"></a>网络

* 为 Azure Stack 添加了对 2017-03-09-profile 的 dns 支持 

### <a name="resource"></a>资源

* 将 `--rollback-on-error` 添加到 `group deployment create` 以在出错时执行已知良好的部署
* 修复了将 `--parameters {}` 用于 `group deployment create` 导致错误的问题

### <a name="role"></a>角色

* 添加了对堆栈配置文件 2017-03-09-profile 的支持
* 修复了 `app update` 的通用更新参数无法正常工作的问题

### <a name="search"></a>搜索

* 为 Azure 搜索服务添加了命令

### <a name="service-bus"></a>服务总线

* 添加了迁移命令组，以将命名空间从服务总线标准版迁移到高级版
* 为服务总线队列和订阅添加了新的可选属性
  *  `queue` 中的 `--enable-batched-operations` 和 `--enable-dead-lettering-on-message-expiration`
  *  `subscriptions` 中的 `--dead-letter-on-filter-exceptions`

### <a name="storage"></a>存储

* 添加了对使用单个连接下载大型文件的支持
* 转换了在缺少资源时未能失败并显示退出代码 3 的 `show` 命令

### <a name="vm"></a>VM

* 添加了对按订阅列出可用性集的支持
* 添加了对 `StandardSSD_LRS` 的支持
* 添加了在创建 VM 规模集时对应用程序安全组的支持
* [中断性变更E]更改了`[vm|vmss] create`、`[vm|vmss] identity assign` 和 `[vm|vmss] identity remove`，以便以字典格式输出用户指定的标识

## <a name="july-18-2018"></a>2018 年 7 月 18 日

版本 2.0.42

### <a name="core"></a>核心

* 在 WSL bash 窗口中添加了对基于浏览器的登录的支持
* 为所有通用更新命令添加了 `--force-string` 标志
* [中断性变更E]更改了“show”命令以记录错误消息，并在缺少资源时退出代码为 3

### <a name="acr"></a>ACR

* [中断性变更E]在“acr build”命令中将“--no-push”更新为纯标志
* 在 `acr repository` 组下添加了 `show` 和 `update` 命令
* 为 `show-manifests` 和 `show-tags`添加了 `--detail` 标记以显示更详细的信息
* 添加了 `--image` 参数以支持通过图像获取构建详细信息或日志

### <a name="acs"></a>ACS

* 如果 `--max-pods` 小于 5，则将 `az aks create` 更改为错误输出

### <a name="appservice"></a>应用服务

* 添加了对 PremiumV2 sku 的支持

### <a name="batch"></a>Batch

* 修复了在 Cloud Shell 模式下使用令牌凭据时的 bug
* 将 JSON 输入更改为不区分大小写

### <a name="batch-ai"></a>Batch AI

* 修复了 `az batchai job exec` 命令

### <a name="container"></a>容器

* 删除了非 dockerhub 注册表的用户名和密码要求
* 修复了从 yaml 文件创建容器组时的错误

### <a name="network"></a>网络

* 为 `network nic [create|update|delete]` 添加了 `--no-wait` 支持 
* 添加了 `network nic wait`
* 对 `network vnet [subnet|peering] list` 弃用了 `--ids` 参数
* 添加了 `--include-default` 标志以在 `network nsg rule list` 的输出中包含默认安全规则  

### <a name="resource"></a>资源

* 为 `group deployment delete` 添加了 `--no-wait` 支持
* 为 `deployment delete` 添加了 `--no-wait` 支持
* 添加了 `deployment wait` 命令
* 修复了订阅级别 `az deployment` 命令对于配置文件 2017-03-09-profile 错误显示的问题

### <a name="sql"></a>SQL

* 修复了为 `sql db copy` 和 `sql db replica create` 命令指定弹性池名称时“提供的资源组名称与 URL 中的名称不匹配”错误
* 允许通过执行 `az configure --defaults sql-server=<name>` 配置默认 SQL Server
* 为 `sql server`、`sql server firewall-rule`、`sql list-usages` 和 `sql show-usage` 命令实现了表格式化程序

### <a name="storage"></a>存储

* 将 `pageRanges` 属性添加到了将为页 blob 填充的 `storage blob show` 输出

### <a name="vm"></a>VM

* [中断性变更E] 将 `vmss create` 更改为使用 `Standard_DS1_v2` 作为默认实例大小
* 向 `vm extension [set|delete]` 和 `vmss extension [set|delete]` 添加了 `--no-wait` 支持
* 添加了 `vm extension wait`

## <a name="july-3-2018"></a>2018 年 7 月 3 日

版本 2.0.41

### <a name="aks"></a>AKS

* 更改了监视以使用订阅 ID

## <a name="july-3-2018"></a>2018 年 7 月 3 日

版本 2.0.40

### <a name="core"></a>核心

* 添加了用于交互式登录的新授权代码流

### <a name="acr"></a>ACR

* 添加了轮询生成状态
* 添加了对不区分大小写的枚举值的支持
* 为 `show-manifests` 添加了 `--top` 和 `--orderby` 参数

### <a name="acs"></a>ACS

* [中断性变更E] 默认情况下启用 Kubernetes 基于角色的访问控制
* 添加了 `--disable-rbac` 参数并弃用了 `--enable-rbac`，因为它现在是默认值
* 更新了 `aks browse` 命令的选项。 增加了 `--listen-port` 支持
* 为 `aks install-connector` 命令更新了默认 helm chart 包。 使用 virtual-kubelet-for-aks-latest.tgz
* 添加了 `aks enable-addons` 和 `aks disable-addons` 命令以更新现有群集

### <a name="appservice"></a>应用服务

* 添加了对通过 `webapp identity remove` 禁用标识的支持
* 删除了标识功能的 `preview` 标记

### <a name="backup"></a>备份

* 更新了模块定义

### <a name="batchai"></a>BatchAI

* 修复了 `batchai cluster node list` 和 `batchai job node list` 命令的表输出

### <a name="cloud"></a>云

* 为云配置添加了 `acr login` 服务器后缀

### <a name="container"></a>容器

* 更改了 `container create` 以将其默认为长时间运行的操作
* 添加了 Log Analytics 参数 `--log-analytics-workspace` 和 `--log-analytics-workspace-key`
* 添加了 `--protocol` 参数来指定要使用哪个网络协议

### <a name="extension"></a>分机

* 更改了 `extension list-available` 以仅显示与 CLI 版本兼容的扩展

### <a name="network"></a>网络

* 修复了记录类型区分大小写的问题 ([#6602](https://github.com/Azure/azure-cli/issues/6602))

### <a name="rdbms"></a>Rdbms

* 添加了 `[postgres|myql] server vnet-rule` 命令

### <a name="resource"></a>资源

* 添加了新操作组 `deployment`

### <a name="vm"></a>VM

* 添加了对删除系统分配标识的支持

## <a name="june-25-2018"></a>2018 年 6 月 25日

版本 2.0.39

### <a name="cli"></a>CLI

* 更新了 MSI 安装程序中的文件修整以修复扩展安装问题

## <a name="june-19-2018"></a>2018 年 6 月 19 日

版本 2.0.38

### <a name="core"></a>核心

* 为大多数命令增加了对 `--subscription` 的全局支持

### <a name="acr"></a>ACR

* 增加了 `azure-storage-blob` 作为依赖项
* 更改了 `acr build-task create` 的默认 CPU 配置，允许使用 2 核心

### <a name="acs"></a>ACS

* 更新了 `aks use-dev-spaces` 命令的选项。 增加了 `--update` 支持
* 更改了 `aks get-credentials --admin`，不替换 `$HOME/.kube/config` 中的用户上下文
* 公开了托管群集上的只读 `nodeResourceGroup` 属性
* 修复了 `acs browse` 命令错误
* 将 `--connector-name` 设置为 `aks install-connector`、`aks upgrade-connector` 和 `aks remove-connector` 的可选项
* 为 `aks install-connector` 增加了新的 Azure 容器实例区域
* 为 helm 版本名称增加了规范化位置，并为 `aks install-connector` 增加了节点名称

### <a name="appservice"></a>应用服务

* 增加了对更新版 urllib 的支持
* 为 `functionapp create` 增加了支持，可以使用外部资源组的应用服务计划

### <a name="batch"></a>Batch

* 删除了 `azure-batch-extensions` 依赖项

### <a name="batch-ai"></a>Batch AI

* 添加了对工作区的支持。 可以通过工作区将群集、文件服务器和试验按组分组，去除了对可以创建的资源数的限制
* 增加了对试验的支持。 可以通过试验将作业按集合分组，去除了对已创建作业的数目限制
* 增加了为 Docker 容器中运行的作业配置 `/dev/shm` 的支持
* 增加了 `batchai cluster node exec` 和 `batchai job node exec` 命令。 这些命令允许在节点上直接执行任何命令，提供用于端口转发的功能。
* 为 `batchai` 命令增加了对 `--ids` 的支持
* [中断性变更E] 所有群集和文件服务器必须在工作区创建
* [中断性变更E] 作业必须在试验中创建
* [中断性变更E] 从 `cluster create` 和 `job create` 命令中删除了 `--nfs-resource-group`。 若要装载属于其他工作区/资源组的 NFS，请通过 `--nfs` 选项提供文件服务器的 ARM ID
* [中断性变更E] 从 `job create` 命令中删除了 `--cluster-resource-group`。 若要在属于其他工作区/资源组的群集上提交作业，请通过 `--cluster` 选项提供群集的 ARM ID
* [中断性变更E] 从作业、群集和文件服务器中删除了 `location` 特性。 位置现在是工作区的特性。
* [中断性变更E] 从 `job create`、`cluster create` 和 `file-server create` 命令中删除了 `--location`
* [中断性变更E] 更改了短选项的名称，使接口更一致：
  - [`--config`, `-c`] 已重命名为 [`--config-file`, `-f`]
  - [`--cluster`, `-r`] 已重命名为 [`--cluster`, `-c`]
  - [`--cluster`, `-n`] 已重命名为 [`--cluster`, `-c`]
  - [`--job`, `-n`] 已重命名为 [`--job`, `-j`]

### <a name="maps"></a>地图

* [中断性变更E] 更改了 `maps account create`，要求通过交互式提示或 `--accept-tos` 标志接受《服务条款》

### <a name="network"></a>网络

* 为 `network lb probe create` 添加了对 `https` 的支持 [6571](https://github.com/Azure/azure-cli/issues/6571)
* 修复了 `--endpoint-status` 区分大小写的问题。 [#6502](https://github.com/Azure/azure-cli/issues/6502)

### <a name="reservations"></a>预留

* [中断性变更E] 为 `reservations catalog show` 增加了必需参数 `ReservedResourceType`
* 为 `reservations catalog show` 增加了参数 `Location`
* [中断性变更E] 从 `ReservationProperties` 中删除了 `kind`
* [中断性变更E] 已在 `Catalog` 中将 `capabilities` 重命名为 `sku_properties`
* [中断性变更E] 从 `Catalog` 中删除了 `size` 和 `tier` 属性
* 为 `reservations reservation update` 增加了参数 `InstanceFlexibility`

### <a name="role"></a>角色

* 改进了错误处理

### <a name="sql"></a>SQL

* 修复了针对不可供订阅使用的位置运行 `az sql db list-editions` 时出现的令人困惑的错误

### <a name="storage"></a>存储

* 更改了 `storage blob download` 的表输出，使之更为可读

### <a name="vm"></a>VM

* 针对 `vm create` 中的加速网络支持，改进了 refine vm size check
* 增加了针对 `vmss create` 的警告：默认的 VM 大小将从 `Standard_D1_v2` 切换为 `Standard_DS1_v2`
* 为 `[vm|vmss] extension set` 增加了 `--force-update`，即使在配置未变的情况下也可以更新扩展

## <a name="june-13-2018"></a>2018 年 6 月 13 日

版本 2.0.37

### <a name="core"></a>核心

* 改进了交互式遥测

## <a name="june-13-2018"></a>2018 年 6 月 13 日

版本 2.0.36

### <a name="aks"></a>AKS

* 为 `aks create` 添加了高级网络选项
* 为 `aks create` 添加了参数以启用监视和 HTTP 路由
* 为 `aks create` 添加了 `--no-ssh-key` 参数
* 为 `aks create` 添加了 `--enable-rbac` 参数
* [PREVIEW] 为 `aks create` 添加了对 Azure Active Directory 身份验证的支持

### <a name="appservice"></a>应用服务

* 修复了不兼容 urllib 版本的问题

## <a name="june-5-2018"></a>2018 年 6 月 5 日

版本 2.0.35

### <a name="interactive"></a>交互

* 添加了对交互模式的依赖项的限制

## <a name="june-5-2018"></a>2018 年 6 月 5 日

版本 2.0.34

### <a name="core"></a>核心

* 增加了对跨租户资源引用的支持
* 增强了遥测数据上传可靠性

### <a name="acr"></a>ACR

* 增加了对 VSTS 充当远程源位置的支持
* 添加了 `acr import` 命令

### <a name="aks"></a>AKS

* 更改了 `aks get-credentials`，可以使用更安全的文件系统权限来创建 kube 配置文件

### <a name="batch"></a>Batch

* 修复了池列表表格式设置中的 Bug [[问题 #4378](https://github.com/Azure/azure-cli/issues/4378)]

### <a name="iot"></a>IOT

* 增加了对创建基本层 IoT 中心的支持

### <a name="network"></a>网络

* 改进了 `network vnet peering`

### <a name="policy-insights"></a>策略见解

* 初始版本

### <a name="arm"></a>ARM

* 增加了 `account management-group` 命令。

### <a name="sql"></a>SQL

* 增加了新的托管实例命令：
  * `sql mi create`
  * `sql mi show`
  * `sql mi list`
  * `sql mi update`
  * `sql mi delete`
* 增加了新的托管数据库命令：
  * `sql midb create`
  * `sql midb show`
  * `sql midb list`
  * `sql midb restore`
  * `sql midb delete`

### <a name="storage"></a>存储

* 增加了可以从文件扩展名推断且适用于 json 和 javascript 的额外 mimetype

### <a name="vm"></a>VM

* 更改了 `vm list-skus`，可以使用固定列并可添加表明要删除 `Tier` 和 `Size` 的警告
* 为 `vm create` 添加了 `--accelerated-networking` 选项
* 为 `identity create` 添加了 `--tags`

## <a name="may-22-2018"></a>2018 年 5 月 22 日

版本 2.0.33

### <a name="core"></a>核心

* 增加了对在文件名中扩展 `@` 的支持

### <a name="acs"></a>ACS

* 增加了新的 Dev-Spaces 命令：`aks use-dev-spaces` 和 `aks remove-dev-spaces`
* 修复了帮助消息中的拼写错误

### <a name="appservice"></a>应用服务

* 改进了泛型更新命令
* 增加了对 `webapp deployment source config-zip` 的异步支持

### <a name="container"></a>容器

* 增加了对以 yaml 格式导出容器组的支持
* 增加了对使用 yaml 文件创建/更新容器组的支持

### <a name="extension"></a>分机

* 改进了扩展的删除方式

### <a name="interactive"></a>交互

* 更改了日志记录，在完成时将分析器静音
* 改进了错误的帮助缓存的处理

### <a name="keyvault"></a>KeyVault

* 修复了 keyvault 命令，使之可以在 Cloud Shell 或 VM 中与标识一起使用

### <a name="network"></a>网络

* 修复了 `network watcher show-topology` 无法与 vnet 和/或子网名称一起使用的问题 [#6326](https://github.com/Azure/azure-cli/issues/6326)
* 修复了某些 `network watcher` 命令宣称未为区域启用网络观察程序而实际上却已经启用的问题 [#6264](https://github.com/Azure/azure-cli/issues/6264)

### <a name="sql"></a>SQL

* [中断性变更E] 更改了从 `db` 和 `dw` 命令返回的响应对象：
    * 已将 `serviceLevelObjective` 属性重命名为 `currentServiceObjectiveName`
    * 删除了 `currentServiceObjectiveId` 和 `requestedServiceObjectiveId` 属性
    * 已将 `maxSizeBytes` 属性更改为整数值而不是字符串
* [中断性变更E] 已将下面的 `db` 和 `dw` 属性更改为只读：
    * `requestedServiceObjectiveName`.  若要更新，请使用 `--service-objective` 参数或设置 `sku.name` 属性
    * `edition`. 若要更新，请使用 `--edition` 参数或设置 `sku.tier` 属性
    * `elasticPoolName`. 若要更新，请使用 `--elastic-pool` 参数或设置 `elasticPoolId` 属性
* [中断性变更E] 已将下面的 `elastic-pool` 属性更改为只读：
    * `edition`. 若要更新，请使用 `--edition` 参数
    * `dtu`. 若要更新，请使用 `--capacity` 参数
    *  `databaseDtuMin`. 若要更新，请使用 `--db-min-capacity` 参数
    *  `databaseDtuMax`. 若要更新，请使用 `--db-max-capacity` 参数
* 为 `db`、`dw` 和 `elastic-pool` 命令增加了 `--family` 和 `--capacity` 参数。
* 为 `db`、`dw` 和 `elastic-pool` 命令增加了表格式化程序。

### <a name="storage"></a>存储

* 增加了 `--account-name` 参数的补全选项
* 修复了 `storage entity query` 的问题

### <a name="vm"></a>VM

* [中断性变更E] 从 `vm create` 中删除了 `--write-accelerator`。 可以通过 `vm update` 或 `vm disk attach` 访问同一支持
* 修复了 `[vm|vmss] extension` 中的扩展映像匹配问题
* 为 `vm create` 添加了 `--boot-diagnostics-storage`，可以捕获启动日志
* 为 `[vm|vmss] update` 添加了 `--license-type`

## <a name="may-7-2018"></a>2018 年 5 月 7 日

版本 2.0.32

### <a name="core"></a>核心

* 修复了使用证书从服务主体帐户检索机密时引发未处理异常的问题
* 增加了对位置参数的有限支持
* 修复了 `--query` 无法与 `--ids` 一起使用的问题。 [#5591](https://github.com/Azure/azure-cli/issues/5591)
* 改进了使用 `--ids` 时通过命令执行的管道方案。 支持在指定查询的情况下使用 `-o tsv`，或者在不指定查询的情况下使用 `-o json`
* 增加了在用户的命令中存在拼写错误的情况下，针对错误提供命令建议的功能
* 改进了用户键入 `az ''` 时出现的错误
* 增加了针对命令模块和扩展自定义资源类型的功能

### <a name="acr"></a>ACR

* 增加了 ACR 生成命令
* 改进了“找不到资源”类型的错误消息
* 改进了资源创建性能和错误处理
* 改进了 acr 登录非标准控制台和 WSL
* 改进了存储库命令错误消息
* 更新了表列和排序

### <a name="acs"></a>ACS

* 增加了“`az aks` 是预览版服务”警告
* 修复了未指定 `--aci-resource-group` 时 `aks install-connector` 中的权限问题

### <a name="ams"></a>AMS

* 初始版本 - 管理 Azure 媒体服务资源

### <a name="appservice"></a>应用服务

* 修复了提供 `--slot` 时 `webapp delete` 中的 Bug
* 从 `webapp auth update` 删除了 `--runtime-version`
* 增加了对 min\_tls\_version & https2.0 的支持
* 增加了对多容器的支持

### <a name="batch-ai"></a>Batch AI

* 更改了 `batchai create cluster`，以便遵循在群集的配置文件中配置的 VM 优先级

### <a name="cognitive-services"></a>认知服务

* 修复了在 `cognitiveservices account create` 的示例中的拼写错误 [5603](https://github.com/Azure/azure-cli/issues/5603)

### <a name="consumption"></a>消耗

* 增加了预算 API 的新命令

### <a name="container"></a>容器

* 删除了在映像名称中包括注册表服务器时对 `container create` 命令的 `--registry-server` 参数的要求

### <a name="cosmos-db"></a>Cosmos DB

* 引入针对 Azure CLI 的 VNET 支持 - Cosmos DB

### <a name="dms"></a>DMS

* 初始版本 - 为 Azure SQL 迁移方案增加了对 SQL 的支持

### <a name="extension"></a>分机

* 修复了停止显示扩展元数据的 Bug

### <a name="interactive"></a>交互

* 允许交互式补全选项使用位置参数
* 增强用户键入 '\' 时的输出的用户友好性
* 修复了在没有帮助的情况下参数的补全问题
* 修复了命令组的说明问题

### <a name="lab"></a>实验室

* 修复了 Knack 转换的回归问题

### <a name="network"></a>网络

* [中断性变更E] 删除了以下项的 `--ids` 参数：
  * `express-route auth list`
  * `express-route peering list`
  * `nic ip-config list`
  * `nsg rule list`
  * `route-filter rule list`
  * `route-table route list`
  * `traffic-manager endpoint list`

### <a name="profile"></a>配置文件

* 修复了 `disk create` 源检测问题
* [中断性变更E] 删除了不再使用的 `--msi-port` 和 `--identity-port`
* 修复了 `account get-access-token` 短摘要中的拼写错误

### <a name="redis"></a>Redis

* 弃用 `redis patch-schedule patch-schedule show` 而改用 `redis patch-schedule show`
* 弃用 `redis list-all`。 此功能已加入 `redis list` 中
* 弃用 `redis import-method` 而改用 `redis import`
* 为各种命令增加了对 `--ids` 的支持

### <a name="role"></a>角色

* [中断性变更E] 删除了已弃用的 `ad sp reset-credentials`

### <a name="storage"></a>存储

* 在源 SAS 和帐户密钥未指定的情况下，允许将目标 SAS 令牌应用到源，以便进行 Blob 复制
* 公开了用于 Blob 上传和下载的 --socket-timeout 参数
* 将以路径分隔符开头的 Blob 名称视为相对路径
* 允许 `storage blob copy --source-sas` 以查询字符“?”开头
* 修复了 `storage entity query --marker` 问题，接受“键=值”列表

### <a name="vm"></a>VM

* 修复了非托管 Blob URI 上的检测逻辑无效的问题
* 增加了在没有用户提供的服务主体的情况下进行磁盘加密的功能
* [中断性变更E] 请勿使用 VM“ManagedIdentityExtension”来寻求 MSI 支持
* 增加了对 `vmss` 使用逐出策略的支持
* [中断性变更E] 从以下项删除了 `--ids`：
  * `vm extension list`
  * `vm secret list`
  * `vm unmanaged-disk list`
  * `vmss nic list`
* 增加了写入加速器支持
* 添加了 `vmss perform-maintenance`
* 修复了 `vm diagnostics set` 问题，可以可靠地检测 VM 的 OS 类型
* 更改了 `vm resize`，系统会检查请求的大小是否不同于当前设置的大小，只在二者有变化时进行更新


## <a name="april-10-2018"></a>2018 年 4 月 10 日

版本 2.0.31

### <a name="acr"></a>ACR

* 改进了 wincred 回退错误处理

### <a name="acs"></a>ACS

* 更改了 aks 创建的 SPN，使其有效期达到 5 年

### <a name="appservice"></a>应用服务

* [重大更改]: Removed `assign-identity`
* 修复了 Web 应用计划不存在导致的未捕获异常

### <a name="batchai"></a>BatchAI

* 添加了对 2018-03-01 API 的支持

  - 作业级装载
  - 使用机密值的环境变量
  - 性能计数器设置
  - 报告作业特定的路径段
  - 支持“列出文件”API 中的子文件夹
  - 使用情况和限制报告
  - 允许为 NFS 服务器指定缓存类型
  - 支持自定义映像
  - 添加了 pyTorch 工具包支持

* 添加了 `job wait` 命令，以允许等待作业完成和报告作业退出代码
* 添加了 `usage show` 命令，用于列出不同区域的当前 Batch AI 资源使用情况和限制
* 支持国家云
* 添加了作业命令行参数，以便除了装载配置文件以外，还能装载作业级别的文件系统
* 添加了更多选项用于自定义群集 - VM 优先级、子网、自动缩放群集的初始节点计数，以及指定自定义映像
* 添加了命令行选项用于指定 Batch AI 托管 NFS 的缓存类型
* 简化了在配置文件中指定装载文件系统的方法。 现在，可以省略 Azure 文件共享和 Azure Blob 容器的凭据 - CLI 将会使用通过命令行参数提供的或通过环境变量指定的存储帐户密钥来填充缺少的凭据，或者从 Azure 存储查询密钥（如果存储帐户属于当前订阅）
* 作业完成后（成功、失败、已终止或已删除），作业文件流命令现在会自动完成
* 改进了 `show` 操作的 `table` 输出
* 添加了 `--use-auto-storage` 选项用于创建群集。 使用此选项可以更方便地管理存储帐户，以及将 Azure 文件共享和 Azure Blob 容器装载到群集
* 为 `cluster create` 和 `file-server create` 添加了 `--generate-ssh-keys` 选项
* 添加了通过命令行提供节点设置任务的功能
* [中断性变更E]移动了 `job file` 组下面的 `job stream-file` 和 `job list-files` 命令
* [中断性变更E]已将 `file-server create` 命令中的 `--admin-user-name` 重命名为 `--user-name`，使其与 `cluster create` 命令一致

### <a name="billing"></a>计费

* 添加了登记帐户命令

### <a name="consumption"></a>消耗

* 添加了 `marketplace` 命令
* [中断性变更E] 已将 `reservations summaries` 重命名为 `reservation summary`
* [中断性变更E] 已将 `reservations details` 重命名为 `reservation detail`
* [中断性变更E]删除了 `reservation` 命令的 `--reservation-order-id` 和 `--reservation-id` 短选项
* [中断性变更E]删除了 `reservation summary` 命令的 `--grain` 短选项
* [中断性变更E]删除了 `pricesheet` 命令的 `--include-meter-details` 短选项

### <a name="container"></a>容器

* 添加了 git 存储库卷装载参数 `--gitrepo-url`、`--gitrepo-dir`、`--gitrepo-revision` 和 `--gitrepo-mount-path`
* 修复了 [#5926](https://github.com/Azure/azure-cli/issues/5926)：指定 --container-name 时 `az container exec` 失败

### <a name="extension"></a>分机

* 已将分配检查消息更改为调试级别

### <a name="interactive"></a>交互

* 更改为在命令不可识别时停止完成
* 添加了创建命令子树之前和之后所用的事件挂钩
* 添加了 `--ids` 参数补全

### <a name="network"></a>网络

* 修复了 [#5936](https://github.com/Azure/azure-cli/issues/5936)：无法设置 `application-gateway create` 标记
* 添加了参数 `--auth-certs`，以附加 `application-gateway http-settings [create|update]` 的身份验证证书。 [#4910](https://github.com/Azure/azure-cli/issues/4910)
* 添加了 `ddos-protection` 命令用于创建 DDoS 保护计划
* 为 `vnet [create|update]` 添加了 `--ddos-protection-plan` 支持，以便将 VNet 关联到 DDoS 保护计划
* 修复了 `network route-table [create|update]` 中 `--disable-bgp-route-propagation` 标志的问题
* 删除了 `network lb [create|update]` 的虚拟参数 `--public-ip-address-type` 和 `--subnet-type`
* 为 `network dns zone [import|export]` 和 `network dns record-set txt add-record` 添加了采用 RFC 1035 转义序列的 TXT 记录支持

### <a name="profile"></a>配置文件

* 在 `account list` 中添加了 Azure 经典帐户支持
* [中断性变更E]删除了 `--msi` & `--msi-port` 参数

### <a name="rdbms"></a>RDBMS

* 添加了 `georestore` 命令
* 删除了 `create` 命令的存储大小限制

### <a name="resource"></a>资源

* 在 `policy definition create` 中添加了对 `--metadata` 的支持
* 为 `policy definition update` 添加了对 `--metadata`、`--set`、`--add` 和 `--remove` 的支持

### <a name="sql"></a>SQL

* 添加了 `sql elastic-pool op list` 和 `sql elastic-pool op cancel`

### <a name="storage"></a>存储

* 改进了有关连接字符串格式不当的错误消息

### <a name="vm"></a>VM

* 为 `vmss create` 添加了配置平台容错域计数的支持
* 已将 `vmss create` 的默认值更改为区域性、大型或禁用单一位置组的规模集的标准负载均衡器
* [重大更改]: Removed `vm assign-identity`, `vm remove-identity and `vm format-secret`
* 为 `vm create` 添加了对公共 IP SKU 的支持
* 为 `vm secret format` 添加了 `--keyvault` 和 `--resource-group` 参数，以便在命令无法解析保管库 ID 的情况下提供支持。 [#5718](https://github.com/Azure/azure-cli/issues/5718)
* 改进了当资源组的位置不支持区域时，`[vm|vmss create]` 发生的错误


## <a name="march-27-2018"></a>2018 年 3 月 27 日

版本 2.0.30

### <a name="core"></a>核心

* 为帮助中标记为预览版的扩展显示消息

### <a name="acs"></a>ACS

* 为 Cloud Shell 中的 `aks install-cli` 修复 SSL 证书验证错误

### <a name="appservice"></a>应用服务

* 为 `webapp update` 添加了仅限 HTTPS 的支持
* 为 `az webapp identity [assign|show]` 和 `az functionapp identity [assign|show]` 添加了对插槽的支持

### <a name="backup"></a>备份

* 添加了新命令 `az backup protection isenabled-for-vm`。 此命令可用于检查 VM 是否已由订阅中的任何保管库备份
* 为下列命令的 `--resource-group` 和 `--vault-name` 参数启用了 Azure 对象 ID：
  * `backup container show`
  * `backup item set-policy`
  * `backup item show`
  * `backup job show`
  * `backup job stop`
  * `backup job wait`
  * `backup policy delete`
  * `backup policy get-default-for-vm`
  * `backup policy list-associated-items`
  * `backup policy set`
  * `backup policy show`
  * `backup protection backup-now`
  * `backup protection disable`
  * `backup protection enable-for-vm`
  * `backup recoverypoint show`
  * `backup restore files mount-rp`
  * `backup restore files unmount-rp`
  * `backup restore restore-disks`
  * `backup vault delete`
  * `backup vault show`
* 更改了 `--name` 参数以接受 `backup ... show` 命令的输出格式

### <a name="container"></a>容器

* 添加了 `container exec` 命令。 在正在运行的容器组的容器中执行命令
* 允许将表输出用于创建和更新容器组

### <a name="extension"></a>分机

* 为 `extension add` 添加了消息（如果扩展处于预览状态）
* 更改了 `extension list-available` 以通过 `--show-details` 显示完整扩展数据
* [中断性变更E] 更改了 `extension list-available` 以在默认情况下显示简化的扩展数据

### <a name="interactive"></a>交互

* 已将“完成”更改为在执行命令表加载后立即激活
* 修复了使用 `--style` 参数时的 bug
* 交互式词法分析器在命令表转储后实例化（如果缺失）
* 改进了完成符支持

### <a name="lab"></a>实验室

* 修复了 `create environment` 命令的 bug

### <a name="monitor"></a>监视

* 为 `metrics list` 添加了对 `--top`、`--orderby` 和 `--namespace` 的支持 [5785](https://github.com/Azure/azure-cli/issues/5785)
* 修复了 [#4529](https://github.com/Azure/azure-cli/issues/5785)：`metrics list` 接受要检索的指标的空格分隔列表
* 为 `metrics list-definitions` 添加了对 `--namespace` 的支持 [5785](https://github.com/Azure/azure-cli/issues/5785)

### <a name="network"></a>网络

* 添加了对专用 DNS 区域的支持

### <a name="profile"></a>配置文件

* 为 `login` 添加了针对 `--identity-port` 和 `--msi-port` 的警告

### <a name="rdbms"></a>RDBMS

* 添加了业务模型 GA API 版本 2017-12-01

### <a name="resource"></a>资源

* [重大更改]: Changed `provider operation [list|show]` to not require `--api-version`

### <a name="role"></a>角色

* 为 `az ad app create` 添加了对所需访问权限配置和本机客户端的支持
* 更改了 `rbac` 命令以在对象解析时返回少于 1000 个的 ID
* 添加了凭据管理命令 `ad sp credential [reset|list|delete]`
* [中断性变更E] 从 `az role assignment [list|show]` 输出中删除了“properties”
* 为 `role definition` 添加了对 `dataActions` 和 `notDataActions` 权限的支持

### <a name="storage"></a>存储

* 修复了在上传大小介于 195GB 和 200GB 之间的文件时存在的问题
* 修复了 [#4049](https://github.com/Azure/azure-cli/issues/4049)：上传追加 Blob 时忽略条件参数的问题

### <a name="vm"></a>VM

* 针对即将到来的、适用于包含 100 个以上实例的集合的重大更改，为 `vmss create` 添加了警告
* 为 `vm [snapshot|image]` 添加了区域弹性支持
* 更改了磁盘实例视图以更好地报告加密状态
* [中断性变更E] 更改了 `vm extension delete` 以不再返回输出

## <a name="march-13-2018"></a>2018 年 3 月 13 日

版本 2.0.29

### <a name="acr"></a>ACR

* 为 `repository delete` 添加了对 `--image` 参数的支持
* 弃用了 `repository delete` 命令的 `--manifest` 和 `--tag` 参数
* 添加了 `repository untag` 命令以在不删除数据的情况下删除标记

### <a name="acs"></a>ACS

* 添加了 `aks upgrade-connector` 命令以升级现有连接器
* 更改了 `kubectl` 配置文件以使用更具可读性的块样式 YAML

### <a name="advisor"></a>顾问

* [中断性变更E] 已将 `advisor configuration get` 重命名为 `advisor configuration list`
* [中断性变更E] 已将 `advisor configuration set` 重命名为 `advisor configuration update`
* [中断性变更E] 删除了 `advisor recommendation generate`
* 为 `advisor recommendation list` 添加了 `--refresh` 参数
* 添加了 `advisor recommendation show` 命令

### <a name="appservice"></a>应用服务

* 弃用了 `[webapp|functionapp] assign-identity`
* 添加了托管标识命令 `webapp identity [assign|show]` 和 `functionapp identity [assign|show]`

### <a name="eventhubs"></a>Eventhubs

* 初始版本

### <a name="extension"></a>分机

* 添加了检查，以在使用的发行版不是程序包源文件中存储的发行版时提醒用户，因为这可能导致错误

### <a name="interactive"></a>交互

* 修复了 [#5625](https://github.com/Azure/azure-cli/issues/5625)：在不同会话之间永久保留历史记录
* 修复了 [#3016](https://github.com/Azure/azure-cli/issues/3016)：在作用域中时不记录历史记录
* 修复了 [#5688](https://github.com/Azure/azure-cli/issues/5688)：当命令表加载遇到异常时不显示完成项
* 修复了用于长时间运行的操作的进度指示器

### <a name="monitor"></a>监视

* 弃用了 `monitor autoscale-settings` 命令
* 添加了 `monitor autoscale` 命令
* 添加了 `monitor autoscale profile` 命令
* 添加了 `monitor autoscale rule` 命令

### <a name="network"></a>网络

* [中断性变更E] 从 `route-filter rule create` 中删除了 `--tags` 参数
* 删除了以下命令的某些错误默认值：
  * `network express-route update`
  * `network nsg rule update`
  * `network public-ip update`
  * `traffic-manager profile update`
  * `network vnet-gateway update`
* 添加了 `network watcher connection-monitor` 命令
* 为 `network watcher show-topology` 添加了 `--vnet` 和 `--subnet`

### <a name="profile"></a>配置文件

* 弃用了 `az login` 的 `--msi` 参数
* 为 `az login` 添加了 `--identity` 参数以替换 `--msi`

### <a name="rdbms"></a>RDBMS

* [预览] 已更改为使用 API 2017-12-01-preview

### <a name="service-bus"></a>服务总线

* 初始版本

### <a name="storage"></a>存储

* 修复了 [#4971](https://github.com/Azure/azure-cli/issues/4971)：`storage blob copy` 现在支持其他 Azure 云
* 修复了 [#5286](https://github.com/Azure/azure-cli/issues/5286)：Batch 命令 `storage blob [delete-batch|download-batch|upload-batch]` 在前置条件失败时不再引发错误

### <a name="vm"></a>VM

* 为 `[vm|vmss] create` 添加了支持，以附加非托管数据磁盘和配置缓存
* 弃用了 `[vm|vmss] assign-identity` 和 `[vm|vmss] remove-identity`
* 添加了 `vm identity [assign|remove|show]` 和 `vmss identity [assign|remove|show]` 以替换弃用的命令
* 已将 `vmss create` 中的默认优先级更改为 None

## <a name="february-27-2018"></a>2018 年 2 月 27 日

版本 2.0.28

### <a name="core"></a>核心

* 修复了 [#5184](https://github.com/Azure/azure-cli/issues/5184)：Homebrew 安装问题
* 添加了对具有自定义密钥的扩展遥测的支持
* 为 `--debug` 添加了 HTTP 日志记录

### <a name="acs"></a>ACS

* 已更改为默认情况下对 `aks install-connector` 使用 `virtual-kubelet-for-aks` Helm 图
* 修复了问题：服务主体没有足够的权限来创建 ACI 容器组的问题
* 已为 `aks install-connector` 添加了 `--aci-container-group`、`--location` 和 `--image-tag`
* 从 `aks get-versions` 中删除了弃用通知

### <a name="appservice"></a>应用服务

* 针对新 SDK 版本 (azure-mgmt-web 0.35.0) 的更新
* 已修复 [#5538](https://github.com/Azure/azure-cli/issues/5538)：`Free` 被报告为无效 SKU

### <a name="cognitive-services"></a>认知服务

* 更新了创建新的认知服务帐户时的“通知”

### <a name="consumption"></a>消耗

* 为 pricesheet API 添加了新命令
* 更新了现有“使用情况详细信息”和“预订详细信息”格式

### <a name="container"></a>容器

* 为 `container create` 添加了 `--secrets` 和 `--secrets-mount-path` 参数以在 ACI 中使用机密

### <a name="network"></a>网络

* 修复了 [#5559](https://github.com/Azure/azure-cli/issues/5559)：`network vnet-gateway vpn-client generate` 中缺少客户端

### <a name="resource"></a>资源

* 更改了 `group deployment export` 以在失败时显示部分模板和错误

### <a name="role"></a>角色

* 添加了 `role assignment list-changelogs` 以允许审核服务主体角色

### <a name="sql"></a>SQL

* 添加了在创建和更新时对数据库和弹性池的区域冗余支持

### <a name="storage"></a>存储

* 已允许为 `storage blob [upload-batch|download-batch]` 指定 destination-path/prefix

### <a name="vm"></a>VM

* 添加了对在单个 VMSS 实例上附加/分离磁盘的支持


## <a name="february-13-2018"></a>2018 年 2 月 13 日

版本 2.0.27

### <a name="core"></a>核心

* 更改了同时根据订阅 ID 和在 MSI 登录名对密钥进行身份验证

### <a name="acs"></a>ACS

* [中断性变更E] 为了提高准确性，已将 `aks get-versions` 重命名为 `aks get-upgrades`
* 更改了 `aks get-versions` 以显示可用于 `aks create` 的 Kubernetes 版本
* 更改了 `aks create` 默认值以允许服务器选择 Kubernetes 版本
* 更新了引用由 AKS 生成的服务主体的帮助消息
* 更改了 `aks create` 的默认节点大小，从“Standard\_D1\_v2”更改为“Standard\_DS1\_v2”
* 改进了定位 `az aks browse` 的仪表板 pod 时的可靠性
* 修复了 `aks get-credentials` 以便在加载 Kubernetes 配置文件时处理 Unicode 错误
* 向 `az aks install-cli` 添加了一条消息，以便帮助在 `$PATH` 中获取 `kubectl`

### <a name="appservice"></a>应用服务

* 修复了由于空引用 `webapp [backup|restore]` 失败的问题
* 通过 `az configure --defaults appserviceplan=my-asp` 添加了对默认应用服务计划的支持

### <a name="cdn"></a>CDN

* 添加了 `cdn custom-domain [enable-https|disable-https]` 命令

### <a name="container"></a>容器

* 向 `az container logs` 添加了 `--follow` 选项，用于流式处理日志
* 添加了 `container attach` 命令，用于将本地标准输出和错误流附加到容器组中的容器

### <a name="cosmosdb"></a>CosmosDB

* 添加了对设置功能的支持

### <a name="extension"></a>分机

* 向 `az extension [add|update]` 命令添加了对 `--pip-proxy` 参数的支持
* 向 `az extension [add|update]` 命令添加了对 `--pip-extra-index-urls` 参数的支持

### <a name="feedback-command"></a>反馈命令

* 将扩展信息添加到了遥测数据

### <a name="interactive"></a>交互

* 修复了在 Cloud Shell 中使用交互模式时提示用户登录的问题
* 修复了缺少参数补全时的回归问题

### <a name="iot"></a>IoT

* 修复了 `iot dps access policy [create|update]` 在成功时返回“not found”错误的问题
* 修复了 `iot dps linked-hub [create|update]` 在成功时返回“not found”错误的问题
* 向 `iot dps access policy [create|update]` 和 `iot dps linked-hub [create|update]` 添加了 `--no-wait` 支持
* 更改了 `iot hub create` 以允许指定分区数

### <a name="monitor"></a>监视

* 修复了 `az monitor log-profiles create` 命令

### <a name="network"></a>网络

* 修复了以下命令的 `--tags` 选项：
  * `network public-ip create`
  * `network lb create`
  * `network local-gateway create`
  * `network nic create`
  * `network vnet-gateway create`
  * `network vpn-connection create`

### <a name="profile"></a>配置文件

* 在交互模式中启用了 `az login`

### <a name="resource"></a>资源

* 重新添加了 `feature show`

### <a name="role"></a>角色

* 为 `ad app update` 添加了 `--available-to-other-tenants` 参数

### <a name="sql"></a>SQL

* 添加了 `sql server dns-alias` 命令
* 添加了 `sql db rename`
* 向所有 sql 命令添加了对 `--ids` 参数的支持

### <a name="storage"></a>存储

* 添加了 `storage blob service-properties delete-policy` 和 `storage blob undelete` 命令以启用软删除

### <a name="vm"></a>VM

* 修复了无法完全初始化 VM 加密时出现的崩溃
* 添加了启用 MSI 时的主体 ID 输出
* 固定 `vm boot-diagnostics get-boot-log`


## <a name="january-31-2018"></a>2018 年 1 月 31 日

版本 2.0.26

### <a name="core"></a>核心

* 添加了支持在 MSI 上下文中检索原始令牌
* 删除了完成对 Windows cmd.exe 进行 LRO 操作后轮询指示器字符串
* 添加了将使用配置的默认值时显示的警告更改为信息级别条目。 请使用 `--verbose` 查看
* 为等待命令添加了进度指示器

### <a name="acs"></a>ACS

* 说明了 `--disable-browser` 参数
* 改进了 `--vm-size` 参数的 tab 键补全功能

### <a name="appservice"></a>应用服务

* 固定 `webapp log [tail|download]`
* 删除了对 Web 应用和函数的 `kind` 检查

### <a name="cdn"></a>CDN

* 修复了运行 `cdn custom-domain create` 时出现的缺少客户端问题

### <a name="cosmosdb"></a>CosmosDB

* 修复了故障转移策略的参数说明

### <a name="interactive"></a>交互

* 修复了不再显示命令选项补全的问题

### <a name="network"></a>网络

* 向 `application-gateway create` 添加了对 `--cert-password` 的保护
* 修复了 `application-gateway update` 出现的 `--sku` 错误应用默认值的问题
* 向 `vpn-connection create` 添加了对 `--shared-key` 和 `--authorization-key` 的保护
* 修复了运行 `asg create` 时出现的缺少客户端问题
* 向 `dns zone export` 添加了用于导出名称的 `--file-name / -f` 参数
* 修复了 `dns zone export` 存在的以下问题：
  * 修复了未正确导出长 TXT 记录的问题
  * 修复了不使用转义引号无法正确导出带引号的 TXT 记录的问题
* 修复了使用 `dns zone import` 某些记录会导入两次的问题
* 已还原 `vnet-gateway root-cert` 和 `vnet-gateway revoked-cert` 命令

### <a name="profile"></a>配置文件

* 修复了 `get-access-token`，使其在 VM 中使用标识正常工作

### <a name="resource"></a>资源

* 修复了 `deployment [create|validate]` 存在的 bug，即当模板的 type 字段包含大写值时错误地显示警告

### <a name="storage"></a>存储

* 修复了将存储 V1 帐户迁移到存储 V2 时出现的问题
* 为所有上传/下载命令添加了进度报告
* 修复了 `storage account check-name` 不显示“-n”参数选项的 bug
* 向 `blob [list|show]` 的表输出添加了“snapshot”列
* 修复了需要作为整数分析的各种参数的 bug

### <a name="vm"></a>VM

* 添加了 `vm image accept-terms` 命令，以允许使用额外费用从映像创建 VM
* 修复了 `[vm|vmss create]`，以确保可以在使用未签名证书的代理下运行命令
* [预览] 向 VMSS 添加了对“低”优先级的支持
* 向 `[vm|vmss] create` 添加了对 `--admin-password` 的保护


## <a name="january-17-2018"></a>2018 年 1 月 17 日

版本 2.0.25

### <a name="acr"></a>ACR

* 添加了发生 Windows 凭据错误时执行 acr 登录回退的功能
* 启用了注册表日志

### <a name="acs"></a>ACS

* 修复了 `get-credentials` 命令
* 删除了 SPN 角色要求

### <a name="appservice"></a>应用服务

* 修复了 `hosting_environment_profile` 为 null 时 `config ssl upload` 存在的 bug
* 为 `browse` 添加了自定义 URL 支持
* 修复了 `log tail` 的槽位支持

### <a name="backup"></a>备份

* 将 `backup item list` 的 `--container-name` 选项更改为可选
* 为 `backup restore restore-disks` 添加了存储帐户选项
* 修复了 `backup protection enable-for-vm` 中的位置检查，使之区分大小写
* 修复了容器名称无效时命令失败的问题
* 已将 `backup item list` 更改为默认包含“Health Status”

### <a name="batch"></a>Batch

* 已将 `batch login` 更改为返回身份验证详细信息

### <a name="cloud"></a>云

* 已更改为在云中设置 `--profile` 时不需要终结点

### <a name="consumption"></a>消耗

* 添加了用于预留的新命令：`consumption reservations summaries` 和 `consumption reservations details`

### <a name="event-grid"></a>事件网格

* [中断性变更E] 已将 `az eventgrid topic event-subscription` 命令变动为 `eventgrid event-subscription`
* [中断性变更E] 已将 `az eventgrid resource event-subscription` 命令变动为 `eventgrid event-subscription`
* [中断性变更E] 已删除 `eventgrid event-subscription show-endpoint-url` 命令。 请改用 `eventgrid event-subscription show --include-full-endpoint-url`
* 添加了命令 `eventgrid topic update`
* 添加了命令 `eventgrid event-subscription update`
* 为 `eventgrid topic` 命令添加了 `--ids` 参数
* 添加了主题名称的 Tab 键补全支持

### <a name="interactive"></a>交互

* 修复了无法在 Python 2.x 中使用交互模式的问题
* 修复了启动时出错的问题
* 修复了无法在交互模式下运行某些命令的问题

### <a name="iot"></a>IoT

* 添加了对设备预配服务的支持
* 在命令和命令帮助中添加了弃用消息
* 添加了 IoT 检查，以告知用户有 IoT 扩展可用

### <a name="monitor"></a>监视

* 添加了多诊断设置支持。 `az monitor diagnostic-settings create` 现在必需 `--name` 参数。
* 添加了命令 `monitor diagnostic-settings categories` 用于获取诊断设置类别

### <a name="network"></a>网络

* 修复了使用 `vnet-gateway update` 进入/退出主动-待机模式时出现的问题
* 在 `application-gateway [create|update]` 中添加了对 HTTP2 的支持

### <a name="profile"></a>配置文件

* 添加了使用用户分配的标识进行登录的支持

### <a name="role"></a>角色

* 为 `role assignment create` 添加了 `--assignee-object-id` 参数用于绕过图形查询

### <a name="service-fabric"></a>Service Fabric

* 在创建群集时生成的验证响应中添加了详细错误
* 修复了运行多个命令时出现的缺少客户端问题

### <a name="vm"></a>VM

* [预览] `vmss` 的跨区域支持
* [中断性变更E] 已将单区域 `vmss` 默认值更改为“Standard”负载均衡器
* [中断性变更E] 已将EMSI 的 `externalIdentities` 更改为 `userAssignedIdentities`
* [预览] 添加了 OS 磁盘交换支持
* 添加了使用其他订阅中的 VM 映像的支持
* 为 `[vm|vmss] create` 添加了 `--plan-name`、`--plan-product`、`--plan-promotion-code` 和 `--plan-publisher` 参数
* 修复了 `[vm|vmss] create` 出错问题
* 修复了 `vm image list --all` 导致资源使用过度的问题

## <a name="december-19-2017"></a>2017 年 12 月 19 日

版本 2.0.23

* 添加了使用用户分配的标识进行登录的支持

### <a name="container"></a>容器

* 纠正了容器日志参数的错误顺序

### <a name="network"></a>网络

* 为 `route-table [create|update]` 添加了 `--disable-bgp-route-propagation` 参数
* 为 `public-ip [create|update]` 添加了 `--ip-tags` 参数

### <a name="storage"></a>存储

* 添加了对存储 V2 的支持

### <a name="vm"></a>VM

* [预览] 添加了对 VM 和 VMSS 的用户分配标识的支持


## <a name="december-5-2017"></a>2017 年 12 月 5 日

版本 2.0.22

* 已删除 `az component` 命令。 请改用 `az extension`

### <a name="core"></a>核心
* 已将 `AZURE_US_GOV_CLOUD` AAD 颁发机构终结点从 login.microsoftonline.com 修改为 login.microsoftonline.us
* 已修复持续重新发送遥测数据的问题

### <a name="acs"></a>ACS

* 已添加 `aks install-connector` 和 `aks remove-connector` 命令
* 已改进 `acs create` 的错误报告
* 已修复不带完全限定路径的 `aks get-credentials -f` 的用法

### <a name="advisor"></a>顾问

* 初始版本

### <a name="appservice"></a>应用服务

* 已修复使用 `webapp config ssl upload` 时的证书名称生成问题
* 已修复 `webapp [list|show]` 和 `functionapp [list|show]` 以显示正确的应用
* 已为 `WEBSITE_NODE_DEFAULT_VERSION` 添加了默认值

### <a name="consumption"></a>消耗

* 已添加对 API 版本 2017-11-30 的支持

### <a name="container"></a>容器

* 已修复默认端口回归

### <a name="monitor"></a>监视

* 已添加对指标命令的多维支持

### <a name="resource"></a>资源

* 为 `resource show` 添加了 `--include-response-body` 参数

### <a name="role"></a>角色

* 已将“经典”管理员的默认分配显示添加到 `role assignment list`
* 已添加对 `ad sp reset-credentials` 的支持以便添加凭据而不是覆盖
* 已改进 `ad sp create-for-rbac` 的错误报告

### <a name="sql"></a>SQL

* 已添加 `sql db list-usages` 和 `sql db show-usage` 命令
* 已添加 `sql server conn-policy show` 和 `sql server conn-policy update` 命令

### <a name="vm"></a>VM

* 已对 `az vm list-skus` 添加区域信息


## <a name="november-14-2017"></a>2017 年 11 月 14 日

版本 2.0.21

### <a name="acr"></a>ACR

* 添加了在复制区域中创建 Webhook 的支持


### <a name="acs"></a>ACS

* 已在 AKS 中将所有“代理”一词更改为“节点”
* 弃用了 `acs create` 的 `--orchestrator-release` 选项
* 已将 AKS 的默认 VM 大小更改为 `Standard_D1_v2`
* 在 Windows 上修复了 `az aks browse`
* 在 Windows 上修复了 `az aks get-credentials`

### <a name="appservice"></a>应用服务

* 添加了 Web 应用和函数应用的部署源 `config-zip`
* 为 `az webapp log config` 添加了 `--docker-container-logging` 选项
* 从 `az webapp log config` 的参数 `--web-server-logging` 中删除了 `storage` 选项
* 完善了 `deployment user set` 的错误消息
* 添加了创建 Linux 函数应用的支持
* 固定 `list-locations`

### <a name="batch"></a>Batch

* 修复了在 pool create 命令中结合 `--image` 标志使用资源 ID 时出现的 bug

### <a name="batchai"></a>Batchai

* 在 `file-server create` 命令中提供了 `--vm-size` 的简短选项 `-s`（提供 VM 大小）
* 为 `cluster create` 参数添加了存储帐户名称和密钥自变量
* 纠正了 `job list-files` 和 `job stream-file` 的文档
* 在 `job create` 命令中提供了 `--cluster-name` 的简短选项 `-r`（提供群集名称）

### <a name="cloud"></a>云

* 更改了 `cloud [register|update]`，以防止注册缺少所需终结点的云

### <a name="container"></a>容器

* 添加了打开多个端口的支持
* 添加了容器组重启策略
* 添加了将 Azure 文件共享装载为卷的支持
* 更新了帮助器文档

### <a name="data-lake-analytics"></a>Data Lake Analytics

* 更改了 `[job|account] list` 以返回更简洁的信息

### <a name="data-lake-store"></a>Data Lake Store

* 更改了 `account list` 以返回更简洁的信息

### <a name="extension"></a>分机

* 添加了 `extension list-available` 用于列出官方的 Microsoft 扩展
* 为 `extension [add|update]` 添加了 `--name`，以便按名称安装扩展

### <a name="iot"></a>IoT

* 添加了对证书颁发机构 (CA) 和证书链的支持

### <a name="monitor"></a>监视

* 添加了 `activity-log alert` 命令

### <a name="network"></a>网络

* 添加了对 CAA DNS 记录的支持
* 修复了无法使用 `traffic-manager profile update` 更新终结点的问题
* 修复了在采用某种 VNET 创建方式时 `vnet update --dns-servers` 无法正常运行的问题
* 修复了 `dns zone import` 错误导入相对 DNS 名称的问题

### <a name="reservations"></a>预留

* 初始预览版

### <a name="resource"></a>资源

* 添加了在 `--resource` 参数中指定资源 ID 的支持，以及对资源级锁的支持

### <a name="sql"></a>SQL

* 为 `sql server vnet-rule [create|update]` 添加了 `--ignore-missing-vnet-service-endpoint` 参数

### <a name="storage"></a>存储

* 更改了 `storage account create` 以使用 SKU `Standard_RAGRS` 作为默认值
* 修复了处理包含非 ASCII 字符的文件/Blob 名称时出现的 bug
* 修复了阻止在 `storage [blob|file] copy start-batch` 中使用 `--source-uri` 的 bug
* 添加了在 `storage [blob|file] delete-batch` 中包含和删除多个对象的命令
* 修复了使用 `storage metrics update` 启用指标时出现的问题
* 修复了使用 `storage blob upload-batch` 时，如果文件超过 200GB 所出现的问题
* 修复了 `storage account [create|update]` 忽略 `--bypass` 和 `--default-action` 的问题

### <a name="vm"></a>VM

* 修复了 `vmss create` 阻止使用 `Basic` 大小层的 bug
* 针对包含计费信息的自定义映像，为 `[vm|vmss] create` 添加了 `--plan` 参数
* 添加了 `vm secret `[add|remove|list]` 命令
* 已将 `vm format-secret` 重命名为 `vm secret format`
* 为 `vm encryption enable` 添加了 `--encrypt format` 参数

## <a name="october-24-2017"></a>2017 年 10 月 24 日

版本 2.0.20

### <a name="core"></a>核心

* 已更新 `2017-03-09-profile` 以使用 `MGMT_STORAGE` API 版本 `2016-01-01`

### <a name="acr"></a>ACR

* 已更新资源管理以指向 `2017-10-01` API 版本
* 已将“带来你自己的存储”SKU 更改为“经典”
* 已将注册表 SKU 重命名为“基本”、“标准”和“高级”

### <a name="acs"></a>ACS

* [PREVIEW] 添加了 `az aks` 命令
* 已修复 Kubernetes `get-credentials`

### <a name="appservice"></a>应用服务

* 已修复所下载 `webapp` 日志可能无效的问题

### <a name="component"></a>组件

* 为所有安装程序添加了更清晰的弃用消息并添加了确认提示

### <a name="monitor"></a>监视

* 添加了 `action-group` 命令

### <a name="resource"></a>资源

* 修复了 `group export` 中与 msrest 依赖项的最新版本不兼容的问题
* 修复了 `policy assignment create` 以使用内置策略定义和策略集定义

### <a name="vm"></a>VM

* 为 `vmss create` 添加了 `--accelerated-networking` 参数


## <a name="october-9-2017"></a>2017 年 10 月 9 日

版本 2.0.19

### <a name="core"></a>核心

* 在 Azure Stack 中添加了一个尾随斜杠用于处理 ADFS 机构 URL

### <a name="appservice"></a>应用服务

* 添加了新命令 `webapp update` 用于执行常规更新

### <a name="batch"></a>Batch

* 已更新为 Batch SDK 4.0.0
* 更新了 VirtualMachineConfiguration 的 `--image`，用于支持除 publish:offer:sku:version 以外的 ARM 映像引用
* 添加了对 Batch 扩展命令新 CLI 扩展模型的支持
* 从组件模型中删除了 Batch 支持

### <a name="batchai"></a>Batchai

* Batch AI 模块初始版本

### <a name="keyvault"></a>KeyVault

* 修复了在 Azure Stack 中使用 ADFS 时发生的 Key Vault 身份验证问题。 [(#4448)](https://github.com/Azure/azure-cli/issues/4448)

### <a name="network"></a>网络

* 已将 `application-gateway address-pool create` 的 `--server` 参数更改为可选，以允许空地址池
* 更新了 `traffic-manager` 以支持最新功能

### <a name="resource"></a>资源

* 在 `group` 中添加了对资源组名称使用 `--resource-group/-g` 选项的支持
* 为 `account lock` 添加了命令用于处理订阅级锁
* 为 `group lock` 添加了命令用于处理组级锁
* 为 `resource lock` 添加了命令用于处理资源级锁

### <a name="sql"></a>Sql

* 添加了 SQL 透明数据加密 (TDE) 和自带密钥 TDE 的支持
* 添加了 `db list-deleted` 命令和 `db restore --deleted-time` 参数，以便能够找到和还原已删除的数据库
* 添加了 `db op list` 和 `db op cancel`，以便能够列出和取消正在对数据库执行的操作

### <a name="storage"></a>存储

* 添加了对文件共享快照的支持

### <a name="vm"></a>Vm

* 修复了 `vm show` 中的一个 bug：在缺少专用 IP 地址的情况下使用 `-d` 会导致崩溃
* [预览] 添加了滚动升级到 `vmss create` 的支持
* 添加了使用 `vm encryption enable` 更新加密设置的支持
* 为 `vm create` 添加了 `--os-disk-size-gb` 参数
* 为 Windows 中的 `vmss create` 添加了 `--license-type` 参数


## <a name="september-22-2017"></a>2017 年 9 月 22 日

版本 2.0.18

### <a name="resource"></a>资源

* 添加了对显示内置策略定义的支持
* 添加了用于创建策略定义的支持模式参数
* 为 `managedapp definition create` 添加了对 UI 定义和模板的支持
* [中断性变更E] 已将 `managedapp` 资源类型从 `appliances` 更改到 `applications`，从 `applianceDefinitions` 更改到 `applicationDefinitions`

### <a name="network"></a>网络

* 为 `network lb` 和 `network public-ip` 子命令添加了对可用性区域的支持
* 为 `express-route` 添加了对 IPv6 Microsoft 对等互连的支持
* 添加了 `asg` 应用程序安全组命令
* 为 `nic [create|ip-config create|ip-config update]` 添加了 `--application-security-groups` 参数
* 为 `nsg rule [create|update]` 添加了 `--source-asgs` 和 `--destination-asgs` 参数
* 为 `vnet [create|update]` 添加了 `--ddos-protection` 和 `--vm-protection` 参数
* 添加了 `network [vnet-gateway|vpn-client|show-url]` 命令

### <a name="storage"></a>存储

* 已修复了 `storage account network-rule` 命令在更新 SDK 后可能会失败的问题

### <a name="eventgrid"></a>Eventgrid

* 更新了 Azure 事件网格 Python SDK 以使用较新的 API 版本“2017-09-15-preview”

### <a name="sql"></a>SQL

* 将 `sql server list` 的参数 `--resource-group` 更改为可选。 如果未指定，将返回订阅中的所有 SQL 服务器
* 为 `db [create|copy|restore|update|replica create|create|update]` 和 `dw [create|update]` 添加了 `--no-wait` 参数

### <a name="keyvault"></a>KeyVault

* 添加了对从代理后执行 Keyvault 命令的支持

### <a name="vm"></a>VM

* 为 `[vm|vmss|disk] create` 添加了对可用性区域的支持
* 已修复了将 `--app-gateway ID` 与 `vmss create` 一起使用会导致故障的问题
* 为 `vm create` 添加了 `--asgs` 参数
* 添加了对使用 `vm run-command` 在 VM 上运行命令的支持
* [预览] 添加了对使用 `vmss encryption` 进行 VMSS 磁盘加密的支持
* 添加了对使用 `vm perform-maintenance` 在 VM 上执行维护的支持

### <a name="acs"></a>ACS

* [预览] 为适用于 ACS 预览区域的 `acs create` 添加了 `--orchestrator-release` 参数

### <a name="appservice"></a>应用服务

* 添加了使用 `webapp auth [update|show]` 更新和显示身份验证设置的功能

### <a name="backup"></a>备份

* 预览版


## <a name="september-11-2017"></a>2017 年 9 月 11 日

版本 2.0.17

### <a name="core"></a>核心

* 启用了命令模块在遥测中设置其自己的相关 ID
* 修复了在遥测设置为诊断模式时的 JSON 转储问题

### <a name="acs"></a>Acs

* 添加了 `acs list-locations` 命令
* 使 `ssh-key-file` 附带预期的默认值

### <a name="appservice"></a>应用服务

* 添加了在未包含活动服务计划的资源组中创建 Web 应用的功能

### <a name="cdn"></a>CDN

* 修复了 `cdn custom-domain create` 的“CustomDomain is not interable”bug

### <a name="extension"></a>分机

* 初始版本

### <a name="keyvault"></a>KeyVault

* 为 `keyvault set-policy` 修复了权限区分大小写的问题

### <a name="network"></a>网络

* 已将 `vnet list-private-access-services` 重命名为 `vnet list-endpoint-services`
* 已为 `vnet subnet create/update` 将 `--private-access-services` 参数重命名为 `--service-endpoints`
* 在 `nsg rule create/update` 中添加了对多个 IP 范围和端口范围的支持
* 在 `lb create` 中添加了对 SKU 的支持
* 在 `public-ip create` 中添加了对 SKU 的支持

### <a name="resource"></a>资源

* 允许在 `policy definition create` 和 `policy definition update` 中传入资源策略参数定义
* 允许为 `policy assignment create` 传入参数值
* 允许为所有参数传入 JSON 或文件
* 更新了 API 版本

### <a name="sql"></a>SQL

* 添加了 `sql server vnet-rule` 命令

### <a name="vm"></a>VM

* 已修复：除非提供 `--scope`，否则不分配访问权限
* 已修复：使用与门户相同的扩展命名
* 已从 `[vm|vmss] create` 输出中删除了 `subscription`
* 已修复：`[vm|vmss] create` SKU 无法应用于带映像的数据磁盘
* 已修复：`vm format-secret --secrets` 不接受新行分隔的 ID

## <a name="august-31-2017"></a>2017 年 8 月31 日

版本 2.0.16

### <a name="keyvault"></a>KeyVault

* 修复了在尝试使用 `secret download` 自动解析机密编码时的 bug

### <a name="sf"></a>Sf

* 弃用所有支持 Service Fabric CLI (sfctl) 的命令

### <a name="storage"></a>存储

* 修复了无法在不支持 NetworkACLs 功能的区域中创建存储帐户的问题
* 在 Blob 和文件上载过程中确定内容类型和内容编码（如果既未指定内容类型，也未指定内容编码）

## <a name="august-28-2017"></a>2017 年 8 月 28 日

版本 2.0.15

### <a name="cli"></a>CLI

* 在 `--version` 中添加了法律说明

### <a name="acs"></a>ACS

* 更正了预览区域
* 正确设置了默认 `dns_name_prefix` 的格式
* 优化了 acs 命令输出

### <a name="appservice"></a>应用服务

* [中断性变更E] 修复了 `az webapp config appsettings [delete|set]` 输出中的不一致
* 为 `az webapp config container set --docker-custom-image-name` 的 `-i` 添加了新别名
* 公开了 `az webapp log show`
* 公开了 `az webapp delete` 中的新参数，用于保留应用服务计划、指标或 dns 注册
* 已修复：正确检测槽位设置

### <a name="iot"></a>IoT

* 修复了 #3934：策略创建操作不再清除现有策略

### <a name="network"></a>网络

* [中断性变更E] 已将 `vnet list-private-access-services` 重命名为 `vnet list-endpoint-services`
* [中断性变更E] 已将 `vnet subnet [create|update]` 的选项 `--private-access-services` 重命名为 `--service-endpoints`
* 在 `nsg rule [create|update]` 中添加了对多个 IP 和端口范围的支持
* 在 `lb create` 中添加了对 SKU 的支持
* 在 `public-ip create` 中添加了对 SKU 的支持

### <a name="profile"></a>配置文件

* 公开了 `--msi` 和 `--msi-port`，以便使用虚拟机的标识登录

### <a name="service-fabric"></a>Service Fabric

* 预览版
* 简化了命令的注册表用户/密码规则
* 修复了即使在参数中传入了密码，也提示用户输入密码的问题
* 添加了对空 `registry_cred` 的支持

### <a name="storage"></a>存储

* 启用了设置 Blob 层
* 为 `storage account [create|update]` 添加了 `--bypass` 和 `--default-action` 参数用于支持服务隧道
* 添加了用于在 `storage account network-rule` 中添加 VNET 规则和基于 IP 的规则的命令
* 启用了使用客户管理的密钥进行服务加密的功能
* [中断性变更E] 已将 `az storage account create and az storage account update` 命令的选项 `--encryption` 重命名为 `--encryption-services`
* 修复了 #4220：`az storage account update encryption` - 语法不匹配

### <a name="vm"></a>VM

* 修复了使用 `--instance-id *` 时，针对 `vmss get-instance-view` 显示多余且错误的信息的问题
* 在 `vmss create` 中添加了对 `--lb-sku` 的支持：
* 从 `[vm|vmss] create` 的管理员名称阻止列表中删除了人员名称
* 修复了当无法从映像中提取计划信息时，`[vm|vmss] create` 引发错误的问题
* 修复了创建包含内部 LB 的 vmms 规模集时发生崩溃的问题
* 修复了 `--no-wait` 参数无法配合 `vm availability-set create` 工作的问题


## <a name="august-15-2017"></a>2017 年 8 月 15 日

版本 2.0.14

### <a name="acs"></a>ACS

* 更正了 kubernetes 的 sshMaster0 端口号

### <a name="appservice"></a>应用服务

* 修复了创建基于新 Git 的 Linux Web 应用时发生异常的问题

### <a name="event-grid"></a>事件网格

* 添加了 SDK 依赖项

## <a name="august-11-2017"></a>2017 年 8 月 11 日

版本 2.0.13

### <a name="acs"></a>ACS

* 添加了更多预览区域

### <a name="batch"></a>Batch

* 已更新到 Batch SDK 3.1.0 和 Batch Management SDK 4.1.0
* 添加了新命令用于显示作业的任务计数
* 修复了处理资源文件 SAS URL 时的 bug
* Batch 帐户终结点现在支持可选的 “https://” 前缀
* 支持将包含 100 多个任务的列表添加到作业
* 添加了加载扩展命令模块的调试日志记录

### <a name="component"></a>组件

* 为“az component”命令添加了弃用警告

### <a name="container"></a>容器

* `create`：修复了某个环境变量中不允许等于号的问题


### <a name="data-lake-store"></a>Data Lake Store

* 启用了进度控件

### <a name="event-grid"></a>事件网格

* 初始版本

### <a name="network"></a>网络

* `lb`：修复了某些子资源名称在省略时无法正确解析的问题
* `application-gateway {subresource} delete`：修复了不遵循 `--no-wait` 的问题
* `application-gateway http-settings update`：修复了无法关闭 `--connection-draining-timeout` 的问题
* 修复了 `az network vpn-connection ipsec-policy add` 包含意外关键字参数 `sa_data_size_kilobyes` 的错误

### <a name="profile"></a>配置文件

* `account list`：添加了 `--refresh` 以用于从服务器同步最新订阅

### <a name="storage"></a>存储

* 启用了使用系统分配的标识更新存储帐户的功能

### <a name="vm"></a>VM

* `availability-set`：公开了转换时的容错域计数
* 公开了 `list-skus` 命令
* 支持在不创建角色分配的情况下分配标识
* 附加数据磁盘时应用存储 SKU
* 删除了使用托管磁盘时的默认 OS 磁盘名称和存储 SKU


## <a name="july-28-2017"></a>2017 年 7 月 28 日

版本 2.0.12

* 添加了容器命令
* 添加了计费和消耗模块

```text
azure-cli (2.0.12)

acr (2.0.9)
acs (2.0.11)
appservice (0.1.11)
batch (3.0.3)
billing (0.1.3)
cdn (0.0.6)
cloud (2.0.7)
cognitiveservices (0.1.6)
command-modules-nspkg (2.0.1)
component (2.0.6)
configure (2.0.10)
consumption (0.1.3)
container (0.1.7)
core (2.0.12)
cosmosdb (0.1.11)
dla (0.0.10)
dls (0.0.11)
feedback (2.0.6)
find (0.2.6)
interactive (0.3.7)
iot (0.1.10)
keyvault (2.0.8)
lab (0.0.9)
monitor (0.0.8)
network (2.0.11)
nspkg (3.0.1)
profile (2.0.9)
rdbms (0.0.5)
redis (0.2.7)
resource (2.0.11)
role (2.0.9)
sf (1.0.5)
sql (2.0.8)
storage (2.0.11)
vm (2.0.11)
```

### <a name="core"></a>核心

* 输出包含证书的服务主体的 SDK 身份验证信息
* 修复了部署进度异常
* 使用当前云中的 arm 终结点创建订阅客户端
* 改进了 clouds.config 文件的并发处理 (#3636)
* 刷新每个命令执行进程的客户端请求 ID
* 使用适当的 SDK 配置文件创建订阅客户端 (#3635)
* 模板部署的进度报告 (#3510)
* 添加了通过 jmespath 查询选择表输出字段的支持 (#3581)
* 改进了分析参数的静默和包含手势的追加历史记录 (#3434)
* 使用适当的 SDK 配置文件创建订阅客户端
* 将所有现有记录文件移到最新的文件夹
* 修复了 VM/VMSS 创建操作的幂等性 (#3586)
* 命令路径不再区分大小写
* 某些布尔类型的参数不再区分大小写
* 支持登录到 Azure Stack 等本地服务器上的 ADFS
* 修复了并发写入 clouds.config 的问题 (#3255)

### <a name="acr"></a>ACR

* 针对托管注册表添加了 `show-usage` 命令
* 支持托管注册表的 SKU 更新
* 添加了包含托管 SKU 的托管注册表
* 通过 ACR Webhook 命令模块添加了托管注册表的 Webhook
* 添加了使用 acr login 命令进行 AAD 身份验证的功能
* 添加了 Docker 存储库、清单和标记的 delete 命令

### <a name="acs"></a>ACS

* API 版本 2017-07-01 的支持

### <a name="appservice"></a>应用服务

* 修复了列出 Linux Web 应用时不返回任何内容的 bug
* 支持从 ACR 检索凭据
* 删除 `appservice web` 下的所有命令
* 将命令输出中的 Docker 注册表密码掩码 (#3656)
* 确保在 macOS 上使用默认浏览器且不出错 (#3623)
* 改进了 `webapp log tail` 和 `webapp log download` 的帮助 (#3624)
* 公开了 `traffic-routing` 命令用于配置静态路由 (#3566)
* 添加了用于配置源代码管理的可靠性修复 (#3245)
* 从 `webapp config update` 中删除了 Windows Web 应用不支持的 `--node-version` 参数。 需改用 `webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=...`。

### <a name="batch"></a>Batch

* 已更新到 Batch SDK 3.0.0，支持池中的低优先级 VM
* 已将 `pool create` 选项 `--target-dedicated` 重命名为 `--target-dedicated-nodes`
* 添加了 `pool create` 选项 `--target-low-priority-nodes` 和 `--application-licenses`

### <a name="cdn"></a>CDN

* 当 `--profile-name` 指定的配置文件不存在时，为 `cdn endpoint list` 提供更完善的错误消息

### <a name="cloud"></a>云

* 已将云元数据终结点的 API 版本更改为 YYYY-MM-DD 格式
* 不需要库终结点
* 支持只将云注册到 ARM 资源管理器终结点
* 提供 `cloud set` 的选项用于在选择当前云时选择配置文件
* 公开了 `endpoint_vm_image_alias_doc`

### <a name="cosmosdb"></a>CosmosDB

* 修复了允许使用自定义分区键创建集合的问题
* 添加了对集合默认 TTL 的支持

### <a name="data-lake-analytics"></a>Data Lake Analytics

* 在 `dla account compute-policy` 标题下添加了用于计算策略管理的命令
* 添加了 `dla job pipeline show`
* 添加了 `dla job recurrence list`

### <a name="data-lake-store"></a>Data Lake Store

* 在 `dls account update` 中添加了用户管理的 Key Vault 密钥轮换的支持
* 更新了底层 Data Lake Store 文件系统 SDK 版本，解决了一个性能问题
* 添加了命令 `dls enable-key-vault`。 此命令尝试使用用户提供的 Key Vault 加密 Data Lake Store 帐户中的数据

### <a name="interactive"></a>交互

* 使用缓存的命令改进启动时间
* 增大了测试覆盖率
* 增强了“?”手势，使之也可注入到下一条命令
* 修复了配置文件 2017-03-09-profile-preview 的交互错误 (#3587)
* 允许使用 `--version` 作为交互模式的参数 (#3645)
* 阻止交互模式在验证填写内容中引发错误 (#3570)
* 模板部署的进度报告 (#3510)
* 添加了 `--progress` 标志
* 从填写内容中删除了 `--debug` 和 `--verbose`
* 从填写内容中删除了 `interactive` (#3324)

### <a name="iot"></a>IoT

* 修复了策略创建操作不再清除现有策略的问题。 (#3934)

### <a name="key-vault"></a>密钥保管库

* 添加了 Key Vault 恢复功能的命令：
  * `keyvault` 子命令 `purge`、`recover`、`keyvault list-deleted`
  * `keyvault secret` 子命令 `backup`、`restore`、`purge`、`recover`、`list-deleted`
  * `keyvault certificate` 子命令 `purge`、`recover`、`list-deleted`
  * `keyvault key` 子命令 `purge`、`recover`、`list-deleted`
* 添加了服务主体的 Key Vault 集成 (#3133)
* 已将 Key Vault 数据平面更新到 0.3.2。 (#3307)

### <a name="lab"></a>实验室

* 添加了通过 `az lab vm claim` 在实验室中声明任何 VM 的支持
* 为 `az lab vm list` 和 `az lab vm show` 添加了表输出格式化程序

### <a name="monitor"></a>监视

* 修复了与 `monitor autoscale-settings get-parameters-template` 命令结合使用的模板文件 (#3349)
* 已将 `monitor alert-rule-incidents list` 重命名为 `monitor alert list-incidents`
* 已将 `monitor alert-rule-incidents show` 重命名为 `monitor alert show-incident`
* 已将 `monitor metric-defintions list` 重命名为 `monitor metrics list-definitions`
* 已将 `monitor alert-rules` 重命名为 `monitor alert`
* 更改了 `monitor alert create`：
  * `condition` 和 `action` 子命令不再接受 JSON
  * 添加了大量的参数来简化规则创建过程
  * `location` 不再是必需的
  * 添加了目标的名称和 ID 支持
  * 删除了 `--alert-rule-resource-name`
  * `is-enabled` 重命名为 `enabled`，不再是必需的
  * `description` 默认值现在基于提供的条件
  *  添加了示例用于帮助澄清新格式
* 支持在 `monitor metric` 命令中使用名称或 ID
* 为 `monitor alert rule update` 添加了方便的参数和示例

### <a name="network"></a>网络

* 添加了 `list-private-access-services` 命令
* 为 `vnet subnet create` 和 `vnet subnet update` 添加了 `--private-access-services` 参数
* 修复了 `application-gateway redirect-config create` 失败的问题
* 修复了无法结合 `--no-wait` 使用 `application-gateway redirect-config update` 的问题
* 修复了结合 `application-gateway address-pool create` 和 `application-gateway address-pool update` 使用 `--servers` 参数时的 bug
* 添加了 `application-gateway redirect-config` 命令
* 添加了 `application-gateway ssl-policy` 的命令：`list-options`、`predefined list`、`predefined show`
* 添加了 `application-gateway ssl-policy set` 的参数：`--name`、`--cipher-suites`、`--min-protocol-version`
* 添加了 `application-gateway http-settings create` 和 `application-gateway http-settings update` 的参数：`--host-name-from-backend-pool`、`--affinity-cookie-name`、`--enable-probe`、`--path`
* 添加了 `application-gateway url-path-map create` 和 `application-gateway url-path-map update` 的参数：`--default-redirect-config`、`--redirect-config`
* 为 `application-gateway url-path-map rule create` 添加了 `--redirect-config` 参数
* 在 `application-gateway url-path-map rule delete` 中添加了对 `--no-wait` 的支持
* 添加了 `application-gateway probe create` 和 `application-gateway probe update` 的参数：`--host-name-from-http-settings`、`--min-servers`、`--match-body`、`--match-status-codes`
* 为 `application-gateway rule create` 和 `application-gateway rule update` 添加了 `--redirect-config` 参数
* 在 `nic create` 和 `nic update` 中添加了对 `--accelerated-networking` 的支持
* 从 `nic create` 中删除了 `--internal-dns-name-suffix` 参数
* 在 `nic update` 和 `nic create` 中添加了对 `--dns-servers` 的支持：添加了对 --dns-servers 的支持
* 修复了 `local-gateway create` 忽略 `--local-address-prefixes` 的 bug
* 在 `vnet update` 中添加了对 `--dns-servers` 的支持
* 修复了使用 `express-route peering create` 创建不包含路由筛选的对等互连时的 bug
* 修复了无法结合 `--provider` 和 `--bandwidth` 参数使用 `express-route update` 的 bug
* 修复了 `network watcher show-topology` 默认逻辑的 bug
* 改进了 `network list-usages` 的输出格式
* 如果只存在一个，则对 `application-gateway http-listener create` 使用默认前端 IP
* 如果只存在一个，则对 `application-gateway rule create` 使用默认地址池、HTTP 设置和 HTTP 侦听器
* 如果只存在一个，则对 `lb rule create` 使用默认前端 IP 和后端池
* 如果只存在一个，则对 `lb inbound-nat-rule create` 使用默认前端 IP

### <a name="profile"></a>配置文件

* 支持使用托管标识在 VM 内部登录
* 支持采用 SDK 身份验证文件格式的 `account show` 输出
* 使用 '--expanded-view' 时显示弃用警告
* 添加了 `get-access-token` 命令来提供原始 AAD 令牌
* 支持使用不包含关联订阅的用户帐户登录

### <a name="rdbms"></a>RDBMS

* 支持跨订阅列出服务器 (#3417)
* 修复了由于缺少 `% server_type` 而不处理 `%s` 的问题 (#3393)
* 修复了文档源映射并添加了 CI 任务用于验证 (#3361)
* 修复了 MySQL 和 PostgreSQL 帮助 (#3369)

### <a name="resource"></a>资源

* 改进了有关 `group deployment create` 缺少参数的提示
* 改进了 `--parameters KEY=VALUE` 语法分析
* 修复了不再能够使用 `@<file>` 语法识别 `group deployment create` 参数文件的问题
* 支持 `resource` 和 `managedapp` 命令的 `--ids` 参数
* 修复了一些分析和错误消息 (#3584)
* 修复了 `lock` 命令的 `--resource-type` 分析，接受 `<resource-namespace>` 和 `<resource-type>`
* 添加了模板链接模板的参数检查 (#3629)
* 添加了使用 `KEY=VALUE` 语法指定部署参数的支持

### <a name="role"></a>角色

* 支持采用 SDK 身份验证文件格式的 `create-for-rbac` 输出
* 删除服务主体时清理角色分配和相关的 AAD 应用程序 (#3610)
* 在 `app create` 参数 `--start-date` 和 `--end-date` 说明中包含时间格式
* 使用 `--expanded-view` 时显示弃用警告
* 在 `create-for-rbac` 和 `reset-credentials` 命令中添加了 Key Vault 集成

### <a name="service-fabric"></a>Service Fabric
* 修复了上传时截断应用程序中的大型文件的问题 (#3666)
* 添加了 Service Fabric 命令的测试 (#3424)
* 添加了大量的 Service Fabric 命令 (#3234)

### <a name="sql"></a>SQL

* 删除了无效的 `sql server create` `--identity` 参数
* 从 `sql server create` 和 `sql server update` 命令输出中删除了密码值
* 添加了命令 `sql db list-editions` 和 `sql elastic-pool list-editions`

### <a name="storage"></a>存储

* 从 `storage blob list`、`storage container list` 和 `storage share list` 命令中删除了 `--marker` 选项 (#3745)
* 启用了创建仅限 https 的存储帐户
* 更新了存储指标、日志记录和 CORS 命令 (#3495)
* 重新编写了 CORS add 命令的异常消息 (#3638) (#3362)
* 已在下载批处理命令试运行模式下将生成器转换为列表 (#3592)
* 修复了 Blob 下载批处理试运行问题 (#3640) (#3592)

### <a name="vm"></a>VM

* 支持配置 NSG
* 修复了无法正确配置 DNS 服务器的 bug
* 支持托管服务标识
* 修复了包含现有负载均衡器的 `cmss create` 需要 `--backend-pool-name` 的问题
* 要求使用 `vm image create` LUN 创建的数据磁盘以 0 开头


## <a name="may-10-2017"></a>2017 年 5 月 10 日

版本 2.0.6

* Documentdb 已重命名为 Cosmosdb
* 添加 rdbms (mysql, postgres)
* 包括 Data Lake Analytics 和 Data Lake Store 模块
* 包括认知服务模块
* 包括 Service Fabric 模块
* 包括交互式模块（重命名 az-shell）
* 添加对 CDN 命令的支持
* 删除容器模块
* 添加“az -v”作为“az --version”的快捷方式 ([#2926](https://github.com/Azure/azure-cli/issues/2926))
* 提高加载包和执行命令的性能 ([#2819](https://github.com/Azure/azure-cli/issues/2819))

```text
azure-cli (2.0.6)

acr (2.0.4)
acs (2.0.6)
appservice (0.1.6)
batch (2.0.4)
cdn (0.0.2)
cloud (2.0.2)
cognitiveservices (0.1.2)
command-modules-nspkg (2.0.0)
component (2.0.4)
configure (2.0.6)
core (2.0.6)
cosmosdb (0.1.6)
dla (0.0.6)
dls (0.0.6)
feedback (2.0.2)
find (0.2.2)
interactive (0.3.1)
iot (0.1.5)
keyvault (2.0.4)
lab (0.0.4)
monitor (0.0.4)
network (2.0.6)
nspkg (3.0.0)
profile (2.0.4)
rdbms (0.0.1)
redis (0.2.3)
resource (2.0.6)
role (2.0.4)
sf (1.0.1)
sql (2.0.3)
storage (2.0.6)
vm (2.0.6)
```

### <a name="core"></a>核心

* 核心：捕获未注册提供程序引发的异常并自动注册
* 性能：将 ADAL 令牌缓存保留在内存中，直至进程退出 ([#2603](https://github.com/Azure/azure-cli/issues/2603))
* 修复从十六进制指纹 -o tsv 返回的字节 ([#3053](https://github.com/Azure/azure-cli/issues/3053))
* 改进的 Key Vault 证书下载和 AAD SP 集成 ([#3003](https://github.com/Azure/azure-cli/issues/3003))
* 将 Python 位置添加到“az —version”([#2986](https://github.com/Azure/azure-cli/issues/2986))
* 登录：无订阅时支持登录 ([#2929](https://github.com/Azure/azure-cli/issues/2929))
* 核心：修复重复使用服务主体登录时出现的故障 ([#2800](https://github.com/Azure/azure-cli/issues/2800))
* 核心：允许通过 env var 配置 accessTokens.json 的文件路径 ([#2605](https://github.com/Azure/azure-cli/issues/2605))
* 核心：允许配置的默认值应用于可选参数 ([#2703](https://github.com/Azure/azure-cli/issues/2703))
* 核心：提高了性能
* 核心：自定义 CA 证书 - 支持设置 REQUESTS_CA_BUNDLE 环境变量
* 核心：云配置 - 如果未设置“management”终结点，则使用“resource manager”终结点

### <a name="acs"></a>ACS

* 将主计数和代理计数修复为整数而不是字符串
* 公开“az acs create --no-wait”和“az acs wait”用于异步创建
* 公开“az acs create --validate”用于试运行验证
* 在 PUT 调用 scale 命令前删除 Windows 配置文件 ([#2755](https://github.com/Azure/azure-cli/issues/2755))

### <a name="appservice"></a>应用服务

* Function App：添加完整的 Function App 支持，包括 create、show、list、delete、hostname、ssl 等
* 将 Team Services (vsts) 作为持续交付选项添加到“appservice web source-control config”
* 创建“az webapp”以替换“az appservice web”（为了向后兼容，“az appservice web”将保留 2 个版本）
* 公开参数以针对 webapp create 配置部署和“运行时堆栈”
* 公开“webapp list-runtimes”
* 支持配置连接字符串 ([#2647](https://github.com/Azure/azure-cli/issues/2647))
* 支持与预览版交换槽
* 修改 appservice 命令的错误 ([#2948](https://github.com/Azure/azure-cli/issues/2948))
* 将应用服务计划的资源组用于证书操作 ([#2750](https://github.com/Azure/azure-cli/issues/2750))

### <a name="cosmosdb"></a>CosmosDB

* 将 documentdb 模块重命名为 cosmosdb
* 增加对 DocumentDB 数据平面 API 的支持：数据库和集合管理
* 增加对数据库帐户启用自动故障转移的支持
* 增加对新一致性策略 ConsistentPrefix 的支持

### <a name="data-lake-analytics"></a>Data Lake Analytics

* 修复了在筛选作业结果和状态列表时会引发错误的 bug
* 增加对新目录项类型的支持：包。 访问方法：`az dla catalog package`
* 可从数据库内列出下列目录项（无需架构规范）：

  * 表
  * 表值函数
  * 查看
  * 表统计信息。 这也可以使用架构（但无需指定表名）列出

### <a name="data-lake-store"></a>Data Lake Store

* 更新基础文件系统 SDK 的版本，以便为处理服务器端限制场景提供更好的支持
* 提高加载包和执行命令的性能 ([#2819](https://github.com/Azure/azure-cli/issues/2819))
* 访问显示帮助丢失。 正在添加。 ([#2743](https://github.com/Azure/azure-cli/issues/2743))

### <a name="find"></a>查找

* 改进搜索结果，允许搜索索引的版本控制

### <a name="keyvault"></a>KeyVault

* BC：`az keyvault certificate download` 将 -e 从字符串或二进制更改为 PEM 或 DER，从而更好地表示选项
* BC：从 `keyvault certificate create` 中删除了 --expires 和 --not-before，因为此服务不支持这些参数
* 将 --validity 参数添加到 `keyvault certificate create`，有选择地替代 --policy 中的值
* 修复了 `keyvault certificate get-default-policy` 中已公开“expires”和“not_before”但未公开“validity_in_months”的问题
* KeyVault 解决了 pem 和 pfx 的导入问题 ([#2754](https://github.com/Azure/azure-cli/issues/2754))

### <a name="lab"></a>实验室

* 为实验室中的环境添加 create、show、delete 和 list 命令
* 添加 show 和 list 命令以查看实验室中的 ARM 模板
* 在 `az lab vm list` 中添加 --environment 标志，以便按实验室中的环境筛选 VM
* 添加方便命令 `az lab formula export-artifacts`，以便导出实验室公式中的项目基架
* 添加命令以管理实验室中的机密

### <a name="monitor"></a>监视

* Bug 修复：将 `az alert-rules create` 的 `--actions` 建模为使用 JSON 字符串 ([#3009](https://github.com/Azure/azure-cli/issues/3009))
* Bug 修复 - diagnostic settings create 不接受来自 show 命令的日志/指标 ([#2913](https://github.com/Azure/azure-cli/issues/2913))

### <a name="network"></a>网络

* 添加 `network watcher test-connectivity` 命令
* 为 `network watcher packet-capture create` 添加对 `--filters` 参数的支持
* 添加对应用程序网关连接排出的支持
* 添加对应用程序网关 WAF 规则集配置的支持
* 添加对 ExpressRoute 路由筛选器和规则的支持
* 添加对 TrafficManager 地理路由的支持
* 添加对基于 VPN 连接策略的流量选择器的支持
* 添加对 VPN 连接 IPSec 策略的支持
* 修复使用 `--no-wait` 或 `--validate` 参数时 `vpn-connection create` 出现的 bug
* 添加对主动-主动 VNet 网关的支持
* 从 `network vpn-connection list/show` 命令的输出中删除 null 值
* BC：修复了 `vpn-connection create` 的输出中的 bug
* 修复无法正确分析“vpn-connection create”的“--key-length”参数的 bug
* 修复 `dns zone import` 中无法正确导入记录的 bug
* 修复 `traffic-manager endpoint update` 不起作用的 bug
* 添加“network watcher”预览命令

### <a name="profile"></a>配置文件

* 支持在未找到订阅时登录 ([#2560](https://github.com/Azure/azure-cli/issues/2560))
* 支持在 az account set --subscription 中使用短参数名 ([#2980](https://github.com/Azure/azure-cli/issues/2980))

### <a name="redis"></a>Redis

* 添加 update 命令，也增加了对 redis 缓存进行缩放的功能
* 弃用“update-settings”命令

### <a name="resource"></a>资源

* 添加 managedapp 和 managedapp 定义命令 ([#2985](https://github.com/Azure/azure-cli/issues/2985))
* 支持“provider operation”命令([#2908](https://github.com/Azure/azure-cli/issues/2908))
* 支持 generic resource create ([#2606](https://github.com/Azure/azure-cli/issues/2606))
* 修复资源分析和 API 版本查找。 ([#2781](https://github.com/Azure/azure-cli/issues/2781))
* 为 az lock update 添加文档。 ([#2702](https://github.com/Azure/azure-cli/issues/2702))
* 尝试为不存在的组列出资源时出错。 ([#2769](https://github.com/Azure/azure-cli/issues/2769))
* [计算] 修复 VMSS 和 VM 可用性集更新的相关问题。 ([#2773](https://github.com/Azure/azure-cli/issues/2773))
* parent-resource-path 为 None 时修复 lock create 和 delete ([#2742](https://github.com/Azure/azure-cli/issues/2742))

### <a name="role"></a>角色

* create-for-rbac：确保 SP 的结束日期不超过证书的到期日期 ([#2989](https://github.com/Azure/azure-cli/issues/2989))
* RBAC：添加对“ad group”的完整支持 ([#2016](https://github.com/Azure/azure-cli/issues/2016))
* role：解决角色定义更新的相关问题 ([#2745](https://github.com/Azure/azure-cli/issues/2745))
* create-for-rbac：确保已选取用户提供的密码

### <a name="sql"></a>SQL

* 添加了 az sql server list-usages 和 az sql db list-usages 命令
* SQL - 能够直接连接到资源提供程序 ([#2832](https://github.com/Azure/azure-cli/issues/2832))

### <a name="storage"></a>存储

* 对于 `storage account create`，将位置默认为资源组位置
* 添加对增量 blob 复制的支持
* 添加对大型块 blob 上传的支持
* 如果要上传的文件大于 200GB，则将块大小更改为 100MB

### <a name="vm"></a>VM

* avail-set：将 UD 和 FD 域计数设为可选

  注意：最高等级云中的 VM 命令。请避免与托管磁盘相关的功能，包括以下项：
  1. az disk/snapshot/image
  2. az vm/vmss disk
  3. 在“az vm/vmss create”内，使用“—use-unmanaged-disk”避免托管磁盘 其他命令应有效
* vm/vmss：改进生成 SSH 密钥对时的警告文本
* vm/vmss：支持通过需要计划信息的市场映像创建 ([#1209](https://github.com/Azure/azure-cli/issues/1209))


## <a name="april-3-2017"></a>2017 年 4 月 3 日

版本 2.0.2

此版本中已发布 ACR、Batch、KeyVault 和 SQL 组件

```text
azure-cli (2.0.2)

acr (2.0.0)
acs (2.0.2)
appservice (0.1.2)
batch (2.0.0)
cloud (2.0.0)
component (2.0.0)
configure (2.0.2)
container (0.1.2)
core (2.0.2)
documentdb (0.1.2)
feedback (2.0.0)
find (0.0.1b1)
iot (0.1.2)
keyvault (2.0.0)
lab (0.0.1)
monitor (0.0.1)
network (2.0.2)
nspkg (2.0.0)
profile (2.0.2)
redis (0.1.1b3)
resource (2.0.2)
role (2.0.1)
sql (2.0.0)
storage (2.0.2)
vm (2.0.2)
```

### <a name="core"></a>核心

* 在默认列表中添加了 acr、lab、monitor 和 find 模块
* 登录：跳过错误的租户 ([#2634](https://github.com/Azure/azure-cli/pull/2634))
* 登录：将默认订阅设置为处于“已启用”状态的订阅 ([#2575](https://github.com/Azure/azure-cli/pull/2575))
* 添加了 wait 命令，并添加了对其他命令的 --no-wait 支持 ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* 核心：支持结合证书使用服务主体登录 ([#2457](https://github.com/Azure/azure-cli/pull/2457))
* 添加了有关缺少模板参数的提示。 ([#2364](https://github.com/Azure/azure-cli/pull/2364))
* 支持为资源组、默认 Web、 默认 VM 等常见参数设置默认值
* 支持登录到特定的租户

### <a name="acs"></a>ACS

* [ACS] 添加了配置默认 ACS 群集的支持 ([#2554](https://github.com/Azure/azure-cli/pull/2554))
* 添加了 SSH 密钥密码提示的支持。 ([#2044](https://github.com/Azure/azure-cli/pull/2044))
* 添加了对 Windows 群集的支持。 ([#2211](https://github.com/Azure/azure-cli/pull/2211))
* 从“所有者”角色切换到“参与者”角色。 ([#2321](https://github.com/Azure/azure-cli/pull/2321))

### <a name="appservice"></a>应用服务

* 应用服务：支持获取用于 DNS A 记录的外部 IP 地址 ([#2627](https://github.com/Azure/azure-cli/pull/2627))
* 应用服务：支持绑定通配符证书 ([#2625](https://github.com/Azure/azure-cli/pull/2625))
* 应用服务：支持列出发布配置文件 ([#2504](https://github.com/Azure/azure-cli/pull/2504))
* 应用服务 - 配置后触发源代码管理同步 ([#2326](https://github.com/Azure/azure-cli/pull/2326))

### <a name="datalake"></a>DataLake

* Data Lake Analytics 模块的初始版本
* Data Lake Store 模块的初始版本

### <a name="docuemntdb"></a>DocuemntDB

* DocumentDB：添加了对列出连接字符串的支持 ([#2580](https://github.com/Azure/azure-cli/pull/2580))

### <a name="vm"></a>VM

* [计算] 添加了用于创建虚拟机规模集的应用网关支持 ([#2570](https://github.com/Azure/azure-cli/pull/2570))
* [VM/VMSS] 改进了磁盘缓存支持 ([#2522](https://github.com/Azure/azure-cli/pull/2522))
* VM/VMSS：合并了门户使用的凭据验证逻辑 ([#2537](https://github.com/Azure/azure-cli/pull/2537))
* 添加了 wait 命令和 --no-wait 支持 ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* 虚拟机规模集：支持使用 * 列出不同 VM 上的实例视图 ([#2467](https://github.com/Azure/azure-cli/pull/2467))
* 为 VM 和虚拟机规模集添加了 --secrets ([#2212}(<https://github.com/Azure/azure-cli/pull/2212>))
* 允许使用专用 VHD 创建 VM ([#2256](https://github.com/Azure/azure-cli/pull/2256))

## <a name="february-27-2017"></a>2017 年 2 月 27 日

版本 2.0.0

此 Azure CLI 2.0 发布版是第一个“正式版”。正式版适用于以下命令模块：
- 容器服务 (ACS)
- 计算（包括 Resource Manager、VM、虚拟机规模集、托管磁盘）
- 网络
- 存储

这些命令模块可在生产中使用，并且受标准 Microsoft SLA 支持。可以直接通过 Microsoft 支持部门创建问题，也可以在我们的 [github 问题列表](https://github.com/azure/azure-cli/issues/)中创建问题。可以[使用 azure-cli 标记在 StackOverflow 上](http://stackoverflow.com/questions/tagged/azure-cli)提问，也可以通过 [azfeedback@microsoft.com](mailto:azfeedback@microsoft.com) 联系产品团队。可以从命令行使用 `az feedback` 命令提供反馈。

这些模块中的命令非常稳定，其语法在此 Azure CLI 版本即将到来的发行版中预期不会变化。

若要验证 CLI 的版本，请使用 `az --version`。输出中将列出 CLI 本身的版本（在此发行版中为 2.0.0）、各个命令模块的版本，以及所用 Python 和 GCC 的版本。

```text
azure-cli (2.0.0)

acs (2.0.0)
appservice (0.1.1b5)
batch (0.1.1b4)
cloud (2.0.0)
component (2.0.0)
configure (2.0.0)
container (0.1.1b4)
core (2.0.0)
documentdb (0.1.1b2)
feedback (2.0.0)
iot (0.1.1b3)
keyvault (0.1.1b5)
network (2.0.0)
nspkg (2.0.0)
profile (2.0.0)
redis (0.1.1b3)
resource (2.0.0)
role (2.0.0)
sql (0.1.1b5)
storage (2.0.0)
vm (2.0.0)

Python (Darwin) 2.7.10 (default, Jul 30 2016, 19:40:32)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)]
```

> [!Note]
> 其中的一些命令模块有“b*n*”或“rc*n*”后缀。这些命令模块仍在预览版中提供，将来会发布正式版。

此外，我们还提供 CLI 夜间预览版。有关信息，请参阅有关[获取夜间预览版](https://github.com/Azure/azure-cli#nightly-builds)的说明，以及有关[开发人员设置与贡献代码](https://github.com/Azure/azure-cli#developer-setup)的说明。

可通过以下方式报告夜间预览版的问题：
- 在 [github 问题列表](https://github.com/azure/azure-cli/issues/)中报告问题
- 通过 [azfeedback@microsoft.com](mailto:azfeedback@microsoft.com) 联系产品团队
- 通过命令行使用 `az feedback` 命令提供反馈
