---
title: "如何配置 Spring Boot Initializer 应用，以使用 Redis 缓存"
description: "了解如何配置使用 Spring Initializer 创建的 Spring Boot 应用程序，以使用 Azure Redis 缓存。"
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Spring Starter, Redis Cache
ms.assetid: 
ms.service: cache
ms.workload: na
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: c5e9a9214762e014e463dd3277671fc56237d4a0
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a><span data-ttu-id="ab1dd-104">如何配置 Spring Boot Initializer 应用，以使用 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="ab1dd-104">How to configure a Spring Boot Initializer app to use Redis Cache</span></span>

## <a name="overview"></a><span data-ttu-id="ab1dd-105">概述</span><span class="sxs-lookup"><span data-stu-id="ab1dd-105">Overview</span></span>

<span data-ttu-id="ab1dd-106">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-106">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="ab1dd-107">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-107">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="ab1dd-108">为帮助开发人员开始使用 Spring Boot，在 <https://github.com/spring-guides/> 网站中提供了几个 Spring Boot 包。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="ab1dd-109">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="ab1dd-110">本文提供以下分步指导：使用 Azure 门户创建 Redis 缓存，使用 Spring Initializr 创建自定义应用程序，然后创建使用 Redis 缓存存储并检索数据的 Java web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-110">This article walks you through creating a Redis cache using the Azure portal, then using the **Spring Initializr** to create a custom application, and then creating a Java web application that stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab1dd-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="ab1dd-111">Prerequisites</span></span>

<span data-ttu-id="ab1dd-112">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-112">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="ab1dd-113">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-113">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="ab1dd-114">[Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="ab1dd-115">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="ab1dd-116">在 Azure 上创建 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="ab1dd-116">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="ab1dd-117">浏览到 Azure 门户 <https://portal.azure.com/>，然后单击“+新建”项。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-117">Browse to the Azure portal at <https://portal.azure.com/> and click the item for **+New**.</span></span>

   ![Azure 门户][AZ01]

1. <span data-ttu-id="ab1dd-119">单击“数据库”，然后单击“Redis 缓存”。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-119">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure 门户][AZ02]

1. <span data-ttu-id="ab1dd-121">在“新建 Redis 缓存”页上，指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-121">On the **New Redis Cache** page, specify the following information:</span></span>

   * <span data-ttu-id="ab1dd-122">输入缓存的“DNS 名称”。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-122">Enter the **DNS name** for your cache.</span></span>
   * <span data-ttu-id="ab1dd-123">指定“订阅”、“资源组”、“位置”和“定价层”。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-123">Specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>
   * <span data-ttu-id="ab1dd-124">对于本教程，选择“取消阻止端口 6379”。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-124">For this tutorial, choose **Unblock port 6379**.</span></span>

   > [!NOTE]
   >
   > <span data-ttu-id="ab1dd-125">可以通过 Redis 缓存使用 SSL，但需要使用其他 Redis 客户端，如 Jedis。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-125">You can use SSL with Redis caches, but you would need to use a different Redis client like Jedis.</span></span> <span data-ttu-id="ab1dd-126">有关详细信息，请参阅[如何将 Azure Redis 缓存与 Java 配合使用][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-126">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>
   >

   <span data-ttu-id="ab1dd-127">指定这些选项后，单击“创建”以创建缓存。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-127">When you have specified these options, click **Create** to create your cache.</span></span>

   ![Azure 门户][AZ03]

1. <span data-ttu-id="ab1dd-129">创建缓存完成后，会看到其列在 Azure“仪表板”上，并显示在“所有资源”和“Redis 缓存”页下。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-129">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="ab1dd-130">可在任何上述位置上单击缓存，打开缓存的属性页。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-130">You can click on your cache on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 门户][AZ04]

1. <span data-ttu-id="ab1dd-132">显示包含缓存属性列表的页面后，单击“访问密匙”，然后复制缓存的访问密钥。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-132">When the page that contains the list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure 门户][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="ab1dd-134">使用 Spring Initializr 创建自定义应用程序</span><span class="sxs-lookup"><span data-stu-id="ab1dd-134">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="ab1dd-135">浏览到 https://start.spring.io/<>。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-135">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="ab1dd-136">指定要使用 Java 生成的 Maven 项目，输入应用程序的“组”名称和“Aritifact”名称，然后单击链接切换到 Spring Initializr 完整版。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-136">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="ab1dd-138">Spring Initializr 会使用“组”和“Artifact”名称创建包名称，例如：com.contoso.myazuredemo。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-138">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="ab1dd-139">向下滚动到“Web”部分，选中“Web”框，然后向下滚动到“NoSQL”，选中“Redis”框，再滚动到页面底部，单击“生成项目”按钮。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-139">Scroll down to the **Web** section and check the box for **Web**, then scroll down to the **NoSQL** section and check the box for **Redis**, then scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的完整选项][SI02]

1. <span data-ttu-id="ab1dd-141">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-141">When prompted, download the project to a path on your local computer.</span></span>

   ![下载自定义 Spring Boot 项目][SI03]

1. <span data-ttu-id="ab1dd-143">提取本地系统上的文件之后，自定义 Spring Boot 应用程序便可进行编辑。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-143">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![自定义 Spring Boot 项目文件][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a><span data-ttu-id="ab1dd-145">配置自定义 Spring Boot 以使用 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="ab1dd-145">Configure your custom Spring Boot to use your Redis Cache</span></span>

1. <span data-ttu-id="ab1dd-146">在应用的“资源”目录中找到 application.properties 文件，或创建此文件（若此文件不存在）。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-146">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![找到 application.properties 文件][RE01]

1. <span data-ttu-id="ab1dd-148">在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为缓存中的相应属性：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-148">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties from your cache:</span></span>

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![编辑 application.properties 文件][RE02]

   > [!NOTE] 
   > 
   > <span data-ttu-id="ab1dd-150">如果使用其他启用了 SSL 的 Redis 客户端（如 Jedis），可能需要在 application.properties 文件中指定端口 6380。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-150">If you were using a different Redis client like Jedis that enables SSL, you would specify port 6380 in your *application.properties* file.</span></span> <span data-ttu-id="ab1dd-151">例如：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-151">For example:</span></span>
   > 
   > ```yaml
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > spring.redis.ssl=true
   > spring.redis.port=6380
   > ```
   > 
   > <span data-ttu-id="ab1dd-152">有关详细信息，请参阅[如何将 Azure Redis 缓存与 Java 配合使用][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-152">For more information, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span> 
   > 

1. <span data-ttu-id="ab1dd-153">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-153">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="ab1dd-154">在包的源文件夹中创建名为“控制器”的文件夹，例如：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-154">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="ab1dd-155">-或-</span><span class="sxs-lookup"><span data-stu-id="ab1dd-155">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="ab1dd-156">在 controller 文件夹中创建一个名为 HelloController.java 的新文件。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-156">Create a new file named *HelloController.java* in the *controller* folder.</span></span> <span data-ttu-id="ab1dd-157">在文本编辑器中打开该文件，然后向其添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-157">Open the file in a text editor and add the following code to it:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   <span data-ttu-id="ab1dd-158">需要将 `com.contoso.myazuredemo` 替换为项目的包名称的地方。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-158">Where you will need to replace `com.contoso.myazuredemo` with the package name for your project.</span></span>

1. <span data-ttu-id="ab1dd-159">保存并关闭 HelloController.java 文件。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-159">Save and close the *HelloController.java* file.</span></span>

1. <span data-ttu-id="ab1dd-160">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-160">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="ab1dd-161">使用 Web 浏览器浏览到 http://localhost:8080 以测试 Web 应用，如果有可用的 Curl，也可使用如以下示例所示的语法：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-161">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="ab1dd-162">应会看到“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="ab1dd-162">You should see the "Hello World!"</span></span> <span data-ttu-id="ab1dd-163">消息在示例控制器中显示，这是从 Redis 缓存中动态检索到的。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-163">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab1dd-164">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ab1dd-164">Next steps</span></span>

<span data-ttu-id="ab1dd-165">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="ab1dd-165">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="ab1dd-166">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="ab1dd-166">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="ab1dd-167">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="ab1dd-167">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="ab1dd-168">有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-168">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="ab1dd-169">若要深入了解如何在 Azure 上开始将 Redis 缓存用于 Java，请参阅[如何将 Azure Redis 缓存用于 Java][Redis Cache with Java]。</span><span class="sxs-lookup"><span data-stu-id="ab1dd-169">For more information about getting started using Redis Cache with Java on Azure, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<!-- URL List -->

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: ./media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
