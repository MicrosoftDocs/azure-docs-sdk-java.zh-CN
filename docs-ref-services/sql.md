---
title: 用于 Java 的 Azure SQL 数据库库
description: 配合管理 API 使用 JDBC 驱动程序或管理 Azure SQL 数据库实例连接到 Azure SQL 数据库。
keywords: Azure, Java, SDK, API, SQL, 数据库, JDBC
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/05/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: sql-database
ms.openlocfilehash: 37f7d3caf10e6b709cee2452c63a543d49e0ead8
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823710"
---
# <a name="azure-sql-database-libraries-for-java"></a><span data-ttu-id="62a88-104">用于 Java 的 Azure SQL 数据库库</span><span class="sxs-lookup"><span data-stu-id="62a88-104">Azure SQL Database libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="62a88-105">概述</span><span class="sxs-lookup"><span data-stu-id="62a88-105">Overview</span></span>

<span data-ttu-id="62a88-106">[Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)是使用 Microsoft SQL Server 引擎的关系型数据库服务，支持表、JSON、空间和 XML 数据。</span><span class="sxs-lookup"><span data-stu-id="62a88-106">[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) is a relational database service using the Microsoft SQL Server engine that supports table, JSON, spatial, and XML data.</span></span> 

<span data-ttu-id="62a88-107">若要开始使用 Azure SQL 数据库，请参阅 [Azure SQL 数据库：使用 Java 连接和查询数据](/azure/sql-database/sql-database-connect-query-java)。</span><span class="sxs-lookup"><span data-stu-id="62a88-107">To get started with Azure SQL Database, see [Azure SQL Database: Use Java to connect and query data](/azure/sql-database/sql-database-connect-query-java).</span></span>

## <a name="client-jdbc-driver"></a><span data-ttu-id="62a88-108">客户端 JDBC 驱动程序</span><span class="sxs-lookup"><span data-stu-id="62a88-108">Client JDBC driver</span></span>

<span data-ttu-id="62a88-109">使用 [SQL 数据库 JDBC 驱动程序](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)从应用程序连接到 Azure SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="62a88-109">Connect to Azure SQL Database from your applications using the [SQL Database JDBC driver](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server).</span></span> <span data-ttu-id="62a88-110">可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接连接数据库，或使用通过 JDBC 与数据库交互的数据访问框架（例如 [Hibernate](http://hibernate.org/)）。</span><span class="sxs-lookup"><span data-stu-id="62a88-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect with the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="62a88-111">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端 JDBC 驱动程序。</span><span class="sxs-lookup"><span data-stu-id="62a88-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="62a88-112">示例</span><span class="sxs-lookup"><span data-stu-id="62a88-112">Example</span></span>

<span data-ttu-id="62a88-113">使用 JDBC 连接到 SQL 数据库并选择表中的所有记录。</span><span class="sxs-lookup"><span data-stu-id="62a88-113">Connect to SQL database and select all records in a table using JDBC.</span></span>

```java
String connectionString = "jdbc:sqlserver://fabrikam.database.windows.net:1433;database=fiber;user=raisa;password=testpass;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
try {
    Connection conn = DriverManager.getConnection(connectionString);
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery("SELECT * FROM SALES");
}  
```

## <a name="management-api"></a><span data-ttu-id="62a88-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="62a88-114">Management API</span></span>

<span data-ttu-id="62a88-115">使用管理 API 在订阅中创建和管理 Azure SQL 数据库资源。</span><span class="sxs-lookup"><span data-stu-id="62a88-115">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span>   

<span data-ttu-id="62a88-116">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="62a88-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-sql</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="62a88-117">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="62a88-117">Explore the Management APIs</span></span>](/java/api/overview/azure/sql/management)

### <a name="example"></a><span data-ttu-id="62a88-118">示例</span><span class="sxs-lookup"><span data-stu-id="62a88-118">Example</span></span>

<span data-ttu-id="62a88-119">创建 SQL 数据库资源，并使用防火墙规则限制对 IP 地址范围的访问。</span><span class="sxs-lookup"><span data-stu-id="62a88-119">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlDbName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(resourceGroupName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    .withNewFirewallRule("172.16.0.0", "172.31.255.255")
                    .create();
```

## <a name="samples"></a><span data-ttu-id="62a88-120">示例</span><span class="sxs-lookup"><span data-stu-id="62a88-120">Samples</span></span>

[!INCLUDE [java-sql-samples](../docs-ref-conceptual/includes/sql.md)]

<span data-ttu-id="62a88-121">详细了解可在应用中使用的 [Azure SQL 数据库示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL)。</span><span class="sxs-lookup"><span data-stu-id="62a88-121">Explore more [sample Java code for Azure SQL Database](https://azure.microsoft.com/resources/samples/?platform=java&term=SQL) you can use in your apps.</span></span>