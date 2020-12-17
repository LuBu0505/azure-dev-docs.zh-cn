---
title: Azure SDK for Java 入门
description: 了解如何创建 Azure 云资源，以及如何在 Java 应用程序中连接和使用这些资源。
keywords: Azure, Java, SDK, API, 身份验证, 入门
author: bmitchell287
ms.author: brendm
ms.date: 11/20/2020
ms.topic: article
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.custom: seo-java-august2019, devx-track-java, devx-track-azurecli
ms.openlocfilehash: 7c34254cbe45fdb6e60f9f1a30ab409f72c2aee4
ms.sourcegitcommit: c1ef7aa8ed2e88e98b190e42cffde52cf301958d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97034538"
---
# <a name="get-started-with-cloud-development-using-java-on-azure"></a>在 Azure 上使用 Java 开始云开发

本文逐步讲解如何针对使用 Java 的 Azure 开发设置开发环境。 然后，创建一些 Azure 资源并连接到它们，以执行一些基本任务，例如，上传文件或部署 Web 应用程序。 完成本指南后，便可以在自己的 Java 应用程序中开始使用 Azure 服务。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](/cli/azure/install-az-cli2)。
- [Java 8](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support)（Azure Cloud Shell 中已随附）。
- [Maven 3](https://maven.apache.org/download.cgi)（Azure Cloud Shell 中已随附）。

## <a name="set-up-authentication"></a>设置身份验证

Java 应用程序需要 Azure 订阅中的读取和创建权限才能运行本教程中的示例代码。 创建一个服务主体，并将应用程序配置为使用该服务主体的凭据运行。 通过服务主体可以创建与标识关联的非交互式帐户，该帐户仅拥有运行应用所需的特权。

[使用 Azure CLI 2.0 创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)并捕获输出：

```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest
```

这会提供采用以下格式的回复：

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

接下来，配置环境变量：

- `AZURE_SUBSCRIPTION_ID`：在 Azure CLI 2.0 中使用 `az account show` 中的 ID 值。
- `AZURE_CLIENT_ID`：使用从服务主体输出中获取的输出中的 appId 值。
- `AZURE_CLIENT_SECRET`：使用服务主体输出中的 password 值。
- `AZURE_TENANT_ID`：使用服务主体输出中的 tenant 值。

有关身份验证的更多选项，请参阅 [Azure 标识](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#azure-identity-client-library-for-java)。

## <a name="tooling"></a>工具

### <a name="create-a-new-maven-project"></a>创建新的 Maven 项目

> [!NOTE]
> 本文使用 Maven 生成工具来生成和运行示例代码。 其他生成工具（例如 Gradle）也可与适用于 Java 的 Azure 库一起使用。

在系统上的新目录中通过命令行创建 Maven 项目。

```shell
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp \
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

此步骤在 `testAzureApp` 文件夹下创建基本的 Maven 项目。 将以下条目添加到项目 `pom.xml` 中，以导入本教程的示例代码中使用的库。

```XML
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.0</version>
</dependency>
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager</artifactId>
    <version>2.1.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>12.8.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

在顶级 `project` 元素下添加 `build` 条目，以使用 [maven-exec-plugin](https://www.mojohaus.org/exec-maven-plugin/) 来运行示例。

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```

### <a name="install-the-azure-toolkit-for-intellij"></a>安装用于 IntelliJ 的 Azure 工具包

如果打算以编程方式部署 Web 应用或 API，则必须使用 [Azure 工具包](../toolkit-for-intellij/index.yml)。 当前，它不用于任何其他类型的开发。 以下步骤概述了安装过程。 有关快速入门，请参阅 [Azure Toolkit for IntelliJ](../toolkit-for-intellij/create-hello-world-web-app.md)。

1. 选择“文件”菜单，然后选择“设置” 。
1. 选择“浏览存储库”，然后搜索“Azure”并安装“Azure toolkit for Intellij”  。
1. 重启 IntelliJ。

### <a name="install-the-azure-toolkit-for-eclipse"></a>安装用于 Eclipse 的 Azure 工具包

如果打算以编程方式部署 Web 应用或 API，则必须使用 [Azure 工具包](../toolkit-for-eclipse/index.yml)。 当前，它不用于任何其他类型的开发。 以下步骤概述了安装过程。 有关快速入门，请参阅 [Azure Toolkit for Eclipse](../toolkit-for-eclipse/create-hello-world-web-app.md)。

1. 选择“帮助”菜单，然后选择“安装新软件” 。
1. 在“使用”框中输入 `http://dl.microsoft.com/eclipse/`，然后按 Enter 。
1. 选中“Azure toolkit for Java”旁边的复选框。 清除“在安装过程中联系所有更新站点以找到所需的软件”复选框。 然后，选择“下一步”。

## <a name="create-a-linux-virtual-machine"></a>创建 Linux 虚拟机

在项目的 `src/main/java/com/fabrikam` 目录中创建名为 `AzureApp.java` 的新文件，并在其中粘贴以下代码块。 使用计算机的实际值更新 `userName` 和 `sshKey` 变量。 该代码会在美国东部 Azure 区域中运行的资源组 `sampleResourceGroup` 内创建名为 `testLinuxVM` 的新 Linux 虚拟机 (VM)。

```java
package com.fabrikam;

import com.azure.core.credential.TokenCredential;
import com.azure.core.http.policy.HttpLogDetailLevel;
import com.azure.core.management.AzureEnvironment;
import com.azure.core.management.Region;
import com.azure.core.management.profile.AzureProfile;
import com.azure.identity.AzureAuthorityHosts;
import com.azure.identity.EnvironmentCredentialBuilder;
import com.azure.resourcemanager.AzureResourceManager;
import com.azure.resourcemanager.compute.models.KnownLinuxVirtualMachineImage;
import com.azure.resourcemanager.compute.models.VirtualMachine;
import com.azure.resourcemanager.compute.models.VirtualMachineSizeTypes;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {
            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile)
                    .withDefaultSubscription();

            // Create an Ubuntu virtual machine in a new resource group.
            VirtualMachine linuxVM = azureResourceManager.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

通过命令行运行示例。

```shell
mvn compile exec:java
```

当 SDK 向 Azure REST API 发出基础调用来配置 VM 及其资源时，控制台中会显示一些 REST 请求和响应。 程序完成后，使用 Azure CLI 2.0 验证订阅中的 VM。

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

验证代码正常运行后，使用 CLI 删除 VM 及其资源。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>从 GitHub 存储库部署 Web 应用

将 `AzureApp.java` 中的 main 方法替换为以下方法。 在运行代码之前，请将 `appName` 变量更新为唯一值。 此代码会将公共 GitHub 存储库的 `master` 分支中的某个 Web 应用程序部署到免费定价层中运行的新 [Azure 应用服务 Web 应用](/azure/app-service-web/app-service-web-overview)。

```java
    public static void main(String[] args) {
        try {

            final String appName = "YOUR_APP_NAME";

            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);
            
            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile)
                    .withDefaultSubscription();

            WebApp app = azureResourceManager.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

如前所述使用 Maven 运行代码。

```shell
mvn clean compile exec:java
```

使用 CLI 打开指向该应用程序的浏览器。

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

验证部署后，请从订阅中删除 Web 应用和计划。

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>连接到 Azure SQL 数据库

将 `AzureApp.java` 中的当前 main 方法替换为以下代码。 设置变量的实际值。
此代码创建具有允许远程访问的防火墙规则的新 SQL 数据库。 然后，代码使用 SQL 数据库 JDBC 驱动程序连接到该数据库。

```java
    public static void main(String args[])
    {
        // Create the db using the management libraries.
        try {
            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile);

            final String adminUser = "YOUR_USERNAME_HERE";
            final String sqlServerName = "YOUR_SERVER_NAME_HERE";
            final String sqlDbName = "YOUR_DB_NAME_HERE";
            final String dbPassword = "YOUR_PASSWORD_HERE";
            final String firewallRuleName = "YOUR_RULE_NAME_HERE";

            SqlServer sampleSQLServer = azureResourceManager.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleSqlResourceGroup")
                    .withAdministratorLogin(adminUser)
                    .withAdministratorPassword(dbPassword)
                    .defineFirewallRule(firewallRuleName)
                        .withIpAddressRange("0.0.0.0","255.255.255.255")
                        .attach()
                    .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // Assemble the connection string to the database.
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // Connect to the database, create a table, and insert an entry into it.
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```

通过命令行运行示例。

```shell
mvn clean compile exec:java
```

然后使用 CLI 清理资源。

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>将 Blob 写入新存储帐户

将 `AzureApp.java` 中的当前 main 方法替换为以下代码。 此代码会创建 [Azure 存储帐户](/azure/storage/common/storage-introduction)。 然后，代码使用适用于 Java 的 Azure 存储库在云中创建新的文本文件。

```java
    public static void main(String[] args) {

        try {
            TokenCredential tokenCredential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(tokenCredential, profile)
                    .withDefaultSubscription();

            // Create a new storage account.
            String storageAccountName = "YOUR_STORAGE_ACCOUNT_NAME_HERE";
            StorageAccount storage = azureResourceManager.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

            // Create a storage container to hold the file.
            List<StorageAccountKey> keys = storage.getKeys();
            PublicEndpoints endpoints = storage.endPoints();
            String accountName = storage.name();
            String accountKey = keys.get(0).value();
            String endpoint = endpoints.primary().blob();

            StorageSharedKeyCredential credential = new StorageSharedKeyCredential(accountName, accountKey);

            BlobServiceClient storageClient =new BlobServiceClientBuilder()
                    .endpoint(endpoint)
                    .credential(credential)
                    .buildClient();

            // Container name must be lowercase.
            BlobContainerClient blobContainerClient = storageClient.getBlobContainerClient("helloazure");
            blobContainerClient.create();

            // Make the container public.
            blobContainerClient.setAccessPolicy(PublicAccessType.CONTAINER, null);

            // Write a blob to the container.
            String fileName = "helloazure.txt";
            String textNew = "Hello Azure";

            BlobClient blobClient = blobContainerClient.getBlobClient(fileName);
            InputStream is = new ByteArrayInputStream(textNew.getBytes());
            blobClient.upload(is, textNew.length());

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

通过命令行运行示例。

```shell
mvn clean compile exec:java
```

可以通过 Azure 门户或使用 [Azure 存储资源管理器](/azure/vs-azure-tools-storage-explorer-blobs)浏览存储帐户中的 `helloazure.txt` 文件。

使用 CLI 清理存储帐户。

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>学习更多示例

若要详细了解如何使用适用于 Java 的 Azure 管理库来管理资源和自动执行任务，请参阅针对[虚拟机](java-sdk-azure-virtual-machine-samples.md)、[Web 应用](java-sdk-azure-web-apps-samples.md)和 [SQL 数据库](java-sdk-azure-sql-database-samples.md)的示例代码。

## <a name="reference-and-release-notes"></a>参考和发行说明

我们为所有包提供了[参考](/java/api)文档。

## <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure+java) 社区中提问。 在[项目 GitHub](https://github.com/Azure/azure-sdk-for-java) 中针对用于 Java 的 Azure 库报告 bug 和反映问题。
