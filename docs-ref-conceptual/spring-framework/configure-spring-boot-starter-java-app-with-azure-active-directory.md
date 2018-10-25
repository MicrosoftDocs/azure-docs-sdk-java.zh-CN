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
ms.date: 07/02/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 665768ffe7bec977d553ffa62e1dbd6b968eb9de
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799903"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>如何使用适用于 Azure Active Directory 的 Spring Boot 起动器

## <a name="overview"></a>概述

本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 创建自定义应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。

   ![指定组和项目名称][security-01]

1. 向下滚动到“核心”部分并选中“安全”对应的框，然后在“Web”部分选中“Web”对应的框。

   ![选择“安全”和“Web”起动器][security-02]

1. 向下滚动到“Azure”部分，并选中“Azure Active Directory”对应的框。

   ![选择 Azure Active Directory 起动器][security-03]

1. 滚动到页面底部，单击“生成项目”对应的按钮。

   ![生成 Spring Boot 项目][security-04]

1. 出现提示时，将项目下载到本地计算机中的路径。

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a>创建并配置新的 Azure Active Directory 实例

### <a name="create-the-active-directory-instance"></a>创建 Active Directory 实例

1. 登录到 <https://portal.azure.com>。

1. 依次单击“+新建”、“安全 + 标识”、“Azure Active Directory”。

   ![创建新的 Azure Active Directory 实例][directory-01]

1. 输入“组织名称”和“初始域名”。 复制你的目录的完整 URL；在本教程中，稍后你将使用它来添加用户帐户。 （例如：`wingtiptoysdirectory.onmicrosoft.com`。）完成后，单击“创建”。

   ![指定 Azure Active Directory 名称][directory-02]

1. 从 Azure 门户顶部的下拉菜单中选择新的 Azure Active Directory。

   ![选择 Azure Active Directory][directory-03]

1. 从门户菜单中选择“Azure Active Directory”，单击“属性”，并复制“目录 ID”；在本教程中，稍后将使用该值来配置 *application.properties* 文件。

   ![复制 Azure Active Directory ID][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>添加 Spring Boot 应用的应用程序注册

1. 从门户菜单中选择“Azure Active Directory”，依次单击“概述”、“应用注册”。

   ![添加新的应用注册][directory-04]

2. 单击“新建应用程序注册”，指定应用程序的“名称”，使用 http://localhost:8080 作为“登录 URL”，并单击“创建”。

   ![新建应用注册][directory-05]

3. 创建应用程序注册后，请单击它。

   ![选择应用注册][directory-06]

4. 当应用注册页出现时，复制你的**应用 ID**；在本教程中，稍后你将使用此值来配置 *application.properties* 文件。 单击“设置”，然后单击“密钥”。

   ![创建应用注册密钥][directory-07]

5. 添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，你需要复制该密钥的值，本教程中稍后将使用该值来配置 *application.properties* 文件。 （以后无法检索此值。）

   ![指定应用注册密钥参数][directory-08]

6. 在应用注册的主页上，依次单击“设置”、“所需的权限”。

   ![应用注册 - 所需的权限][directory-09]

7. 单击“Windows Azure Active Directory”。

   ![选择“Windows Azure Active Directory”][directory-10]

8. 选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。

   ![启用访问权限][directory-11]

9. 在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。

   ![授予访问权限][directory-12]

10. 在应用注册的主页上，依次单击“设置”、“回复 URL”。

    ![编辑回复 URL][directory-14]

11. 输入“<http://localhost:8080/login/oauth2/code/azure>”作为新的回复 URL，并单击“保存”。

    ![添加新的回复 URL][directory-15]

12. 从应用注册主页面上，单击“清单”，将 `oauth2AllowImplicitFlow` 参数的值设置为 `true`，然后单击“保存”。

    ![配置应用清单][directory-16]

    > [!NOTE]
    > 
    > 有关 `oauth2AllowImplicitFlow` 参数和其他应用程序设置的详细信息，请参阅 [Azure Active Directory 应用程序清单][AAD app manifest]。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>将用户帐户添加到你的目录，并将该帐户添加到某个组

1. 从 Active Directory 的“概述”页面上，单击“用户”。

   ![打开“用户”面板][directory-17]

1. 当“用户”面板显示时，单击“新建用户”。

   ![添加新用户帐户][directory-18]

1. 当“用户”面板显示时，输入**名称**和**用户名**。

   ![输入用户帐户信息][directory-19]

   > [!NOTE]
   > 
   > 在输入用户名时，需要指定本教程前文中的目录 URL，例如：
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. 单击“组”，选择要用于在应用程序中进行身份验证的组，然后单击“选择”。 （针对在本教程中的用途，请将帐户添加到 _Users_ 组。）

   ![选择用户的组][directory-20]

1. 单击“显示密码”，然后复制密码；在本教程中，登录到应用程序时将使用此密码。

   ![显示密码][directory-21]

1. 单击“创建”将新的用户帐户添加到目录。

   ![创建新的用户帐户][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a>配置并编译 Spring Boot 应用程序

1. 将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。

1. 导航到项目的父文件夹，并在文本编辑器中打开 *pom.xml* 文件。

1. 添加 Spring OAuth2 安全性的依赖项，例如：

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

   | 参数 | Description |
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

   > [!NOTE]
   > 
   > 可为不同的请求映射指定不同的授权设置，例如：
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

## <a name="next-steps"></a>后续步骤

有关使用 Azure Active Directory 的详细信息，请参阅以下文章：

* [Azure Active Directory 文档]。

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

有关更详细示例，请参阅 GitHub 上的 [Azure Active Directory Spring Boot 示例][AAD Spring Boot Sample]。

<!-- URL List -->

[Azure Active Directory 文档]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[面向 Java 开发人员的 Azure]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[security-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-01.png
[security-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-02.png
[security-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-03.png
[security-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/security-04.png

[directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-01.png
[directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-02.png
[directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-03.png
[directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-04.png
[directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-05.png
[directory-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-06.png
[directory-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-07.png
[directory-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-08.png
[directory-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-09.png
[directory-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-10.png
[directory-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-11.png
[directory-12]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-12.png
[directory-13]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-13.png
[directory-14]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-14.png
[directory-15]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-15.png
[directory-16]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-16.png
[directory-17]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-17.png
[directory-18]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-18.png
[directory-19]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-19.png
[directory-20]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-20.png
[directory-21]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-21.png
[directory-22]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/directory-22.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
