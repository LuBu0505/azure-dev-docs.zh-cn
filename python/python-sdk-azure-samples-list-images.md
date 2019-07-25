---
title: 列出映像
description: 列显所有可用于创建虚拟机的映像。
author: sptramer
manager: carmonm
ms.devlang: python
ms.topic: conceptual
ms.date: 6/15/2017
ms.author: sttramer
ms.openlocfilehash: 738f47b973492c3458f0d8ced0f4fdcfa57d5399
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285728"
---
# <a name="list-images"></a>列出映像

使用以下代码来列显所有可用于创建虚拟机的映像，包括所有 SKU 和版本。

```python
region = 'eastus'

result_list_pub = compute_client.virtual_machine_images.list_publishers(
    region,
)

for publisher in result_list_pub:
    result_list_offers = compute_client.virtual_machine_images.list_offers(
        region,
        publisher.name,
    )

    for offer in result_list_offers:
        result_list_skus = compute_client.virtual_machine_images.list_skus(
            region,
            publisher.name,
            offer.name,
        )

        for sku in result_list_skus:
            result_list = compute_client.virtual_machine_images.list(
                region,
                publisher.name,
                offer.name,
                sku.name,
            )

            for version in result_list:
                result_get = compute_client.virtual_machine_images.get(
                    region,
                    publisher.name,
                    offer.name,
                    sku.name,
                    version.name,
                )

                print('PUBLISHER: {0}, OFFER: {1}, SKU: {2}, VERSION: {3}'.format(
                    publisher.name,
                    offer.name,
                    sku.name,
                    version.name,
                ))
```
