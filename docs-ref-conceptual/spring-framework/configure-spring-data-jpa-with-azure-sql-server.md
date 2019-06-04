---
title: 如何将 Spring Data JPA 用于 Azure SQL 数据库
description: 了解如何将 Spring Data JPA 用于 Azure SQL 数据库。
services: sql-database
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 02b6eff059c8b7dff1c7473d0460ca44e76f6f2e
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64673956"
---
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a><span data-ttu-id="9989c-103">如何将 Spring Data JPA 用于 Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="9989c-103">How to use Spring Data JPA with Azure SQL Database</span></span>

## <a name="overview"></a><span data-ttu-id="9989c-104">概述</span><span class="sxs-lookup"><span data-stu-id="9989c-104">Overview</span></span>

<span data-ttu-id="9989c-105">本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Java 持久性 API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) 在 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="9989c-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9989c-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="9989c-106">Prerequisites</span></span>

<span data-ttu-id="9989c-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="9989c-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="9989c-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="9989c-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="9989c-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="9989c-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="9989c-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="9989c-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="9989c-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="9989c-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="9989c-112">用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。</span><span class="sxs-lookup"><span data-stu-id="9989c-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="9989c-113">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="9989c-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="9989c-114">创建 Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="9989c-114">Create an Azure SQL Database</span></span>

### <a name="create-a-sql-database-server-using-the-azure-portal"></a><span data-ttu-id="9989c-115">使用 Azure 门户创建 SQL 数据库服务器</span><span class="sxs-lookup"><span data-stu-id="9989c-115">Create a SQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="9989c-116">可以在[在 Azure 门户中创建 Azure SQL 数据库](/azure/sql-database/sql-database-get-started-portal)中阅读有关创建 Azure SQL 数据库的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="9989c-116">You can read more detailed information about creating Azure SQL databases in [Create an Azure SQL database in the Azure portal](/azure/sql-database/sql-database-get-started-portal).</span></span>

1. <span data-ttu-id="9989c-117">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="9989c-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="9989c-118">依次单击“+创建资源”、“数据库”和“SQL 数据库”。   </span><span class="sxs-lookup"><span data-stu-id="9989c-118">Click **+Create a resource**, then **Databases**, and then click **SQL Database**.</span></span>

   ![创建 SQL 数据库][SQL01]

1. <span data-ttu-id="9989c-120">指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="9989c-120">Specify the following information:</span></span>

   - <span data-ttu-id="9989c-121">**数据库名称**：为 SQL 数据库选择一个唯一名称；这将在你稍后指定的 SQL 服务器中创建。</span><span class="sxs-lookup"><span data-stu-id="9989c-121">**Database name**: Choose a unique name for your SQL database; this will be created in the SQL server that you will specify later.</span></span>
   - <span data-ttu-id="9989c-122">**订阅**：指定要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="9989c-122">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="9989c-123">**资源组**：指定是要创建新资源组，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="9989c-123">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="9989c-124">**选择源**：对于本教程，请选择 `Blank database` 以创建新数据库。</span><span class="sxs-lookup"><span data-stu-id="9989c-124">**Select source**: For this tutorial, select `Blank database` to create a new database.</span></span>

   ![指定 SQL 数据库属性][SQL02]
   
1. <span data-ttu-id="9989c-126">依次单击“服务器”、“创建新服务器”   ，然后指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="9989c-126">Click **Server**, then **Create a new server**, and then specify the following information:</span></span>

   - <span data-ttu-id="9989c-127">**服务器名称**：为 SQL 服务器选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoyssql.database.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="9989c-127">**Server name**: Choose a unique name for your SQL server; this will be used to create a fully-qualified domain name like *wingtiptoyssql.database.windows.net*.</span></span>
   - <span data-ttu-id="9989c-128">**服务器管理员登录名**：指定数据库管理员名称。</span><span class="sxs-lookup"><span data-stu-id="9989c-128">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="9989c-129">**密码**和**确认密码**：指定数据库管理员的密码。</span><span class="sxs-lookup"><span data-stu-id="9989c-129">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="9989c-130">**位置**：指定最靠近你的数据库的地理区域。</span><span class="sxs-lookup"><span data-stu-id="9989c-130">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 SQL Server][SQL03]

1. <span data-ttu-id="9989c-132">输入上述所有信息后，单击“选择”  。</span><span class="sxs-lookup"><span data-stu-id="9989c-132">When you have entered all of the above information, click **Select**.</span></span>

1. <span data-ttu-id="9989c-133">对于本教程，请指定价格最低的**定价层**，然后单击“创建”  。</span><span class="sxs-lookup"><span data-stu-id="9989c-133">For this tutorial, specify the least-expensive **Pricing tier**, and then click **Create**.</span></span>

   ![创建 SQL 数据库][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="9989c-135">使用 Azure 门户为 SQL Server 配置防火墙规则</span><span class="sxs-lookup"><span data-stu-id="9989c-135">Configure a firewall rule for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="9989c-136">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="9989c-136">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="9989c-137">单击“所有资源”  ，然后单击你刚才创建的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="9989c-137">Click **All Resources**, then click the SQL server you just created.</span></span>

   ![选择 SQL Server][SQL05]

1. <span data-ttu-id="9989c-139">在“概述”  部分中，单击“显示防火墙设置” </span><span class="sxs-lookup"><span data-stu-id="9989c-139">In the **Overview** section, click **Show firewall settings**</span></span>

   ![显示防火墙设置][SQL06]

1. <span data-ttu-id="9989c-141">在“防火墙和虚拟网络”  部分中，通过为规则指定一个唯一名称来创建新规则，输入将需要访问你的数据库的 IP 地址范围，然后单击“保存”  。</span><span class="sxs-lookup"><span data-stu-id="9989c-141">In the **Firewalls and virtual networks** section, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![配置防火墙设置][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a><span data-ttu-id="9989c-143">使用 Azure 门户检索 SQL Server 的连接字符串</span><span class="sxs-lookup"><span data-stu-id="9989c-143">Retrieve the connection string for your SQL server using the Azure Portal</span></span>

1. <span data-ttu-id="9989c-144">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="9989c-144">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="9989c-145">单击“所有资源”  ，然后单击你刚才创建的 SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="9989c-145">Click **All Resources**, then click the SQL database you just created.</span></span>

   ![选择 SQL 数据库][SQL08]

1. <span data-ttu-id="9989c-147">单击“连接字符串”  ，然后单击“JDBC”  并复制 JDBC 文本字段中的值。</span><span class="sxs-lookup"><span data-stu-id="9989c-147">Click **Connection strings**, then click **JDBC**, and copy the value in the JDBC text field.</span></span>

   ![检索 JDBC 连接字符串][SQL09]

## <a name="configure-the-sample-application"></a><span data-ttu-id="9989c-149">配置示例应用程序</span><span class="sxs-lookup"><span data-stu-id="9989c-149">Configure the sample application</span></span>

1. <span data-ttu-id="9989c-150">打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="9989c-150">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. <span data-ttu-id="9989c-151">在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。</span><span class="sxs-lookup"><span data-stu-id="9989c-151">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="9989c-152">在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：</span><span class="sxs-lookup"><span data-stu-id="9989c-152">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   <span data-ttu-id="9989c-153">其中：</span><span class="sxs-lookup"><span data-stu-id="9989c-153">Where:</span></span>

   | <span data-ttu-id="9989c-154">参数</span><span class="sxs-lookup"><span data-stu-id="9989c-154">Parameter</span></span> | <span data-ttu-id="9989c-155">说明</span><span class="sxs-lookup"><span data-stu-id="9989c-155">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="9989c-156">指定上文中所述的 SQL JDBC 字符串的编辑后版本。</span><span class="sxs-lookup"><span data-stu-id="9989c-156">Specifies an edited version of your SQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="9989c-157">指定本文上文中所述的 SQL 管理员名称，并将缩短的服务器名称追加到其末尾。</span><span class="sxs-lookup"><span data-stu-id="9989c-157">Specifies your SQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="9989c-158">指定本文上文中所述的 SQL 管理员密码。</span><span class="sxs-lookup"><span data-stu-id="9989c-158">Specifies your SQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="9989c-159">保存并关闭 application.properties 文件  。</span><span class="sxs-lookup"><span data-stu-id="9989c-159">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="9989c-160">打包并测试示例应用程序</span><span class="sxs-lookup"><span data-stu-id="9989c-160">Package and test the sample application</span></span> 

1. <span data-ttu-id="9989c-161">使用 Maven 生成示例应用程序，例如：</span><span class="sxs-lookup"><span data-stu-id="9989c-161">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P sql
   ```

1. <span data-ttu-id="9989c-162">启动示例应用程序；例如：</span><span class="sxs-lookup"><span data-stu-id="9989c-162">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="9989c-163">在命令提示符下使用 `curl` 创建新记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="9989c-163">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="9989c-164">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="9989c-164">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="9989c-165">在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="9989c-165">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="9989c-166">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="9989c-166">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="9989c-167">摘要</span><span class="sxs-lookup"><span data-stu-id="9989c-167">Summary</span></span>

<span data-ttu-id="9989c-168">在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 JPA 在 Azure SQL 数据库中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="9989c-168">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure SQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9989c-169">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9989c-169">Next steps</span></span>

<span data-ttu-id="9989c-170">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="9989c-170">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9989c-171">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="9989c-171">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="9989c-172">其他资源</span><span class="sxs-lookup"><span data-stu-id="9989c-172">Additional Resources</span></span>

<span data-ttu-id="9989c-173">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="9989c-173">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
