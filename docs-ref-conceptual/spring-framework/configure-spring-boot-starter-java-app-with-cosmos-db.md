---
title: "如何将 Spring Boot Starter 与 Azure Cosmos DB DocumentDB API 配合使用"
description: "了解如何配置使用 Spring Boot Initializer 和 Azure Cosmos DB DocumentDB API 创建的应用程序。"
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Spring Starter, Cosmos DB, DocumentDB
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: a80ac6be1064e40cd0b693ac4e6c0b1a9723cfc4
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="bdfc5-104">如何将 Spring Boot Starter 与 Azure Cosmos DB DocumentDB API 配合使用</span><span class="sxs-lookup"><span data-stu-id="bdfc5-104">How to use the Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="bdfc5-105">概述</span><span class="sxs-lookup"><span data-stu-id="bdfc5-105">Overview</span></span>

<span data-ttu-id="bdfc5-106">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-106">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="bdfc5-107">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-107">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="bdfc5-108">为帮助开发人员开始使用 Spring Boot，在 <https://github.com/spring-guides/> 网站中提供了几个 Spring Boot 包。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="bdfc5-109">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="bdfc5-110">Azure Cosmos DB 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API（如 DocumentDB、MongoDB、Graph 和表 API）处理数据。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-110">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="bdfc5-111">Microsoft 的 Spring Boot Starter 允许开发人员使用 Spring Boot 应用程序，利用 DocumentDB API 可轻松地将 Spring Boot 应用程序与 Azure Cosmos DB 集成。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-111">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="bdfc5-112">本文演示如何使用 Azure 门户创建 Azure Cosmos DB，如何使用 **Spring Initializr** 创建自定义 Java 应用程序，以及如何将 Spring Boot Starter 功能添加到自定义应用程序中以使用 DocumentDB API 在 Azure Cosmos DB 中执行数据的存储和检索操作。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-112">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **Spring Initializr** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdfc5-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="bdfc5-113">Prerequisites</span></span>

<span data-ttu-id="bdfc5-114">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-114">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="bdfc5-115">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="bdfc5-116">[Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="bdfc5-117">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="bdfc5-118">使用 Azure 门户创建 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bdfc5-118">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="bdfc5-119">浏览到 Azure 门户（网址：<https://portal.azure.com/>），然后单击“+新建”。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-119">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 门户][AZ01]

1. <span data-ttu-id="bdfc5-121">单击“数据库”，然后单击“Azure Cosmos DB”。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 门户][AZ02]

1. <span data-ttu-id="bdfc5-123">在“Azure Cosmos DB”页面上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-123">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="bdfc5-124">输入一个唯一的 ID，作为数据库的 URI。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-124">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="bdfc5-125">例如：wingtiptoysdata.documents.azure.com。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="bdfc5-126">为 API 选择“SQL(文档 DB)”。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-126">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="bdfc5-127">选择要用于数据库的订阅。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-127">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="bdfc5-128">指定是否为数据库创建新的“资源组”，或选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-128">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="bdfc5-129">为数据库指定“位置”。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-129">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="bdfc5-130">指定这些选项后，单击“创建”以创建数据库。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-130">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure 门户][AZ03]

1. <span data-ttu-id="bdfc5-132">创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="bdfc5-133">在任意这些位置单击数据库可打开缓存的属性页面。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-133">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 门户][AZ04]

1. <span data-ttu-id="bdfc5-135">当显示数据库的属性页面时，单击“访问密钥”，然后复制数据库的 URI 和访问密钥，在 Spring Boot 应用程序中会用到这些值。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-135">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 门户][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="bdfc5-137">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="bdfc5-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="bdfc5-138">浏览到 https://start.spring.io/<>。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="bdfc5-139">指定希望使用 Java 生成 Maven 项目，输入应用程序的“组”名称和“项目”名称，然后单击“生成项目”按钮。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-139">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="bdfc5-141">Spring Initializr 使用“组”名称和“项目”名称创建程序包名称，例如：com.example.wintiptoys。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="bdfc5-142">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-142">When prompted, download the project to a path on your local computer.</span></span>

   ![下载自定义 Spring Boot 项目][SI02]

1. <span data-ttu-id="bdfc5-144">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="bdfc5-146">配置 Spring Boot 应用，以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="bdfc5-146">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="bdfc5-147">在应用的目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-147">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="bdfc5-148">-或-</span><span class="sxs-lookup"><span data-stu-id="bdfc5-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. <span data-ttu-id="bdfc5-150">在文本编辑器中打开 pom.xml 文件，然后将以下行添加到 `<dependencies>` 列表中：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-150">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

1. <span data-ttu-id="bdfc5-152">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-152">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="bdfc5-153">配置 Spring Boot 应用以使用 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bdfc5-153">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="bdfc5-154">在应用的“资源”目录中找到 application.properties 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-154">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="bdfc5-155">-或-</span><span class="sxs-lookup"><span data-stu-id="bdfc5-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![找到 application.properties 文件][RE01]

1. <span data-ttu-id="bdfc5-157">在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为数据库的相应属性：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-157">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![编辑 application.properties 文件][RE02]

1. <span data-ttu-id="bdfc5-159">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-159">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="bdfc5-160">添加示例代码以实现数据库的基本功能</span><span class="sxs-lookup"><span data-stu-id="bdfc5-160">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="bdfc5-161">本部分会创建两个存储用户数据的 Java 类，然后修改主应用程序类以创建用户类的实例并将其保存在数据库。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-161">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="bdfc5-162">定义存储用户数据的基本类</span><span class="sxs-lookup"><span data-stu-id="bdfc5-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="bdfc5-163">在与主应用程序 Java 文件相同的目录中创建一个名为 User.java 的新文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-163">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="bdfc5-164">在文本编辑器中打开 User.java 文件，然后将以下行添加到文件中，以定义在数据库中存储和检索值的通用用户类：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-164">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="bdfc5-165">保存并关闭 User.java 文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-165">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="bdfc5-166">定义数据存储库接口</span><span class="sxs-lookup"><span data-stu-id="bdfc5-166">Define a data repository interface</span></span>

1. <span data-ttu-id="bdfc5-167">在与主应用程序 Java 文件相同的目录中创建一个名为 UserRepository.java 的新文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-167">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="bdfc5-168">在文本编辑器中打开 UserRepository.java 文件，然后将以下行添加到文件中，以定义可扩展默认 DocumentDB 存储库接口的用户存储库接口：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-168">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="bdfc5-169">保存并关闭 UserRepository.java 文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-169">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="bdfc5-170">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="bdfc5-170">Modify the main application class</span></span>

1. <span data-ttu-id="bdfc5-171">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-171">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="bdfc5-172">-或-</span><span class="sxs-lookup"><span data-stu-id="bdfc5-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. <span data-ttu-id="bdfc5-174">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-174">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="bdfc5-175">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-175">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="bdfc5-176">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="bdfc5-176">Build and test your app</span></span>

1. <span data-ttu-id="bdfc5-177">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-177">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="bdfc5-178">-或-</span><span class="sxs-lookup"><span data-stu-id="bdfc5-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="bdfc5-179">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="bdfc5-180">你的应用程序将显示多个运行时消息，当显示 `User: testFirstName testLastName` 消息时，表示已成功在数据库中存储和检索值。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-180">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![成功地从应用程序输出][JV02]

1. <span data-ttu-id="bdfc5-182">可选：使用 Azure 门户可以从数据库的属性页面查看 Azure Cosmos DB 的内容，方法是单击“文档资源管理器”，然后从现实的列表中选择项目以查看内容。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-182">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a><span data-ttu-id="bdfc5-184">后续步骤</span><span class="sxs-lookup"><span data-stu-id="bdfc5-184">Next steps</span></span>

<span data-ttu-id="bdfc5-185">有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="bdfc5-186">[Azure Cosmos DB 文档]。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="bdfc5-187">[Azure Cosmos DB：使用 Java 和 Azure 门户生成 DocumentDB API 应用][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="bdfc5-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and the Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="bdfc5-188">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="bdfc5-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="bdfc5-189">用于 Azure 的 Spring Boot DocumenDB Starter</span><span class="sxs-lookup"><span data-stu-id="bdfc5-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="bdfc5-190">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="bdfc5-190">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="bdfc5-191">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="bdfc5-191">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="bdfc5-192">有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="bdfc5-192">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV03.png
