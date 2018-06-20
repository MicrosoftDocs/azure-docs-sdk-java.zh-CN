---
title: 如何使用适用于 Azure Active Directory 的 Spring Boot 起动器
description: 了解如何使用 Azure Active Directory 起动器配置 Spring Boot Initializer 应用。
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: cf1cad0b87626058f7204a6565d09fb8901b7ce4
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954678"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a>如何使用适用于 Azure Active Directory 的 Spring Boot 起动器

## <a name="overview"></a>概述

本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。
* [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 创建自定义应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定要使用 Java 生成的 Maven 项目，输入应用程序的“组”名称和“Aritifact”名称，然后单击链接切换到 Spring Initializr 完整版。

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

1. 输入“组织名称”和“初始域名”，单击“创建”。

   ![指定 Azure Active Directory 名称][directory-02]

1. 从 Azure 门户顶部的下拉菜单中选择新的 Azure Active Directory。

   ![选择 Azure Active Directory][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>添加 Spring Boot 应用的应用程序注册

1. 从门户菜单中选择“Azure Active Directory”，依次单击“概述”、“应用注册”。

   ![添加新的应用注册][directory-04]

1. 单击“新建应用程序注册”，指定应用程序的“名称”，使用 http://localhost:8080 作为“登录 URL”，并单击“创建”。

   ![新建应用注册][directory-05]

1. 创建应用程序注册后，请单击它。

   ![选择应用注册][directory-06]

1. 显示应用注册的页面后，请复制“应用程序 ID”供稍后使用，并单击“密钥”。

   ![创建应用注册密钥][directory-07]

1. 添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，需要复制该密钥值供稍后使用。 （以后无法检索此值。）

   ![指定应用注册密钥参数][directory-08]

1. 在应用注册的主页上，单击“所需的权限”。

   ![应用注册 - 所需的权限][directory-09]

1. 单击“Windows Azure Active Directory”。

   ![选择“Windows Azure Active Directory”][directory-10]

1. 选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。

   ![启用访问权限][directory-11]

1. 在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。

   ![授予访问权限][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a>配置并编译 Spring Boot 应用程序

1. 将下载的项目存档中的文件提取到某个目录。

1. 导航到项目中的父文件夹，并在文本编辑器中打开 *pom.xml* 文件。

1. 添加 Spring OAuth2 安全性的依赖项，例如：

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. 保存并关闭 *pom.xml* 文件。

1. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。

1. 使用前面复制的值添加存储帐户的密钥，例如：

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `azure.activedirectory.clientId` | 包含前面复制的“应用程序 ID”。 |
   | `azure.activedirectory.clientSecret` | 包含前面完成的应用注册中的密钥值。 |
   | `azure.activedirectory.activeDirectoryGroups` | 包含用于身份验证的 Active Directory 组列表。 |


1. 保存并关闭 *application.properties* 文件。

1. 在应用程序的 Java 源文件夹中创建名为 *controller* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/controller*。

1. 在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。

1. 输入以下代码，然后保存并关闭该文件：

   ```java
   package com.wingtiptoys.security;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.web.authentication.preauth.PreAuthenticatedAuthenticationToken;
   
   @RestController
   public class HelloController {
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String hello() {
         return "Hello World!";
      }
   }
   ```

1. 在应用程序的 Java 源文件夹中创建名为 *security* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/security*。

1. 在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。

1. 输入以下代码，然后保存并关闭该文件：

   ```java
   package com.wingtiptoys.security;

   import com.microsoft.azure.spring.autoconfigure.aad.AADAuthenticationFilter;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.autoconfigure.security.oauth2.client.EnableOAuth2Sso;
   import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
   import org.springframework.security.web.csrf.CookieCsrfTokenRepository;
   
   @EnableOAuth2Sso
   @EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
   
   public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
      @Autowired
      private AADAuthenticationFilter aadAuthFilter;
      @Override
      protected void configure(HttpSecurity http) throws Exception {
         http.authorizeRequests().anyRequest().permitAll();
         http.addFilterBefore(aadAuthFilter, UsernamePasswordAuthenticationFilter.class);
      }
   }
   ```

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   ```

   ![生成应用程序][build-application]

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 通过 Maven 生成并启动应用程序后，请在 Web 浏览器中打开 <http://localhost:8080>。

## <a name="next-steps"></a>后续步骤

有关使用 Azure Active Directory 的详细信息，请参阅以下文章：

* [Azure Active Directory 文档]。

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，在 <https://github.com/spring-guides/> 网站中提供了几个 Spring Boot 包。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Azure Active Directory 文档]: /azure/active-directory/
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

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

[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
