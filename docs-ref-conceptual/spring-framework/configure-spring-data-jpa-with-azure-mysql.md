---
title: 如何将 Spring Data JPA 用于 Azure MySQL
description: 了解如何将 Spring Data JPA 用于 Azure MySQL 数据库。
services: mysql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 95dfc292d8f6d7e03d396afd7da4cfff0bd05b1b
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992040"
---
# <a name="how-to-use-spring-data-jpa-with-azure-mysql"></a><span data-ttu-id="84c3a-103">如何将 Spring Data JPA 用于 Azure MySQL</span><span class="sxs-lookup"><span data-stu-id="84c3a-103">How to use Spring Data JPA with Azure MySQL</span></span>

## <a name="overview"></a><span data-ttu-id="84c3a-104">概述</span><span class="sxs-lookup"><span data-stu-id="84c3a-104">Overview</span></span>

<span data-ttu-id="84c3a-105">本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Java 持久性 API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) 在 Azure [MySQL](https://www.mysql.com/) 数据库中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="84c3a-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information in an Azure [MySQL](https://www.mysql.com/) database using [Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84c3a-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="84c3a-106">Prerequisites</span></span>

<span data-ttu-id="84c3a-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="84c3a-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="84c3a-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="84c3a-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="84c3a-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="84c3a-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="84c3a-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="84c3a-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="84c3a-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="84c3a-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="84c3a-112">用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。</span><span class="sxs-lookup"><span data-stu-id="84c3a-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="84c3a-113">[mysql](https://dev.mysql.com/downloads/) 命令行实用工具。</span><span class="sxs-lookup"><span data-stu-id="84c3a-113">The [mysql](https://dev.mysql.com/downloads/) command-line utility.</span></span>
* <span data-ttu-id="84c3a-114">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="84c3a-114">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-a-mysql-database-for-azure"></a><span data-ttu-id="84c3a-115">创建 MySQL database for Azure</span><span class="sxs-lookup"><span data-stu-id="84c3a-115">Create a MySQL database for Azure</span></span>

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="84c3a-116">使用 Azure 门户创建 MySQL 数据库服务器</span><span class="sxs-lookup"><span data-stu-id="84c3a-116">Create a MySQL database server using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="84c3a-117">可以在[在 Azure 门户中创建 Azure Database for MySQL 服务器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)中阅读有关创建 MySQL 数据库的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="84c3a-117">You can read more detailed information about creating MySQL databases in [Create an Azure Database for MySQL server by using the Azure portal](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).</span></span>

1. <span data-ttu-id="84c3a-118">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="84c3a-118">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="84c3a-119">依次单击“+创建资源”、“数据库”和“Azure Database for MySQL”。</span><span class="sxs-lookup"><span data-stu-id="84c3a-119">Click **+Create a resource**, then **Databases**, and then click **Azure Database for MySQL**.</span></span>

   ![创建 MySQL 数据库][MYSQL01]

1. <span data-ttu-id="84c3a-121">输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="84c3a-121">Enter the following information:</span></span>

   - <span data-ttu-id="84c3a-122">**服务器名称**：为 MySQL 服务器选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoysmysql.mysql.database.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="84c3a-122">**Server name**: Choose a unique name for your MySQL server; this will be used to create a fully-qualified domain name like *wingtiptoysmysql.mysql.database.azure.com*.</span></span>
   - <span data-ttu-id="84c3a-123">**订阅**：指定要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="84c3a-123">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="84c3a-124">**资源组**：指定是要创建新资源组，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="84c3a-124">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="84c3a-125">**选择源**：对于本教程，请选择 `Blank` 以创建新数据库。</span><span class="sxs-lookup"><span data-stu-id="84c3a-125">**Select source**: For this tutorial, select `Blank` to create a new database.</span></span>
   - <span data-ttu-id="84c3a-126">**服务器管理员登录名**：指定数据库管理员名称。</span><span class="sxs-lookup"><span data-stu-id="84c3a-126">**Server admin login**: Specify the database administrator name.</span></span>
   - <span data-ttu-id="84c3a-127">**密码**和**确认密码**：指定数据库管理员的密码。</span><span class="sxs-lookup"><span data-stu-id="84c3a-127">**Password** and **Confirm password**: Specify the password for your database administrator.</span></span>
   - <span data-ttu-id="84c3a-128">**位置**：指定最靠近你的数据库的地理区域。</span><span class="sxs-lookup"><span data-stu-id="84c3a-128">**Location**: Specify the closest geographic region for your database.</span></span>
   - <span data-ttu-id="84c3a-129">**版本**：指定最新的数据库版本。</span><span class="sxs-lookup"><span data-stu-id="84c3a-129">**Version**: Specify the most-up-to-date database version.</span></span>
   - <span data-ttu-id="84c3a-130">**定价层**：对于本教程，请指定最经济的定价层。</span><span class="sxs-lookup"><span data-stu-id="84c3a-130">**Pricing tier**: For this tutorial, specify the least-expensive pricing tier.</span></span>

   ![创建 MySQL 数据库属性][MYSQL02]

1. <span data-ttu-id="84c3a-132">输入上述所有信息后，单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="84c3a-132">When you have entered all of the above information, click **Create**.</span></span>

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a><span data-ttu-id="84c3a-133">使用 Azure 门户为 MySQL 数据库服务器配置防火墙规则</span><span class="sxs-lookup"><span data-stu-id="84c3a-133">Configure a firewall rule for your MySQL database server using the Azure Portal</span></span>

1. <span data-ttu-id="84c3a-134">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="84c3a-134">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="84c3a-135">单击“所有资源”，然后单击你刚才创建的 MySQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="84c3a-135">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![选择 MySQL 数据库][MYSQL03]

1. <span data-ttu-id="84c3a-137">单击“连接安全性”，在“防火墙规则”中通过为规则指定一个唯一名称来创建新规则，输入将需要访问你的数据库的 IP 地址范围，然后单击“保存”。</span><span class="sxs-lookup"><span data-stu-id="84c3a-137">Click **Connection security**, and in the **Firewall rules**, create a new rule by specifying a unique name for the rule, then enter the range of IP addresses that will need access to your database, and then click **Save**.</span></span>

   ![配置连接安全性][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a><span data-ttu-id="84c3a-139">使用 Azure 门户检索 MySQL 服务器的连接字符串</span><span class="sxs-lookup"><span data-stu-id="84c3a-139">Retrieve the connection string for your MySQL server using the Azure Portal</span></span>

1. <span data-ttu-id="84c3a-140">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="84c3a-140">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="84c3a-141">单击“所有资源”，然后单击你刚才创建的 MySQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="84c3a-141">Click **All Resources**, then click the MySQL database you just created.</span></span>

   ![选择 MySQL 数据库][MYSQL03]

1. <span data-ttu-id="84c3a-143">单击“连接字符串”，然后复制“JDBC”文本字段中的值。</span><span class="sxs-lookup"><span data-stu-id="84c3a-143">Click **Connection strings**, and copy the value in the **JDBC** text field.</span></span>

   ![检索 JDBC 连接字符串][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a><span data-ttu-id="84c3a-145">使用 `mysql` 命令行实用工具创建 MySQL 数据库</span><span class="sxs-lookup"><span data-stu-id="84c3a-145">Create MySQL database using the `mysql` command-line utility</span></span>

1. <span data-ttu-id="84c3a-146">打开一个命令 shell，通过输入 `mysql` 命令连接到 MySQL 服务器，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-146">Open a command shell and connect to your MySQL server by entering a `mysql` command like the following example:</span></span>

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   <span data-ttu-id="84c3a-147">其中：</span><span class="sxs-lookup"><span data-stu-id="84c3a-147">Where:</span></span>

   | <span data-ttu-id="84c3a-148">参数</span><span class="sxs-lookup"><span data-stu-id="84c3a-148">Parameter</span></span> | <span data-ttu-id="84c3a-149">说明</span><span class="sxs-lookup"><span data-stu-id="84c3a-149">Description</span></span> |
   |---|---|
   | `host` | <span data-ttu-id="84c3a-150">指定本文上文中所述的完全限定的 MySQL 服务器名称。</span><span class="sxs-lookup"><span data-stu-id="84c3a-150">Specifies your fully qualified MySQL server name from earlier in this article.</span></span> |
   | `user` | <span data-ttu-id="84c3a-151">指定本文上文中所述的 MySQL 管理员和缩短的服务器名称。</span><span class="sxs-lookup"><span data-stu-id="84c3a-151">Specifies your MySQL administrator and shortened server name from earlier in this article.</span></span> |
   | `p` | <span data-ttu-id="84c3a-152">指定等待至提示输入密码。</span><span class="sxs-lookup"><span data-stu-id="84c3a-152">Specifies to wait until prompted for a password.</span></span> |


   <span data-ttu-id="84c3a-153">MySQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="84c3a-153">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

1. <span data-ttu-id="84c3a-154">通过输入 `mysql` 命令创建名为 *mysqldb* 的数据库，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-154">Create a database named *mysqldb* by entering a `mysql` command like the following example:</span></span>

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   <span data-ttu-id="84c3a-155">MySQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="84c3a-155">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. <span data-ttu-id="84c3a-156">可选：可以通过输入 `mysql` 命令验证数据库是否已创建，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-156">OPTIONAL: You can verify that your database was created by entering a `mysql` command like the following example:</span></span>

   ```SQL
   SHOW DATABASES;
   ```

   <span data-ttu-id="84c3a-157">MySQL 服务器应当如以下示例所示进行响应：</span><span class="sxs-lookup"><span data-stu-id="84c3a-157">Your MySQL server should respond with a display like the following example:</span></span>

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. <span data-ttu-id="84c3a-158">输入 `\q` 以退出 `mysql` 实用工具。</span><span class="sxs-lookup"><span data-stu-id="84c3a-158">Enter `\q` to exit the `mysql` utility.</span></span>

## <a name="configure-the-sample-application"></a><span data-ttu-id="84c3a-159">配置示例应用程序</span><span class="sxs-lookup"><span data-stu-id="84c3a-159">Configure the sample application</span></span>

1. <span data-ttu-id="84c3a-160">打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-160">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. <span data-ttu-id="84c3a-161">在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。</span><span class="sxs-lookup"><span data-stu-id="84c3a-161">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="84c3a-162">在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：</span><span class="sxs-lookup"><span data-stu-id="84c3a-162">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   <span data-ttu-id="84c3a-163">其中：</span><span class="sxs-lookup"><span data-stu-id="84c3a-163">Where:</span></span>

   | <span data-ttu-id="84c3a-164">参数</span><span class="sxs-lookup"><span data-stu-id="84c3a-164">Parameter</span></span> | <span data-ttu-id="84c3a-165">说明</span><span class="sxs-lookup"><span data-stu-id="84c3a-165">Description</span></span> |
   |---|---|
   | `spring.jpa.database-platform` | <span data-ttu-id="84c3a-166">指定 JPA 数据库平台。</span><span class="sxs-lookup"><span data-stu-id="84c3a-166">Specifies the JPA database platform.</span></span> |
   | `spring.datasource.url` | <span data-ttu-id="84c3a-167">指定本文上文中所述的 MySQL JDBC 字符串。</span><span class="sxs-lookup"><span data-stu-id="84c3a-167">Specifies your MySQL JDBC string from earlier in this article.</span></span> |
   | `spring.datasource.username` | <span data-ttu-id="84c3a-168">指定本文上文中所述的 MySQL 管理员名称，并将缩短的服务器名称追加到其末尾。</span><span class="sxs-lookup"><span data-stu-id="84c3a-168">Specifies your MySQL administrator name from earlier in this article, with the shortened server name appended to it.</span></span> |
   | `spring.datasource.password` | <span data-ttu-id="84c3a-169">指定本文上文中所述的 MySQL 管理员密码。</span><span class="sxs-lookup"><span data-stu-id="84c3a-169">Specifies your MySQL administrator password from earlier in this article.</span></span> |

1. <span data-ttu-id="84c3a-170">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="84c3a-170">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="84c3a-171">打包并测试示例应用程序</span><span class="sxs-lookup"><span data-stu-id="84c3a-171">Package and test the sample application</span></span> 

1. <span data-ttu-id="84c3a-172">使用 Maven 生成示例应用程序，例如：</span><span class="sxs-lookup"><span data-stu-id="84c3a-172">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package -P mysql
   ```

1. <span data-ttu-id="84c3a-173">启动示例应用程序；例如：</span><span class="sxs-lookup"><span data-stu-id="84c3a-173">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-jpa-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="84c3a-174">在命令提示符下使用 `curl` 创建新记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-174">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="84c3a-175">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="84c3a-175">Your application should return values like the following:</span></span>

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. <span data-ttu-id="84c3a-176">在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="84c3a-176">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="84c3a-177">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="84c3a-177">Your application should return values like the following:</span></span>

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="84c3a-178">摘要</span><span class="sxs-lookup"><span data-stu-id="84c3a-178">Summary</span></span>

<span data-ttu-id="84c3a-179">在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 JPA 在 Azure MySQL 数据库中存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="84c3a-179">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information in an Azure MySQL database using JPA.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84c3a-180">后续步骤</span><span class="sxs-lookup"><span data-stu-id="84c3a-180">Next steps</span></span>

<span data-ttu-id="84c3a-181">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="84c3a-181">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="84c3a-182">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="84c3a-182">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="84c3a-183">其他资源</span><span class="sxs-lookup"><span data-stu-id="84c3a-183">Additional Resources</span></span>

<span data-ttu-id="84c3a-184">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="84c3a-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[MYSQL01]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jpa-with-azure-mysql/create-mysql-05.png
