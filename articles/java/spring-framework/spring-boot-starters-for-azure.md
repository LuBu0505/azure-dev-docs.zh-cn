---
title: 适用于 Azure 的 Spring Boot 起动器
description: 本文介绍适用于 Azure 的各种 Spring Boot 起动器。
documentationcenter: java
ms.date: 09/29/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 3e73fda380f2743f6a3d4b2fb726ee62173b8563
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035365"
---
# <a name="spring-boot-starters-for-azure"></a>适用于 Azure 的 Spring Boot 起动器

本文介绍适用于 [Spring Initializr] 的，可为 Java 开发人员提供集成功能来使用 Microsoft Azure 的各种 Spring Boot 起动器。

>[!div class="mx-imgBorder"]
![使用 Initializr 配置 Azure Spring Boot 起动器][configure-azure-spring-boot-starters-with-initializr]

> [!NOTE]
>
> Spring Initializr 使用 Java 11 作为默认版本。 若要使用本主题中所述的 Spring Boot 起动器，必须改为选择 Java 8。
> 

以下 Spring Boot 起动器目前适用于 Azure：

* **[Azure 支持](#azure-support)**

   为服务总线、存储、Active Directory 等 Azure 服务提供自动配置支持。

* **[Azure Active Directory](#azure-active-directory)**

   为 Spring 安全性与用于身份验证的 Azure Active Directory 提供集成支持。

* **[Azure Key Vault](#azure-key-vault)**

   为 Azure Key Vault 机密集成提供 Spring 值注释支持。

* **[Azure 存储](#azure-storage)**

   为 Azure 存储服务提供 Spring Boot 支持。
   
   > [!NOTE]
   >
   > 用于 Azure 存储的 Spring Boot 起动器的新版本目前不支持从 Spring Initializr 内部添加 Azure 存储依赖项。 但在生成项目后，可修改 pom.xml 文件来添加此依赖项。
   > 

<a name="azure-support"></a>
## <a name="azure-support"></a>Azure 支持

此 Spring Boot 起动器为服务总线、存储、Active Directory、Cosmos DB、Key Vault 等 Azure 服务提供自动配置支持。

有关如何使用此起动器提供的各种 Azure 功能的示例，请参阅以下文章：

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    ```

* 将以下节添加到文件：

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a>Azure Active Directory

此 Spring Boot 起动器为 Spring 安全性提供自动配置支持，以便能够与用于身份验证的 Azure Active Directory 集成。

有关如何使用此起动器提供的 Azure Active Directory 功能的示例，请参阅以下文章：

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-active-directory-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    ```

* 将以下节添加到文件：

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a>Azure Key Vault

此 Spring Boot 起动器为 Azure Key Vault 机密集成提供 Spring 值注释支持。

有关如何使用此起动器提供的 Azure Key Vault 功能的示例，请参阅以下文章：

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-keyvault-secrets>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    ```

* 将以下节添加到文件：

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a>Azure 存储

此 Spring Boot 起动器为 Azure 存储服务提供 Spring Boot 集成支持。

有关如何使用此起动器提供的 Azure 存储功能的示例，请参阅以下文章：

* [如何使用适用于 Azure 存储的 Spring Boot 起动器](configure-spring-boot-starter-java-app-with-azure-storage.md)
* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-cloud-sample-storage-queue-operation>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>spring-starter-azure-storage</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    ```

* 将以下节添加到文件：

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](./index.yml)

### <a name="additional-resources"></a>其他资源

有关使用 Azure 上的 [Spring Boot] 应用程序的详细信息，请参阅 [Azure 上的 Spring]。

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: ../index.yml
[使用 Azure DevOps 和 Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: ./index.yml
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[configure-azure-spring-boot-starters-with-initializr]: media/spring-boot-starters-for-azure/configure-azure-spring-boot-starters-with-initializr.png
