---
title: 通过适用于 Python 的 Azure 库使用 Azure 托管磁盘
description: 使用 Azure SDK 创建、更新托管磁盘和重设其大小。
ms.topic: conceptual
ms.date: 11/18/2020
ms.custom: devx-track-python
ms.openlocfilehash: fe2378bcb836dbfc52ad1d5d3e88f048d6ef117e
ms.sourcegitcommit: b70a38d46616f5e519d5b9c1a1eaf3fe0ecb9605
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "94932411"
---
# <a name="use-azure-managed-disks-with-the-azure-libraries-sdk-for-python"></a>通过适用于 Python 的 Azure 库 (SDK) 使用 Azure 托管磁盘

Azure 托管磁盘提供了简化的磁盘管理、增强的可伸缩性、更好的安全性和更好的缩放性，而无需直接使用存储帐户。

使用 [`azure-mgmt-compute`](/python/api/overview/azure/virtualmachines) 库来管理托管磁盘。 （若要通过示例了解如何使用 `azure-mgmt-compute` 库来预配虚拟机，请参阅[示例 - 预配虚拟机](azure-sdk-example-virtual-machines.md)。）

## <a name="standalone-managed-disks"></a>独立托管磁盘

可以通过以下各部分中介绍的多种方法创建独立的托管磁盘。

### <a name="create-an-empty-managed-disk"></a>创建空托管磁盘

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>从 blob 存储创建托管磁盘

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-image-from-blob-storage"></a>从 blob 存储创建托管磁盘映像

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.images.begin_create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
        'storage_profile': {
           'os_disk': {
              'os_type': 'Linux',
              'os_state': "Generalized",
              'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
              'caching': "ReadWrite",
           }
        }
    }
)
image_resource = poller.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>从自己的映像创建托管磁盘

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = poller.result()
```

## <a name="virtual-machine-with-managed-disks"></a>包含托管磁盘的虚拟机

可以为特定的磁盘映像创建具有隐式托管磁盘的虚拟机，这样便无需指定所有详细信息。

从 Azure 中的 OS 映像创建 VM 时，会隐式创建托管磁盘。 在 `storage_profile` 参数中，`os_disk` 是可选的，并且无需在创建虚拟机之前事先创建存储帐户。

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

有关如何使用适用于 Python 的 Azure 管理库创建虚拟机的完整示例，请参阅[示例 - 预配虚拟机](azure-sdk-example-virtual-machines.md)。

也可以从自己的映像创建 `storage_profile`：

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')

storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

可以轻松附加以前预配的托管磁盘：

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})

async_update = compute_client.virtual_machines.begin_create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>带托管磁盘的虚拟机规模集

在托管磁盘推出之前，需针对要放入规模集的所有 VM 手动创建存储帐户，然后使用列表参数 `vhd_containers` 将所有存储帐户名称提供给规模集 RestAPI。 （若要获取迁移指南，请参阅[将规模集模板转换为托管磁盘规模集模板](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md)。）

由于不需要使用 Azure 托管磁盘管理存储帐户，因此 `storage_profile` 现在可以与 VM 创建过程中使用的配置文件完全相同：

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

完整的示例如下所示：

```python
naming_infix = "PyTestInfix"

vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    }
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.begin_create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>可对托管磁盘执行的其他操作

### <a name="resizing-a-managed-disk"></a>调整托管磁盘的大小

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>更新托管磁盘的存储帐户类型

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-blob-storage"></a>从 Blob 存储创建映像

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>创建当前已附加到虚拟机的托管磁盘的快照

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

async_snapshot_creation = self.compute_client.snapshots.begin_create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```

## <a name="see-also"></a>请参阅

- [示例 - 预配虚拟机](azure-sdk-example-virtual-machines.md)
