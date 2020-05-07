---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671863"
---
创建[身份验证文件](../java-sdk-azure-authenticate.md#mgmt-file)，并在命令行中使用文件的完整路径导出环境变量 `AZURE_AUTH_LOCATION`。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

身份验证文件用于配置入口点 `Azure` 对象，该对象是管理库用来定义、创建和配置 Azure 资源的。

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[详细了解](../java-sdk-azure-authenticate.md#mgmt-auth)在使用用于 Java 的 Azure 管理库时提供的身份验证选项。