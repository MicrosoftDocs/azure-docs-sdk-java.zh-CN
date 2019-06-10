---
title: 如何使用适用于 Azure Active Directory B2C 的 Spring Boot 起动器
description: 了解如何使用 Azure Active Directory B2C 起动器配置 Spring Boot Initializer 应用。
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
editor: panli
ms.assetid: ''
ms.author: panli
ms.date: 02/28/2019
ms.devlang: java
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 21aa6c812a519d686356851a57f00688d9a24fa4
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625711"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>教程：使用适用于 Azure Active Directory B2C 的 Spring Boot 起动器保护 Java Web 应用。

## <a name="overview"></a>概述

本文演示了如何使用 [Spring Initializr](https://start.spring.io/) 创建一个 Java 应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 使用 Spring Initializr 创建 Java 应用程序
> * 配置 Azure Active Directory B2C
> * 使用 Spring Boot 类和批注保护应用程序
> * 生成和测试 Java 试应用程序

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-app-using-spring-initializr"></a>使用 Spring Initialzr 创建应用

1. 浏览到 <https://start.spring.io/>。

2. 指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后选择 Spring Initializr 的“Web”和“安全性”模块。    

   ![指定组和项目名称](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. 出现提示时，单击“`Generate Project`”，将项目下载到本地计算机中的路径。

## <a name="create-azure-active-directory-instance"></a>创建 Azure Active Directory 实例

### <a name="create-the-active-directory-instance"></a>创建 Active Directory 实例

1. 登录到 <https://portal.azure.com>。

2. 依次单击“+创建资源”、“标识”、“Azure Active Directory B2C”。   

   ![创建新的 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. 输入“组织名称”和“初始域名”，将**域名**作为 `${your-tenant-name}` 记录，然后单击“创建”。   

   ![获取 B2C 租户名称](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. 在 Azure 门户工具栏的右上角选择你的帐户名称，然后单击“切换目录”。 

   ![选择 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. 从下拉菜单中选择你的新 Azure Active Directory。

   ![选择 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. 搜索 `b2c` 并单击 `Azure AD B2C` 服务。

   ![找到 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>添加 Spring Boot 应用的应用程序注册

1. 从门户菜单中选择“Azure AD B2C”，单击“应用程序”，然后单击“添加”。   

   ![添加新的应用注册](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. 指定应用程序的“名称”，添加 `http://localhost:8080/home` 作为“回复 URL”，将“应用程序 ID”作为 `${your-client-id}` 记录，然后单击“保存”。    

   ![添加应用程序回复 URL](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. 从应用程序中选择“密钥”，单击“生成密钥”以生成 `${your-client-secret}`，然后单击“保存”。   

4. 在左侧选择“用户流”，然后单击“新建用户流”。   ****

   ![创建用户流](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. 选择“注册或登录”、“配置文件编辑”和“密码重置”，分别创建用户流。    指定用户流的“名称”和“用户特性和声明”，然后单击“创建”。   

   ![配置用户流](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a>配置并编译你的应用

1. 将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。

2. 导航到项目的父文件夹，并在文本编辑器中打开 `pom.xml` Maven 项目文件。

3. 将 Spring OAuth2 安全性的依赖项添加到 `pom.xml`：

   ```xml
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
       <version>2.1.6.M2</version>
   </dependency>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   </dependency>
   ```

4. 保存并关闭 pom.xml 文件  。

5. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.yml* 文件。

6. 使用之前创建的值指定用于应用注册的设置，例如：

   ```yaml
   azure:
     activedirectory:
       b2c:
         tenant: ${your-tenant-name}
         client-id: ${your-client-id}
         client-secret: ${your-client-secret}
         reply-url: ${your-reply-url-from-aad} # should be absolute url.
         logout-success-url: ${you-logout-success-url}
         user-flows:
           sign-up-or-sign-in: ${your-sign-up-or-in-user-flow}
           profile-edit: ${your-profile-edit-user-flow}     # optional
           password-reset: ${your-password-reset-user-flow} # optional
   ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | 包含前面复制的 AD B2C 的 `${your-tenant-name`。 |
   | `azure.activedirectory.b2c.client-id` | 包含前面填写的应用程序中的 `${your-client-id}`。 |
   | `azure.activedirectory.b2c.client-secret` | 包含前面填写的应用程序中的 `${your-client-secret}`。 |
   | `azure.activedirectory.b2c.reply-url` | 包含前面填写的应用程序中的某个“回复 URL”  。 |
   | `azure.activedirectory.b2c.logout-success-url` | 指定应用程序成功注销时的 URL。 |
   | `azure.activedirectory.b2c.user-flows` | 包含前面填写的用户流的名称。

   > [!NOTE]
   > 
   > 如需 *application.yml* 文件中提供的值的完整列表，请参阅 GitHub 上的 [Azure Active Directory B2C Spring Boot 示例][AAD B2C Spring Boot 示例]。
   >

7. 保存并关闭 application.yml  文件。

8. 在应用程序的 Java 源文件夹中创建名为“controller”的文件夹。 

9. 在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。

10. 输入以下代码，然后保存并关闭该文件：

    ```java
    package sample.aad.controller;
    
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

11. 在应用程序的 Java 源文件夹中创建名为“security”的文件夹。 

12. 在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。

13. 输入以下代码，然后保存并关闭该文件：

    ```java
    package sample.aad.security;
    
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
14. 从 [Azure AD B2C Spring Boot 示例](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)复制 `greeting.html` 和 `home.html`，并将 `${your-profile-edit-user-flow}` 和 `${your-password-reset-user-flow}` 分别替换为此前填写的用户流名称。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。

2. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. 在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 <http://localhost:8080/>；系统会将你重定向到登录页。

   ![登录页](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. 单击名称为 `${your-sign-up-or-in}` 用户流的链接，系统会将你重定向到 Azure AD B2C 以启动身份验证过程。

   ![Azure AD B2C 登录](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. 成功登录后，应该会在浏览器中看到示例 `home page`。

   ![成功登录](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a>摘要

在本教程中，我们使用 Azure Active Directory B2C 起动器创建了新的 Java Web 应用程序，配置了新的 Azure AD B2C 租户并在其中注册了新的应用程序，然后将应用程序配置为使用 Spring 批注和类来保护 Web 应用。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)
