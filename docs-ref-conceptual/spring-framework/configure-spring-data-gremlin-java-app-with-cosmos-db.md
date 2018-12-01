---
title: 如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.date: 11/21/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 47251c6bca1186a400020ba38e4b6596c7c5f2f1
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339021"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a><span data-ttu-id="a0610-103">如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用</span><span class="sxs-lookup"><span data-stu-id="a0610-103">How to use the Spring Data Gremlin Starter with the Azure Cosmos DB SQL API</span></span>

## <a name="overview"></a><span data-ttu-id="a0610-104">概述</span><span class="sxs-lookup"><span data-stu-id="a0610-104">Overview</span></span>

<span data-ttu-id="a0610-105">Spring Data Gremlin Starter 为 Apache 中的 Gremlin 查询语言提供 Spring Data 支持，开发人员可对 Gremlin 兼容的任何数据存储使用这项支持。</span><span class="sxs-lookup"><span data-stu-id="a0610-105">The Spring Data Gremlin Starter provides Spring Data support for the Gremlin query language from Apache, which developers can use with any Gremlin-compatible data store.</span></span>

<span data-ttu-id="a0610-106">本文演示如何使用 Azure 门户创建可与 Gremlin API 配合使用的 Azure Cosmos DB，使用 **[Spring Initializr]** 创建自定义的 Java 应用程序，然后将 Spring Data Gremlin Starter 功能添加到自定义应用程序用于存储数据，并使用 Gremlin 从 Azure Cosmos DB 检索数据。</span><span class="sxs-lookup"><span data-stu-id="a0610-106">This article demonstrates creating an Azure Cosmos DB by using the Azure portal for use with Gremlin API, then using the **[Spring Initializr]** to create a custom java application, and then add the Spring Data Gremlin Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using Gremlin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0610-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="a0610-107">Prerequisites</span></span>

<span data-ttu-id="a0610-108">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="a0610-108">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="a0610-109">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="a0610-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="a0610-110">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="a0610-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="a0610-111">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="a0610-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="a0610-112">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="a0610-112">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="a0610-113">完成本文中的步骤需要 Spring Boot 2.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="a0610-113">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a><span data-ttu-id="a0610-114">使用 Azure 门户创建 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a0610-114">Create an Azure Cosmos DB using the Azure portal</span></span>

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a><span data-ttu-id="a0610-115">创建与 Gremlin API 配合使用的 Azure Cosmos 数据库</span><span class="sxs-lookup"><span data-stu-id="a0610-115">Create your Azure Cosmos Database for use with Gremlin API</span></span>

1. <span data-ttu-id="a0610-116">浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“创建资源”。</span><span class="sxs-lookup"><span data-stu-id="a0610-116">Browse to the Azure portal at <https://portal.azure.com/> and click **+Create a resource**.</span></span>

   ![创建资源][AZ01]

1. <span data-ttu-id="a0610-118">单击“数据库”，然后单击“Azure Cosmos DB”。</span><span class="sxs-lookup"><span data-stu-id="a0610-118">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![创建 Azure Cosmos DB][AZ02]

1. <span data-ttu-id="a0610-120">在“Azure Cosmos DB”页面上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="a0610-120">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="a0610-121">输入唯一的 **ID**，用作数据库 Gremlin URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="a0610-121">Enter a unique **ID**, which you will use as part of the Gremlin URI for your database.</span></span> <span data-ttu-id="a0610-122">例如：如果输入了 **wingtiptoysdata** 作为 **ID**，则 Gremlin URI 将是 *wingtiptoysdata.gremlin.cosmosdb.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="a0610-122">For example: if you entered **wingtiptoysdata** for the **ID**, the Gremlin URI would be *wingtiptoysdata.gremlin.cosmosdb.azure.com*.</span></span>
   * <span data-ttu-id="a0610-123">选择“Gremlin (图形)”作为 API。</span><span class="sxs-lookup"><span data-stu-id="a0610-123">Choose **Gremlin (Graph)** for the API.</span></span>
   * <span data-ttu-id="a0610-124">选择要用于数据库的订阅。</span><span class="sxs-lookup"><span data-stu-id="a0610-124">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="a0610-125">指定是否为数据库创建新的“资源组”，或选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="a0610-125">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="a0610-126">为数据库指定“位置”。</span><span class="sxs-lookup"><span data-stu-id="a0610-126">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="a0610-127">指定这些选项后，单击“创建”以创建数据库。</span><span class="sxs-lookup"><span data-stu-id="a0610-127">When you have specified these options, click **Create** to create your database.</span></span>

   ![指定 Azure Cosmos DB 选项][AZ03]

1. <span data-ttu-id="a0610-129">创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出。</span><span class="sxs-lookup"><span data-stu-id="a0610-129">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="a0610-130">在任意这些位置单击数据库可打开缓存的属性页面。</span><span class="sxs-lookup"><span data-stu-id="a0610-130">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![所有资源][AZ04]

1. <span data-ttu-id="a0610-132">当显示数据库的属性页面时，单击“访问密钥”，然后复制数据库的 URI 和访问密钥，在 Spring Boot 应用程序中会用到这些值。</span><span class="sxs-lookup"><span data-stu-id="a0610-132">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![访问密钥][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a><span data-ttu-id="a0610-134">将图形添加到 Azure Cosmos 数据库</span><span class="sxs-lookup"><span data-stu-id="a0610-134">Add a graph to your Azure Cosmos Database</span></span>

1. <span data-ttu-id="a0610-135">依次单击“数据资源管理器”、“新建图形”。</span><span class="sxs-lookup"><span data-stu-id="a0610-135">Click **Data Explorer**, and then click **New Graph**.</span></span>

   ![新建图形][AZ06]

1. <span data-ttu-id="a0610-137">显示“添加图形”后，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="a0610-137">When the **Add Graph** is displayed, enter the following information:</span></span>

   * <span data-ttu-id="a0610-138">指定数据库的唯一“数据库 ID”。</span><span class="sxs-lookup"><span data-stu-id="a0610-138">Specify a unique **Database id** for your database.</span></span>
   * <span data-ttu-id="a0610-139">指定图形的唯一“图形 ID”。</span><span class="sxs-lookup"><span data-stu-id="a0610-139">Specify a unique **Graph id** for your graph.</span></span>
   * <span data-ttu-id="a0610-140">可以选择指定“存储容量”，或者接受默认值。</span><span class="sxs-lookup"><span data-stu-id="a0610-140">You can choose to specify your **Storage capacity**, or you can accept the default.</span></span>
   * <span data-ttu-id="a0610-141">指定“吞吐量”，对于本示例，可以选择 400 个请求单位 (RU)。</span><span class="sxs-lookup"><span data-stu-id="a0610-141">Specify your **Throughput**, and for this example you can choose 400 Request Units (RUs).</span></span>
   
   <span data-ttu-id="a0610-142">指定这些选项后，单击“确定”以创建图形。</span><span class="sxs-lookup"><span data-stu-id="a0610-142">When you have specified these options, click **OK** to create your graph.</span></span>

   ![添加图形][AZ07]

1. <span data-ttu-id="a0610-144">创建图形后，可以使用“数据资源管理器”来查看它。</span><span class="sxs-lookup"><span data-stu-id="a0610-144">After your graph has been created, you can use the **Data Explorer** to view it.</span></span>

   ![图形属性界面][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="a0610-146">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="a0610-146">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="a0610-147">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="a0610-147">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="a0610-148">指定想要使用 Java 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，指定 **Spring Boot** 版本（2.0 或以上），然后单击“生成项目”。</span><span class="sxs-lookup"><span data-stu-id="a0610-148">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, specify your **Spring Boot** version with a version that is equal to or greater than 2.0, and then click **Generate Project**.</span></span>

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="a0610-150">Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：\*com.example.wintiptoysdata。</span><span class="sxs-lookup"><span data-stu-id="a0610-150">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: \*com.example.wintiptoysdata.</span></span>
   >

1. <span data-ttu-id="a0610-151">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="a0610-151">When prompted, download the project to a path on your local computer.</span></span>

   ![下载自定义 Spring Boot 项目][SI02]

1. <span data-ttu-id="a0610-153">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="a0610-153">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a><span data-ttu-id="a0610-155">将 Spring Boot 应用配置为使用 Spring Data Gremlin Starter</span><span class="sxs-lookup"><span data-stu-id="a0610-155">Configure your Spring Boot app to use the Spring Data Gremlin Starter</span></span>

1. <span data-ttu-id="a0610-156">在应用的目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-156">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   <span data-ttu-id="a0610-157">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-157">-or-</span></span>

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. <span data-ttu-id="a0610-159">在文本编辑器中打开 *pom.xml* 文件，将 Spring Data Gremlin Starter 添加到 `<dependencies>` 列表：</span><span class="sxs-lookup"><span data-stu-id="a0610-159">Open the *pom.xml* file in a text editor, and add the Spring Data Gremlin Starter to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

1. <span data-ttu-id="a0610-161">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-161">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="a0610-162">配置 Spring Boot 应用以使用 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a0610-162">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="a0610-163">找到应用的 *resources* 目录，并创建名为 *application.yml* 的新文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-163">Locate the *resources* directory of your app, and create a new file named *application.yml*.</span></span> <span data-ttu-id="a0610-164">例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-164">For example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   <span data-ttu-id="a0610-165">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-165">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![创建 application.yml 文件][RE01]

1. <span data-ttu-id="a0610-167">在文本编辑器中打开 *application.yml* 文件，将以下行添加到该文件，然后将示例值替换为数据库的相应属性：</span><span class="sxs-lookup"><span data-stu-id="a0610-167">Open the *application.yml* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   
   <span data-ttu-id="a0610-168">其中：</span><span class="sxs-lookup"><span data-stu-id="a0610-168">Where:</span></span>
   
   | <span data-ttu-id="a0610-169">字段</span><span class="sxs-lookup"><span data-stu-id="a0610-169">Field</span></span> | <span data-ttu-id="a0610-170">说明</span><span class="sxs-lookup"><span data-stu-id="a0610-170">Description</span></span> |
   |---|---|
   | `endpoint` | <span data-ttu-id="a0610-171">指定数据库的 Gremlin URI，此 URI 派生自前面在本教程中创建 Azure Cosmos DB 时指定的唯一“ID”。</span><span class="sxs-lookup"><span data-stu-id="a0610-171">Specifies the Gremlin URI for your database, which is derived from the unique **ID** that you specified when you created your Azure Cosmos DB earlier in this tutorial.</span></span> |
   | `port` | <span data-ttu-id="a0610-172">指定 TCP/IP 端口，对于 HTTPS 通信，应是 **443**。</span><span class="sxs-lookup"><span data-stu-id="a0610-172">Specifies the TCP/IP port, which should be **443** for HTTPS.</span></span> |
   | `username` | <span data-ttu-id="a0610-173">指定前面在本教程中添加图形时使用的唯一“数据库 ID”和“图形 ID”；必须使用以下语法输入这些值："/dbs/**{数据库ID}**/colls/**{图形 ID}**"。</span><span class="sxs-lookup"><span data-stu-id="a0610-173">Specifies the unique **Database id** and **Graph id** that you used when you added your graph earlier in this tutorial; this must be entered using the following syntax: "/dbs/**{Database id}**/colls/**{Graph id}**".</span></span> |
   | `password` | <span data-ttu-id="a0610-174">指定前面在本教程中复制的主要或辅助“访问密钥”。</span><span class="sxs-lookup"><span data-stu-id="a0610-174">Specifies either the primary or secondary **Access key** that you copied earlier in this tutorial.</span></span> |
   | `telemetryAllowed` | <span data-ttu-id="a0610-175">若要启用遥测，请指定 **true**；否则指定 **false**。</span><span class="sxs-lookup"><span data-stu-id="a0610-175">Specify **true** if you want to enable telemetry; otherwise, **false**.</span></span>

1. <span data-ttu-id="a0610-176">保存并关闭 application.yml 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-176">Save and close the *application.yml* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="a0610-177">添加示例代码以实现数据库的基本功能</span><span class="sxs-lookup"><span data-stu-id="a0610-177">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="a0610-178">在本部分，我们将创建所需的 Java 类，用于在数据库中存储数据。</span><span class="sxs-lookup"><span data-stu-id="a0610-178">In this section, you create the necessary Java classes for storing data in your database.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="a0610-179">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="a0610-179">Modify the main application class</span></span>

1. <span data-ttu-id="a0610-180">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-180">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   <span data-ttu-id="a0610-181">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-181">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. <span data-ttu-id="a0610-183">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="a0610-183">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoysdata;
   
   // These imports are required for the application.
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.example.wingtiptoysdata.domain.Network;
   import com.example.wingtiptoysdata.domain.Person;
   import com.example.wingtiptoysdata.domain.Relation;
   import com.example.wingtiptoysdata.repository.NetworkRepository;
   import com.example.wingtiptoysdata.repository.PersonRepository;
   import com.example.wingtiptoysdata.repository.RelationRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   @SpringBootApplication
   public class WingtiptoysdataApplication {
   
       // Define several person classes to store personal data.
       private final Person person1 = new Person("01", "Nellie Hughes", "23");
       private final Person person2 = new Person("02", "Delmar Alfred", "34");
       private final Person person3 = new Person("03", "Kelley Hunter", "45");
       private final Person person4 = new Person("04", "Roscoe Guerin", "56");
       private final Person person5 = new Person("05", "Gracie Chavez", "67");
   
       // Define relationship classes to define the relationships between some of the persons.
       private final Relation relation1 = new Relation("0102", "siblings", person1, person2);
       private final Relation relation2 = new Relation("0203", "coworkers", person2, person3);
       private final Relation relation3 = new Relation("0301", "parent", person3, person1);
       private final Relation relation4 = new Relation("0302", "parent", person3, person2);
       private final Relation relation5 = new Relation("0501", "grandparent", person5, person1);
       private final Relation relation6 = new Relation("0502", "grandparent", person5, person2);
   
       // Define the network.
       private final Network network = new Network();
   
       // Autowire the repositories and factory.
       @Autowired
       private PersonRepository personRepo;
       @Autowired
       private RelationRepository relationRepo;
       @Autowired
       private NetworkRepository networkRepo;
       @Autowired
       private GremlinFactory factory;
   
       // Run the Spring application and exit.
    public static void main(String[] args) {
           SpringApplication.run(WingtiptoysdataApplication.class, args);
           System.exit(0);
    }
   
       // Perform post-construct operations.    
       @PostConstruct
       public void setup() {
           // Delete any existing data from the database.
           this.networkRepo.deleteAll();
   
           // Add the relationship classes as edges.
           this.network.getEdges().add(this.relation1);
           this.network.getEdges().add(this.relation2);
           this.network.getEdges().add(this.relation3);
           this.network.getEdges().add(this.relation4);
           this.network.getEdges().add(this.relation5);
           this.network.getEdges().add(this.relation6);
   
           // Add the person classes as vertices.
           this.network.getVertexes().add(this.person1);
           this.network.getVertexes().add(this.person2);
           this.network.getVertexes().add(this.person3);
           this.network.getVertexes().add(this.person4);
           this.network.getVertexes().add(this.person5);
   
           // Save the network.
           this.networkRepo.save(this.network);
       }
   }
   ```

1. <span data-ttu-id="a0610-184">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-184">Save and close the main application Java file.</span></span>

### <a name="define-a-basic-class-for-storing-configuration-information"></a><span data-ttu-id="a0610-185">定义用于存储配置信息的基本类</span><span class="sxs-lookup"><span data-stu-id="a0610-185">Define a basic class for storing configuration information</span></span>

1. <span data-ttu-id="a0610-186">在应用的 package 目录下创建名为 *config* 的文件夹；例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-186">Create a folder named *config* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   <span data-ttu-id="a0610-187">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-187">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. <span data-ttu-id="a0610-188">在 *config* 目录中创建名为 *UserRepositoryConfiguration.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-188">Create a new Java file named *UserRepositoryConfiguration.java* in the *config* directory, then open the file in a text editor, and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.config;

   import com.microsoft.spring.data.gremlin.common.GremlinConfiguration;
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.microsoft.spring.data.gremlin.config.AbstractGremlinConfiguration;
   import com.microsoft.spring.data.gremlin.repository.config.EnableGremlinRepositories;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   
   @Configuration
   @EnableGremlinRepositories(basePackages = "com.example.wingtiptoysdata.repository")
   @EnableConfigurationProperties(GremlinConfiguration.class)
   @PropertySource("classpath:application.yml")
   public class UserRepositoryConfiguration extends AbstractGremlinConfiguration {
   
       @Autowired
       private GremlinConfiguration config;
   
       @Override
       public GremlinConfiguration getGremlinConfiguration() {
           return this.config;
       }
   }
   ```

1. <span data-ttu-id="a0610-189">保存并关闭 *UserRepositoryConfiguration.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-189">Save and close the *UserRepositoryConfiguration.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a><span data-ttu-id="a0610-190">定义一组类用于定义图形数据库的元素</span><span class="sxs-lookup"><span data-stu-id="a0610-190">Define a set of classes that define the elements of your graph database</span></span>

1. <span data-ttu-id="a0610-191">在应用的 package 目录下创建名为 *domain* 的文件夹；例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-191">Create a folder named *domain* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   <span data-ttu-id="a0610-192">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-192">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. <span data-ttu-id="a0610-193">在 *domain* 目录中创建名为 *Person.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-193">Create a new Java file named *Person.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Vertex;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Vertex
   @AllArgsConstructor
   @NoArgsConstructor
   public class Person {
   
       @Id
       private String id;
   
       private String name;
   
       private String age;
   }
   ```

1. <span data-ttu-id="a0610-194">保存并关闭 *Person.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-194">Save and close the *Person.java* file.</span></span>

1. <span data-ttu-id="a0610-195">在 *domain* 目录中创建名为 *Relation.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-195">Create a new Java file named *Relation.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Edge;
   import com.microsoft.spring.data.gremlin.annotation.EdgeFrom;
   import com.microsoft.spring.data.gremlin.annotation.EdgeTo;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Edge
   @AllArgsConstructor
   @NoArgsConstructor
   public class Relation {
   
       @Id
       private String id;
   
       private String name;
   
       @EdgeFrom
       private Person personFrom;
   
       @EdgeTo
       private Person personTo;
   }
   ```

1. <span data-ttu-id="a0610-196">保存并关闭 *Relation.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-196">Save and close the *Relation.java* file.</span></span>

1. <span data-ttu-id="a0610-197">在 *domain* 目录中创建名为 *Network.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-197">Create a new Java file named *Network.java* in the *domain* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.EdgeSet;
   import com.microsoft.spring.data.gremlin.annotation.Graph;
   import com.microsoft.spring.data.gremlin.annotation.VertexSet;
   import lombok.Getter;
   import org.springframework.data.annotation.Id;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Graph
   public class Network {
   
       @Id
       private String id;
   
       public Network() {
           this.edges = new ArrayList<Object>();
           this.vertexes = new ArrayList<Object>();
       }
   
       @EdgeSet
       @Getter
       private List<Object> edges;
   
       @VertexSet
       @Getter
       private List<Object> vertexes;
   }
   ```

1. <span data-ttu-id="a0610-198">保存并关闭 *Network.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-198">Save and close the *Network.java* file.</span></span>

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a><span data-ttu-id="a0610-199">定义一组类用于定义图形数据库的存储库</span><span class="sxs-lookup"><span data-stu-id="a0610-199">Define a set of classes that define the repositories for your graph database</span></span>

1. <span data-ttu-id="a0610-200">在应用的 package 目录下创建名为 *repository* 的文件夹；例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-200">Create a folder named *repository* under the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   <span data-ttu-id="a0610-201">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-201">-or-</span></span>

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. <span data-ttu-id="a0610-202">在 *repository* 目录中创建名为 *NetworkRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-202">Create a new Java file named *NetworkRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. <span data-ttu-id="a0610-203">保存并关闭 *NetworkRepository.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-203">Save and close the *NetworkRepository.java* file.</span></span>

1. <span data-ttu-id="a0610-204">在 *repository* 目录中创建名为 *PersonRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-204">Create a new Java file named *PersonRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. <span data-ttu-id="a0610-205">保存并关闭 *PersonRepository.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-205">Save and close the *PersonRepository.java* file.</span></span>

1. <span data-ttu-id="a0610-206">在 *repository* 目录中创建名为 *RelationRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="a0610-206">Create a new Java file named *RelationRepository.java* in the *repository* directory, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. <span data-ttu-id="a0610-207">保存并关闭 *RelationRepository.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="a0610-207">Save and close the *RelationRepository.java* file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="a0610-208">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="a0610-208">Build and test your app</span></span>

1. <span data-ttu-id="a0610-209">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-209">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoysdata`

   <span data-ttu-id="a0610-210">-或-</span><span class="sxs-lookup"><span data-stu-id="a0610-210">-or-</span></span>

   `cd /users/example/home/wingtiptoysdata`

1. <span data-ttu-id="a0610-211">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="a0610-211">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="a0610-212">应用程序将显示多个运行时消息，如果未出错，则你可以使用 Azure 门户来查看 Azure Cosmos DB 的内容。</span><span class="sxs-lookup"><span data-stu-id="a0610-212">Your application will display several runtime messages, and if there were no errors, you can use the Azure portal to view the contents of your Azure Cosmos DB.</span></span> <span data-ttu-id="a0610-213">为此，请在数据库的属性页上单击“数据资源管理器”，单击“执行 Gremlin 查询”，然后从结果列表中选择一个项以查看数据。</span><span class="sxs-lookup"><span data-stu-id="a0610-213">To do so, click **Data Explorer** on the properties page for your database, then click **Execute Gremlin Query**, and then select an item from the list of results to view the data.</span></span>

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a><span data-ttu-id="a0610-215">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a0610-215">Next steps</span></span>

<span data-ttu-id="a0610-216">有关 Gremlin 和图形 API 的 Azure 支持的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="a0610-216">For more information about Azure support for Gremlin and Graph API, see the following articles:</span></span>

* [<span data-ttu-id="a0610-217">Azure Cosmos DB 简介：图形 API</span><span class="sxs-lookup"><span data-stu-id="a0610-217">Introduction to Azure Cosmos DB: Graph API</span></span>](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)

* [<span data-ttu-id="a0610-218">Azure Cosmos DB Gremlin 图形支持</span><span class="sxs-lookup"><span data-stu-id="a0610-218">Azure Cosmos DB Gremlin graph support</span></span>](https://docs.microsoft.com/azure/cosmos-db/gremlin-support)

* [<span data-ttu-id="a0610-219">Azure Cosmos DB：使用 Java 和 Azure 门户创建图形数据库</span><span class="sxs-lookup"><span data-stu-id="a0610-219">Azure Cosmos DB: Create a graph database using Java and the Azure portal</span></span>](https://docs.microsoft.com/azure/cosmos-db/create-graph-java)

* [<span data-ttu-id="a0610-220">教程：使用 Gremlin 查询 Azure Cosmos DB 图形 API</span><span class="sxs-lookup"><span data-stu-id="a0610-220">Tutorial: Query Azure Cosmos DB Graph API by using Gremlin</span></span>](https://docs.microsoft.com/azure/cosmos-db/tutorial-query-graph)

* <span data-ttu-id="a0610-221">[Spring Data Gremlin Starter]</span><span class="sxs-lookup"><span data-stu-id="a0610-221">[Spring Data Gremlin Starter]</span></span>

<span data-ttu-id="a0610-222">有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="a0610-222">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="a0610-223">[Azure Cosmos DB 文档]。</span><span class="sxs-lookup"><span data-stu-id="a0610-223">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="a0610-224">[Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]</span><span class="sxs-lookup"><span data-stu-id="a0610-224">[Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]</span></span>

* <span data-ttu-id="a0610-225">[Spring Data for Azure Cosmos DB SQL API]</span><span class="sxs-lookup"><span data-stu-id="a0610-225">[Spring Data for Azure Cosmos DB SQL API]</span></span>

<span data-ttu-id="a0610-226">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="a0610-226">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="a0610-227">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="a0610-227">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="a0610-228">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="a0610-228">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="a0610-229">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="a0610-229">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="a0610-230">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0610-230">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="a0610-231">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="a0610-231">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="a0610-232">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="a0610-232">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="a0610-233">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0610-233">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>


<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ05.png
[AZ06]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ06.png
[AZ07]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ07.png
[AZ08]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ08.png

[SI01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV03.png
