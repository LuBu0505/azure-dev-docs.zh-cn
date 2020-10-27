---
title: 如何使用适用于 Azure 存储的 Spring Boot 起动器
description: 了解如何使用 Azure 存储起动器配置 Spring Boot Initializer 应用。
services: storage
documentationcenter: java
ms.date: 12/19/2018
ms.service: storage
ms.topic: article
ms.workload: storage
ms.custom: devx-track-java
ms.openlocfilehash: 18b02ab5ffbb5fc84878685d4301f284d431a216
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92337145"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>如何使用适用于 Azure 存储的 Spring Boot 起动器

本文逐步讲解如何使用 **Spring Initializr** 创建自定义应用程序，然后将 Azure Storage Starter 添加到应用程序，再使用应用程序将 Blob 上传到 Azure 存储帐户。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册[免费的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。
* [Azure 命令行接口 (CLI)](/cli/azure/index)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](http://maven.apache.org/) 3.0 或更高版本。

> [!IMPORTANT]
>
> 完成本文中的步骤需要 Spring Boot 2.0 或更高版本。
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>为应用程序创建 Azure 存储帐户和 Blob 容器

以下过程创建 Azure 存储帐户和容器。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次单击“+创建资源”、“存储”、“存储帐户”。  

   ![创建 Azure 存储帐户][IMG01]

1. 在“创建存储帐户”页中，输入以下信息：

   * 选择“订阅”。
   * 选择“资源组”或创建新资源组。
   * 输入独一无二的“存储帐户名称”，该名称将成为存储帐户 URI 的一部分。 例如，如果输入 **wingtiptoysstorage** 作为 **名称** ，则 URI 将为 *wingtiptoysstorage.core.windows.net* 。
   * 指定存储帐户的“位置”。
1. 指定上面列出的选项后，请单击“查看 + 创建”。 
1. 查看具体细节，然后单击“创建”以创建存储帐户。
1. 部署完成后，单击“转到资源”。
1. 单击“容器”。
1. 单击“+ 容器”。
   * 为该容器命名。
   * 从下拉列表中选择“Blob”。

   ![创建 Blob 容器][IMG02]

1. Blob 容器创建好以后，Azure 门户会将其列出。

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

以下过程创建该 Spring Boot 应用程序。

1. 浏览到 <https://start.spring.io/>。

1. 指定以下选项：

   * 生成 **Maven** 项目。
   * 指定“Java”。
   * 指定一个其值大于或等于 2.0 的 **Spring Boot** 版本。
   * 指定应用程序的“组”和“项目”名称。 
   * 添加 **Web** 依赖项。

      ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.wingtiptoys.storage 。
   >

1. 指定上面列出的选项后，请单击“生成”。

1. 出现提示时，将项目下载到本地计算机中的路径。

1. 在本地系统中提取文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>配置 Spring Boot 应用，以使用 Azure Storage Starter

以下过程将 Spring Boot 应用程序配置为使用 Azure 存储。

1. 在应用的根目录中找到 pom.xml 文件，例如：

   `C:\SpringBoot\storage\pom.xml`

   -或-

   `/users/example/home/storage/pom.xml`

1. 在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Azure Storage Starter 添加到 `<dependencies>` 列表：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-starter-azure-storage</artifactId>
      <version>1.2.7</version>
   </dependency>
   ```

1. 如果使用的是 JDK 版本 9 或更高版本，请添加以下依赖项：

   ```xml
   <dependency>
       <groupId>javax.xml.bind</groupId>
       <artifactId>jaxb-api</artifactId>
       <version>2.3.1</version>
   </dependency>
   <dependency>
       <groupId>org.glassfish.jaxb</groupId>
       <artifactId>jaxb-runtime</artifactId>
       <version>2.3.1</version>
       <scope>runtime</scope>
   </dependency>
   ```

1. 保存并关闭 pom.xml 文件。

## <a name="create-an-azure-credential-file"></a>创建 Azure 凭据文件

以下过程创建 Azure 凭据文件。

1. 打开命令提示符。

1. 导航到 Spring Boot 应用的 *resources* 目录，例如：

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   -或-

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. 请登录到 Azure 帐户：

   ```azurecli
   az login
   ```

1. 列出订阅：

   ```azurecli
   az account list
   ```
   Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. 指定要用于 Azure 的订阅的 GUID；例如：

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. 创建 Azure 凭据文件：

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>配置 Spring Boot 应用以使用 Azure 存储帐户

以下过程将 Spring Boot 应用程序配置为使用你的 Azure 存储帐户。

1. 在应用的 *resources* 目录中找到 *application.properties* ，例如：

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   -或-

   `/users/example/home/storage/src/main/resources/application.properties`

2. 在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为存储帐户的相应属性：

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=westUS
   spring.cloud.azure.storage.account=wingtiptoysstorage
   blob=azure-blob://containerName/blobName
   ```
   其中：

   |                   字段                   |                                            说明                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            指定之前在本教程中创建的 Azure 凭据文件。             |
   |    `spring.cloud.azure.resource-group`    |           指定包含 Azure 存储帐户的 Azure 资源组。            |
   |        `spring.cloud.azure.region`        | 指定你在创建 Azure 存储帐户时指定的地理区域。 |
   |   `spring.cloud.azure.storage.account`    |            指定之前在本教程中创建的 Azure 存储帐户。             |
   |                   `blob`                  |           指定要存储数据的容器和 Blob 的名称。         |
    
3. 保存并关闭 application.properties 文件。

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>添加示例代码以实现 Azure 存储的基本功能

在本部分，请创建所需的 Java 类，用于在 Azure 存储帐户中存储 Blob。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   -或-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中。 将 wingtiptoys 替换成自己的值：

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. 保存并关闭主应用程序 Java 文件。

### <a name="add-a-blob-controller-class"></a>添加 Blob 控制器类

1. 在应用的包目录中创建名为 *BlobController.java* 的新 Java 文件，例如：

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\BlobController.java`

   -或-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/BlobController.java`

1. 在文本编辑器中打开 Blob 控制器 Java 文件，然后将以下行添加到文件中。  将 *wingtiptoys* 更改为你的资源组，将 *storage* 更改为你的项目名称。

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.*;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   @RequestMapping("blob")
   public class BlobController {
   
       @Value("${blob}")
       private Resource blobFile;
   
       @GetMapping
       public String readBlobFile() throws IOException {
           return StreamUtils.copyToString(
                   this.blobFile.getInputStream(),
                   Charset.defaultCharset());
       }
   
       @PostMapping
       public String writeBlobFile(@RequestBody String data) throws IOException {
           try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
               os.write(data.getBytes());
           }
           return "file was updated";
       }
   }
   ```

1. 保存并关闭 Blob 控制器 Java 文件。

1. 打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   `cd C:\SpringBoot\storage`

   -或-

   `cd /users/example/home/storage`

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 应用程序运行以后，即可使用 *curl* 对其进行测试，例如：

   a. 发送一个要求更新文件内容的 POST 请求：

      ```shell
      curl -d 'new message' -H 'Content-Type: text/plain' localhost:8080/blob
      ```

      此时会看到响应，指出文件已更新。

   b. 发送一个要求验证文件内容的 GET 请求：

      ```shell
      curl -X GET http://localhost:8080/
      ```

     此时会看到已发布的“Hello World”文本。

## <a name="summary"></a>总结

在本教程中，你已使用 **[Spring Initializr]** 创建了一个新的 Java 应用程序，将 Azure Storage Starter 添加到了该应用程序，然后对应用程序进行了配置来将 Blob 上传到 Azure 存储帐户。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](index.yml)

### <a name="additional-resources"></a>其他资源

有关适用于 Microsoft Azure 的其他 Spring Boot 起动器的详细信息，请参阅[适用于 Azure 的 Spring Boot 起动器](spring-boot-starters-for-azure.md)。

有关可从 Spring Boot 应用程序调用的其他 Azure 存储 API 的详细信息，请参阅以下文章：
* [如何通过 Java 使用 Azure Blob 存储](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [如何通过 Java 使用 Azure 队列存储](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [如何通过 Java 使用 Azure 表存储](/azure/cosmos-db/table-storage-how-to-use-java)
* [如何通过 Java 使用 Azure 文件存储](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
