---
title: 使用 Maven 将 KumuluzEE Web 应用部署到 Azure 应用服务
description: 了解如何使用 Azure Web 应用的 Maven 插件将 KumuluzEE 应用部署到 Linux 上的应用服务。
services: app-service
documentationcenter: java
ms.date: 06/10/2020
ms.service: app-service
ms.topic: article
ms.custom: ''
ms.openlocfilehash: bafc670e22838e7fe28965f993be9fa7aa343f12
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846628"
---
# <a name="deploy-a-kumuluzee-web-app-to-azure-app-service-with-maven"></a>使用 Maven 将 KumuluzEE Web 应用部署到 Azure 应用服务

在本快速入门中，你将使用[适用于 Azure 应用服务 Web 应用的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将 KumuluzEE 应用程序部署到 [Linux 上的 Azure 应用服务](/azure/app-service/containers/)。 如果要将应用的依赖项、运行时和配置整合到单个可部署项目中，你需要选择通过 [Tomcat 和 WAR 文件](/azure/app-service/containers/quickstart-java)进行 Java SE 部署。

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

* [Azure CLI](/cli/azure/)，在本地或通过 [Azure Cloud Shell](https://shell.azure.com)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](https://maven.apache.org/) 版本 3）。

## <a name="install-and-sign-in-to-azure-cli"></a>安装并登录 Azure CLI

获取用于部署 KumuluzEE 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](/cli/azure/)。

通过使用 Azure CLI 登录到 Azure 帐户：

```shell
az login
```

按照说明完成登录过程。

## <a name="create-sample-app-from-microprofile-starter"></a>从 MicroProfile Starter 创建示例应用

在本部分中，你将创建一个 KumuluzEE 应用程序并在本地测试它。

### <a name="create-java-se-8-base-project"></a>创建 Java SE 8 基本项目

1. 打开 Web 浏览器并访问 [MicroProfile Starter](https://start.microprofile.io/) 站点。

   ![适用于 KumuluzEE 的 MicroProfile Starter](./media/kumuluzee/microprofile-starter-kumuluzee.png)

2. 输入或选择字段，如下所示。  

   |  字段  |  值  |
   | ---- | ---- |
   |  groupId  |  com.microsoft.azure.samples.kumuluzee  |
   |  artifactId  |  kumuluzEE-hello-azure  |
   |  MicroProfile 版本  |  MP 3.2  |
   |  Java SE 版本  |  Java 8  |
   |  MicroProfile 运行时  |  KumuluzEE  |
   |  规范示例  |  指标，OpenAPI  |

3. 选择“下载”按钮下载该项目。

4. 解压缩存档文件，例如：

   ```bash
   unzip kumuluzEE-hello-azure.zip
   ```

### <a name="run-the-application-in-local-environment"></a>在本地环境中安装应用程序

1. 将目录更改为已完成项目；例如：

   ```bash
   cd kumuluzEE-hello-azure/
   ```

2. 使用 Maven 生成项目，例如：

   ```bash
   mvn clean package
   ```

3. 使用以下命令运行该应用程序：

   ```bash
   java -jar target/kumuluzEE-hello-azure.jar
   ```

4. 使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。 例如，如果有可用的 Curl，可以使用以下命令：

   ```bash
   curl http://localhost:8080/data/hello
   ```

5. 应当会看到显示了以下消息：Hello World。

## <a name="configure-maven-plugin-for-azure-app-service"></a>配置适用于 Azure 应用服务的 Maven 插件

在本部分中，你将配置 KumuluzEE 项目的 pom.xml 文件，以便 Maven 可以将应用部署到 Linux 上的 Azure 应用服务。

1. 在代码编辑器中打开 pom.xml 文件。

2. 在 pom.xml 文件的 `<build>` 部分，在 `<plugins>` 标记内插入以下 `<plugin>` 项。

   ```xml
   <build>
     <finalName>kumuluzEE-hello-azure</finalName>
     <plugins>
       <plugin>
         <groupId>com.microsoft.azure</groupId>
         <artifactId>azure-webapp-maven-plugin</artifactId>
         <version>1.10.0</version>
       </plugin>
     </plugins>
   </build>
   ```

3. 然后可以配置部署，在命令提示符下运行以下 maven 命令，并使用**数字**选择提示中的这些选项：

   ```bash
   mvn azure-webapp:config
   ```

   选项参数：  

   |  输入字段  |  输入/选择值  |
   | ---- | ---- |
   |  定义 OS 值（默认值：Linux）：  | 1. Linux  |
   |  定义 javaVersion 值（默认值：Java 8）：   | 2.Java 8  |
   |  确认（是/否） | y |

   你可以使用以下命令进行配置：

   ```bash
   $ mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO] 
   [INFO] ----< com.microsoft.azure.samples.kumuluzee:kumuluzEE-hello-azure >-----
   [INFO] Building kumuluzEE-hello-azure 1.0-SNAPSHOT
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO] 
   [INFO] --- azure-webapp-maven-plugin:1.10.0:config (default-cli) @ kumuluzEE-hello-azure ---
   1. linux [*]
   2. windows
   3. docker
   Enter index to use: 1
   Define value for javaVersion(Default: Java 8): 
   1. Java 11
   2. Java 8 [*]
   Enter index to use: 2
   Please confirm webapp properties
   AppName : kumuluzEE-hello-azure-1601006602397
   ResourceGroup : kumuluzEE-hello-azure-1601006602397-rg
   Region : westeurope
   PricingTier : PremiumV2_P1v2
   OS : Linux
   RuntimeStack : JAVA 8-jre8
   Deploy to slot : false
   Confirm (Y/N)? : y
   [INFO] Saving configuration to pom.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  44.223 s
   [INFO] Finished at: 2020-09-25T13:04:02+09:00
   [INFO] ------------------------------------------------------------------------
   ```

4. 将 `<appSettings>` 部分添加到 `PORT`、`WEBSITES_PORT` 和 `WEBSITES_CONTAINER_START_TIME_LIMIT` 的 `<configuration>` 部分。  
 最后，你可以查看 `azure-webapp-maven-plugin` 的以下 XML 条目。

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.10.0</version>
     <configuration>
       <schemaVersion>V2</schemaVersion>
       <resourceGroup>microprofile</resourceGroup>
       <appName>kumuluzEE-hello-azure-1601006602397</appName>
       <pricingTier>P1v2</pricingTier>
       <region>japaneast</region>
       <runtime>
         <os>linux</os>
         <javaVersion>jre8</javaVersion>
         <webContainer>jre8</webContainer>
       </runtime>
       <appSettings>
         <property>
           <name>PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_CONTAINER_START_TIME_LIMIT</name>
           <value>600</value>
         </property>
       </appSettings>
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
   ```

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。 为此，请使用以下步骤：

1. 在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如**：

   ```bash
   mvn clean package
   ```

2. 使用 Maven 将 Web 应用部署到 Azure；例如：

   ```bash
   mvn azure-webapp:deploy
   ```

   如果部署成功，你可以在控制台上看到以下消息。

   ```bash
   mvn azure-webapp:deploy

   [INFO] Successfully deployed the artifact to https://kumuluzee-hello-azure-1601006602397.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:23 min
   [INFO] Finished at: 2020-09-25T13:13:14+09:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven 会将 Web 应用部署到 Azure；如果 Web 应用或 Web 应用计划尚不存在，则将为你创建一个。 可能需要等待数分钟，然后才能通过输出中显示的 URL 查看 Web 应用。 在 Web 浏览器中导航到该 URL。  应该会看到以下屏幕。

   ![KumuluzEE 标题页](./media/kumuluzee/kumuluzee-front-page.png)

   Web 部署完成后即可通过 [Azure 门户]进行管理。

   * 你的 Web 应用将在 microprofile 资源组中列出：

   ![Azure 门户应用服务中列出的 Web 应用](./media/kumuluzee/kumuluzee-azure-portal-rg.png)

   * 在 Web 应用的“概述”中单击 `Browse` 按钮即可访问 Web 应用。  
   验证部署成功并且正在运行。 应该会看到显示以下屏幕：

   ![在 Azure 门户应用服务中查找 Web 应用的 URL](./media/kumuluzee/kumuluzee-azure-portal-manage.png)

## <a name="confirm-the-log-stream-from-running-app-service"></a>确认来自正在运行的应用服务的日志流
 
你可以查看（或“跟踪”）来自正在运行的应用服务的日志。 在站点代码中对 `console.log` 的任何调用都将显示在终端中。
 
   ```azurecli
   az webapp log tail -g microprofile -n kumuluzEE-hello-azure-1601006602397
   ```
 
   ![确认日志流](./media/kumuluzee/azure-cli-app-service-log-stream.png)
 

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

* 在 Azure 门户上的左侧菜单中选择“资源组”。
* 在“按名称筛选”字段中输入 microprofile，在本教程中创建的资源组应具有此前缀 。
* 选择在本教程中创建的资源组。
* 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

若要了解有关 MicroProfile 和 Azure 的详细信息，请继续访问“Azure 上的 MicroProfile”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 MicroProfile](./index.yml)

### <a name="additional-resources"></a>其他资源

有关本文中讨论的各项技术的详细信息，请参阅以下文章：

* [适用于 Azure Web 应用的 Maven 插件]

* [使用 Azure CLI 2.0 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 设置参考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: ../index.yml
[Azure 门户]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[适用于 Azure Web 应用的 Maven 插件]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png