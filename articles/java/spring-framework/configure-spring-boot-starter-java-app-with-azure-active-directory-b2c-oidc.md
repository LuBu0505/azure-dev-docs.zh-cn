---
title: 使用适用于 Azure Active Directory B2C 的 Spring Boot Starter
description: 了解如何使用 Azure Active Directory B2C 起动器配置 Spring Boot Initializer 应用。
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
ms.author: edburns
ms.date: 06/04/2020
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.custom: devx-track-java
ms.openlocfilehash: b682c3b17e13f8a4491119f995fe2d21cdeda974
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379331"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>教程：使用适用于 Azure Active Directory B2C 的 Spring Boot 起动器保护 Java Web 应用。

## <a name="overview"></a>概述

本文演示了如何使用 [Spring Initializr](https://start.spring.io/) 创建一个 Java 应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 使用 Spring Initializr 创建 Java 应用程序
> * 配置 Azure Active Directory B2C
> * 使用 Spring Boot 类和批注保护应用程序
> * 生成和测试 Java 试应用程序

[Azure Active Directory](https://azure.microsoft.com/services/active-directory) 是 Microsoft 的云规模企业标识解决方案。 [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory/external-identities/b2c/) 是 Azure Active Directory 功能集的补充，借助它可以管理客户、消费者和公民对你的企业对消费者 (B2C) 应用程序的访问。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果还没有此订阅，请在开始之前创建[一个免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 创建应用

1. 浏览到 <https://start.spring.io/>。

2. 根据本指南填写相应值。 请注意，标签和布局可能与此处显示的图像不同。

    * 在“项目”下，选择“Maven 项目”。
    * 在“语言”下，选择“Java”。
    * 在“Spring Boot”下，选择“2.2.7”。
    * 在“组”、“项目”和“名称”下，使用简短的描述性字符串，输入相同的值。 在键入时，UI 可能会自动填充其中一部分内容。
    * 在“依赖项”窗格中，选择“添加依赖项”。 使用 UI 添加“Spring Web”和“Spring 安全性”的依赖关系。

   ![填写值以生成项目](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/si-n.png)

3. 选择“生成项目”，然后将项目下载到本地计算机上的一个路径下。 将下载的文件移动到以项目命名的目录中，并将文件解压缩。 文件布局应如下所示，其中 `yourProject` 的值已替换为你为“组”输入的值。

    ```
    .
    ├── HELP.md
    ├── mvnw
    ├── mvnw.cmd
    ├── pom.xml
    └── src
        ├── main
        │   ├── java
        │   │   └── yourProject
        │   │       └── yourProject
        │   │           └── YourProjectApplication.java
        │   └── resources
        │       ├── application.properties
        │       ├── static
        │       └── templates
        └── test
            └── java
                └── yourProject
                    └── yourProject
                        └── YourProjectApplicationTests.java
    ```

## <a name="create-and-initialize-an-azure-active-directory-instance"></a>创建并初始化 Azure Active Directory 实例

### <a name="create-the-active-directory-instance"></a>创建 Active Directory 实例

1. 登录到 <https://portal.azure.com>。

2. 依次选择“+创建资源”、“标识”和“全部查看”  。 搜索“Azure Active Directory B2C”。

    ![创建新的 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-1-n.png)

3. 选择“创建”。

    ![获取 B2C 租户名称](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-5-n.png)

4. 选择“创建新的 Azure AD B2C 租户”。

    ![创建新的 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-2-n.png)

5. 为“组织名称”和“初始域名”提供适当的值，然后选择“创建”。

    ![选择 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-3-n.png)

6. Active Directory 创建完成后，导航到新目录。 或搜索 `b2c` 并选择“Azure AD B2C”。

    ![找到 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-4-n.ng.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>添加 Spring Boot 应用的应用程序注册

1. 在左侧的“管理”窗格中，选择“应用程序”，然后选择“添加”。

    ![添加新的应用注册](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c1-n.png)

2. 在“名称”字段中，输入上面的“组”的值，然后将“包括 web 应用/web API”控件设置为“是”。

3. 将“回复 URL”设置为 `http://localhost:8080/home`。

4. 将其他字段保留为其默认值。

5. 选择“创建”。 可能需要等一会儿应用程序才会显示。

    ![添加应用程序重定向 URI](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c2-n.png)

6. 选择“概述”，然后选择“应用程序”。

7. 在应用程序表中，选择有你项目名称的那一行。

8. 在“常规”窗格中选择“密钥”，然后选择“生成密钥”。

9. 将“应用密钥”设置为 `yourGroupIdkey`，将 `yourGroupId` 替换为你在上面为“组”输入的值。

10. 选择“保存”。 等待“应用密钥”部分中显示出密钥，然后复制它以供稍后使用。

    > [!NOTE]
    > 如果离开“密钥”部分再返回，密钥值会消失。 在这种情况下，必须再创建一个密钥并复制它以供稍后使用。
    > 有时，生成的密钥可能会包含无法在 application.yml 文件中包含的字符，如反斜杠或反撇号。 在这种情况下，请丢弃该密钥，并重新生成一个密钥。

    ![创建机密](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c3-n.png)

11. 选择“概述”。

12. 在左侧窗格的“策略”部分中，选择“用户流”，然后选择“新用户流”。

13. 现在需要离开本教程，去执行另一个教程，并在完成后返回本教程。 下面是转到其他教程时需要记住的一些事项。

    * 从要求你选择“新用户流”的步骤开始。
    * 当此教程提到 `webapp1` 时，请改为使用你为“组”输入的值。
    * 当选择要从流中返回的声明时，请确保选中“显示名称”。 如果没有此声明，本教程中生成的应用将无法正常运行。
    * 当系统要求运行用户流时，前面指定的重定向 url 尚未激活。 仍可以运行这些流，但重定向不会成功。 这是正常情况。
    * 当到达“后续步骤”时，请返回本教程。

    执行[教程：在 Azure Active Directory B2C 中创建用户流](/azure/active-directory-b2c/tutorial-create-user-flows)中列出的所有步骤，创建用于“注册和登录”、“配置文件编辑”和“密码重置”的用户流。

    AAD B2C 支持本地帐户以及社交标识提供者。 有关创建 GitHub 标识提供者的示例，请参阅[使用 Azure Active Directory B2C 设置通过 GitHub 帐户注册与登录](/azure/active-directory-b2c/identity-provider-github)。

## <a name="configure-and-compile-your-app"></a>配置并编译你的应用

现已创建 AAD B2C 实例和一些用户流，接下来将 Spring 应用连接到 AAD B2C 实例。

1. 从命令行中，通过 cd 转到从“Spring 启动器”下载的 .zip 文件解压缩出来的目录。

2. 导航到项目的父文件夹，并在文本编辑器中打开 pom.xml Maven 项目文件。

3. 将 Spring OAuth2 安全性的依赖项添加到 pom.xml：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        <version>See Below</version>
    </dependency>
    ```

    对于 `azure-active-directory-b2c-spring-boot-starter`，使用可用的最新版本。 你也许可以使用 [mvnrepository.com](https://mvnrepository.com/ artifact/com.microsoft.azure/azure-active-directory-spring-boot-starter) 来查找最新版本。 截至本文撰写时，最新版本是 `2.2.4`。

    对于 `spring-boot-starter-thymeleaf`，使用与前面选择的Spring Boot 版本相对应的版本，例如 `2.2.7.RELASE`。

    对于 `thymeleaf-extras-springsecurity5`，使用可用的最新版本。 你也许可以使用 [mvnrepository.com](https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity5) 来查找最新版本。 截至本文撰写时，最新版本是 `3.0.4.RELEASE`。

4. 保存并关闭 pom.xml 文件。

    * 通过运行 `mvn -DskipTests clean install` 验证依赖项是否正确。 如果看不到 `BUILD SUCCESS`，请在继续操作之前排查和解决该问题。

5. 导航到项目中的 src/main/resources 文件夹，并在文本编辑器中创建 application.yml 文件。

6. 使用之前创建的值指定用于应用注册的设置，例如：

    ```yaml
    azure:
      activedirectory:
        b2c:
          tenant: ejb0518domain
          client-id: ejb0518
          client-secret: '<yourAppKey>'
          reply-url: http://localhost:8080/home
          logout-success-url: http://localhost:8080/home
          user-flows:
            sign-up-or-sign-in: B2C_1_signupsignin1
            profile-edit: B2C_1_profileediting1
            password-reset: B2C_1_passwordreset1
    ```

    请注意，`client-secret` 值要用单引号引起来。 这是必需的，因为在 YAML 中显示时，`<yourAppKey>` 的值几乎都会包含一些需要由单引号引起来的字符。

    > [!NOTE]
    > 截至本文撰写时，可在 application.yml 中使用的 Active Directory B2C Spring 集成值的完整列表如下所示：
    >
    > ```
    > azure:
    >   activedirectory:
    >     b2c:
    >       tenant:
    >       oidc-enabled:
    >       client-id:
    >       client-secret:
    >       reply-url:  # should be absolute url.
    >       logout-success-url:
    >       user-flows:
    >         sign-up-or-sign-in:
    >         profile-edit: # optional
    >         password-reset: # optional
    > ```
    >
    > Application.yml 文件可通过 GitHub 上的 [Azure Active Directory B2C Spring Boot 示例](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/application.yml)获取。

7. 保存并关闭 application.yml 文件。

8. 在 src/main/java/<yourGroupId>/<yourGroupId>中创建一个名为“controller”的文件夹，并将 `<yourGroupId>` 替换为之前为“组”输入的值。

9. 在 *controller* 文件夹中创建名为 *WebController.java* 的新 Java 文件，并在文本编辑器中打开该文件。

10. 输入以下代码，适当更改 `yourGroupId`，然后保存并关闭该文件：

    ```java
    package yourGroupId.yourGroupId.controller;

    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class WebController {

        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();

                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }

        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }

        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "greeting";
        }

        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }
    }
    ```

    由于控制器中的每个方法都调用 `initializeModel()`，且该方法调用 `model.addAllAttributes(user.getAttributes());`，所以 src/main/resources/templates 中的任何 HTML 页面都能访问这些属性中的任何一个，例如 `${name}`、`${grant_type}` 或 `${auth_time}`。 从 `user.getAttributes()` 返回的值实际上是用于身份验证的 `id_token` 的声明。 [Microsoft 标识平台 ID 令牌](/azure/active-directory/develop/id-tokens#payload-claims)中列出了可用声明的完整列表。

11. 在 src/main/java/<yourGroupId>/<yourGroupId>中创建名为“security”的文件夹，并将 `yourGroupId` 替换为之前为“组”输入的值。

12. 在 *security* 文件夹中创建名为 *WebSecurityConfiguration.java* 的新 Java 文件，并在文本编辑器中打开该文件。

13. 输入以下代码，适当更改 `yourGroupId`，然后保存并关闭该文件：

    ```java
    package yourGroupId.yourGroupId.security;

    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

        private final AADB2COidcLoginConfigurer configurer;

        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```

14. 将 greeting.html 和 home.html 文件从 [Azure AD B2C Spring Boot 示例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/templates)复制到 src/main/resources/templates 中，并将 `${your-profile-edit-user-flow}` 和 `${your-password-reset-user-flow}` 替换为之前创建的用户流的名称。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。

2. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

    > [!NOTE]
    > 本地 spring boot 应用运行时所参照的系统时钟所反映的时间必须准确，这一点非常重要。 使用 OAuth 2.0 时，可允许非常小的时钟偏差。 即使是 3 分钟的偏差都可能导致发生类似于 `[invalid_id_token] An error occurred while attempting to decode the Jwt: Jwt used before 2020-05-19T18:52:10Z` 的错误，从而导致登录失败。 截至本文撰写时，[time.gov](https://time.gov/) 会提供一个指示器，指示时钟与实际时间之间的偏差。 应用已成功运行，时间偏差为 +0.019 秒。

    ```shell
    mvn -DskipTests clean package
    mvn -DskipTests spring-boot:run
    ```

3. 在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 `http://localhost:8080/`；系统会将你重定向到登录页。

    ![登录页](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo1-n.png)

4. 选择与登录相关的文本的链接。 系统应会重定向到 Azure AD B2C 以启动身份验证过程。

5. 成功登录后，应该会在浏览器中看到示例 `home page`。

    ![成功登录](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo3-n.png)

## <a name="summary"></a>总结

在本教程中，我们使用 Azure Active Directory B2C 起动器创建了新的 Java Web 应用程序，配置了新的 Azure AD B2C 租户并在其中注册了新的应用程序，然后将应用程序配置为使用 Spring 批注和类来保护 Web 应用。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/developer/java/spring-framework)
