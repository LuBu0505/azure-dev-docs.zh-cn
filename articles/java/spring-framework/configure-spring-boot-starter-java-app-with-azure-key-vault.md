---
title: 关于在 Spring Boot 应用程序中从 Azure Key Vault 读取机密的教程
description: 关于在 Spring Boot 应用程序中从 Azure Key Vault 读取机密的教程
services: key-vault
documentationcenter: java
ms.date: 08/15/2020
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: identity
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: c6a81f5fb08985626909fe499584e67351a70ad0
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92688847"
---
# <a name="tutorial-reading-a-secret-from-azure-key-vault-in-a-spring-boot-application"></a>教程：在 Spring Boot 应用程序中从 Azure Key Vault 读取机密

Spring Boot 应用程序将用户名和密码等敏感信息外部化。  外部化敏感信息可以提高可维护性、可测试性和安全性。  在代码之外存储机密优于对信息进行硬编码或在生成时内联信息。

本教程介绍如何创建从 Azure Key Vault 读取值的 Spring Boot 应用，然后将该应用部署到 Azure 应用服务和 Azure Spring Cloud。

> [!div class="checklist"]
> * 创建 Azure Key Vault 并存储机密
> * 使用 Spring Initializr 创建应用
> * 向应用添加 Key Vault 集成
> * 部署到 Azure App Service
> * 使用 Azure 资源的托管标识重新部署到 Azure 应用服务
> * 部署到 Azure Spring Cloud

## <a name="prerequisites"></a>先决条件

* 一个有效的 Azure 订阅。
  * 如果没有 Azure 订阅，可以[创建一个免费帐户](https://azure.microsoft.com/free/)。
* [安装 Azure CLI 版本 2.0.67 或更高版本](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)，并使用以下命令安装 Azure Spring Cloud 扩展：`az extension add --name spring-cloud`
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* `curl` 命令。  大多数类似 UNIX 的操作系统都预安装了此命令。  特定于操作系统的客户端可通过 [cURL 官方网站](https://curl.haxx.se/)查找。
* `jq` 命令。 大多数类似 UNIX 的操作系统都预安装了此命令。  特定于操作系统的客户端可通过 [jQuery 官方网站](https://stedolan.github.io/jq/)查找。

## <a name="create-a-new-azure-key-vault"></a>创建新的 Azure Key Vault

以下部分演示如何登录到 Azure 并创建 Azure Key Vault。

### <a name="sign-into-azure-and-set-your-subscription"></a>登录到 Azure 并设置订阅

首先，通过以下步骤使用 Azure CLI 进行身份验证。

1. （可选）注销并删除一些身份验证文件以删除任何延迟凭据：

   ```azurecli
   az logout
   rm ~/.azure/accessTokens.json
   rm ~/.azure/azureProfile.json
   ```

1. 通过使用 Azure CLI 登录到 Azure 帐户：

   ```azurecli
   az login
   ```

   按照说明完成登录过程。

1. 列出订阅：

   ```azurecli
   az account list
   ```

   Azure 将返回订阅的列表。 复制要使用的订阅的 `id`；例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
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

1. 指定要用于 Azure 的订阅的 GUID；例如：

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

### <a name="create-a-service-principal-for-use-in-by-your-app"></a>创建服务主体以供你的应用使用

Azure AD“服务主体”  提供对订阅中的 Azure 资源的访问权限。 可以将服务主体视为服务的用户标识。  “服务”是任何需要访问 Azure 资源的应用程序、服务或平台，包括本教程中生成的示例应用。 可以为服务主体配置作用域仅限于你指定的那些资源的访问权限。 然后，将应用程序或服务配置为使用服务主体的凭据来访问这些资源。

使用此命令创建服务主体。

```azurecli
az ad sp create-for-rbac --name contososp
```

`name` 选项的值在你的订阅中必须具有唯一性。  将从命令返回的值保存下来，以便稍后在教程中使用。  返回的 JSON 将如下所示。

```json
{
  "appId": "8r7o486s-o5q9-450s-8457-pr26p86n0497",
  "displayName": "ejbcontososp",
  "name": "http://ejbcontososp",
  "password": "4bt.lCKJKlbYLn_3XF~wWtUwyHU0jKggu2",
  "tenant": "72s988os-86s1-41ns-91no-2d7cd011db47"
}
```

### <a name="create-the-key-vault-instance"></a>创建 Key Vault 实例

以下过程创建并初始化该 Key Vault。

1. 确定哪个 Azure 区域将保存你的资源。
   1. 可查看[区域及其位置的列表](https://azure.microsoft.com/regions/)。
   1. 可以使用 `az account list-locations` 命令为所选区域查找正确的 `Name`。

      ```azurecli
      az account list-locations -o table
      ```

      在本教程中，我们将使用 `eastus`。
1. 创建资源组以保存 Key Vault 和应用服务应用。  值在 Azure 订阅中必须具有唯一性。  在本教程中，我们将使用 `contosorg`。

   ```azurecli
   az group create --name contosorg --location eastus
   ```

1. 在资源组中创建新的 Key Vault。

   ```azurecli
   az keyvault create \
       --resource-group contosorg \
       --name contosokv \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --location eastus
       --query properties.vaultUri \
       --sku standard 
   ```

    > [!NOTE]
    > `--name` 选项的值在 Azure 订阅中必须具有唯一性。

   下表说明了上面所示的选项。

   | 参数 | 说明 |
   |---|---|
   | `enabled-for-deployment` | 指定 [Key Vault 部署选项](/cli/azure/keyvault)。 |
   | `enabled-for-disk-encryption` | 指定 [Key Vault 加密选项](/cli/azure/keyvault)。 |
   | `enabled-for-template-deployment` | 指定 [Key Vault 加密选项](/cli/azure/keyvault)。 |
   | `location` | 指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。 |
   | `name` | 指定 Key Vault 的唯一名称。 |
   | `query` | 从响应中检索 Key Vault URI。  要完成本教程，你需要 URI。 |
   | `sku` | 指定 [Key Vault SKU 选项](/cli/azure/keyvault)。 |

   Azure CLI 将显示稍后要使用的 Key Vault URI，例如：

   ```output
   "https://contosokv.vault.azure.net/"
   ```

1. 配置 Key Vault 以允许来自该托管标识的 `get` 和 `list` 操作。  `object-id` 的值是来自上述 `az ad sp create-for-rbac` 命令的 `appId`。

   ```azurecli
   az keyvault set-policy --name contosokv --spn http://ejbcontososp --secret-permissions get list
   ```

   输出将是一个 JSON 对象，其中包含的都是 Key Vault 的信息。  其中将具有一个值为 `Microsoft.KeyVault/vaults` 的 `type` 项。

   下表说明了上面所示的属性。

   | 参数 | 说明 |
   |---|---|
   | name | Key Vault 的名称。 |
   | SPN | 上述 `az ad sp create-for-rbac` 命令输出的 `name`。 |
   | secret-permissions | 允许来自指定主体的操作列表。 |

    > [!NOTE]
    > 尽管最低权限原则建议向资源授予尽可能少的权限集，但 Key Vault 集成的设计至少需要 `get` 和 `list`。

1. 在新的 Key Vault 中存储机密。  常见用例是存储 JDBC 连接字符串。  例如：

   ```azurecli
   az keyvault secret set --name "connectionString" \
       --vault-name "contosokv" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   下表说明了上面所示的选项。

   | 参数 | 说明 |
   |---|---|
   | `name` | 指定机密的名称。 |
   | `value` | 指定机密的值。 |
   | `vault-name` | 指定前面创建的 Key Vault 名称。 |

   Azure CLI 将显示机密的创建结果，例如：

   ```json
   {
     "attributes": {
       "created": "2020-08-24T21:48:09+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2020-08-24T21:48:09+00:00"
     },
     "contentType": null,
     "id": "https://contosokv.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

现在，你已创建 Key Vault 并存储了机密，接下来的部分将演示如何使用 Spring Initializr 创建应用。

## <a name="create-the-app-with-spring-initializr"></a>使用 Spring Initializr 创建应用

本部分介绍如何使用 Spring Initializr 和 `RestController` 在本地创建和运行 Spring Boot 应用程序。

1. 浏览到 <https://start.spring.io/>。
1. 选择此列表后面的图片中所示的选项。
   1. **项目** ：`Maven Project`
   1. **语言** ：`Java`
   1. **Spring Boot** ：`2.3.3`
   1. **组** ：`com.contoso`（你可以在此处输入任何有效的 Java 包名称。）
   1. **项目** ：keyvault（你可以在此处输入任何有效的 Java 类名。）
   1. **打包** ：`Jar`
   1. **Java** ：`11`（你可以选择 8，但本教程使用 11 进行验证。）
1. 选择“添加依赖关系…”
1. 在文本字段中，输入 `Spring Web` 并按 Ctrl+Enter。
1. 在文本字段中，输入 `Azure Key Vault` 并按 Enter。  你的屏幕应与下图中所示类似。
   :::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault/spring-initializr-choices.png" alt-text="已选中正确选择的 Spring Initializr。":::
1. 在页面底部，选择“生成”。
1. 出现提示时，将项目下载到本地计算机中的路径。  为进行讨论，我们将在当前用户的主目录中使用目录 keyvault。  上述值将在该目录中为你提供 keyvault.zip 文件。

请按照下面的步骤检查应用程序并在本地运行它。

1. 解压缩 keyvault.zip 文件。  文件布局将如下所示。  在本教程中，我们将忽略“测试”目录及其内容。

   ```bash
   ├── HELP.md
   ├── mvnw
   ├── mvnw.cmd
   ├── pom.xml
   └── src
       ├── main
       │   ├── java
       │   │   └── com
       │   │       └── contoso
       │   │           └── keyvault
       │   │               └── KeyvaultApplication.java
       │   └── resources
       │       ├── application.properties
       │       ├── static
       │       └── templates
   ```

1. 在文本编辑器中打开 KeyvaultApplication.java 文件。  修改该文件，使其如下。

   * 类用 `@RestController` 注释。  `@RestController` 通知 Spring Boot 该类可以响应 RESTful HTTP 请求。
   * 该类有一个用 `@GetMapping(get)` 注释的方法。  `@GetMapping` 通知 Spring Boot 将路径为 `/get` 的 HTTP 请求发送到该方法，从而使该方法的响应返回到 HTTP 客户端。
   * 该类具有一个专用实例变量 `connectionString`。  此实例变量的值从 `get()` 方法返回。

   ```java
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @SpringBootApplication
   @RestController
   public class KeyvaultApplication {

      public static void main(String[] args) {
        SpringApplication.run(KeyvaultApplication.class, args);
      }

     @GetMapping("get")
     public String get() {
       return connectionString;
     }

     private String connectionString = "defaultValue\n";  

     public void run(String... varl) throws Exception {
       System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
     }  

   }
   ```

1. 在 pom.xml 文件所在的顶级 keyvault 目录中，输入 `mvn spring-boot:run` 。  
1. 命令输出中的“已完成初始化”消息表示服务器已就绪。  在单独的 shell 窗口中，输入以下命令。

   ```bash
   $ curl http://localhost:8080/get
   ```

   输出将显示 `defaultValue`。

1. 终止从 `mvn spring-boot:run` 运行的进程。  可以键入 Ctrl-C，也可以使用 `jps` 命令获取 `Launcher` 进程的 PID 并终止它。

下一部分将演示如何将 Key Vault 集成添加到本地运行的应用程序。

## <a name="add-key-vault-integration-to-the-app"></a>向应用添加 Key Vault 集成

以下步骤将演示对 Spring Boot 应用程序 `KeyvaultApplication` 的必要修改。

正如 Key Vault 使应用程序代码中的机密外部化那样，Spring 配置也可以从代码中对配置进行外部化。  Spring 配置的最简单形式是 application.properties 文件。  在 Maven 项目中，这个文件位于 src/main/resources/application.properties。  Spring Initializer 在这个位置很好地包含了一个零长度的文件。

按照以下步骤将必要配置添加到此文件。

1. 在编辑器中打开 src/main/resources/application.properties 并使其具有以下内容，同时调整 Azure 订阅的值。

   ```txt
   azure.keyvault.client-id=685on005-ns8q-4o04-8s16-n7os38o2ro5n
   azure.keyvault.client-key=4bt.lCKJKlbYLn_3XF~wWtUwyHU0jKggu2
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91no-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/
   ```

   下表说明了上面所示的属性。

   | 参数 | 说明 |
   |---|---|
   | azure.keyvault.client-id | 从 `az ad sp create-for-rbac`返回的 JSON 的 `appId`。|
   | azure.keyvault.client-key | 从 `az ad sp create-for-rbac`返回的 JSON 的 `password`。|
   | azure.keyvault.enabled | 如果应在部署时设置 `enabled` 或 `disabled`，此配置可能很有用。  有关 Spring 配置的详细信息，请参阅 [Spring 文档](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/#boot-features-external-config)。
   | azure.keyvault.tenant-id | 从 `az ad sp create-for-rbac`返回的 JSON 的 `tenant`。|
   | azure.keyvault.uri | 从上述 `az keyvault create` 命令输出的值。 |

   [属性引用](https://aka.ms/azure-spring-boot-starter-keyvault-secrets)中记录了可用属性的完整列表。

1. 保存文件并将其关闭。

对 KeyvaultApplication.java 文件（或你所用的任何类名）进行一项简单更改。

1. 在文本编辑器中打开 src/main/java/com/contoso/keyvault/KeyvaultApplication.java。
1. 添加此导入。

   ```java
   import org.springframework.beans.factory.annotation.Value;
   ```

1. 向 `connectionString` 实例变量中添加注释。

   ```java
   @Value("${connectionString}")
   private String connectionString;  
   ```

   Key Vault 集成提供了一个由 Key Vault 的值填充的 Spring `PropertySource`。  [参考文档](https://aka.ms/azure-spring-boot-starter-keyvault-secrets)中提供了更多实现细节。

1. 在 pom.xml 文件所在的顶级 keyvault 目录中，输入 `mvn clean package spring-boot:run` 。  
1. 命令输出中的“已完成初始化”消息表示服务器已就绪。  在单独的 shell 窗口中，输入以下命令。

   ```bash
   $ curl http://localhost:8080/get
   ```

   输出将显示 `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE`，而不是 `defaultValue`。

1. 终止从 `mvn spring-boot:run` 运行的进程。  可以键入 Ctrl-C，也可以使用 `jps` 命令获取 `Launcher` 进程的 PID 并终止它。


下一部分将演示如何将此应用部署到 Azure 应用服务。

## <a name="deploy-to-azure-app-service"></a>部署到 Azure App Service

按照部分中的步骤将 `KeyvaultApplication` 部署到 Azure 应用服务。

### <a name="use-the-azure-maven-web-app-plugin-to-deploy-the-application-to-azure-app-service"></a>使用 Azure Maven Web 应用插件将应用程序部署到 Azure 应用服务

请按照以下步骤进行操作，使你的 POM 准备就绪，以将 `KeyvaultApplication` 部署到 Azure 应用服务。

1. 在顶级 keyvault 目录中，打开 pom.xml 文件 。
1. 在 `<build><plugins>` 部分中，通过插入此 XML 来添加 `azure-webapp-maven-plugin`。

   ```xml
    <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.12.0</version>
    </plugin>
   ```

   > [!NOTE]
   > 不用担心格式设置。  在此过程中，`azure-webapp-maven-plugin` 将重新设置整个 POM 的格式。

1. 保存并关闭 pom.xml 文件。
1. 在命令行中，调用新添加插件的 `config` 目标。  Maven 插件将询问你一些问题并根据回答编辑 pom.xml 文件。  你将进一步编辑 POM。

   ```bash
   mvn azure-webapp:config
   ```

1. 对于 `Subscription`，请确保选择与创建的 Key Vault 相同的订阅 ID。
1. 对于 `Web App`，你可以选择现有的 Web 应用，也可以选择 `<create>` 新建一个。如果选择现有的 Web 应用，将直接跳到最后的“确认”步骤。
1. 对于 `OS`，请确保已选择 `linux`。
1. 对于 `javaVersion`，请确保选择了 Spring Initializr 中所选的 Java 版本。  我们在上面选择了 `11`，所以在这里选择 11。
1. 接受其余问题的默认值。
1. 当系统要求确认时，回答 Y 继续，或回答 N 重新开始回答问题。  当插件完成运行后，就可以编辑 POM 了。

按照这些步骤进行操作，以便对 POM 进行进一步的必要编辑。

1. 在顶级 keyvault 目录中，打开 pom.xml 文件 。
1. 在 <plugins> 部分中查找 `azure-webapp-maven-plugin` 项。
1. 修改 `<resourceGroup>``<appName>` 和 `<region>` 的值。  
   1. 将 `<resourceGroup>` 的值设置为创建 Key Vault 时指定的值。
   1. 为 `<appName>` 选择一个在订阅中唯一的合理值。
   1. 将 `<region>` 的值设置为创建 Key Vault 时指定的值。
1. 包括一个 `<appSettings>` 元素，该元素使服务器侦听 TCP 端口 80。
1. 接下来将显示 `azure-webapp-maven-plugin` 的完整修改后的 `<plugin>`。  必须修改的值用 `*` 表示。

   ```xml
   <plugins> 
     <plugin> 
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-maven-plugin</artifactId> 
     </plugin>  
     <plugin> 
       <groupId>com.microsoft.azure</groupId>  
       <artifactId>azure-webapp-maven-plugin</artifactId>  
       <version>1.12.0</version>  
       <configuration>
         <schemaVersion>V2</schemaVersion>
         *<subscriptionId>********-****-****-****-************</subscriptionId>
         *<resourceGroup>contosorg</resourceGroup>
         *<appName>contosokeyvault</appName>
         <pricingTier>P1v2</pricingTier>
         *<region>eastus</region>
         <runtime>
           <os>linux</os>
           <javaVersion>java 11</javaVersion>
           <webContainer>Java SE</webContainer>
         </runtime>
         *<!-- Begin of App Settings  -->
         *<appSettings>
         *  <property>
         *    <name>JAVA_OPTS</name>
         *    <value>-Dserver.port=80</value>
         *  </property>
         *</appSettings>
         *<!-- End of App Settings  -->          
         <deployment>
           <resources>
             <resource>
               <directory>${project.basedir}/target</directory>
               <includes>
                 <include>*.jar</include>
               </includes>
             </resource>
           </resources>
         </deployment>
       </configuration>
     </plugin> 
   </plugins>
   ```

1. 保存并关闭 POM。
1. 将应用部署到 Azure 应用服务。

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. 该命令可能需要几分钟的时间，具体取决于无法控制的许多因素。  看到输出如下时，便可知应用已成功部署。

   ```bash
   [INFO] Deploying the zip package contosokeyvault-22b7c1a3-b41b-4082-a9f0-9339723fa36a11893059035499017844.zip...
   [INFO] Successfully deployed the artifact to https://contosokeyvault.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:45 min
   [INFO] Finished at: 2020-08-16T22:47:48-04:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 请等待 3 到 5 分钟让部署完成。  然后，可以使用像上面那样的 `curl` 命令来访问部署，但这次使用的是 `BUILD SUCCESS` 输出中显示的主机名。  例如，这个 `curl` 命令输出表示成功。

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

应用现已部署到 Azure 应用服务。

## <a name="redeploy-to-azure-app-service-and-use-managed-identities-for-azure-resources"></a>重新部署到 Azure 应用服务并使用 Azure 资源的托管标识

本部分介绍如何将标识与应用的 Azure 资源关联。  这是必需的，以便 Azure 可以应用安全和跟踪访问。  只为使用的资源付费是云计算的一项基础原则。  只有当每个资源都与一个标识关联时，才可能实现这种精细的资源跟踪。  在众多利用 Azure 资源的托管标识的 Azure 服务中，Azure 应用服务和 Azure Key Vault 是其中两项。  从 [Azure 资源的托管标识是什么？](/azure/active-directory/managed-identities-azure-resources/overview)一文中了解有关这一重要技术的详细信息

> [!NOTE]
> Azure 资源托管标识是以前称为托管服务标识 (MSI) 的服务的新名称。

按照以下步骤为 Azure 应用服务应用创建托管标识，然后允许该标识访问 Key Vault。

1. 为应用服务应用创建托管标识。  将选项替换为 POM 中的 `<resourceGroup>` 和 `<appName>` 元素的值。

   ```azurecli
   az webapp identity assign --resource-group contosorg --name contosokeyvault
   ```

   输出类似于：  记下 `principalId` 的值供下一步使用。

   ```json
   {
     "principalId": "2r7s6r00-92o9-4sq7-n10r-popq294ssr8s",
     "tenantId": "72s988os-86s1-41ns-91no-2q7pq011qo47",
     "type": "SystemAssigned",
     "userAssignedIdentities": null
   }
   ```

1. 编辑 application.properties，以便对上一步中创建的 Azure 资源的托管标识进行命名。

   1. 删除 `azure.keyvault.client-key`。
   1. 更新 `azure.keyvault.client-id`，使其具有上一步中 `principalId` 中的值。  完成的文件应如下所示。

   ```bash
   azure.keyvault.client-id=56rqs994-0o66-43o3-9roo-8e3534d0cb23
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91ab-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/    
   ```

1. 配置 Key Vault 允许来自该托管标识的 `get` 和 `list` 操作。  `object-id` 的值是前面的输出中的 `principalId`。

   ```azurecli
   az keyvault set-policy --name contosokv \
     --object-id 2r7s6r00-92o9-4sq7-n10r-popq294ssr8s --secret-permissions get list
   ```

   输出将是一个 JSON 对象，其中包含的都是 Key Vault 的信息。  其中将具有一个值为 `Microsoft.KeyVault/vaults` 的 `type` 项

   下表说明了上面所示的属性。

   | 参数 | 说明 |
   |---|---|
   | name | Key Vault 的名称。 | 
   | object-id | 前面命令中的 `principalId`。 |
   | secret-permissions | 允许来自指定主体的操作列表。 |

1. 打包并重新部署应用程序。

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. 为谨慎起见，请再等几分钟，让部署稳定下来。  然后，你可以使用像上面那样的 `curl` 命令来联系部署，但这次使用的是 `BUILD SUCCESS` 输出中显示的主机名。  例如，这个 `curl` 命令输出表示成功。

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

将从 Key Vault 中查找并返回 `connectionString` 的值，而不是返回 `defaultValue`。  在下一部分中，我们会将同一应用部署到 Azure Spring Cloud。

## <a name="deploy-to-azure-spring-cloud"></a>部署到 Azure Spring Cloud

Azure Spring Cloud 是一个完全托管的平台，用于在 Azure 中部署和运行 Spring Boot 应程序。  有关 Azure Spring Cloud 的概述，请参阅[什么是 Azure Spring Cloud？](/azure/spring-cloud/spring-cloud-overview)

本部分将使用现有的 Spring Boot 应用和先前使用 Azure Spring Cloud 的新实例创建的 Key Vault。

以下步骤将演示如何创建 Azure Spring Cloud 资源并将应用部署到该资源。  请确保已安装用于 Azure Spring Cloud 的 Azure CLI 扩展，如[先决条件](#prerequisites)中所示。

1. 确定服务实例的名称。  若要在 Azure 订阅中使用 Azure Spring Cloud，必须创建类型为 Azure Spring Cloud 的 Azure 资源。  与所有其他 Azure 资源一样，服务实例必须位于资源组内。  使用已经创建的资源组来保存服务实例。  使用此命令创建服务实例。 

   ```bash
   az spring-cloud create --resource-group "contosorg" --name "contososvc" 
   ```

   此命令需要几分钟才能完成。

1. 在服务中创建 Spring Cloud 应用。

   ```bash
   az spring-cloud app create --resource-group "contosorg" --name "contosoascsapp" --assign-identity --is-public true
     --runtime-version Java_11 --service "contososvc"
   ```

   下表说明了上面所示的选项。

   | 参数 | 说明 |
   |---|---|
   | assign-identity | 使服务为 Azure 资源的托管标识创建标识。 |
   | is-public | 为服务分配公共 DNS 域名。 |
   | name | 应用的名称。 |
   | resource-group | 在其中创建现有服务实例的资源组的名称。 |
   | runtime-version | Java 运行时版本。  该值必须与上面的 Spring Initializr 中选择的值匹配。 |
   | 服务 | 现有服务的名称。 |

   若要了解服务和应用之间的区别，请参阅[了解 Azure Spring Cloud 中的应用和部署](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment) 。

1. 获取 Azure 资源的托管标识。  将其用来配置现有 Key Vault 以允许从此应用进行访问。

   ```bash
   SERVICE_IDENTITY=$(az spring-cloud app show --resource-group "contosorg" --name "contosoascsapp" --service "contososvc" | jq -r '.identity.principalId')
   az keyvault set-policy --name "contosokv" --object-id <the value of the environment variable SERVICE_IDENTITY> --secret-permissions set get list
   ```

1. 由于现有 Spring Boot 应用已经有一个具有必要配置的 application.properties 文件，因此我们可以使用以下命令将该应用直接部署到 Spring Cloud。  在包含 POM 的目录中运行命令。

   ```bash
   az spring-cloud app deploy --resource-group "contosorg" --name "contosoascsapp" --jar-path target/keyvault-0.0.1-SNAPSHOT.jar \
     --service "contososvc"
   ```

   此命令在应用内的服务中创建部署。  有关服务实例、应用和部署的概念的详细信息，请参阅[了解 Azure Spring Cloud 中的应用和部署](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment)。

   如果部署不成功，请按照[配置应用程序日志](https://aka.ms/azure-spring-cloud-configure-logs)中的说明配置日志以进行故障排除。  日志可能会包含有用的信息来诊断和解决问题。

1. 成功部署应用后，可以使用 `curl` 验证 Key Vault 集成是否正常工作。  因为你指定了 `--is-public`，所以服务的默认 URL 为 `https://<service name>-<app name>.azuremicroservices.io/`。  我们可以使用 `curl` 命令，替换正确的值并附加 `@GetMapping` 注释的值。

   ```bash
   curl https://contososvc-contosoascsapp.azuremicroservices.io/get
   ```

   输出将显示 `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE`。

## <a name="summary"></a>总结

你使用 Spring Initializr 创建了新的 Java Web 应用程序。  你创建了一个 Azure Key Vault 来存储敏感信息，然后将你的应用程序配置为从 Key Vault 检索信息。  在本地对应用进行测试后，你将其部署到了 Azure App Service 和 Azure Spring Cloud。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [将 Spring Boot Initializer 应用配置为使用 Application Insights](configure-spring-boot-java-applicationinsights.md)
