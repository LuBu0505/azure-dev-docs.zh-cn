---
title: Azure 中的 Spring Cloud Function 入门
description: 了解如何使用 Azure 中的 Spring Cloud Function。
documentationcenter: java
author: jdubois
manager: brborges
ms.author: judubois
ms.date: 07/17/2019
ms.service: azure-functions
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 5cfdbb3542222a7ff9547b25836237d5d0991ddb
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86378471"
---
# <a name="getting-started-with-spring-cloud-function-in-azure"></a>Azure 中的 Spring Cloud Function 入门

本文介绍如何使用 [Spring Cloud Function](https://spring.io/projects/spring-cloud-function) 开发 Java 函数并将其发布到 Azure Functions。 完成后，你的函数代码将在 Azure 中的[使用计划](/azure/azure-functions/functions-scale#consumption-plan)上运行，并可使用 HTTP 请求触发。

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>必备条件

若要使用 Java 开发函数，必须安装以下软件：

- [Java 开发人员工具包](https://aka.ms/azure-jdks)版本 8
- [Apache Maven](https://maven.apache.org) 版本 3.0 或更高版本
- [Azure CLI](/cli/azure)
- [Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 2.7.1158 或更高版本

> [!IMPORTANT]
> JAVA_HOME 环境变量必须设置为 JDK 的安装位置，以完成本快速入门。

## <a name="what-we-are-going-to-build"></a>我们将要构建的内容

我们将要构建一个在 Azure Functions 上运行并使用 Spring Cloud Function 进行配置的经典“Hello, World”函数。

该函数会接收一个简单的 `User` JSON 对象，此对象包含一个用户名并发送回一个 `Greeting` 对象，后者包含显示给该用户的欢迎消息。

我们在这里构建的项目在 [https://github.com/Azure-Samples/hello-spring-function-azure](https://github.com/Azure-Samples/hello-spring-function-azure) 上提供，因此若要查看本快速入门中详述的最终项目，可以直接使用该示例存储库。

## <a name="create-a-new-maven-project"></a>创建新的 Maven 项目

我们将创建一个空的 Maven 项目，然后使用 Spring Cloud Function 和 Azure Functions 对其进行配置。

在空文件夹中创建新的 *pom.xml*，然后复制/粘贴 [https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml](https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml) 上的示例项目中的内容。

> [!NOTE]
> 该文件使用 Spring Boot 和 Spring Cloud Function 中的 Maven 依赖项，并配置 Spring Boot 和 Azure Functions Maven 插件。

需要为应用程序自定义一些属性：

- `<functionAppName>` 是 Azure 函数的名称
- `<functionAppRegion>` 是部署函数的 Azure 区域的名称
- `<functionResourceGroup>` 是要使用的 Azure 资源组的名称

应该在 *pom.xml* 文件顶部附近直接更改这些属性：

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <azure.functions.maven.plugin.version>1.6.0</azure.functions.maven.plugin.version>
    <functionAppName>my-spring-function</functionAppName>
    <functionAppRegion>westus</functionAppRegion>
    <stagingDirectory>${project.build.directory}/azure-functions/${functionAppName}</stagingDirectory>
    <functionResourceGroup>my-resource-group</functionResourceGroup>
    <start-class>com.example.HelloFunction</start-class>
    <spring.boot.wrapper.version>1.0.25.RELEASE</spring.boot.wrapper.version>
</properties>
```

## <a name="create-azure-configuration-files"></a>创建 Azure 配置文件

创建 *src/main/azure* 文件夹，向其添加以下 Azure Functions 配置文件。

*host.json*：

```json
{
  "version": "2.0",
  "functionTimeout": "00:10:00"
}
```

*local.settings.json*：

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "java",
    "AzureWebJobsDashboard": ""
  }
}
```

## <a name="create-domain-objects"></a>创建域对象

Azure Functions 可以接收和发送 JSON 格式的对象。
我们现在将创建表示域模型的 `User` 和 `Greeting` 对象。
若要自定义本快速入门，使之更有趣，可以创建包含更多属性的更复杂的对象。

创建 *src/main/java/com/example/model* 文件夹，添加以下两个文件：

*User.java*：

```java
package com.example.model;

public class User {

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

*Greeting.java*：

```java
package com.example.model;

public class Greeting {

    public Greeting() {
    }

    public Greeting(String message) {
        this.message = message;
    }

    private String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```

## <a name="create-the-spring-boot-application"></a>创建 Spring Boot 应用程序

此应用程序将管理所有业务逻辑，并可访问完整的 Spring Boot 生态系统。
因此，与标准的 Azure 函数相比，这可以为你带来两大优势：

- 它不依赖于 Azure Functions API，因此可以轻松地移植到其他系统。 例如，它可以在常规 Spring Boot 应用程序中重复使用。
- 它可以使用 Spring Boot 的所有 `@Enable` 注释轻松添加强大的新功能。

在 *src/main/java/com/example* 文件夹中创建以下文件，该文件是一个常规 Spring Boot 应用程序：

*HelloFunction.java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.util.function.Function;

@SpringBootApplication
public class HelloFunction {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(HelloFunction.class, args);
    }

    @Bean
    public Function<User, Greeting> hello() {
        return user -> new Greeting("Welcome, " + user.getName());
    }
}
```

> [!NOTE] 
> `hello()` 函数很特别：
> 
> - 它返回 `java.util.function.Function`，后者是将要在本快速入门中使用的函数。 它包含业务逻辑，可以用作标准 Java API，将一个对象转换成另一个对象。
> - 它有 `@Bean` 注释，因此是一个 Spring Bean，默认情况下其名称是方法的名称 `hello`。 若要在应用程序中创建其他函数，则必须牢记这一点，因为该名称必须与我们在下一部分创建的 Azure Functions 名称匹配。

## <a name="create-the-azure-function"></a>创建 Azure 函数

为了利用完整的 Azure Functions API，我们现在将编码一个特定的类，该类是一个 Azure 函数，后者会将其执行委托给我们在上一步创建的 Spring Cloud Function。

在 *src/main/java/com/example* 文件夹中，创建以下 Azure 函数：

*HelloHandler.java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.AuthorizationLevel;
import com.microsoft.azure.functions.annotation.FunctionName;
import com.microsoft.azure.functions.annotation.HttpTrigger;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import java.util.Optional;

public class HelloHandler extends AzureSpringBootRequestHandler<User, Greeting> {

    @FunctionName("hello")
    public HttpResponseMessage execute(
            @HttpTrigger(name = "request", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<User>> request,
            ExecutionContext context) {

        context.getLogger().info("Greeting user name: " + request.getBody().get().getName());
        return request
                .createResponseBuilder(HttpStatus.OK)
                .body(handleRequest(request.getBody().get(), context))
                .header("Content-Type", "application/json")
                .build();
    }
}
```

此 Java 类是一个 Azure 函数，具有下述有趣的功能：

- 它扩展了 `AzureSpringBootRequestHandler`，后者充当 Azure Functions 和 Spring Cloud Function 之间的链接。 这是提供 `handleRequest()` 方法的东西，该方法用在其 `execute()` 方法中。
- 根据 `@FunctionName("hello")` 注释的定义，此函数的名称与我们在上一步配置的 Spring Bean 的名称 `hello` 相同。
- 它是真实的 Azure 函数，因此你可以在这里使用完整的 Azure Functions API。

## <a name="add-unit-tests"></a>添加单元测试

当然，此步骤为可选步骤，但你作为开发人员，应该养成良好的开发习惯，因此应添加单元测试来验证应用程序是否正常运行。

创建 *src/test/java/com/example* 文件夹，添加以下 JUnit 测试：

*HelloFunctionTest.java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.junit.Test;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import static org.assertj.core.api.Assertions.assertThat;

public class HelloFunctionTest {

    @Test
    public void test() {
        Greeting result = new HelloFunction().hello().apply(new User("foo"));
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }

    @Test
    public void start() throws Exception {
        AzureSpringBootRequestHandler<User, Greeting> handler = new AzureSpringBootRequestHandler<>(
                HelloFunction.class);
        Greeting result = handler.handleRequest(new User("foo"), null);
        handler.close();
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }
}
```

现在可以使用 Maven 测试 Azure 函数：

```bash
mvn clean test
```

## <a name="run-the-function-locally"></a>在本地运行函数

在将应用程序部署到 Azure 函数之前，让我们先在本地测试它。

首先需将应用程序打包到 Jar 文件中：

```bash
mvn package
```

将应用程序打包后，即可使用 `azure-functions` Maven 插件来运行它：

```bash
mvn azure-functions:run
```

现在应该可以通过端口 7071 在 localhost 上使用 Azure 函数。 可以对此函数进行测试，方法是使用 JSON 格式的 `User` 对象向其发送 POST 请求。 例如，使用 cURL：

```bash
curl http://localhost:7071/api/hello -d "{\"name\":\"Azure\"}"
```

此函数会使用仍为 JSON 格式的 `Greeting` 对象进行回应：

```Output
{
  "message": "Welcome, Azure"
}
```

下面是一个屏幕截图，cURL 请求位于屏幕顶部，本地 Azure 函数位于底部：

 ![在本地运行的 Azure 函数][RFL01]

## <a name="deploy-the-function-to-azure-functions"></a>将函数部署到 Azure Functions

现在可以将 Azure 函数部署到生产中了。 请记住，已在 *pom.xml* 中定义的 `<functionAppName>`、`<functionAppRegion>` 和 `<functionResourceGroup>` 属性将用于配置函数。

> [!NOTE]
> Maven 插件需要在 Azure 中进行身份验证，如果安装了 Azure CLI，请使用 `az login`，然后继续。
> 查看[此处](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication)以了解更多身份验证选项。

通过运行 Maven 来自动部署函数：

```bash
mvn azure-functions:deploy
```

现在请转到 [Azure 门户](https://portal.azure.com)，找到已创建的 `Function App`。

单击该函数：

- 在函数概览中，记下函数的 URL。
- 选择“平台功能”选项卡，找到“日志流式处理”服务，然后选择该服务来检查正在运行的函数。  

现在使用 cURL 访问正在运行的函数，正如在上一部分所做的那样。 请将 `your-function-name` 替换为实际的函数名称：

```bash
curl https://your-function-name.azurewebsites.net/api/hello -d "{\"name\":\"Azure\"}"
```

与上一部分一样，此函数会使用仍为 JSON 格式的 `Greeting` 对象进行回应：

```Output
{
  "message": "Welcome, Azure"
}
```

恭喜，你有了一个在 Azure Functions 上运行的 Spring Cloud Function！

<!-- IMG List -->

[RFL01]: media/getting-started-with-spring-cloud-function-in-azure/RFL01.png
