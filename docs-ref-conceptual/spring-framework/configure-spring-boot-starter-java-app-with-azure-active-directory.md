---
title: 如何使用适用于 Azure Active Directory 的 Spring Boot 起动器
description: 了解如何使用 Azure Active Directory 起动器配置 Spring Boot Initializer 应用。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 89e294fa739b59f87667f901e914fd5f050b5b8c
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338771"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>教程：使用适用于 Azure Active Directory 的 Spring Boot 起动器保护 Java Web 应用

## <a name="overview"></a>概述

本文演示了如何使用 **[Spring Initializr]** 创建一个 Java 应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 使用 Spring Initializr 创建 Java 应用程序
> * 配置 Azure Active Directory
> * 使用 Spring Boot 类和批注保护应用程序
> * 生成和测试 Java 试应用程序

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 创建应用

1. 浏览到 <https://start.spring.io/>。

1. 指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。

   ![指定组和项目名称][create-spring-app-01]

1. 向下滚动到“核心”部分并选中“安全性”的复选框，在“Web”部分中选中“Web”的复选框，然后向下滚动到“Azure”部分并选中“Azure Active Directory”的复选框。

   ![选择“安全性”、“Web”和“Azure Active Directory”起动器][create-spring-app-02]

1. 滚动到页面顶部或底部，单击“生成项目”对应的按钮。

   ![生成 Spring Boot 项目][create-spring-app-03]

1. 出现提示时，将项目下载到本地计算机中的路径。

## <a name="create-azure-active-directory-instance"></a>创建 Azure Active Directory 实例

### <a name="create-the-active-directory-instance"></a>创建 Active Directory 实例

1. 登录到 <https://portal.azure.com>。

1. 依次单击“+创建资源”、“标识”和“Azure Active Directory”。

   ![创建新的 Azure Active Directory 实例][create-directory-01]

1. 输入“组织名称”和“初始域名”。 复制你的目录的完整 URL；在本教程中，稍后你将使用它来添加用户帐户。 （例如：`wingtiptoysdirectory.onmicrosoft.com`。）完成后，单击“创建”。

   ![指定 Azure Active Directory 名称][create-directory-02]

1. 在 Azure 门户工具栏的右上角选择你的帐户名称，然后单击“切换目录”。

   ![选择你的 Azure 帐户名称][create-directory-03]

1. 从下拉菜单中选择你的新 Azure Active Directory。

   ![选择 Azure Active Directory][create-directory-04]

1. 从门户菜单中选择“Azure Active Directory”，单击“属性”，并复制“目录 ID”；在本教程中，稍后将使用该值来配置 *application.properties* 文件。

   ![复制 Azure Active Directory ID][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>添加 Spring Boot 应用的应用程序注册

1. 从门户菜单中选择“Azure Active Directory”，依次单击“应用注册”和“新建应用注册”。

   ![添加新的应用注册][create-app-registration-01]

2. 指定应用程序的**名称**，使用 http://localhost:8080 作为**登录 URL**，然后单击“创建”。

   ![新建应用注册][create-app-registration-02]

4. 当应用注册页出现时，复制你的**应用 ID**；在本教程中，稍后你将使用此值来配置 *application.properties* 文件。 单击“设置”，然后单击“密钥”。

   ![创建应用注册密钥][create-app-registration-03]

5. 添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，你需要复制该密钥的值，本教程中稍后将使用该值来配置 *application.properties* 文件。 （以后无法检索此值。）

   ![指定应用注册密钥参数][create-app-registration-04]

6. 在应用注册的主页上，依次单击“设置”、“所需的权限”。

   ![应用注册 - 所需的权限][create-app-registration-05]

7. 单击“Windows Azure Active Directory”。

   ![选择“Windows Azure Active Directory”][create-app-registration-06]

8. 选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。

   ![启用访问权限][create-app-registration-07]

9. 在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。

   ![授予访问权限][create-app-registration-08]

10. 在应用注册的主页上，依次单击“设置”、“回复 URL”。

    ![编辑回复 URL][create-app-registration-09]

11. 输入“<http://localhost:8080/login/oauth2/code/azure>”作为新的回复 URL，并单击“保存”。

    ![添加新的回复 URL][create-app-registration-10]

12. 从应用注册主页面上，单击“清单”，将 `oauth2AllowImplicitFlow` 参数的值设置为 `true`，然后单击“保存”。

    ![配置应用清单][create-app-registration-11]

    > [!NOTE]
    > 
    > 有关 `oauth2AllowImplicitFlow` 参数和其他应用程序设置的详细信息，请参阅 [Azure Active Directory 应用程序清单][AAD app manifest]。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>将用户帐户添加到你的目录，并将该帐户添加到某个组

1. 从你的 Active Directory 的“概述”页面上，单击“所有用户”，然后单击“新建用户”。

   ![添加新用户帐户][create-user-01]

1. 当“用户”面板显示时，输入**名称**和**用户名**。

   ![输入用户帐户信息][create-user-02]

   > [!NOTE]
   > 
   > 在输入用户名时，需要指定本教程前文中的目录 URL，例如：
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. 单击“组”，选择要用于在应用程序中进行身份验证的组，然后单击“选择”。 （针对在本教程中的用途，请将帐户添加到 _Users_ 组。）

   ![选择用户的组][create-user-03]

1. 单击“显示密码”，然后复制密码；在本教程中，登录到应用程序时将使用此密码。 复制密码后，单击“创建”将新的用户帐户添加到你的目录。

   ![显示密码][create-user-04]

## <a name="configure-and-compile-your-app"></a>配置并编译你的应用

1. 将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。

1. 导航到项目的父文件夹，并在文本编辑器中打开 `pom.xml` Maven 项目文件。

1. 将 Spring OAuth2 安全性的依赖项添加到 `pom.xml`：

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

1. 保存并关闭 pom.xml 文件。

1. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。

1. 使用之前创建的值指定用于应用注册的设置，例如：

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `azure.activedirectory.tenant-id` | 包含前面复制的 Active Directory“目录 ID”。 |
   | `spring.security.oauth2.client.registration.azure.client-id` | 包含前面填写的、应用注册的“应用程序 ID”。 |
   | `spring.security.oauth2.client.registration.azure.client-secret` | 包含前面填写的、应用注册密钥中的“值”。 |
   | `azure.activedirectory.active-directory-groups` | 包含用于授权的 Active Directory 组列表。 |

   > [!NOTE]
   > 
   > 有关可在 *application.properties* 文件中使用的值的完整列表，请参阅 GitHub 上的 [Azure Active Directory Spring Boot 示例][AAD Spring Boot Sample]。
   >

1. 保存并关闭 application.properties 文件。

1. 在应用程序的 Java 源文件夹中创建名为 *controller* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/controller*。

1. 在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。

1. 输入以下代码，然后保存并关闭该文件：

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > 为 `@PreAuthorize("hasRole('')")` 方法指定的组名称必须包含 *application.properties* 文件的 `azure.activedirectory.active-directory-groups` 字段中指定的某个组。
   > 
   > 还可以为不同的请求映射指定不同的授权设置，例如：
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

1. 在应用程序的 Java 源文件夹中创建名为 *security* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/security*。

1. 在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。

1. 输入以下代码，然后保存并关闭该文件：

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![生成应用程序][build-application]

1. 在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 <http://localhost:8080>；系统应会提示输入用户名和密码。

   ![登录到应用程序][application-login]

   > [!NOTE]
   > 
   > 如果这是新用户帐户的首次登录，可能会提示你更改密码。
   > 
   > ![更改密码][update-password]
   > 

1. 成功登录后，控制器中应会显示“Hello World”示例文本。

   ![成功登录][hello-world]

   > [!NOTE]
   > 
   > 未授权的用户帐户会收到“HTTP 403 未授权”消息。
   >

## <a name="summary"></a>摘要

在本教程中，使用 Azure Active Directory 起动器创建了新的 Java Web 应用程序，配置了新的 Azure AD 租户并在其中注册了新的应用程序，并且已将应用程序配置为使用 Spring 批注和类来保护 Web 应用。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /java/azure/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
