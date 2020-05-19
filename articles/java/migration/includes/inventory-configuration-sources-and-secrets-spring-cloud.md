---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: b15ebf1491dd494701dd5c18e0248e73bdd86f2c
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990199"
---
### <a name="inventory-configuration-sources-and-secrets"></a>清单配置源和机密

#### <a name="inventory-passwords-and-secure-strings"></a>清单密码和安全字符串

检查生产部署上的所有属性和配置文件以及所有环境变量中是否有机密字符串和密码。 在 Spring Cloud 应用程序中，通常可以在各个服务的 application.properties 和 application.yml 文件中找到此类字符串，或在 Spring Cloud Config 存储库中找到此类字符串 。

[!INCLUDE [inventory-certificates-h4](inventory-certificates-h4.md)]

#### <a name="determine-whether-spring-cloud-vault-is-used"></a>确定是否使用 Spring Cloud Vault

如果使用 Spring Cloud Vault 来存储和访问机密，请标识备用机密存储（例如，HashiCorp Vault 或 CredHub）。 然后标识应用程序代码使用的所有机密。

#### <a name="locate-the-configuration-server-source"></a>查找配置服务器源

如果应用程序使用 [Spring Cloud Config 服务器](https://cloud.spring.io/spring-cloud-config/reference/html/#_spring_cloud_config_server)，请确定配置的存储位置。 通常可以在 bootstrap.yml 或 bootstrap.properties 文件中找到此设置，有时也可以在 application.yml 或 application.properties 文件中找到此设置   。 此设置与以下示例类似：

```properties
spring.cloud.config.server.git.uri: file://${user.home}/spring-cloud-config-repo
```

尽管 git 最常用作 Spring Cloud Config 的备用数据存储（如前文所述），但也可能使用一种其他的后端。 请参阅 [Spring Cloud Config 文档](https://cloud.spring.io/spring-cloud-config/reference/html/#_environment_repository)，了解有关其他后端的信息，如[关系数据库 (JDBC)](https://cloud.spring.io/spring-cloud-config/reference/html/#_jdbc_backend)、[SVN](https://cloud.spring.io/spring-cloud-config/reference/html/#_version_control_backend_filesystem_use) 和 [本地文件系统](https://cloud.spring.io/spring-cloud-config/reference/html/#_file_system_backend)。

> [!NOTE]
> 如果配置服务器数据存储在本地（例如 GitHub Enterprise），则需要通过 Git 存储库将其提供给 Azure Spring Cloud。
