---
title: 如何将 Spring Data JPA 用于 Azure PostgreSQL
description: 了解如何将 Spring Data JPA 用于 Azure PostgreSQL 数据库。
services: postgresql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 1849e03674534882a579f85b2654a628bd867487
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992023"
---
# <a name="how-to-use-spring-data-jpa-with-azure-postgresql"></a><span data-ttu-id="7d007-103">如何将 Spring Data JPA 用于 Azure PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="7d007-103">How to use Spring Data JPA with Azure PostgreSQL</span></span>

## <a name="overview"></a><span data-ttu-id="7d007-104">概述</span><span class="sxs-lookup"><span data-stu-id="7d007-104">Overview</span></span>

<span data-ttu-id="7d007-105">本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Java 持久性 API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) 在 Azure [PostgreSQL]https://www.postgresql.org/ 数据库中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="7d007-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [PostgreSQL]https://www.postgresql.org/ database using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d007-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="7d007-106">Prerequisites</span></span>

<span data-ttu-id="7d007-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="7d007-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="7d007-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="7d007-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7d007-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="7d007-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="7d007-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="7d007-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="7d007-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="7d007-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="7d007-112">用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。</span><span class="sxs-lookup"><span data-stu-id="7d007-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span> <span data-ttu-id="7d007-113">或用来测试功能的类似 HTTP 实用工具。</span><span class="sxs-lookup"><span data-stu-id="7d007-113">or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="7d007-114">[psql](https://www.postgresql.org/docs/current/app-psql.html) 命令行实用工具。</span><span class="sxs-lookup"><span data-stu-id="7d007-114">The [psql](https://www.postgresql.org/docs/current/app-psql.html) command-line utility.</span></span>
* <span data-ttu-id="7d007-115">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="7d007-115">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-postgresql-database-for-azure"></a><span data-ttu-id="7d007-116">创建 PostgreSQL database for Azure</span><span class="sxs-lookup"><span data-stu-id="7d007-116">Create a PostgreSQL database for Azure</span></span>

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="7d007-117">使用 Azure 门户创建 PostgreSQL 数据库服务器</span><span class="sxs-lookup"><span data-stu-id="7d007-117">Create a PostgreSQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="7d007-118">可以在[在 Azure 门户中创建 Azure Database for PostgreSQL 服务器](/azure/postgresql/quickstart-create-server-database-portal)中阅读有关创建 PostgreSQL 数据库的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="7d007-118">You can read more detailed information about creating PostgreSQL databases in [Create an Azure Database for PostgreSQL server by using the Azure portal](/azure/postgresql/quickstart-create-server-database-portal).</span></span>

1. <span data-ttu-id="7d007-119">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="7d007-119">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="7d007-120">依次单击“+创建资源”、“数据库”和“Azure Database for PostgreSQL”。</span><span class="sxs-lookup"><span data-stu-id="7d007-120">Click **+Create a resource**, then **Databases**, and then click **Azure Database for PostgreSQL**.</span></span>

   ![创建 PostgreSQL 数据库][POSTGRESQL01]

1. <span data-ttu-id="7d007-122">输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="7d007-122">Enter the following information:</span></span>

   - <span data-ttu-id="7d007-123">**服务器名称**：为 PostgreSQL 服务器选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoyspostgresql.postgres.database.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="7d007-123">**Server name**: Choose a unique name for your PostgreSQL server; this will be used to create a fully-qualified domain name like *wingtiptoyspostgresql.postgres.database.azure.com*.</span></span>
   - <span data-ttu-id="7d007-124">**订阅**：指定要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="7d007-124">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="7d007-125">**资源组**：指定是要创建新资源组，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="7d007-125">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="7d007-126">**选择源**：对于本教程，请选择 `Blank` 以创建新数据库。</span><span class="sxs-lookup"><span data-stu-id="7d007-126">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="7d007-127">**服务器管理员登录名**：指定数据库管理员名称。</span><span class="sxs-lookup"><span data-stu-id="7d007-127">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="7d007-128">**密码**和**确认密码**：指定数据库管理员的密码。</span><span class="sxs-lookup"><span data-stu-id="7d007-128">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="7d007-129">**位置**：指定最靠近你的数据库的地理区域。</span><span class="sxs-lookup"><span data-stu-id="7d007-129">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="7d007-130">**版本**：指定最新的数据库版本。</span><span class="sxs-lookup"><span data-stu-id="7d007-130">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="7d007-131">**定价层**：对于本教程，请指定最经济的定价层。</span><span class="sxs-lookup"><span data-stu-id="7d007-131">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![创建 PostgreSQL 数据库属性][POSTGRESQL02]

1. <span data-ttu-id="7d007-133">输入上述所有信息后，单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="7d007-133">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a><span data-ttu-id="7d007-134">使用 Azure 门户为 PostgreSQL 数据库服务器配置防火墙规则</span><span class="sxs-lookup"><span data-stu-id="7d007-134">Configure a firewall rule for your PostgreSQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="7d007-135">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="7d007-135">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="7d007-136">单击“所有资源”，然后单击你刚才创建的 PostgreSQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="7d007-136">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![选择 PostgreSQL 数据库][POSTGRESQL03]

1. <span data-ttu-id="7d007-138">单击“连接安全性”，在“防火墙规则”中通过为规则指定一个唯一名称来创建新规则，输入将需要访问你的数据库的 IP 地址范围，然后单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="7d007-138">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![配置连接安全性][POSTGRESQL04]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a><span data-ttu-id="7d007-140">使用 Azure 门户检索 PostgreSQL 服务器的连接字符串</span><span class="sxs-lookup"><span data-stu-id="7d007-140">Retrieve the connection string for your PostgreSQL server using the Azure Portal</span></span>

1. <span data-ttu-id="7d007-141">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="7d007-141">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="7d007-142">单击“所有资源”，然后单击你刚才创建的 PostgreSQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="7d007-142">Click **All Resources**, then click the PostgreSQL database you just created.</span></span>

   ![选择 PostgreSQL 数据库][POSTGRESQL03]

1. <span data-ttu-id="7d007-144">单击“连接字符串”，然后复制“JDBC”文本字段中的值。</span><span class="sxs-lookup"><span data-stu-id="7d007-144">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![检索 JDBC 连接字符串][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a><span data-ttu-id="7d007-146">使用 `psql` 命令行实用工具创建 PostgreSQL 数据库</span><span class="sxs-lookup"><span data-stu-id="7d007-146">Create PostgreSQL database using the `psql` command-line utility</span></span>

1. <span data-ttu-id="7d007-147">打开一个命令 shell，通过输入 `psql` 命令连接到 PostgreSQL 服务器，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7d007-147">Open a command shell and connect to your PostgreSQL server by entering a `psql` command like the following example:</span></span>

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   <span data-ttu-id="7d007-148">其中：</span><span class="sxs-lookup"><span data-stu-id="7d007-148">Where:</span></span>

   | <span data-ttu-id="7d007-149">参数</span><span class="sxs-lookup"><span data-stu-id="7d007-149">Parameter</span></span> | <span data-ttu-id="7d007-150">说明</span><span class="sxs-lookup"><span data-stu-id="7d007-150">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="7d007-151">指定本文上文中所述的完全限定的 PostgreSQL 服务器名称。</span><span class="sxs-lookup"><span data-stu-id="7d007-151">Specifies your fully qualified PostgreSQL server name from earlier in this article.</span></span> |
   | `host` | <span data-ttu-id="7d007-152">指定 PostgreSQL 服务器端口，默认情况下为 `5432`。</span><span class="sxs-lookup"><span data-stu-id="7d007-152">Specifies the PostgreSQL server port, which is `5432` by default.</span></span> |
   | `username` | <span data-ttu-id="7d007-153">指定本文上文中所述的 PostgreSQL 管理员和缩短的服务器名称。</span><span class="sxs-lookup"><span data-stu-id="7d007-153">Specifies your PostgreSQL administrator and shortened server name from earlier in this article.</span></span> |
   | `dbname` | <span data-ttu-id="7d007-154">现在，指定要使用默认的 `postgres` 数据库。</span><span class="sxs-lookup"><span data-stu-id="7d007-154">Specifies that you want to use the default `postgres` database for now.</span></span> |

   <span data-ttu-id="7d007-155">PostgreSQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="7d007-155">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```

1. <span data-ttu-id="7d007-156">通过输入 `psql` 命令创建名为 *mypgsqldb* 的数据库，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7d007-156">Create a database named *mypgsqldb* by entering a `psql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   <span data-ttu-id="7d007-157">PostgreSQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="7d007-157">Your PostgreSQL server should respond with a display like the following example:</span></span>

   ```shell
   CREATE DATABASE
   ```

1. <span data-ttu-id="7d007-158">可选：可以通过在 `psql` 处输入 `\l` 来验证数据库是否已创建；PostgreSQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="7d007-158">OPTIONAL: You can verify that your database was created by entering a `\l` at the `psql`; your PostgreSQL server should respond with something like the following example:</span></span>

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. <span data-ttu-id="7d007-159">输入 `\q` 以退出 `psql` 实用工具。</span><span class="sxs-lookup"><span data-stu-id="7d007-159">Enter `\q` to exit the `psql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="7d007-160">配置示例应用程序</span><span class="sxs-lookup"><span data-stu-id="7d007-160">Configure the sample application</span></span>

1. <span data-ttu-id="7d007-161">打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7d007-161">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. <span data-ttu-id="7d007-162">在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。</span><span class="sxs-lookup"><span data-stu-id="7d007-162">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="7d007-163">在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：</span><span class="sxs-lookup"><span data-stu-id="7d007-163">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   <span data-ttu-id="7d007-164">其中：</span><span class="sxs-lookup"><span data-stu-id="7d007-164">Where:</span></span>

   | <span data-ttu-id="7d007-165">参数</span><span class="sxs-lookup"><span data-stu-id="7d007-165">Parameter</span></span> | <span data-ttu-id="7d007-166">说明</span><span class="sxs-lookup"><span data-stu-id="7d007-166">Description</span></span> |
   |---|---|
   | `spring.datasource.url` | <span data-ttu-id="7d007-167">指定本文上文中所述的 PostgreSQL JDBC 字符串。</span><span class="sxs-lookup"><span data-stu-id="7d007-167">Specifies your PostgreSQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="7d007-168">指定本文上文中所述的 PostgreSQL 管理员名称，并将缩短的服务器名称追加到其末尾。</span><span class="sxs-lookup"><span data-stu-id="7d007-168">Specifies your PostgreSQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="7d007-169">指定本文上文中所述的 PostgreSQL 管理员密码。</span><span class="sxs-lookup"><span data-stu-id="7d007-169">Specifies your PostgreSQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="7d007-170">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="7d007-170">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="7d007-171">打包并测试示例应用程序</span><span class="sxs-lookup"><span data-stu-id="7d007-171">Package and test the sample application</span></span> 

1. <span data-ttu-id="7d007-172">使用 Maven 生成示例应用程序，例如：</span><span class="sxs-lookup"><span data-stu-id="7d007-172">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P postgresql
   ```

1. <span data-ttu-id="7d007-173">启动示例应用程序；例如：</span><span class="sxs-lookup"><span data-stu-id="7d007-173">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="7d007-174">在命令提示符下使用 `curl` 创建新记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7d007-174">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="7d007-175">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="7d007-175">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="7d007-176">在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7d007-176">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="7d007-177">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="7d007-177">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="7d007-178">摘要</span><span class="sxs-lookup"><span data-stu-id="7d007-178">Summary</span></span>

<span data-ttu-id="7d007-179">在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 JPA 在 Azure PostgreSQL 数据库中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="7d007-179">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure PostgreSQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d007-180">后续步骤</span><span class="sxs-lookup"><span data-stu-id="7d007-180">Next steps</span></span>

<span data-ttu-id="7d007-181">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="7d007-181">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7d007-182">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="7d007-182">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="7d007-183">其他资源</span><span class="sxs-lookup"><span data-stu-id="7d007-183">Additional Resources</span></span>

<span data-ttu-id="7d007-184">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="7d007-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[POSTGRESQL01]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-05.png
