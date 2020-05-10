---
title: 如何使用适用于 Azure Key Vault 的 Spring Boot 起动器
description: 了解如何使用 Azure Key Vault 起动器配置 Spring Boot Initializer 应用。
services: key-vault
documentationcenter: java
ms.date: 10/29/2019
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 2df574104376ec1900c7dc5cbd4f0a49ef1f4732
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138733"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>如何使用适用于 Azure Key Vault 的 Spring Boot 起动器

本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Key Vault 的 Spring Boot 起动器检索密钥保管库中以机密形式存储的连接字符串。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 创建应用

以下过程使用 Spring Initializr 创建该应用程序。

1. 浏览到 <https://start.spring.io/>。

1. 指定你希望使用 **Java** 生成一个 **Maven** 项目。  

1. 输入应用程序的“组”和“项目”名称。  

1. 在“依赖项”部分，输入“Azure Key Vault”。  

1. 滚动到页面底部，单击“生成”  。

   ![生成 Spring Boot 项目][secrets-01]

1. 出现提示时，将项目下载到本地计算机中的路径。

## <a name="sign-into-azure"></a>登录 Azure

以下过程在 Azure CLI 中进行用户身份验证。

1. 打开命令提示符。

1. 通过使用 Azure CLI 登录到 Azure 帐户：

   ```azurecli
   az login
   ```

按照说明完成登录过程。

1. 列出订阅：

   ```azurecli
   az account list
   ```

   Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. 指定要用于 Azure 的帐户的 GUID；例如：

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a>创建新的 Azure 密钥保管库

以下过程创建并初始化该密钥保管库。

1. 为要用于 Key Vault 的 Azure 资源创建资源组，例如：

   ```azurecli
   az group create --name vged-rg2 --location westus
   ```

   其中：

   | 参数 | 说明 |
   |---|---|
   | `name` | 指定资源组的唯一名称。 |
   | `location` | 指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。 |

   Azure CLI 将显示资源组创建的结果；例如：  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/vged-rg2",
     "location": "westus",
     "managedBy": null,
     "name": "vged-rg2",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. 在资源组中创建新的 Key Vault，例如：

   ```azurecli
   az keyvault create --resource-group vged-rg2 \
       --name vgedkeyvault \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --sku standard --query properties.vaultUri \
       --location westus
   ```

   其中：

   | 参数 | 说明 |
   |---|---|
   | `name` | 指定 Key Vault 的唯一名称。 |
   | `enabled-for-deployment` | 指定 [Key Vault 部署选项](/cli/azure/keyvault)。 |
   | `enabled-for-disk-encryption` | 指定 [Key Vault 加密选项](/cli/azure/keyvault)。 |
   | `enabled-for-template-deployment` | 指定 [Key Vault 加密选项](/cli/azure/keyvault)。 |
   | `sku` | 指定 [Key Vault SKU 选项](/cli/azure/keyvault)。 |
   | `query` | 指定要从响应中检索的值，即完成本教程所需的 Key Vault URI。 |
   | `location` | 指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。 |

   Azure CLI 将显示稍后要使用的 Key Vault URI，例如：  

   ```output
   "https://vgedkeyvault.vault.azure.net"
   ```

3. 在新的 Key Vault 中存储一个机密，例如：

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" \
       --name "connectionString" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   其中：

   | 参数 | 说明 |
   |---|---|
   | `vault-name` | 指定前面创建的 Key Vault 名称。 |
   | `name` | 指定机密的名称。 |
   | `value` | 指定机密的值。 |

   Azure CLI 将显示机密的创建结果，例如：  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://vgedkeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a>配置并编译你的应用

使用以下过程配置并编译应用程序。

1. 将前面下载的 Spring Boot 项目存档中的文件提取到某个目录中。

2. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。

3. 使用前面在完成本教程的步骤时创建的值添加 Key Vault 的值，例如：

   ```yaml
   azure.keyvault.uri=https://vgedkeyvault.vault.azure.net/
   azure.keyvault.enabled=true
   ```

   其中：

   |          参数          |                                 说明                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           指定创建 Key Vault 时使用的 URI。           |
    
    
4. 导航到项目的主源代码文件，例如： */src/main/java/com/vged/secrets*。

5. 在文本编辑器中打开应用程序的主 Java 文件；例如：*SecretsApplication.java*，并向文件中添加以下行：

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @SpringBootApplication
   @RestController
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }
   
      @GetMapping("get")
      public String get() {
         return connectionString;
      }
   
      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   此代码示例从密钥保管库中检索连接字符串，并将其显示到 url `https://{your-appservice-name}.azurewebsites.net/get`。

6. 保存并关闭该 Java 文件。

7. 禁用测试并使用 Maven 生成 JAR 文件。
    
   ```bash
   mvn clean package -Dmaven.test.skip=true
   ```

## <a name="configure-maven-plugin-for-azure-app-service"></a>配置适用于 Azure 应用服务的 Maven 插件

本部分将帮助你配置 Spring Boot 项目，使你的应用能够部署到 Azure 应用服务上。

1.  访问指向[配置适用于 Azure 应用服务的 Maven 插件]的链接。
    
    此链接指向的部分说明了如何创建新 Azure 应用服务。 如果要将应用部署到现有 Azure 应用服务，可以通过命令 `mvn azure-webapp:config` 重新配置部署，并选择“Application”部分进行配置。
    
    ```bash
    [INFO] Scanning for projects...                                                     
    [INFO]                                                                              
    [INFO] ----------------------< com.wingtiptoys:secrets >-----------------------     
    [INFO] Building secrets 0.0.1-SNAPSHOT                                              
    [INFO] --------------------------------[ jar ]---------------------------------     
    [INFO]                                                                              
    [INFO] --- azure-webapp-maven-plugin:1.9.0:config (default-cli) @ secrets ---       
    Please choose which part to config                                                  
    1. Application                                                                      
    2. Runtime                                                                          
    3. DeploymentSlot                                                                   
    Enter index to use: 1                                                              
    Define value for appName(Default: ********):                                      
    Define value for resourceGroup(Default: ********):                                 
    Define value for region(Default: ********):                                           
    Define value for pricingTier(Default: P1v2):                                        
    1. b1                                                                               
    2. b2                                                                               
    3. b3                                                                               
    4. d1                                                                               
    5. f1                                                                               
    6. p1v2 [*]                                                                         
    7. p2v2                                                                             
    8. p3v2                                                                             
    9. s1                                                                               
    10. s2                                                                              
    11. s3                                                                              
    Enter index to use:                                                                 
    Please confirm webapp properties                                                                                                          
    ```
    
    还可以直接编辑 *pom.xml* 中 `<azure-webapp-maven-plugin>` 的 `<configuration>` 部分。 将 `<resourceGroup>`、`<appName>` 和 `<region>` 值修改为适用于你的特定应用服务的值。

2. 将标识分配给应用服务，并记下要在下一步中使用的 `principalId`。

   ```bash
   az webapp identity assign --name your-appservice-name \
      --resource-group vged-rg2
   ```
   
3. 向 MSI 授予权限。

   ```bash
   az keyvault set-policy --name vgedkeyvault \
       --object-id your-managed-identity-objectId \
       --secret-permissions get list
   ```

## <a name="deploy-the-app-to-azure-and-run-app-service"></a>将应用部署到 Azure 并运行应用服务

现在，可以在 Azure 上部署你的 Web 应用了。 为此，请按照以下步骤操作：

1. 如果对 *pom.xml* 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件。

   ```bash
   mvn clean package
   ```
   
2. 使用 Maven 在 Azure 上部署你的应用。

   ```bash
   mvn azure-webapp:deploy
   ```
   
3. 重新启动应用服务。

4. 请在浏览器中访问 URL `https://{your-appservice-name}.azurewebsites.net/get` 以获取 `connectionString`。
   

## <a name="summary"></a>总结

在本教程中，你使用 **[Spring Initializr]** 创建了一个新的 Java Web 应用程序，创建了一个 Azure 密钥保管库来存储敏感信息，然后将你的应用程序配置为从密钥保管库检索信息。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure Key Vault 的详细信息，请参阅以下文章：

* [Key Vault 文档]。

* [Azure 密钥保管库入门]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

有关将托管标识用于应用服务的详细信息，请参阅[将托管标识用于应用服务]。

<!-- URL List -->

[Key Vault 文档]: /azure/key-vault/
[Azure 密钥保管库入门]: /azure/key-vault/key-vault-get-started
[面向 Java 开发人员的 Azure]: /azure/developer/java/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[将托管标识用于应用服务]: /azure/app-service/overview-managed-identity
[配置适用于 Azure 应用服务的 Maven 插件]: /azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
