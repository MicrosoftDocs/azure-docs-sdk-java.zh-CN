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
ms.openlocfilehash: d3b6bdc4aaae79864d370c581585167cf3732160
ms.sourcegitcommit: bb7286fad75a2bb43e6ce1a8f1b09e701147c9f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "48047175"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="6a40c-103">如何使用适用于 Azure Active Directory 的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="6a40c-103">How to use the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="6a40c-104">概述</span><span class="sxs-lookup"><span data-stu-id="6a40c-104">Overview</span></span>

<span data-ttu-id="6a40c-105">本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。</span><span class="sxs-lookup"><span data-stu-id="6a40c-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a40c-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="6a40c-106">Prerequisites</span></span>

<span data-ttu-id="6a40c-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="6a40c-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="6a40c-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="6a40c-109">[Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="6a40c-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="6a40c-110">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="6a40c-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="6a40c-111">使用 Spring Initializr 创建自定义应用程序</span><span class="sxs-lookup"><span data-stu-id="6a40c-111">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="6a40c-112">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="6a40c-112">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="6a40c-113">指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。</span><span class="sxs-lookup"><span data-stu-id="6a40c-113">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定组和项目名称][security-01]

1. <span data-ttu-id="6a40c-115">向下滚动到“核心”部分并选中“安全”对应的框，然后在“Web”部分选中“Web”对应的框。</span><span class="sxs-lookup"><span data-stu-id="6a40c-115">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**.</span></span>

   ![选择“安全”和“Web”起动器][security-02]

1. <span data-ttu-id="6a40c-117">向下滚动到“Azure”部分，并选中“Azure Active Directory”对应的框。</span><span class="sxs-lookup"><span data-stu-id="6a40c-117">Scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![选择 Azure Active Directory 起动器][security-03]

1. <span data-ttu-id="6a40c-119">滚动到页面底部，单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="6a40c-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![生成 Spring Boot 项目][security-04]

1. <span data-ttu-id="6a40c-121">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="6a40c-121">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-and-configure-a-new-azure-active-directory-instance"></a><span data-ttu-id="6a40c-122">创建并配置新的 Azure Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="6a40c-122">Create and configure a new Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="6a40c-123">创建 Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="6a40c-123">Create the Active Directory instance</span></span>

1. <span data-ttu-id="6a40c-124">登录到 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="6a40c-124">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="6a40c-125">依次单击“+新建”、“安全 + 标识”、“Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-125">Click **+New**, then **Security + Identity**, and then **Azure Active Directory**.</span></span>

   ![创建新的 Azure Active Directory 实例][directory-01]

1. <span data-ttu-id="6a40c-127">输入“组织名称”和“初始域名”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-127">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="6a40c-128">复制你的目录的完整 URL；在本教程中，稍后你将使用它来添加用户帐户。</span><span class="sxs-lookup"><span data-stu-id="6a40c-128">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="6a40c-129">（例如：`wingtiptoysdirectory.onmicrosoft.com`。）完成后，单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-129">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![指定 Azure Active Directory 名称][directory-02]

1. <span data-ttu-id="6a40c-131">从 Azure 门户顶部的下拉菜单中选择新的 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="6a40c-131">Select your new Azure Active Directory from the drop-down menu on the top toolbar of the Azure portal.</span></span>

   ![选择 Azure Active Directory][directory-03]

1. <span data-ttu-id="6a40c-133">从门户菜单中选择“Azure Active Directory”，单击“属性”，并复制“目录 ID”；在本教程中，稍后将使用该值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-133">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![复制 Azure Active Directory ID][directory-13]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="6a40c-135">添加 Spring Boot 应用的应用程序注册</span><span class="sxs-lookup"><span data-stu-id="6a40c-135">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="6a40c-136">从门户菜单中选择“Azure Active Directory”，依次单击“概述”、“应用注册”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-136">Select **Azure Active Directory** from the portal menu, click **Overview**, and then click **App registrations**.</span></span>

   ![添加新的应用注册][directory-04]

1. <span data-ttu-id="6a40c-138">单击“新建应用程序注册”，指定应用程序的“名称”，使用 http://localhost:8080 作为“登录 URL”，并单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-138">Click **New application registration**, specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新建应用注册][directory-05]

1. <span data-ttu-id="6a40c-140">创建应用程序注册后，请单击它。</span><span class="sxs-lookup"><span data-stu-id="6a40c-140">Click your application registration after it has been created.</span></span>

   ![选择应用注册][directory-06]

1. <span data-ttu-id="6a40c-142">当应用注册页出现时，复制你的**应用 ID**；在本教程中，稍后你将使用此值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-142">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="6a40c-143">单击“设置”，然后单击“密钥”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-143">Click **Settings**, and then click **Keys**.</span></span>

   ![创建应用注册密钥][directory-07]

1. <span data-ttu-id="6a40c-145">添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，你需要复制该密钥的值，本教程中稍后将使用该值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-145">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="6a40c-146">（以后无法检索此值。）</span><span class="sxs-lookup"><span data-stu-id="6a40c-146">(You will not be able to retrieve this value later.)</span></span>

   ![指定应用注册密钥参数][directory-08]

1. <span data-ttu-id="6a40c-148">在应用注册的主页上，依次单击“设置”、“所需的权限”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-148">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![应用注册 - 所需的权限][directory-09]

1. <span data-ttu-id="6a40c-150">单击“Windows Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-150">Click **Windows Azure Active Directory**.</span></span>

   ![选择“Windows Azure Active Directory”][directory-10]

1. <span data-ttu-id="6a40c-152">选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-152">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![启用访问权限][directory-11]

1. <span data-ttu-id="6a40c-154">在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-154">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授予访问权限][directory-12]

1. <span data-ttu-id="6a40c-156">在应用注册的主页上，依次单击“设置”、“回复 URL”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-156">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

   ![编辑回复 URL][directory-14]

1. <span data-ttu-id="6a40c-158">输入“http://localhost:8080/login/oauth2/code/azure”作为新的回复 URL，并单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-158">Enter "http://localhost:8080/login/oauth2/code/azure" as a new reply URL, and then click **Save**.</span></span>

   ![添加新的回复 URL][directory-15]

1. <span data-ttu-id="6a40c-160">从应用注册主页面上，单击“清单”，将 `oauth2AllowImplicitFlow` 参数的值设置为 `true`，然后单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-160">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

   ![配置应用清单][directory-16]

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-162">有关 `oauth2AllowImplicitFlow` 参数和其他应用程序设置的详细信息，请参阅 [Azure Active Directory 应用程序清单][AAD app manifest]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-162">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
   >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="6a40c-163">将用户帐户添加到你的目录，并将该帐户添加到某个组</span><span class="sxs-lookup"><span data-stu-id="6a40c-163">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="6a40c-164">从 Active Directory 的“概述”页面上，单击“用户”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-164">From the **Overview** page of your Active Directory, click **Users**.</span></span>

   ![打开“用户”面板][directory-17]

1. <span data-ttu-id="6a40c-166">当“用户”面板显示时，单击“新建用户”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-166">When the **Users** panel is displayed, click **New user**.</span></span>

   ![添加新用户帐户][directory-18]

1. <span data-ttu-id="6a40c-168">当“用户”面板显示时，输入**名称**和**用户名**。</span><span class="sxs-lookup"><span data-stu-id="6a40c-168">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![输入用户帐户信息][directory-19]

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-170">在输入用户名时，需要指定本教程前文中的目录 URL，例如：</span><span class="sxs-lookup"><span data-stu-id="6a40c-170">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="6a40c-171">单击“组”，选择要用于在应用程序中进行身份验证的组，然后单击“选择”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-171">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="6a40c-172">（针对在本教程中的用途，请将帐户添加到 _Users_ 组。）</span><span class="sxs-lookup"><span data-stu-id="6a40c-172">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![选择用户的组][directory-20]

1. <span data-ttu-id="6a40c-174">单击“显示密码”，然后复制密码；在本教程中，登录到应用程序时将使用此密码。</span><span class="sxs-lookup"><span data-stu-id="6a40c-174">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span>

   ![显示密码][directory-21]

1. <span data-ttu-id="6a40c-176">单击“创建”将新的用户帐户添加到目录。</span><span class="sxs-lookup"><span data-stu-id="6a40c-176">Click **Create** to add the new user account to your directory.</span></span>

   ![创建新的用户帐户][directory-22]

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="6a40c-178">配置并编译 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="6a40c-178">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="6a40c-179">将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。</span><span class="sxs-lookup"><span data-stu-id="6a40c-179">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="6a40c-180">导航到项目的父文件夹，并在文本编辑器中打开 *pom.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-180">Navigate to the parent folder for your project, and open the *pom.xml* file in a text editor.</span></span>

1. <span data-ttu-id="6a40c-181">添加 Spring OAuth2 安全性的依赖项，例如：</span><span class="sxs-lookup"><span data-stu-id="6a40c-181">Add the dependencies for Spring OAuth2 security; for example:</span></span>

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

1. <span data-ttu-id="6a40c-182">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-182">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="6a40c-183">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-183">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="6a40c-184">使用之前创建的值指定用于应用注册的设置，例如：</span><span class="sxs-lookup"><span data-stu-id="6a40c-184">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="6a40c-185">其中：</span><span class="sxs-lookup"><span data-stu-id="6a40c-185">Where:</span></span>

   | <span data-ttu-id="6a40c-186">参数</span><span class="sxs-lookup"><span data-stu-id="6a40c-186">Parameter</span></span> | <span data-ttu-id="6a40c-187">Description</span><span class="sxs-lookup"><span data-stu-id="6a40c-187">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="6a40c-188">包含前面复制的 Active Directory“目录 ID”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-188">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="6a40c-189">包含前面填写的、应用注册的“应用程序 ID”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-189">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="6a40c-190">包含前面填写的、应用注册密钥中的“值”。</span><span class="sxs-lookup"><span data-stu-id="6a40c-190">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="6a40c-191">包含用于授权的 Active Directory 组列表。</span><span class="sxs-lookup"><span data-stu-id="6a40c-191">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-192">有关可在 *application.properties* 文件中使用的值的完整列表，请参阅 GitHub 上的 [Azure Active Directory Spring Boot 示例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-192">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="6a40c-193">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-193">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="6a40c-194">在应用程序的 Java 源文件夹中创建名为 *controller* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/controller*。</span><span class="sxs-lookup"><span data-stu-id="6a40c-194">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="6a40c-195">在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-195">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="6a40c-196">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="6a40c-196">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="6a40c-197">为 `@PreAuthorize("hasRole('')")` 方法指定的组名称必须包含 *application.properties* 文件的 `azure.activedirectory.active-directory-groups` 字段中指定的某个组。</span><span class="sxs-lookup"><span data-stu-id="6a40c-197">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   >

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-198">可为不同的请求映射指定不同的授权设置，例如：</span><span class="sxs-lookup"><span data-stu-id="6a40c-198">You can specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="6a40c-199">在应用程序的 Java 源文件夹中创建名为 *security* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/security*。</span><span class="sxs-lookup"><span data-stu-id="6a40c-199">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="6a40c-200">在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="6a40c-200">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="6a40c-201">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="6a40c-201">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="6a40c-202">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="6a40c-202">Build and test your app</span></span>

1. <span data-ttu-id="6a40c-203">打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="6a40c-203">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="6a40c-204">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="6a40c-204">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![生成应用程序][build-application]

1. <span data-ttu-id="6a40c-206">在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 <http://localhost:8080>；系统应会提示输入用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="6a40c-206">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![登录到应用程序][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-208">如果这是新用户帐户的首次登录，可能会提示你更改密码。</span><span class="sxs-lookup"><span data-stu-id="6a40c-208">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![更改密码][update-password]
   > 

1. <span data-ttu-id="6a40c-210">成功登录后，控制器中应会显示“Hello World”示例文本。</span><span class="sxs-lookup"><span data-stu-id="6a40c-210">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![成功登录][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="6a40c-212">未授权的用户帐户会收到“HTTP 403 未授权”消息。</span><span class="sxs-lookup"><span data-stu-id="6a40c-212">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="6a40c-213">后续步骤</span><span class="sxs-lookup"><span data-stu-id="6a40c-213">Next steps</span></span>

<span data-ttu-id="6a40c-214">有关使用 Azure Active Directory 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="6a40c-214">For more information about using Azure Active Directory, see the following articles:</span></span>

* <span data-ttu-id="6a40c-215">[Azure Active Directory 文档]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-215">[Azure Active Directory Documentation].</span></span>

<span data-ttu-id="6a40c-216">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="6a40c-216">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="6a40c-217">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="6a40c-217">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="6a40c-218">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="6a40c-218">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="6a40c-219">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-219">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="6a40c-220">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="6a40c-220">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="6a40c-221">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="6a40c-221">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="6a40c-222">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="6a40c-222">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="6a40c-223">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="6a40c-223">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="6a40c-224">有关更详细示例，请参阅 GitHub 上的 [Azure Active Directory Spring Boot 示例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="6a40c-224">For a more-detailed sample, see the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>

<!-- URL List -->

[Azure Active Directory 文档]: /azure/active-directory/
[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
