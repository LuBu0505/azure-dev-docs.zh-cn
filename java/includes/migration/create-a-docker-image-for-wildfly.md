---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: ab70d4861cc619417f78cfc053d44b22dd35014f
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897566"
---
### <a name="create-a-docker-image-for-wildfly"></a>创建 WildFly 的 Docker 映像

若要创建 Dockerfile，需要符合以下先决条件：

* 支持的 JDK
* 安装 WildFly
* JVM 运行时选项。
* 可以通过某种方法传入环境变量（如果适用）。

然后，可以执行以下部分所述的步骤（如果适用）。 对于 Dockerfile 和 Web 应用程序，可以从使用 [WildFly 容器快速入门存储库](https://github.com/Azure/wildfly-container-quickstart)着手。

1. [配置 KeyVault FlexVolume](#configure-keyvault-flexvolume)
2. [设置数据源](#set-up-data-sources)
3. [设置 JNDI 资源](#set-up-jndi-resources)
4. [查看 WildFly 配置](#review-wildfly-configuration)

#### <a name="configure-keyvault-flexvolume"></a>配置 KeyVault FlexVolume

创建 Azure KeyVault 并填充所有必要的机密。 有关详细信息，请参阅[快速入门：使用 Azure CLI 在 Azure Key Vault 中设置和检索机密](/azure/key-vault/quick-create-cli)。 然后，配置 [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md)，使这些机密可供 Pod 访问。

还需要更新用于启动 WildFly 的启动脚本。 此脚本必须在启动服务器之前将证书导入 WildFly 使用的密钥存储。

#### <a name="set-up-data-sources"></a>设置数据源

若要配置 WildFly 以访问数据源，需将 JDBC 驱动程序 JAR 添加到 Docker 映像，然后执行相应的 JBoss CLI 命令。 生成 Docker 映像时，这些命令必须设置数据源。

以下步骤提供了有关 PostgreSQL、MySQL 和 SQL Server 的说明。

1. 下载 [PostgreSQL](https://jdbc.postgresql.org/download.html)、[MySQL](https://dev.mysql.com/downloads/connector/j/) 或 [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) 的 JDBC 驱动程序。

    解压缩已下载的存档，获取驱动程序 .jar 文件。

1. 创建一个名称类似于 `module.xml` 的文件，并添加以下标记。 将 `<module name>` 占位符（包括尖括号）替换为适用于 PostgreSQL 的 `org.postgres`、适用于 MySQL 的 `com.mysql` 或适用于 SQL Server 的 `com.microsoft`。 将 `<JDBC .jar file path>` 替换为上一步骤中 .jar 文件的名称，包括将文件置于 Docker 映像中时所在位置的完整路径，例如 `/opt/database`。

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

1. 创建一个名称类似于 `datasource-commands.cli` 的文件，并添加以下代码。 将 `<JDBC .jar file path>` 替换为在上一步使用的值。 将 `<module file path>` 替换为上一步中的文件名和路径，例如 `/opt/database/module.xml`。

    **PostgreSQL**

    ```console
    batch
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)
    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker
    reload
    run batch
    shutdown
    ```

    **MySQL**

    ```console
    batch
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)
    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter
    reload
    run batch
    shutdown
    ```

    **SQL Server**

    ```console
    batch
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)
    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter
    reload
    run batch
    shutdown
    ```

1. 更新应用程序的 JTA 数据源配置：

    打开应用的 `src/main/resources/META-INF/persistence.xml` 文件，找到 `<jta-data-source>` 元素。 替换其内容，如下所示：

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

1. 将以下内容添加到 `Dockerfile`，以便在生成 Docker 映像时创建数据源

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/database/datasource-commands.cli && \
    sleep 30
    ```

1. 确定要使用的 `DATABASE_CONNECTION_URL`，它因数据库服务器而异，并且不同于 Azure 门户中的值。 此处所示的 URL 格式是必需的，方便 WildFly 使用：

    **PostgreSQL**

    ```console
    jdbc:postgresql://<database server name>:5432/<database name>?ssl=true
    ```

    **MySQL**

    ```console
    jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT
    ```

    **SQL Server**

    ```console
    jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

1. 在以后创建部署 YAML 时，需使用适当的值传递环境变量 `DATABASE_CONNECTION_URL`、`DATABASE_SERVER_ADMIN_FULL_NAME` 和 `DATABASE_SERVER_ADMIN_PASSWORD`。

若要详细了解如何通过 WildFly 配置数据库连接性，请参阅 [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7)、[MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) 或 [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898)。

#### <a name="set-up-jndi-resources"></a>设置 JNDI 资源

若要设置每个 JNDI 资源，需要在 WildFly 上进行配置，并且通常需要执行以下步骤：

1. 下载必要的 JAR 文件，并将其复制到 Docker 映像中。
2. 创建一个 WildFly *module.xml* 文件，该文件引用那些 JAR 文件。
3. 创建特定 JNDI 资源所需的任何配置。
4. 创建要在 Docker 生成期间使用的 JBoss CLI 脚本，以便注册 JNDI 资源。
5. 将所有内容添加到 Dockerfile。
6. 在部署 YAML 中传递适当的环境变量。

以下示例演示了创建 JNDI 资源以通过 JMS 连接到 Azure 服务总线所需的步骤。

1. 下载 [Apache Qpid JMS 提供程序](https://qpid.apache.org/components/jms/index.html)

    解压缩已下载的存档，获取 .jar 文件。

1. 创建一个名称类似于 `module.xml` 的文件，并在 `/opt/servicebus` 中添加以下标记。 请确保 JAR 文件的版本号与前一步骤的 JAR 文件的名称相符。

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="org.jboss.genericjms.provider">
     <resources>
      <resource-root path="proton-j-0.31.0.jar"/>
      <resource-root path="qpid-jms-client-0.40.0.jar"/>
      <resource-root path="slf4j-log4j12-1.7.25.jar"/>
      <resource-root path="slf4j-api-1.7.25.jar"/>
      <resource-root path="log4j-1.2.17.jar"/>
      <resource-root path="netty-buffer-4.1.32.Final.jar" />
      <resource-root path="netty-codec-4.1.32.Final.jar" />
      <resource-root path="netty-codec-http-4.1.32.Final.jar" />
      <resource-root path="netty-common-4.1.32.Final.jar" />
      <resource-root path="netty-handler-4.1.32.Final.jar" />
      <resource-root path="netty-resolver-4.1.32.Final.jar" />
      <resource-root path="netty-transport-4.1.32.Final.jar" />
      <resource-root path="netty-transport-native-epoll-4.1.32.Final-linux-x86_64.jar" />
      <resource-root path="netty-transport-native-kqueue-4.1.32.Final-osx-x86_64.jar" />
      <resource-root path="netty-transport-native-unix-common-4.1.32.Final.jar" />
      <resource-root path="qpid-jms-discovery-0.40.0.jar" />
     </resources>
     <dependencies>
      <module name="javax.api"/>
      <module name="javax.jms.api"/>
     </dependencies>
    </module>
    ```

1. 在 `/opt/servicebus` 中创建 `jndi.properties` 文件。

    ```console
    connectionfactory.${MDB_CONNECTION_FACTORY}=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}
    queue.${MDB_QUEUE}=${SB_QUEUE}
    topic.${MDB_TOPIC}=${SB_TOPIC}
    ```

1. 创建一个名称类似于 `servicebus-commands.cli` 的文件，并添加以下代码。

    ```console
    batch
    /subsystem=ee:write-attribute(name=annotation-property-replacement,value=true)
    /system-property=property.mymdb.queue:add(value=myqueue)
    /system-property=property.connection.factory:add(value=java:global/remoteJMS/SBF)
    /subsystem=ee:list-add(name=global-modules, value={"name" => "org.jboss.genericjms.provider", "slot" =>"main"}
    /subsystem=naming/binding="java:global/remoteJMS":add(binding-type=external-context,module=org.jboss.genericjms.provider,class=javax.naming.InitialContext,environment=[java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory,org.jboss.as.naming.lookup.by.string=true,java.naming.provider.url=/opt/servicebus/jndi.properties])
    /subsystem=resource-adapters/resource-adapter=generic-ra:add(module=org.jboss.genericjms,transaction-support=XATransaction)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:add(class-name=org.jboss.resource.adapter.jms.JmsManagedConnectionFactory, jndi-name=java:/jms/${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=ConnectionFactory:add(value=${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=JndiParameters:add(value="java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory;java.naming.provider.url=/opt/servicebus/jndi.properties")
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:write-attribute(name=security-application,value=true)
    /subsystem=ejb3:write-attribute(name=default-resource-adapter-name, value=generic-ra)
    run-batch
    reload
    shutdown
    ```

1. 将以下内容添加到 `Dockerfile`，以便在生成 Docker 映像时创建 JNDI 源

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/servicebus/servicebus-commands.cli && \
    sleep 30
    ```

1. 在以后创建部署 YAML 时，需使用适当的值传递环境变量 `MDB_CONNECTION_FACTORY`、`DEFAULT_SBNAMESPACE`、`SB_SAS_POLICY`、`SB_SAS_KEY`、`MDB_QUEUE`、`SB_QUEUE`、`MDB_TOPIC` 和 `SB_TOPIC`。

#### <a name="review-wildfly-configuration"></a>查看 WildFly 配置

查看 [WildFly Admin Guide](https://docs.wildfly.org/18/Admin_Guide.html)（WildFly 管理指南），该指南涵盖了上一指南未涵盖的任何其他预迁移步骤。
