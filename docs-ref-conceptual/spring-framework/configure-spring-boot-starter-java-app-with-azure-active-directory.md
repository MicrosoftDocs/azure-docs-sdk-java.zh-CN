---
title: "如何使用适用于 Azure Active Directory 的 Spring Boot 起动器"
description: "了解如何使用 Azure Active Directory 起动器配置 Spring Boot Initializer 应用。"
services: active-directory
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm
ms.openlocfilehash: a999e33674ea01e776db10186e8af83ce157ef20
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="f4513-103">如何使用适用于 Azure Active Directory 的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="f4513-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="f4513-104">概述</span><span class="sxs-lookup"><span data-stu-id="f4513-104">Overview</span></span>

<span data-ttu-id="f4513-105">本文演示如何使用 **[Spring Initializr]**（适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器）创建应用。</span><span class="sxs-lookup"><span data-stu-id="f4513-105">This article demonstrates creating an app with the **[Spring Initializr]** which the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4513-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="f4513-106">Prerequisites</span></span>

<span data-ttu-id="f4513-107">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="f4513-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="f4513-108">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="f4513-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="f4513-109">[Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="f4513-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="f4513-110">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="f4513-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="f4513-111">使用 Spring Initializr 创建自定义应用程序</span><span class="sxs-lookup"><span data-stu-id="f4513-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="f4513-112">浏览到 https://start.spring.io/<>。</span><span class="sxs-lookup"><span data-stu-id="f4513-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="f4513-113">指定要使用 Java 生成的 Maven 项目，输入应用程序的“组”名称和“Aritifact”名称，然后单击链接切换到 Spring Initializr 完整版。</span><span class="sxs-lookup"><span data-stu-id="f4513-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定组和项目名称][security-01]

1. <span data-ttu-id="f4513-115">向下滚动到“核心”部分并选中“安全”对应的框，然后在“Web”部分选中“Web”对应的框。</span><span class="sxs-lookup"><span data-stu-id="f4513-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![选择“安全”和“Web”起动器][security-02]

1. <span data-ttu-id="f4513-117">向下滚动到“Azure”部分，并选中“Azure Active Directory”对应的框。</span><span class="sxs-lookup"><span data-stu-id="f4513-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![选择 Azure Active Directory 起动器][security-03]

1. <span data-ttu-id="f4513-119">滚动到页面底部，单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="f4513-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![生成 Spring Boot 项目][security-04]

1. <span data-ttu-id="f4513-121">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="f4513-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="f4513-122">创建并配置新的 Azure Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="f4513-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="f4513-123">创建 Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="f4513-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="f4513-124">登录到 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="f4513-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="f4513-125">依次单击“+新建”、“安全 + 标识”、“Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="f4513-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![创建新的 Azure Active Directory 实例][directory-01]

1. <span data-ttu-id="f4513-127">输入“组织名称”和“初始域名”，单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="f4513-127">Enter your **Organization name** and your **Initial domain name**, and then click **Create**.</span></span>

   ![指定 Azure Active Directory 名称][directory-02]

1. <span data-ttu-id="f4513-129">从 Azure 门户顶部的下拉菜单中选择新的 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="f4513-129">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![选择 Azure Active Directory][directory-03]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="f4513-131">添加 Spring Boot 应用的应用程序注册</span><span class="sxs-lookup"><span data-stu-id="f4513-131">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="f4513-132">从门户菜单中选择“Azure Active Directory”，依次单击“概述”、“应用注册”。</span><span class="sxs-lookup"><span data-stu-id="f4513-132">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![添加新的应用注册][directory-04]

1. <span data-ttu-id="f4513-134">单击“新建应用程序注册”，指定应用程序的“名称”，使用 http://localhost:8080 作为“登录 URL”，并单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="f4513-134">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新建应用注册][directory-05]

1. <span data-ttu-id="f4513-136">创建应用程序注册后，请单击它。</span><span class="sxs-lookup"><span data-stu-id="f4513-136">Click your application registration after it has been created.</span></span>

   ![选择应用注册][directory-06]

1. <span data-ttu-id="f4513-138">显示应用注册的页面后，请复制“应用程序 ID”供稍后使用，并单击“密钥”。</span><span class="sxs-lookup"><span data-stu-id="f4513-138">When the page for your app registration, copy your **Application ID** for later, then click **Keys**.</span></span>

   ![创建应用注册密钥][directory-07]

1. <span data-ttu-id="f4513-140">添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，需要复制该密钥值供稍后使用。</span><span class="sxs-lookup"><span data-stu-id="f4513-140">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key for later.</span></span> <span data-ttu-id="f4513-141">（以后无法检索此值。）</span><span class="sxs-lookup"><span data-stu-id="f4513-141">(You will not be able to retrieve this value later.)</span></span>

   ![指定应用注册密钥参数][directory-08]

1. <span data-ttu-id="f4513-143">在应用注册的主页上，单击“所需的权限”。</span><span class="sxs-lookup"><span data-stu-id="f4513-143">From the main page for your app registration, click **Required permissions**.</span></span>

   ![应用注册 - 所需的权限][directory-09]

1. <span data-ttu-id="f4513-145">单击“Windows Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="f4513-145">Click **Windows Azure Active Directory**.</span></span>

   ![选择“Windows Azure Active Directory”][directory-10]

1. <span data-ttu-id="f4513-147">选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="f4513-147">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![启用访问权限][directory-11]

1. <span data-ttu-id="f4513-149">在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。</span><span class="sxs-lookup"><span data-stu-id="f4513-149">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授予访问权限][directory-12]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="f4513-151">配置并编译 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="f4513-151">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="f4513-152">将下载的项目存档中的文件提取到某个目录。</span><span class="sxs-lookup"><span data-stu-id="f4513-152">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="f4513-153">导航到项目中的父文件夹，并在文本编辑器中打开 *pom.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-153">Navigate to the parent folder in your project and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="f4513-154">添加 Spring OAuth2 安全性的依赖项，例如：</span><span class="sxs-lookup"><span data-stu-id="f4513-154">Add the dependency for Spring OAuth2 security; for example:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.security.oauth</groupId>
      <artifactId>spring-security-oauth2</artifactId>
   </dependency>
   ```

1. <span data-ttu-id="f4513-155">保存并关闭 *pom.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-155">Save and close the  the *pom.xml* file.</span></span>

1. <span data-ttu-id="f4513-156">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="f4513-157">使用前面复制的值添加存储帐户的密钥，例如：</span><span class="sxs-lookup"><span data-stu-id="f4513-157">Add the key for your storage account using the values from earlier; for example:</span></span>

   ```yaml
   # Specifies your Active Directory Application ID:
   azure.activedirectory.clientId=11111111-1111-1111-1111-1111111111111111

   # Specifies your secret key:
   azure.activedirectory.clientSecret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authentication:
   azure.activedirectory.activeDirectoryGroups=Users
   ```
   <span data-ttu-id="f4513-158">其中：</span><span class="sxs-lookup"><span data-stu-id="f4513-158">Where:</span></span>
   <span data-ttu-id="f4513-159">参数</span><span class="sxs-lookup"><span data-stu-id="f4513-159">Parameter</span></span> | <span data-ttu-id="f4513-160">说明</span><span class="sxs-lookup"><span data-stu-id="f4513-160">Description</span></span>
   ---|---|---
   `azure.activedirectory.clientId` | <span data-ttu-id="f4513-161">包含前面复制的“应用程序 ID”。</span><span class="sxs-lookup"><span data-stu-id="f4513-161">Contains your **Application ID** from earlier.</span></span>
   `azure.activedirectory.clientSecret` | <span data-ttu-id="f4513-162">包含前面完成的应用注册中的密钥值。</span><span class="sxs-lookup"><span data-stu-id="f4513-162">Contains the key value from your app registration which you completed earlier.</span></span>
   `azure.activedirectory.activeDirectoryGroups` | <span data-ttu-id="f4513-163">包含用于身份验证的 Active Directory 组列表。</span><span class="sxs-lookup"><span data-stu-id="f4513-163">Contains a list of Active Directory groups to use for authentication.</span></span>


1. <span data-ttu-id="f4513-164">保存并关闭 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-164">Save and close the  the *application.properties* file.</span></span>

1. <span data-ttu-id="f4513-165">在应用程序的 Java 源文件夹中创建名为 *controller* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/controller*。</span><span class="sxs-lookup"><span data-stu-id="f4513-165">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="f4513-166">在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-166">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="f4513-167">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="f4513-167">Enter the following code, then save and close the file:</span></span>

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

1. <span data-ttu-id="f4513-168">在应用程序的 Java 源文件夹中创建名为 *security* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/security*。</span><span class="sxs-lookup"><span data-stu-id="f4513-168">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="f4513-169">在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="f4513-169">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="f4513-170">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="f4513-170">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="f4513-171">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="f4513-171">Build and test your app</span></span>

1. <span data-ttu-id="f4513-172">打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="f4513-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="f4513-173">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="f4513-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   ```

   ![][build-application]

1. <span data-ttu-id="f4513-174">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="f4513-174">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```



1. <span data-ttu-id="f4513-175">通过 Maven 生成并启动应用程序后，请在 Web 浏览器中打开 <http://localhost:8080>。</span><span class="sxs-lookup"><span data-stu-id="f4513-175">After your application is built and started by Maven, open <http://localhost:8080> in a web browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4513-176">后续步骤</span><span class="sxs-lookup"><span data-stu-id="f4513-176">Next steps</span></span>

<span data-ttu-id="f4513-177">有关使用 Azure Active Directory 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="f4513-177">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="f4513-178">[Azure Active Directory 文档]。</span><span class="sxs-lookup"><span data-stu-id="f4513-178">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="f4513-179">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="f4513-179">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="f4513-180">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="f4513-180">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="f4513-181">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="f4513-181">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="f4513-182">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="f4513-182">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="f4513-183">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="f4513-183">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f4513-184">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="f4513-184">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="f4513-185">为帮助开发人员开始使用 Spring Boot，在 <https://github.com/spring-guides/> 网站中提供了几个 Spring Boot 包。</span><span class="sxs-lookup"><span data-stu-id="f4513-185">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="f4513-186">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f4513-186">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

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
