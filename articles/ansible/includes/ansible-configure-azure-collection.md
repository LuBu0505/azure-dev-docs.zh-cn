---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/09/2020
ms.author: tarcher
ms.openlocfilehash: 4f1fbe75f974aadc9e92e518e0e84d49ee73ed3d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81755220"
---
- **配置 Azure 集合**：从终端窗口运行以下命令来安装 Azure 集合。 如果 Azure 集合已安装，则 `--force` 标志会将其更新到最新版本。

    ```bash
    ansible-galaxy collection install azure.azcollection --force
    ```
