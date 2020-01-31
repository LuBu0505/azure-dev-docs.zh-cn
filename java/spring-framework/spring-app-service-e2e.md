---
title: 将 Spring/Tomcat 应用部署到使用 Azure Database for MySQL 的应用服务
description: 使用 MySQL 的 Java 应用服务的端到端教程
author: KarlErickson
ms.author: karler
ms.date: 11/12/2019
ms.service: app-service
ms.topic: article
ms.openlocfilehash: 47f318708fbe786b2fd0b58dc7d68cdd5c975856
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76872132"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>将 Spring 应用部署到使用 MySQL 的应用服务

本教程将详细介绍在 Linux 上的应用服务中生成、配置、部署和缩放 Java Web 应用并对其进行故障排除的过程。

本教程以常用的 Spring PetClinic 示例应用程序为基础。 在本主题中，你将在本地测试 HSQLDB 版本的应用，然后将其部署到 [Azure 应用服务](/azure/app-service/containers)。 之后，你将配置并部署一个使用 [Azure Database for MySQL](/azure/mysql) 的版本。 最后，你将了解如何通过增加运行应用程序的辅助角色的数量来访问应用日志并进行横向扩展。

## <a name="prerequisites"></a>必备条件

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat](https://tomcat.apache.org/download-80.cgi)
* [MySQL CLI](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>获取示例

若要开始使用示例应用，请使用以下命令克隆和准备源存储库。

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux.git
cd e2e-java-experience-in-app-service-linux
yes | cp -rf .prep/* .
```

## <a name="build-and-run-the-hsqldb-sample-locally"></a>在本地生成和运行 HSQLDB 示例

首先，我们将使用 HSQLDB 作为数据库在本地测试示例。

导航到该示例的 HSQLDB 版本，然后生成它。

```bash
cd initial-hsqldb/spring-framework-petclinic
mvn package
```

接下来，将 TOMCAT_HOME 环境变量设置为 Tomcat 安装位置。

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

然后，更新 *pom.xml* 文件，以便为 Tomcat WAR 文件部署配置 Maven。 将以下 XML 添加为现有 `<plugins>` 元素的子级。 如有必要，请将 `1.7.7` 更改为 [Cargo Maven 2 Plugin](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin) 的当前版本。

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.7</version>
    <configuration>
        <container>
            <containerId>tomcat8x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

此配置就绪后，可以将应用在本地部署到 Tomcat。

```bash
mvn cargo:deploy
```

然后启动 Tomcat。

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

现在可以在浏览器中导航到 [http://localhost:8080](http://localhost:8080) 来查看正在运行的应用，并感受它如何工作。 完成后，在 Bash 提示符下选择 Ctrl+C 以停止 Tomcat。

## <a name="deploy-to-azure-app-service"></a>部署到 Azure 应用服务

现在，你已看到它在本地运行，我们会将该应用部署到 Azure。

首先，设置以下环境变量。

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

Maven 将使用这些值来创建具有你提供的名称的 Azure 资源。 通过使用环境变量，可以将帐户机密保存在项目文件之外。

接下来，更新 *pom.xml* 文件，以便为 Azure 部署配置 Maven。 将以下 XML 添加到之前添加的 `<plugin>` 元素之后。 如有必要，请将 `1.7.0` 更改为[适用于 Azure 应用服务的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)的当前版本。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.9.0</version>
    <configuration>
        <schemaVersion>v2</schemaVersion>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>            
            <webContainer>TOMCAT 8.5</webContainer>
        </runtime>
    </configuration>
</plugin>
```

接下来，登录到 Azure。

```azurecli
az login
```

然后，将该应用部署到 Linux 上的应用服务。

```bash
mvn azure-webapp:deploy
```

现在可以导航到 `https://<app-name>.azurewebsites.net`（在替换 `<app-name>` 后）以查看正在运行的应用。

## <a name="set-up-azure-database-for-mysql"></a>设置 Azure Database for MySQL

接下来，我们将切换到使用 MySQL 而不是 HSQLDB。 我们将在 Azure 上创建一个 MySQL 服务器实例并添加一个数据库，然后使用新的数据库连接信息更新应用配置。

首先，设置以下环境变量以便在后面的步骤中使用。

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

接下来，创建并初始化数据库服务器。 使用 [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up) 进行初始配置。 然后，使用 [az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set) 提高连接超时值并设置服务器时区。

```azurecli
az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value -8:00
```

然后，使用 MySQL CLI 创建数据库。

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

在 MySQL CLI 提示符下，运行以下命令，并将 `<database name>` 替换为先前为环境变量 `MYSQL_DATABASE_NAME` 指定的相同值。

```console
CREATE DATABASE <database name>;
```

MySQL 现已可供使用。

## <a name="configure-the-app-for-mysql"></a>为 MySQL 配置应用

接下来，我们会将连接信息添加到应用的 MySQL 版本，然后将其部署到应用服务。

首先，在 Bash 提示符下导航到正确的文件夹。

```bash
cd ../../initial-mysql/spring-framework-petclinic
```

然后，更新 *pom.xml* 文件，以便使 MySQL 成为有效配置。 从 HSQLDB 配置文件中删除 `<activation>` 元素，改为将其放入 MySQL 配置文件中，如下所示。 代码片段的其余部分显示现有配置。 请注意，Maven 将使用你以前设置环境变量的方式来配置你的 MySQL 访问权限。

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

接下来，更新 *pom.xml* 文件，以便为 Azure 部署和 MySQL 的使用配置 Maven。 将以下 XML 添加到之前添加的 `<plugin>` 元素之后。 如有必要，请将 `1.7.0` 更改为[适用于 Azure 应用服务的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)的当前版本。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>

        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>

    </configuration>
</plugin>
```

接下来，生成应用，然后通过使用 Tomcat 部署并运行该应用，对其进行本地测试。

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

现在可以在 [http://localhost:8080](http://localhost:8080) 本地查看应用。 应用的外观和行为与以前相同，但应用使用的是 Azure Database for MySQL 而不是 HSQLDB。 完成后，在 Bash 提示符下选择 Ctrl+C 以停止 Tomcat。

最后，将该应用部署到应用服务。

```bash
mvn azure-webapp:deploy
```

现在可以导航到 `https://<app-name>.azurewebsites.net`，查看正在使用应用服务和 Azure Database for MySQL 运行的应用。

## <a name="access-the-app-logs"></a>访问应用日志

如果需要进行故障排除，可以查看应用日志。 若要在本地计算机上打开远程日志流，请使用以下命令。

```azurecli
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

查看完日志后，请选择 Ctrl + C 停止流。

`https://<app-name>.scm.azurewebsites.net/api/logstream` 上也提供日志流。

## <a name="scale-out"></a>横向扩展

若要支持增加的应用流量，可以使用以下命令横向扩展到多个实例。

```azurecli
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

祝贺你！ 你已使用 Spring Framework、JSP、Spring Data、Hibernate、JDBC、Linux 上的应用服务和 Azure Database for MySQL 构建并横向扩展了 Java Web 应用。

## <a name="clean-up-resources"></a>清理资源

在前面的部分中，你在资源组中创建了 Azure 资源。 如果认为将来不使用这些资源，请运行以下命令删除资源组。

```azurecli
az group delete --name ${RESOURCEGROUP_NAME}
```

## <a name="next-steps"></a>后续步骤

接下来，查看适用于 Java 和应用服务的其他配置和 CI/CD 选项。

> [!div class="nextstepaction"]
> [为 Azure 应用服务配置 Linux Java 应用](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [使用 Azure Pipelines 构建并部署 Java Web 应用](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [使用 Jenkins 插件部署到 Azure 应用服务](/azure/jenkins/deploy-jenkins-app-service-plugin)
