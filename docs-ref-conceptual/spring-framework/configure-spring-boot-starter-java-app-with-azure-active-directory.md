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
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a><span data-ttu-id="21282-103">教程：使用适用于 Azure Active Directory 的 Spring Boot 起动器保护 Java Web 应用</span><span class="sxs-lookup"><span data-stu-id="21282-103">Tutorial: Secure a Java web app using the Spring Boot Starter for Azure Active Directory</span></span>

## <a name="overview"></a><span data-ttu-id="21282-104">概述</span><span class="sxs-lookup"><span data-stu-id="21282-104">Overview</span></span>

<span data-ttu-id="21282-105">本文演示了如何使用 **[Spring Initializr]** 创建一个 Java 应用，该应用使用适用于 Azure Active Directory (Azure AD) 的 Spring Boot 起动器。</span><span class="sxs-lookup"><span data-stu-id="21282-105">This article demonstrates creating a Java app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21282-106">本教程介绍如何执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="21282-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21282-107">使用 Spring Initializr 创建 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="21282-107">Create a Java application using the Spring Initializr</span></span>
> * <span data-ttu-id="21282-108">配置 Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="21282-108">Configure Azure Active Directory</span></span>
> * <span data-ttu-id="21282-109">使用 Spring Boot 类和批注保护应用程序</span><span class="sxs-lookup"><span data-stu-id="21282-109">Secure the application with Spring Boot classes and annotations</span></span>
> * <span data-ttu-id="21282-110">生成和测试 Java 试应用程序</span><span class="sxs-lookup"><span data-stu-id="21282-110">Build and test your Java application</span></span>

<span data-ttu-id="21282-111">如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="21282-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21282-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="21282-112">Prerequisites</span></span>

<span data-ttu-id="21282-113">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="21282-113">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="21282-114">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="21282-114">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="21282-115">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="21282-115">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="21282-116">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="21282-116">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="21282-117">使用 Spring Initialzr 创建应用</span><span class="sxs-lookup"><span data-stu-id="21282-117">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="21282-118">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="21282-118">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="21282-119">指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。</span><span class="sxs-lookup"><span data-stu-id="21282-119">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定组和项目名称][create-spring-app-01]

1. <span data-ttu-id="21282-121">向下滚动到“核心”部分并选中“安全性”的复选框，在“Web”部分中选中“Web”的复选框，然后向下滚动到“Azure”部分并选中“Azure Active Directory”的复选框。</span><span class="sxs-lookup"><span data-stu-id="21282-121">Scroll down to the **Core** section and check the box for **Security**, and in the **Web** section check the box for **Web**, then scroll down to the **Azure** section and check the box for **Azure Active Directory**.</span></span>

   ![选择“安全性”、“Web”和“Azure Active Directory”起动器][create-spring-app-02]

1. <span data-ttu-id="21282-123">滚动到页面顶部或底部，单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="21282-123">Scroll to the top or bottom of the page and click the button to **Generate Project**.</span></span>

   ![生成 Spring Boot 项目][create-spring-app-03]

1. <span data-ttu-id="21282-125">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="21282-125">When prompted, download the project to a path on your local computer.</span></span>

## <a name="create-azure-active-directory-instance"></a><span data-ttu-id="21282-126">创建 Azure Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="21282-126">Create Azure Active Directory instance</span></span>

### <a name="create-the-active-directory-instance"></a><span data-ttu-id="21282-127">创建 Active Directory 实例</span><span class="sxs-lookup"><span data-stu-id="21282-127">Create the Active Directory instance</span></span>

1. <span data-ttu-id="21282-128">登录到 <https://portal.azure.com>。</span><span class="sxs-lookup"><span data-stu-id="21282-128">Log into <https://portal.azure.com>.</span></span>

1. <span data-ttu-id="21282-129">依次单击“+创建资源”、“标识”和“Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="21282-129">Click **+Create a resource**, then **Identity**, and then **Azure Active Directory**.</span></span>

   ![创建新的 Azure Active Directory 实例][create-directory-01]

1. <span data-ttu-id="21282-131">输入“组织名称”和“初始域名”。</span><span class="sxs-lookup"><span data-stu-id="21282-131">Enter your **Organization name** and your **Initial domain name**.</span></span> <span data-ttu-id="21282-132">复制你的目录的完整 URL；在本教程中，稍后你将使用它来添加用户帐户。</span><span class="sxs-lookup"><span data-stu-id="21282-132">Copy the full URL of your directory; you will use that to add user accounts later in this tutorial.</span></span> <span data-ttu-id="21282-133">（例如：`wingtiptoysdirectory.onmicrosoft.com`。）完成后，单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="21282-133">(For example: `wingtiptoysdirectory.onmicrosoft.com`.) When you have finished, click **Create**.</span></span>

   ![指定 Azure Active Directory 名称][create-directory-02]

1. <span data-ttu-id="21282-135">在 Azure 门户工具栏的右上角选择你的帐户名称，然后单击“切换目录”。</span><span class="sxs-lookup"><span data-stu-id="21282-135">Select your account name on the top-right of the Azure portal toolbar, then click **Switch directory**.</span></span>

   ![选择你的 Azure 帐户名称][create-directory-03]

1. <span data-ttu-id="21282-137">从下拉菜单中选择你的新 Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="21282-137">Select your new Azure Active Directory from the drop-down menu.</span></span>

   ![选择 Azure Active Directory][create-directory-04]

1. <span data-ttu-id="21282-139">从门户菜单中选择“Azure Active Directory”，单击“属性”，并复制“目录 ID”；在本教程中，稍后将使用该值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-139">Select **Azure Active Directory** from the portal menu, click **Properties**, and copy the **Directory ID**; you will use that value to configure your *application.properties* file later in this tutorial.</span></span>

   ![复制 Azure Active Directory ID][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a><span data-ttu-id="21282-141">添加 Spring Boot 应用的应用程序注册</span><span class="sxs-lookup"><span data-stu-id="21282-141">Add an application registration for your Spring Boot app</span></span>

1. <span data-ttu-id="21282-142">从门户菜单中选择“Azure Active Directory”，依次单击“应用注册”和“新建应用注册”。</span><span class="sxs-lookup"><span data-stu-id="21282-142">Select **Azure Active Directory** from the portal menu, click **App registrations**, and then click **New application registration**, .</span></span>

   ![添加新的应用注册][create-app-registration-01]

2. <span data-ttu-id="21282-144">指定应用程序的**名称**，使用 http://localhost:8080 作为**登录 URL**，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="21282-144">Specify your application **Name**, use http://localhost:8080 for the **Sign-on URL**, and then click **Create**.</span></span>

   ![新建应用注册][create-app-registration-02]

4. <span data-ttu-id="21282-146">当应用注册页出现时，复制你的**应用 ID**；在本教程中，稍后你将使用此值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-146">When the page for your app registration appears, copy your **Application ID**; you will use this value to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="21282-147">单击“设置”，然后单击“密钥”。</span><span class="sxs-lookup"><span data-stu-id="21282-147">Click **Settings**, and then click **Keys**.</span></span>

   ![创建应用注册密钥][create-app-registration-03]

5. <span data-ttu-id="21282-149">添加“说明”并指定新密钥的“持续时间”，单击“保存”；单击“保存”图标时，会自动填充密钥的值，你需要复制该密钥的值，本教程中稍后将使用该值来配置 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-149">Add a **Description** and specify the **Duration** for a new key and click **Save**; the value for the key will be automatically filled in when you click the **Save** icon, and you need to copy down the value of the key to configure your *application.properties* file later in this tutorial.</span></span> <span data-ttu-id="21282-150">（以后无法检索此值。）</span><span class="sxs-lookup"><span data-stu-id="21282-150">(You will not be able to retrieve this value later.)</span></span>

   ![指定应用注册密钥参数][create-app-registration-04]

6. <span data-ttu-id="21282-152">在应用注册的主页上，依次单击“设置”、“所需的权限”。</span><span class="sxs-lookup"><span data-stu-id="21282-152">From the main page for your app registration, click **Settings**, and then click **Required permissions**.</span></span>

   ![应用注册 - 所需的权限][create-app-registration-05]

7. <span data-ttu-id="21282-154">单击“Windows Azure Active Directory”。</span><span class="sxs-lookup"><span data-stu-id="21282-154">Click **Windows Azure Active Directory**.</span></span>

   ![选择“Windows Azure Active Directory”][create-app-registration-06]

8. <span data-ttu-id="21282-156">选中“以登录用户身份访问该目录”和“登录并读取用户个人资料”对应的框，单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="21282-156">Check the boxes for **Access the directory as the signed-in user** and **Sign in and read user profile**, and then click **Save**.</span></span>

   ![启用访问权限][create-app-registration-07]

9. <span data-ttu-id="21282-158">在“所需的权限”页上，单击“授予权限”，并在出现提示时单击“是”。</span><span class="sxs-lookup"><span data-stu-id="21282-158">On the **Required permissions** page, click **Grant Permissions**, and click **Yes** when prompted.</span></span>

   ![授予访问权限][create-app-registration-08]

10. <span data-ttu-id="21282-160">在应用注册的主页上，依次单击“设置”、“回复 URL”。</span><span class="sxs-lookup"><span data-stu-id="21282-160">From the main page for your app registration, click **Settings**, and then click **Reply URLs**.</span></span>

    ![编辑回复 URL][create-app-registration-09]

11. <span data-ttu-id="21282-162">输入“<http://localhost:8080/login/oauth2/code/azure>”作为新的回复 URL，并单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="21282-162">Enter "<http://localhost:8080/login/oauth2/code/azure>" as a new reply URL, and then click **Save**.</span></span>

    ![添加新的回复 URL][create-app-registration-10]

12. <span data-ttu-id="21282-164">从应用注册主页面上，单击“清单”，将 `oauth2AllowImplicitFlow` 参数的值设置为 `true`，然后单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="21282-164">From the main page for your app registration, click **Manifest**, then set the value of the `oauth2AllowImplicitFlow` parameter to `true`, and then click **Save**.</span></span>

    ![配置应用清单][create-app-registration-11]

    > [!NOTE]
    > 
    > <span data-ttu-id="21282-166">有关 `oauth2AllowImplicitFlow` 参数和其他应用程序设置的详细信息，请参阅 [Azure Active Directory 应用程序清单][AAD app manifest]。</span><span class="sxs-lookup"><span data-stu-id="21282-166">For more information about the `oauth2AllowImplicitFlow` parameter and other application settings, see [Azure Active Directory application manifest][AAD app manifest].</span></span> 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a><span data-ttu-id="21282-167">将用户帐户添加到你的目录，并将该帐户添加到某个组</span><span class="sxs-lookup"><span data-stu-id="21282-167">Add a user account to your directory, and add that account to a group</span></span>

1. <span data-ttu-id="21282-168">从你的 Active Directory 的“概述”页面上，单击“所有用户”，然后单击“新建用户”。</span><span class="sxs-lookup"><span data-stu-id="21282-168">From the **Overview** page of your Active Directory, click **All Users**, and then click **New user**.</span></span>

   ![添加新用户帐户][create-user-01]

1. <span data-ttu-id="21282-170">当“用户”面板显示时，输入**名称**和**用户名**。</span><span class="sxs-lookup"><span data-stu-id="21282-170">When the **User** panel is displayed, enter the **Name** and **User name**.</span></span>

   ![输入用户帐户信息][create-user-02]

   > [!NOTE]
   > 
   > <span data-ttu-id="21282-172">在输入用户名时，需要指定本教程前文中的目录 URL，例如：</span><span class="sxs-lookup"><span data-stu-id="21282-172">You need to specify your directory URL from earlier in this tutorial when you enter the user name; for example:</span></span>
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. <span data-ttu-id="21282-173">单击“组”，选择要用于在应用程序中进行身份验证的组，然后单击“选择”。</span><span class="sxs-lookup"><span data-stu-id="21282-173">Click **Groups**, then select the groups that you will use for authorization in your application, and then click **Select**.</span></span> <span data-ttu-id="21282-174">（针对在本教程中的用途，请将帐户添加到 _Users_ 组。）</span><span class="sxs-lookup"><span data-stu-id="21282-174">(For the purposes of this tutorial, add the account to the _Users_ group.)</span></span>

   ![选择用户的组][create-user-03]

1. <span data-ttu-id="21282-176">单击“显示密码”，然后复制密码；在本教程中，登录到应用程序时将使用此密码。</span><span class="sxs-lookup"><span data-stu-id="21282-176">Click **Show password**, and copy the password; you will use this when you log into your application later in this tutorial.</span></span> <span data-ttu-id="21282-177">复制密码后，单击“创建”将新的用户帐户添加到你的目录。</span><span class="sxs-lookup"><span data-stu-id="21282-177">When you have copied the password, click **Create** to add the new user account to your directory.</span></span>

   ![显示密码][create-user-04]

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="21282-179">配置并编译你的应用</span><span class="sxs-lookup"><span data-stu-id="21282-179">Configure and compile your app</span></span>

1. <span data-ttu-id="21282-180">将本教程中之前创建并下载的项目存档中的文件提取到某个目录中。</span><span class="sxs-lookup"><span data-stu-id="21282-180">Extract the files from the project archive you created and downloaded earlier in this tutorial into a directory.</span></span>

1. <span data-ttu-id="21282-181">导航到项目的父文件夹，并在文本编辑器中打开 `pom.xml` Maven 项目文件。</span><span class="sxs-lookup"><span data-stu-id="21282-181">Navigate to the parent folder for your project, and open the `pom.xml` Maven project file in a text editor.</span></span>

1. <span data-ttu-id="21282-182">将 Spring OAuth2 安全性的依赖项添加到 `pom.xml`：</span><span class="sxs-lookup"><span data-stu-id="21282-182">Add the dependencies for Spring OAuth2 security to the `pom.xml`:</span></span>

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

1. <span data-ttu-id="21282-183">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-183">Save and close the *pom.xml* file.</span></span>

1. <span data-ttu-id="21282-184">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-184">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="21282-185">使用之前创建的值指定用于应用注册的设置，例如：</span><span class="sxs-lookup"><span data-stu-id="21282-185">Specify the settings for your app registration using the values you created earlier; for example:</span></span>

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
   <span data-ttu-id="21282-186">其中：</span><span class="sxs-lookup"><span data-stu-id="21282-186">Where:</span></span>

   | <span data-ttu-id="21282-187">参数</span><span class="sxs-lookup"><span data-stu-id="21282-187">Parameter</span></span> | <span data-ttu-id="21282-188">说明</span><span class="sxs-lookup"><span data-stu-id="21282-188">Description</span></span> |
   |---|---|
   | `azure.activedirectory.tenant-id` | <span data-ttu-id="21282-189">包含前面复制的 Active Directory“目录 ID”。</span><span class="sxs-lookup"><span data-stu-id="21282-189">Contains your Active Directory's **Directory ID** from earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-id` | <span data-ttu-id="21282-190">包含前面填写的、应用注册的“应用程序 ID”。</span><span class="sxs-lookup"><span data-stu-id="21282-190">Contains the **Application ID** from your app registration that you completed earlier.</span></span> |
   | `spring.security.oauth2.client.registration.azure.client-secret` | <span data-ttu-id="21282-191">包含前面填写的、应用注册密钥中的“值”。</span><span class="sxs-lookup"><span data-stu-id="21282-191">Contains the **Value** from your app registration key that you completed earlier.</span></span> |
   | `azure.activedirectory.active-directory-groups` | <span data-ttu-id="21282-192">包含用于授权的 Active Directory 组列表。</span><span class="sxs-lookup"><span data-stu-id="21282-192">Contains a list of Active Directory groups to use for authorization.</span></span> |

   > [!NOTE]
   > 
   > <span data-ttu-id="21282-193">有关可在 *application.properties* 文件中使用的值的完整列表，请参阅 GitHub 上的 [Azure Active Directory Spring Boot 示例][AAD Spring Boot Sample]。</span><span class="sxs-lookup"><span data-stu-id="21282-193">For a full list of values that are available in your *application.properties* file, see  the [Azure Active Directory Spring Boot Sample][AAD Spring Boot Sample] on GitHub.</span></span>
   >

1. <span data-ttu-id="21282-194">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="21282-194">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="21282-195">在应用程序的 Java 源文件夹中创建名为 *controller* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/controller*。</span><span class="sxs-lookup"><span data-stu-id="21282-195">Create a folder named *controller* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/controller*.</span></span>

1. <span data-ttu-id="21282-196">在 *controller* 文件夹中创建名为 *HelloController.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="21282-196">Create a new Java file named *HelloController.java* in the *controller* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="21282-197">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="21282-197">Enter the following code, then save and close the file:</span></span>

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
   > <span data-ttu-id="21282-198">为 `@PreAuthorize("hasRole('')")` 方法指定的组名称必须包含 *application.properties* 文件的 `azure.activedirectory.active-directory-groups` 字段中指定的某个组。</span><span class="sxs-lookup"><span data-stu-id="21282-198">The group name that you specify for the `@PreAuthorize("hasRole('')")` method must contain one of the groups that you specified in the `azure.activedirectory.active-directory-groups` field of your *application.properties* file.</span></span>
   > 
   > <span data-ttu-id="21282-199">还可以为不同的请求映射指定不同的授权设置，例如：</span><span class="sxs-lookup"><span data-stu-id="21282-199">You can also specify different authorization settings for different request mappings; for example:</span></span>
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

1. <span data-ttu-id="21282-200">在应用程序的 Java 源文件夹中创建名为 *security* 的文件夹，例如：*src/main/java/com/wingtiptoys/security/security*。</span><span class="sxs-lookup"><span data-stu-id="21282-200">Create a folder named *security* in the Java source folder for your application; for example: *src/main/java/com/wingtiptoys/security/security*.</span></span>

1. <span data-ttu-id="21282-201">在 *security* 文件夹中创建名为 *WebSecurityConfig.java* 的新 Java 文件，并在文本编辑器中打开该文件。</span><span class="sxs-lookup"><span data-stu-id="21282-201">Create a new Java file named *WebSecurityConfig.java* in the *security* folder and open it in a text editor.</span></span>

1. <span data-ttu-id="21282-202">输入以下代码，然后保存并关闭该文件：</span><span class="sxs-lookup"><span data-stu-id="21282-202">Enter the following code, then save and close the file:</span></span>

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

## <a name="build-and-test-your-app"></a><span data-ttu-id="21282-203">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="21282-203">Build and test your app</span></span>

1. <span data-ttu-id="21282-204">打开命令提示符并将目录切换到应用的 *pom.xml* 文件所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="21282-204">Open a command prompt and change directory to the folder where your app's *pom.xml* file is located.</span></span>

1. <span data-ttu-id="21282-205">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="21282-205">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![生成应用程序][build-application]

1. <span data-ttu-id="21282-207">在 Maven 生成并启动该应用程序之后，请在 Web 浏览器中打开 <http://localhost:8080>；系统应会提示输入用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="21282-207">After your application is built and started by Maven, open <http://localhost:8080> in a web browser; you should be prompted for a user name and password.</span></span>

   ![登录到应用程序][application-login]

   > [!NOTE]
   > 
   > <span data-ttu-id="21282-209">如果这是新用户帐户的首次登录，可能会提示你更改密码。</span><span class="sxs-lookup"><span data-stu-id="21282-209">You may be prompted to change your password if this is the first login for a new user account.</span></span>
   > 
   > ![更改密码][update-password]
   > 

1. <span data-ttu-id="21282-211">成功登录后，控制器中应会显示“Hello World”示例文本。</span><span class="sxs-lookup"><span data-stu-id="21282-211">After you have logged in successfully, you should see the sample "Hello World" text from the controller.</span></span>

   ![成功登录][hello-world]

   > [!NOTE]
   > 
   > <span data-ttu-id="21282-213">未授权的用户帐户会收到“HTTP 403 未授权”消息。</span><span class="sxs-lookup"><span data-stu-id="21282-213">User accounts which are not authorized will receive an **HTTP 403 Unauthorized** message.</span></span>
   >

## <a name="summary"></a><span data-ttu-id="21282-214">摘要</span><span class="sxs-lookup"><span data-stu-id="21282-214">Summary</span></span>

<span data-ttu-id="21282-215">在本教程中，使用 Azure Active Directory 起动器创建了新的 Java Web 应用程序，配置了新的 Azure AD 租户并在其中注册了新的应用程序，并且已将应用程序配置为使用 Spring 批注和类来保护 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="21282-215">In this tutorial, you created a new Java web application using the Azure Active Directory starter, configured a new Azure AD tenant and registered a new application in it, and then configured your application to use the Spring annotations and classes to protect the web app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21282-216">后续步骤</span><span class="sxs-lookup"><span data-stu-id="21282-216">Next steps</span></span>

<span data-ttu-id="21282-217">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="21282-217">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21282-218">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="21282-218">Spring on Azure</span></span>](/java/azure/spring-framework)

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
