---
title: 如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 1d3ae6c12f32a3443f2783d0c88112746197f5be
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991541"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="e8d2b-103">如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用</span><span class="sxs-lookup"><span data-stu-id="e8d2b-103">How to use the Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="e8d2b-104">概述</span><span class="sxs-lookup"><span data-stu-id="e8d2b-104">Overview</span></span>

<span data-ttu-id="e8d2b-105">Azure Cosmos DB 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API（如 SQL、MongoDB、Graph 和表 API）处理数据。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-105">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as SQL, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="e8d2b-106">Microsoft 的 Spring Boot Starter 使开发人员可以使用 Spring Boot 应用程序，这些应用程序通过使用 SQL API 与 Azure Cosmos DB 轻松集成。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-106">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.</span></span>

<span data-ttu-id="e8d2b-107">本文演示如何使用 Azure 门户创建 Azure Cosmos DB，如何使用 **[Spring Initializr]** 创建自定义 Java 应用程序，以及如何将 Spring Boot Starter 功能添加到自定义应用程序，以使用 SQL API 在 Azure Cosmos DB 中执行数据的存储和检索操作。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-107">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the SQL API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8d2b-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="e8d2b-108">Prerequisites</span></span>

<span data-ttu-id="e8d2b-109">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-109">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="e8d2b-110">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="e8d2b-111">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="e8d2b-112">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="e8d2b-113">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-113">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="e8d2b-114">使用 Azure 门户创建 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e8d2b-114">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="e8d2b-115">浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“创建资源”。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-115">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![Azure 门户][AZ01]

1. <span data-ttu-id="e8d2b-117">单击“数据库”，然后单击“Azure Cosmos DB”。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-117">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure 门户][AZ02]

1. <span data-ttu-id="e8d2b-119">在“Azure Cosmos DB”页面上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-119">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="e8d2b-120">输入一个唯一的 ID，作为数据库的 URI。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-120">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="e8d2b-121">例如：wingtiptoysdata.documents.azure.com。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-121">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="e8d2b-122">为 API 选择 **SQL**。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-122">Choose **SQL** for the API.</span></span>
   * <span data-ttu-id="e8d2b-123">选择要用于数据库的订阅。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-123">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="e8d2b-124">指定是否为数据库创建新的“资源组”，或选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-124">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="e8d2b-125">为数据库指定“位置”。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-125">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="e8d2b-126">指定这些选项后，单击“创建”以创建数据库。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-126">When you have specified these options, click **Create** to create your database.</span></span>

   ![Azure 门户][AZ03]

1. <span data-ttu-id="e8d2b-128">创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-128">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="e8d2b-129">在任意这些位置单击数据库可打开缓存的属性页面。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-129">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![Azure 门户][AZ04]

1. <span data-ttu-id="e8d2b-131">当显示数据库的属性页面时，单击“访问密钥”，然后复制数据库的 URI 和访问密钥，在 Spring Boot 应用程序中会用到这些值。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-131">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure 门户][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="e8d2b-133">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="e8d2b-133">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="e8d2b-134">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-134">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="e8d2b-135">指定希望使用 Java 生成 Maven 项目，输入应用程序的“组”名称和“项目”名称，指定 Spring Boot 版本，然后单击“生成项目”按钮。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-135">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version, and then click the button to **Generate Project**.</span></span>

   > [!IMPORTANT]
   >
   > <span data-ttu-id="e8d2b-136">Spring Boot 2.0.n 版本中的 API 有几个重大更改，这些更改将用于完成本文中的步骤。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-136">There were several breaking changes to the APIs in Spring Boot version 2.0.n, which will be used to complete the steps in this article.</span></span> <span data-ttu-id="e8d2b-137">仍可以使用 Spring Boot 1.5.n 版本之一来完成本教程中的步骤，差异将在必要时突出显示。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-137">You can still use one of the Spring Boot 1.5.n versions to complete the steps in this tutorial, and the differences will be highlighted when necessary.</span></span>
   >

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="e8d2b-139">Spring Initializr 使用“组”名称和“项目”名称创建程序包名称，例如：com.example.wintiptoysdata。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-139">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoysdata*.</span></span>
   >

1. <span data-ttu-id="e8d2b-140">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-140">When prompted, download the project to a path on your local computer.</span></span>

   ![下载自定义 Spring Boot 项目][SI02]

1. <span data-ttu-id="e8d2b-142">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-142">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="e8d2b-144">配置 Spring Boot 应用，以使用 Azure Spring Boot Starter</span><span class="sxs-lookup"><span data-stu-id="e8d2b-144">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="e8d2b-145">在应用的目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-145">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="e8d2b-146">-或-</span><span class="sxs-lookup"><span data-stu-id="e8d2b-146">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. <span data-ttu-id="e8d2b-148">在文本编辑器中打开 pom.xml 文件，然后将以下行添加到 `<dependencies>` 列表中：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-148">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>2.0.4</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

   > [!IMPORTANT]
   >
   > <span data-ttu-id="e8d2b-150">如果使用 Spring Boot 1.5.n 版本之一来完成本教程，你将需要指定较旧的 Azure Cosmos DB 入门版；例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-150">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to specify the older version of the Azure Cosmos DB starter; for example:</span></span>
   >
   > ```xml
   > <dependency>
   >   <groupId>com.microsoft.azure</groupId>
   >   <artifactId>azure-documentdb-spring-boot-starter</artifactId>
   >   <version>0.1.4</version>
   > </dependency>
   > ```

1. <span data-ttu-id="e8d2b-151">验证 Spring Boot 版本是否是使用 Spring Initializr 创建应用程序时选择的版本；例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-151">Verify that the Spring Boot version is the version that you chose when you created your application with the Spring Initializr; for example:</span></span>

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.0.1.RELEASE</version>
      <relativePath/>
   </parent>
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="e8d2b-152">如果使用 Spring Boot 1.5.n 版本之一来完成本教程，你将需要验证版本是否正确；例如：`<version>1.5.14.RELEASE</version>`。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-152">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to verify the correct version; for example: `<version>1.5.14.RELEASE</version>`.</span></span>
   >

1. <span data-ttu-id="e8d2b-153">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-153">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="e8d2b-154">配置 Spring Boot 应用以使用 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e8d2b-154">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="e8d2b-155">在应用的“资源”目录中找到 application.properties 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-155">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   <span data-ttu-id="e8d2b-156">-或-</span><span class="sxs-lookup"><span data-stu-id="e8d2b-156">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![找到 application.properties 文件][RE01]

1. <span data-ttu-id="e8d2b-158">在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为数据库的相应属性：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-158">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![编辑 application.properties 文件][RE02]

1. <span data-ttu-id="e8d2b-160">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-160">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="e8d2b-161">添加示例代码以实现数据库的基本功能</span><span class="sxs-lookup"><span data-stu-id="e8d2b-161">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="e8d2b-162">本部分会创建两个存储用户数据的 Java 类，然后修改主应用程序类以创建用户类的实例并将其保存在数据库。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-162">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="e8d2b-163">定义存储用户数据的基本类</span><span class="sxs-lookup"><span data-stu-id="e8d2b-163">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="e8d2b-164">在与主应用程序 Java 文件相同的目录中创建一个名为 User.java 的新文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-164">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="e8d2b-165">在文本编辑器中打开 User.java 文件，然后将以下行添加到文件中，以定义在数据库中存储和检索值的通用用户类：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-165">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   // Define a generic User class.
   public class User {
      private String id;
      private String firstName;
      private String lastName;
   
      public User() {
      }
   
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
         return String.format("User: %s %s %s", id, firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="e8d2b-166">保存并关闭 User.java 文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-166">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="e8d2b-167">定义数据存储库接口</span><span class="sxs-lookup"><span data-stu-id="e8d2b-167">Define a data repository interface</span></span>

1. <span data-ttu-id="e8d2b-168">在与主应用程序 Java 文件相同的目录中创建一个名为 UserRepository.java 的新文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-168">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="e8d2b-169">在文本编辑器中打开 UserRepository.java 文件，然后将以下行添加到文件中，以定义可扩展默认 DocumentDB 存储库接口的用户存储库接口：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-169">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { } 
   ```

1. <span data-ttu-id="e8d2b-170">保存并关闭 UserRepository.java 文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-170">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="e8d2b-171">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="e8d2b-171">Modify the main application class</span></span>

1. <span data-ttu-id="e8d2b-172">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-172">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="e8d2b-173">-或-</span><span class="sxs-lookup"><span data-stu-id="e8d2b-173">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. <span data-ttu-id="e8d2b-175">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-175">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoysdata;

   // These imports are required for the application.
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   // These imports are only used to create an ID for this example.
   import java.util.Date;
   import java.text.SimpleDateFormat;

   @SpringBootApplication
   public class wingtiptoysdataApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;

      public static void main(String[] args) {
         // Execute the command line runner.
         SpringApplication.run(wingtiptoysdataApplication.class, args);
         System.exit(0);
      }

      public void run(String... args) throws Exception {
         // Create a simple date/time ID.
         SimpleDateFormat userId = new SimpleDateFormat("yyyyMMddHHmmssSSS");
         Date currentDate = new Date();

         // Create a new User class.
         final User testUser = new User(userId.format(currentDate), "Gena", "Soto");

         // For this example, remove all of the existing records.
         repository.deleteAll();

         // Save the User class to the Azure database.
         repository.save(testUser);
      
         // Retrieve the database record for the User class you just saved by ID.
         // final User result = repository.findOne(testUser.getId());
         final User result = repository.findById(testUser.getId()).get();

         // Display the results of the database record retrieval.
         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

   > [!IMPORTANT]
   >
   > <span data-ttu-id="e8d2b-176">如果使用 Spring Boot 1.5.n 版本之一来完成本教程，你将需要将 `final User result = repository.findById(testUser.getId()).get();` 语法替换为 `final User result = repository.findOne(testUser.getId());`。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-176">If you are using one of Spring Boot 1.5.n versions to complete this tutorial, you will need to replace the `final User result = repository.findById(testUser.getId()).get();` syntax with `final User result = repository.findOne(testUser.getId());`.</span></span>
   >

1. <span data-ttu-id="e8d2b-177">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-177">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="e8d2b-178">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="e8d2b-178">Build and test your app</span></span>

1. <span data-ttu-id="e8d2b-179">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-179">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="e8d2b-180">-或-</span><span class="sxs-lookup"><span data-stu-id="e8d2b-180">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="e8d2b-181">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-181">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="e8d2b-182">应用程序将显示多个运行时消息，并显示一条类似于以下示例的消息，指示已成功在数据库中存储和检索值。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-182">Your application will display several runtime messages, and it will display a message like the following examples to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ```
   User: 20170724025215132 Gena Soto
   ```

   ![成功地从应用程序输出][JV02]

1. <span data-ttu-id="e8d2b-184">可选：可以使用 Azure 门户从数据库的属性页查看 Azure Cosmos DB 的内容，方法是单击“数据资源管理器”，然后从显示的列表中选择项目以查看内容。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-184">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Data Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a><span data-ttu-id="e8d2b-186">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e8d2b-186">Next steps</span></span>

<span data-ttu-id="e8d2b-187">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-187">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e8d2b-188">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="e8d2b-188">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="e8d2b-189">其他资源</span><span class="sxs-lookup"><span data-stu-id="e8d2b-189">Additional Resources</span></span>

<span data-ttu-id="e8d2b-190">有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-190">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="e8d2b-191">[Azure Cosmos DB 文档]。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-191">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="e8d2b-192">[Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="e8d2b-192">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="e8d2b-193">[Spring Data for Azure Cosmos DB SQL API]</span><span class="sxs-lookup"><span data-stu-id="e8d2b-193">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="e8d2b-194">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="e8d2b-194">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* <span data-ttu-id="e8d2b-195">[用于 Azure 的 Spring Boot Document DB Starter]</span><span class="sxs-lookup"><span data-stu-id="e8d2b-195">[Spring Boot Document DB Starter for Azure]</span></span>

* [<span data-ttu-id="e8d2b-196">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="e8d2b-196">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="e8d2b-197">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="e8d2b-197">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="e8d2b-198">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-198">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="e8d2b-199">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-199">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="e8d2b-200">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-200">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="e8d2b-201">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-201">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="e8d2b-202">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="e8d2b-202">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[用于 Azure 的 Spring Boot Document DB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
