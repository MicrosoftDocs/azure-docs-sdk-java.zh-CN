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
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a><span data-ttu-id="113ca-103">教程：使用适用于 Azure Active Directory B2C 的 Spring Boot 起动器保护 Java Web 应用。</span><span class="sxs-lookup"><span data-stu-id="113ca-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory B2C.</span></span>

## <a name="overview"></a><span data-ttu-id="113ca-104">概述</span><span class="sxs-lookup"><span data-stu-id="113ca-104">Overview</span></span>

<span data-ttu-id="113ca-105">本文演示了如何使用 [Spring Initializr](https://start.spring.io/) 创建一个 Java 应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。</span><span class="sxs-lookup"><span data-stu-id="113ca-105">This article demonstrates creating a Java app with the [Spring Initializr](https://start.spring.io/) that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="113ca-106">本教程介绍如何执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="113ca-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="113ca-107">使用 Spring Initializr 创建 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="113ca-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="113ca-108">配置 Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="113ca-108">Configure Azure Active Directory B2C</span></span>
> * <span data-ttu-id="113ca-109">使用 Spring Boot 类和批注保护应用程序</span><span class="sxs-lookup"><span data-stu-id="113ca-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="113ca-110">生成和测试 Java 试应用程序</span><span class="sxs-lookup"><span data-stu-id="113ca-110">Build and test your Java application</span></span>

<span data-ttu-id="113ca-111">如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="113ca-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="113ca-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="113ca-112">Prerequisites</span></span>

<span data-ttu-id="113ca-113">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="113ca-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="113ca-114">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="113ca-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="113ca-115">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="113ca-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="113ca-116">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="113ca-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="113ca-117">使用 Spring Initialzr 创建应用</span><span class="sxs-lookup"><span data-stu-id="113ca-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="113ca-118">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="113ca-118">Browse to <https://start.spring.io/>.</span></span>

2. <span data-ttu-id="113ca-119">指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后选择 Spring Initializr 的“Web”和“安全性”模块。    </span><span class="sxs-lookup"><span data-stu-id="113ca-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select the **Web** and **Security** module of the Spring Initializr.</span></span>

   ![指定组和项目名称](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/SI.png)


3. <span data-ttu-id="113ca-121">出现提示时，单击“`Generate Project`”，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="113ca-121">Click `Generate Project`, download the project to a path on your local computer when prompted.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="113ca-122">创建 Azure Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="113ca-122">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="113ca-123">创建 Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="113ca-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="113ca-124">登录到 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="113ca-124">Log into <https://portal.azure.com>.</span></span>

2. <span data-ttu-id="113ca-125">依次单击“+创建资源”、“标识”、“Azure Active Directory B2C”。   </span><span class="sxs-lookup"><span data-stu-id="113ca-125">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory B2C**.</span></span>

   ![创建新的 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ1.png)

3. <span data-ttu-id="113ca-127">输入“组织名称”和“初始域名”，将**域名**作为 `${your-tenant-name}` 记录，然后单击“创建”。   </span><span class="sxs-lookup"><span data-stu-id="113ca-127">Enter your **Organization name** and your **Initial domain name**, record the **domain name** as your `${your-tenant-name}` and click **Create**.</span></span>

   ![获取 B2C 租户名称](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ5.png)

4. <span data-ttu-id="113ca-129">在 Azure 门户工具栏的右上角选择你的帐户名称，然后单击“切换目录”。 </span><span class="sxs-lookup"><span data-stu-id="113ca-129">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![选择 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ2.png)

5. <span data-ttu-id="113ca-131">从下拉菜单中选择你的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="113ca-131">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![选择 Azure Active Directory](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ3.png)

6. <span data-ttu-id="113ca-133">搜索 `b2c` 并单击 `Azure AD B2C` 服务。</span><span class="sxs-lookup"><span data-stu-id="113ca-133">Search `b2c` and click `Azure AD B2C` service.</span></span>

   ![找到 Azure Active Directory B2C 实例](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/AZ4.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="113ca-135">添加 Spring Boot 应用的应用程序注册</span><span class="sxs-lookup"><span data-stu-id="113ca-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="113ca-136">从门户菜单中选择“Azure AD B2C”，单击“应用程序”，然后单击“添加”。   </span><span class="sxs-lookup"><span data-stu-id="113ca-136">Select **Azure AD B2C** from the portal menu, click **Applications**, and then click **Add**.</span></span>

   ![添加新的应用注册](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C1.png)

2. <span data-ttu-id="113ca-138">指定应用程序的“名称”，添加 `http://localhost:8080/home` 作为“回复 URL”，将“应用程序 ID”作为 `${your-client-id}` 记录，然后单击“保存”。    </span><span class="sxs-lookup"><span data-stu-id="113ca-138">Specify your application **Name**, add `http://localhost:8080/home` for the **Reply URL**, record the **Application ID** as your `${your-client-id}` and then click **Save**.</span></span>

   ![添加应用程序回复 URL](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C2.png)

3. <span data-ttu-id="113ca-140">从应用程序中选择“密钥”，单击“生成密钥”以生成 `${your-client-secret}`，然后单击“保存”。   </span><span class="sxs-lookup"><span data-stu-id="113ca-140">Select **Keys** from your application, click **Generate key** to generate `${your-client-secret}` and then **Save**.</span></span>

4. <span data-ttu-id="113ca-141">在左侧选择“用户流”，然后单击“新建用户流”。   \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="113ca-141">Select **User flows** on your left, and then **Click** \*\*New user flow \*\*.</span></span>

   ![创建用户流](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C3.png)

5. <span data-ttu-id="113ca-143">选择“注册或登录”、“配置文件编辑”和“密码重置”，分别创建用户流。   </span><span class="sxs-lookup"><span data-stu-id="113ca-143">Choose **Sign up or in**, **Profile editing** and **Password reset** to create user flows respectively.</span></span> <span data-ttu-id="113ca-144">指定用户流的“名称”和“用户特性和声明”，然后单击“创建”。   </span><span class="sxs-lookup"><span data-stu-id="113ca-144">Specify your user flow **Name** and **User attributes and claims**, click **Create**.</span></span>

   ![配置用户流](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/B2C4.png)

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="113ca-146">配置并编译你的应用</span><span class="sxs-lookup"><span data-stu-id="113ca-146">Configure and compile your app</span></span>

1. <span data-ttu-id="113ca-147">将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。</span><span class="sxs-lookup"><span data-stu-id="113ca-147">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

2. <span data-ttu-id="113ca-148">导航到项目的父文件夹，并在文本编辑器中打开 `pom.xml` Maven 项目文件。</span><span class="sxs-lookup"><span data-stu-id="113ca-148">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

3. <span data-ttu-id="113ca-149">将 Spring OAuth2 安全性的依赖项添加到 `pom.xml`：</span><span class="sxs-lookup"><span data-stu-id="113ca-149">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

4. <span data-ttu-id="113ca-150">保存并关闭 pom.xml 文件  。</span><span class="sxs-lookup"><span data-stu-id="113ca-150">Save and close the *pom.xml* file.</span></span>

5. <span data-ttu-id="113ca-151">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.yml* 文件。</span><span class="sxs-lookup"><span data-stu-id="113ca-151">Navigate to the *src/main/resources* folder in your project and open the *application.yml* file in a text editor.</span></span>

6. <span data-ttu-id="113ca-152">使用之前创建的值指定用于应用注册的设置，例如：</span><span class="sxs-lookup"><span data-stu-id="113ca-152">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="113ca-153">其中：</span><span class="sxs-lookup"><span data-stu-id="113ca-153">Where:</span></span>

   | <span data-ttu-id="113ca-154">参数</span><span class="sxs-lookup"><span data-stu-id="113ca-154">Parameter</span></span> | <span data-ttu-id="113ca-155">说明</span><span class="sxs-lookup"><span data-stu-id="113ca-155">Description</span></span> |
   |---|---|
   | `azure.activedirectory.b2c.tenant` | <span data-ttu-id="113ca-156">包含前面复制的 AD B2C 的 `${your-tenant-name`。</span><span class="sxs-lookup"><span data-stu-id="113ca-156">Contains your AD B2C's `${your-tenant-name` from earlier.</span></span> |
   | `azure.activedirectory.b2c.client-id` | <span data-ttu-id="113ca-157">包含前面填写的应用程序中的 `${your-client-id}`。</span><span class="sxs-lookup"><span data-stu-id="113ca-157">Contains the `${your-client-id}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.client-secret` | <span data-ttu-id="113ca-158">包含前面填写的应用程序中的 `${your-client-secret}`。</span><span class="sxs-lookup"><span data-stu-id="113ca-158">Contains the `${your-client-secret}` from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.reply-url` | <span data-ttu-id="113ca-159">包含前面填写的应用程序中的某个“回复 URL”  。</span><span class="sxs-lookup"><span data-stu-id="113ca-159">Contains one of the **Reply URL** from your application that you completed earlier.</span></span> |
   | `azure.activedirectory.b2c.logout-success-url` | <span data-ttu-id="113ca-160">指定应用程序成功注销时的 URL。</span><span class="sxs-lookup"><span data-stu-id="113ca-160">Specify the URL when your application logout successfully.</span></span> |
   | `azure.activedirectory.b2c.user-flows` | <span data-ttu-id="113ca-161">包含前面填写的用户流的名称。</span><span class="sxs-lookup"><span data-stu-id="113ca-161">Contains the name of the user flows that you completed earlier.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="113ca-162">如需 *application.yml* 文件中提供的值的完整列表，请参阅 GitHub 上的 [Azure Active Directory B2C Spring Boot 示例][AAD B2C Spring Boot 示例]。</span><span class="sxs-lookup"><span data-stu-id="113ca-162">For a full list of values that are available in your *application.yml* file, see the [Azure Active Directory B2C Spring Boot Sample][AAD B2C Spring Boot Sample] on GitHub.</span></span>
   >

7. <span data-ttu-id="113ca-163">保存并关闭 application.yml  文件。</span><span class="sxs-lookup"><span data-stu-id="113ca-163">Save and close the *application.yml* file.</span></span>

8. <span data-ttu-id="113ca-164">在应用程序的 Java 源文件夹中创建名为“controller”的文件夹。 </span><span class="sxs-lookup"><span data-stu-id="113ca-164">Create a folder named *controller* in the Java source folder for your application.</span></span>

9. <span data-ttu-id="113ca-165">在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="113ca-165">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

10. <span data-ttu-id="113ca-166">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="113ca-166">Enter the following code, then save and close the file:</span></span>

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

11. <span data-ttu-id="113ca-167">在应用程序的 Java 源文件夹中创建名为“security”的文件夹。 </span><span class="sxs-lookup"><span data-stu-id="113ca-167">Create a folder named *security* in the Java source folder for your application.</span></span>

12. <span data-ttu-id="113ca-168">在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="113ca-168">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

13. <span data-ttu-id="113ca-169">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="113ca-169">Enter the following code, then save and close the file:</span></span>

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
14. <span data-ttu-id="113ca-170">从 [Azure AD B2C Spring Boot 示例](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates)复制 `greeting.html` 和 `home.html`，并将 `${your-profile-edit-user-flow}` 和 `${your-password-reset-user-flow}` 分别替换为此前填写的用户流名称。</span><span class="sxs-lookup"><span data-stu-id="113ca-170">Copy the `greeting.html` and `home.html` from [Azure AD B2C Spring Boot Sample](https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-b2c-oidc-spring-boot-sample/src/main/resources/templates), and replace the `${your-profile-edit-user-flow}` and `${your-password-reset-user-flow}` with your user flow name respectively that completed earlier.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="113ca-171">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="113ca-171">Build and test your app</span></span>

1. <span data-ttu-id="113ca-172">打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="113ca-172">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

2. <span data-ttu-id="113ca-173">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="113ca-173">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

3. <span data-ttu-id="113ca-174">在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 <http://localhost:8080/>；系统会将你重定向到登录页。</span><span class="sxs-lookup"><span data-stu-id="113ca-174">After your application is built and started by Maven, open <http://localhost:8080/> in a web browser; you should be redirected to login page.</span></span>

   ![登录页](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO1.png)

4. <span data-ttu-id="113ca-176">单击名称为 `${your-sign-up-or-in}` 用户流的链接，系统会将你重定向到 Azure AD B2C 以启动身份验证过程。</span><span class="sxs-lookup"><span data-stu-id="113ca-176">Click linke with name of `${your-sign-up-or-in}` user flow, you should be rediected Azure AD B2C to start the authentication process.</span></span>

   ![Azure AD B2C 登录](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO2.png)

4. <span data-ttu-id="113ca-178">成功登录后，应该会在浏览器中看到示例 `home page`。</span><span class="sxs-lookup"><span data-stu-id="113ca-178">After you have logged in successfully, you should see the sample `home page` from the browser,</span></span>

   ![成功登录](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/LO3.png)

## <a name="summary"></a><span data-ttu-id="113ca-180">摘要</span><span class="sxs-lookup"><span data-stu-id="113ca-180">Summary</span></span>

<span data-ttu-id="113ca-181">在本教程中，我们使用 Azure Active Directory B2C 起动器创建了新的 Java Web 应用程序，配置了新的 Azure AD B2C 租户并在其中注册了新的应用程序，然后将应用程序配置为使用 Spring 批注和类来保护 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="113ca-181">In this tutorial, you created a new Java web application using the Azure Active Directory B2C starter, configured a new Azure AD B2C tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="113ca-182">后续步骤</span><span class="sxs-lookup"><span data-stu-id="113ca-182">Next steps</span></span>

<span data-ttu-id="113ca-183">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="113ca-183">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="113ca-184">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="113ca-184">Spring on Azure</span></span>](/java/azure/spring-framework)
