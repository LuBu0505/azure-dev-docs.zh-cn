---
author: yevster
ms.author: yebronsh
ms.date: 7/17/2020
ms.openlocfilehash: 1e7957a03cb96254a0318774a1620c7143ae6fc4
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062041"
---
### <a name="migrate-all-certificates-to-keyvault"></a>将所有证书迁移到 KeyVault

Azure Spring Cloud 不提供对 JRE 密钥存储的访问权限，因此必须将证书迁移到 Azure KeyVault，并更改应用程序代码，才能访问 KeyVault 中的证书。 有关详细信息，请参阅 [Key Vault 证书入门](/azure/key-vault/certificates/certificate-scenarios)和[适用于 Java 的 Azure Key Vault 证书客户端库](/java/api/overview/azure/security-keyvault-certificates-readme)。