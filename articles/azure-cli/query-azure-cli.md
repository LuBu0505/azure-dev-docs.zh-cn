---
title: 使用 Azure CLI 查询命令结果
description: 了解如何针对 Azure CLI 命令的输出执行 JMESPath 查询。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c9f0afdd626ac8d9af027db02275e5a553595473
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030739"
---
# <a name="query-azure-cli-command-output"></a>查询 Azure CLI 命令输出

Azure CLI 使用 `--query` 参数针对命令的结果执行 [JMESPath 查询](http://jmespath.org)。 JMESPath 是用于 JSON 的查询语言，提供在 CLI 输出中选择和修改数据的功能。 在进行任何显示格式化操作之前，先针对 JSON 输出执行查询。

Azure CLI 中的所有命令均支持 `--query` 参数。 本文通过一系列简单的小示例介绍了如何使用 JMESPath 的功能。

## <a name="dictionary-and-list-cli-results"></a>字典和列表 CLI 结果

即使是在使用 JSON 之外的输出格式的时候，也会先将 CLI 命令结果作为适合查询的 JSON 进行处理。 CLI 结果为 JSON 数组或字典。 数组是可以进行索引的对象的序列，字典是通过密钥进行访问的无序对象。 可以返回多个对象的命令会返回一个数组，而始终只返回单个对象的命令则返回字典。   

## <a name="get-properties-in-a-dictionary"></a>获取字典中的属性

处理字典结果时，可以只使用密钥来访问顶级属性。 `.` (__子表达式__) 字符用于访问嵌套字典的属性。 在介绍查询之前，请看看 `az vm show` 命令的未修改输出：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM -o json
```

此命令将输出字典。 省略了一些内容。

```json
{
  "additionalCapabilities": null,
  "availabilitySet": null,
  "diagnosticsProfile": {
    "bootDiagnostics": {
      "enabled": true,
      "storageUri": "https://xxxxxx.blob.core.windows.net/"
    }
  },
  ...
  "osProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "allowExtensionOperations": true,
    "computerName": "TestVM",
    "customData": null,
    "linuxConfiguration": {
      "disablePasswordAuthentication": true,
      "provisionVmAgent": true,
      "ssh": {
        "publicKeys": [
          {
            "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
            "path": "/home/azureuser/.ssh/authorized_keys"
          }
        ]
      }
    },
    "secrets": [],
    "windowsConfiguration": null
  },
  ....
}
```

以下命令通过添加查询获取经授权可以连接到 VM 的 SSH 公钥：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query osProfile.linuxConfiguration.ssh.publicKeys -o json
```

```json
[
  {
    "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
    "path": "/home/azureuser/.ssh/authorized_keys"
  }
]
```

## <a name="get-a-single-value"></a>获取单个值

常见的情况是你需要通过 CLI 命令仅获取_一个_值，如 Azure 资源 ID、资源名称、用户名或密码。 在这种情况下，你通常还想要将该值存储在本地环境变量中。 若要获取的单个属性，首先请确保通过查询仅获取一个属性。 修改最后一个示例，以便仅获取管理员用户名：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json
```

```JSON
"azureuser"
```

此值看上去像有效的单个值，但请注意，`"` 字符作为输出的一部分返回。 这表示该对象是一个 JSON 字符串。 务必要注意，当直接将此值作为命令输出分配给环境变量时，shell 可能__无法__解释引号：

```bash
USER=$(az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json)
echo $USER
```

```output
"azureuser"
```

几乎可以肯定这不是你希望的。 在这种情况下，你需要使用一种不会使用类型信息将返回值括起来的输出格式。 CLI 为实现此目的而提供的最佳输出选项是 `tsv`（制表符分隔值）。 具体而言，当检索某个只能是单值（不是字典或列表）的值时，`tsv` 输出保证不带引号。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o tsv
```

```output
azureuser
```

有关 `tsv` 输出格式的详细信息，请参阅[输出格式 - TSV 输出格式](format-output-azure-cli.md#tsv-output-format)

## <a name="get-multiple-values"></a>获取多个值

若要获取多个属性，请将表达式以逗号分隔列表的形式置于方括号 `[ ]`（__多选列表__）中。 若要一次获取包括 VM 名称、管理员用户和 SSH 密钥在内的所有内容，请使用以下命令：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '[name, osProfile.adminUsername, osProfile.linuxConfiguration.ssh.publicKeys[0].keyData]' -o json
```

```json
[
  "TestVM",
  "azureuser",
  "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
]
```

这些值列在结果数组中，其顺序与在查询中提供这些值时的顺序一样。 由于结果为数组，因此没有与结果关联的密钥。

## <a name="rename-properties-in-a-query"></a>重命名查询中的属性

若要在查询多个值时获取字典而非数组，请使用 `{ }`（__多选哈希__）运算符：
多选哈希的格式为 `{displayName:JMESPathExpression, ...}`。
`displayName` 将是在输出中显示的字符串，`JMESPathExpression` 是要求值的 JMESPath 表达式。 修改最后一部分的示例时，可以将多选列表更改为哈希：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKey:osProfile.linuxConfiguration.ssh.publicKeys[0].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "ssh-key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
}
```

## <a name="get-properties-in-an-array"></a>获取数组中的属性

数组没有自己的属性，但可以对数组进行索引。 此功能显示在表达式为 `publicKeys[0]` 的上一示例中，该表达式获取 `publicKeys` 数组的第一个元素。 无法保证 CLI 输出是有序的，因此请避免使用索引操作，除非你对顺序很确定，或者不在意获取什么元素。 若要访问数组中元素的属性，请执行下述两个操作中的一个：平展和筛选。   本部分介绍如何平展数组。

平展数组的操作通过 `[]` JMESPath 运算符来完成。 `[]` 运算符之后的所有表达式都应用到当前数组中的每个元素。
如果 `[]` 出现在查询的开头，它会平展 CLI 命令结果。 `az vm list` 的结果可以通过此功能来检查。
若要获取资源组中每个 VM 的名称、OS 和管理员名称，请执行以下命令：

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, admin:osProfile.adminUsername}' -o json
```

```json
[
  {
    "Name": "Test-2",
    "OS": "Linux",
    "admin": "sttramer"
  },
  {
    "Name": "TestVM",
    "OS": "Linux",
    "admin": "azureuser"
  },
  {
    "Name": "WinTest",
    "OS": "Windows",
    "admin": "winadmin"
  }
]
```

与 `--output table` 输出格式组合使用时，列名会与多选哈希的 `displayKey` 值匹配：

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, Admin:osProfile.adminUsername}' --output table
```

```output
Name     OS       Admin
-------  -------  ---------
Test-2   Linux    sttramer
TestVM   Linux    azureuser
WinTest  Windows  winadmin
```

> [!NOTE]
>
> 某些键已筛选掉，未在表视图中输出。 这些键为 `id`、`type` 和 `etag`。 若要查看这些值，可以在多选哈希中更改键名。
>
> ```azurecli-interactive
> az vm show -g QueryDemo -n TestVM --query "{objectID:id}" -o table
> ```

任何数组都可以平展，不仅仅是命令返回的顶级结果。 在上一部分，使用了表达式 `osProfile.linuxConfiguration.ssh.publicKeys[0].keyData` 来获取用于登录的 SSH 公钥。 若要获取每个  SSH 公钥，可以改将表达式编写为 `osProfile.linuxConfiguration.ssh.publicKeys[].keyData`。
以下查询表达式平展 `osProfile.linuxConfiguration.ssh.publicKeys` 数组，然后在每个元素上运行 `keyData` 表达式：

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKeys:osProfile.linuxConfiguration.ssh.publicKeys[].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "sshKeys": [
    "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso\n"
  ]
}
```

## <a name="filter-arrays"></a>筛选数组

另一项用于从数组获取数据的操作是筛选。  筛选操作通过 `[?...]` JMESPath 运算符来完成。
此运算符采用谓词作为其内容。 谓词是指其求值结果可能为 `true` 或 `false` 的任何语句。 所含谓词的求值结果为 `true` 的表达式包括在输出中。

JMESPath 提供标准的比较运算符和逻辑运算符。 其中包括 `<`、`<=`、`>`、`>=`、`==`、`!=`。
JMESPath 也支持逻辑 AND (`&&`)、OR (`||`) 和 NOT (`!`)。 表达式可以在括号中进行分组，因此可以形成更复杂的谓词表达式。 有关谓词和逻辑运算的完整详细信息，请参阅 [JMESPath specification](http://jmespath.org/specification.html)（JMESPath 规范）。

在上一部分，我们平展了一个数组，目的是获取资源组中所有 VM 的完整列表。 可以通过筛选器将以下输出限制为 Linux VM：

```azurecli-interactive
az vm list -g QueryDemo --query "[?storageProfile.osDisk.osType=='Linux'].{Name:name,  admin:osProfile.adminUsername}" --output table
```

```output
Name    Admin
------  ---------
Test-2  sttramer
TestVM  azureuser
```

> [!IMPORTANT]
>
> 在 JMESPath 中，字符串始终带单引号 (`'`)。 如果在筛选器谓词中使用双引号作为字符串的一部分，则会获得空的输出。

JMESPath 也有内置函数，可以用来进行筛选。 `contains(string, substring)` 就是这样的一个函数，用于查看字符串是否包含子字符串。 在调用函数之前，会对表达式求值，因此第一个参数可以是完整的 JMESPath 表达式。 下一示例查找使用 SSD 存储作为其 OS 磁盘的所有 VM：

```azurecli-interactive
az vm list -g QueryDemo --query "[?contains(storageProfile.osDisk.managedDisk.storageAccountType,'SSD')].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

此查询有点长。 `storageProfile.osDisk.managedDisk.storageAccountType` 键提到两次，在输出中重新生成了键。 若要将其缩短，一种方式是在平展并选择数据后应用筛选器。

```azurecli-interactive
az vm list -g QueryDemo --query "[].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}[?contains(Storage,'SSD')]" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

对于大型数组，在选择数据之前先应用筛选器可能速度更快。

若要获取函数的完整列表，请参阅 [JMESPath specification - Built-in Functions](http://jmespath.org/specification.html#built-in-functions)（JMESPath 规范 - 内置函数）。

## <a name="change-output"></a>更改输出

JMESPath 函数还有另一用途，即根据查询结果进行运算。 任何返回非布尔值的函数都会改变表达式的结果。
例如，可以通过 `sort_by(array, &sort_expression)` 按属性值对数据排序。 对于应该在后面作为函数的一部分求值的表达式，JMESPath 使用特殊运算符 `&`。 下一示例演示如何按 OS 磁盘大小对 VM 列表排序：

```azurecli-interactive
az vm list -g QueryDemo --query "sort_by([].{Name:name, Size:storageProfile.osDisk.diskSizeGb}, &Size)" --output table
```

```output
Name     Size
-------  ------
TestVM   30
Test-2   32
WinTest  127
```

若要获取函数的完整列表，请参阅 [JMESPath specification - Built-in Functions](http://jmespath.org/specification.html#built-in-functions)（JMESPath 规范 - 内置函数）。

## <a name="experiment-with-queries-interactively"></a>以交互方式试验查询

若要开始试验 JMESPath，请使用 [JMESPath-terminal](https://github.com/jmespath/jmespath.terminal) Python 包，它提供了用于处理查询的交互式环境。 将数据作为输入通过管道传送，然后在编辑器中编写并运行查询。

```bash
pip install jmespath-terminal
az vm list --output json | jpterm
```
