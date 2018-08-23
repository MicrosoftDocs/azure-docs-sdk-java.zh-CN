---
title: 用于 Java 的 Azure Database for MySQL 库
description: 用于 Azure Database for MySQL 的 Java 客户端库的参考文档
keywords: Azure, Java, SDK, API, SQL, 数据库, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: f3c0b84a8c6577e5a844c4084b3d9cfaf3a52323
ms.sourcegitcommit: 1b22376e4ceb3d2f2734c8fc80823a44cc5fe8fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2018
ms.locfileid: "42703319"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a><span data-ttu-id="211a2-104">用于 Java 的 Azure Database for MySQL 库</span><span class="sxs-lookup"><span data-stu-id="211a2-104">Azure Database for MySQL libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="211a2-105">概述</span><span class="sxs-lookup"><span data-stu-id="211a2-105">Overview</span></span>

<span data-ttu-id="211a2-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) 是基于开源 [MySQL](https://www.mysql.com/) Server 引擎的关系型数据库服务。</span><span class="sxs-lookup"><span data-stu-id="211a2-106">[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) is a relational database service based on the open source [MySQL](https://www.mysql.com/) Server engine.</span></span> 

<span data-ttu-id="211a2-107">若要开始使用 Azure Database for MySQL，请参阅[使用 Java 连接和查询数据](/azure/mysql/connect-java)。</span><span class="sxs-lookup"><span data-stu-id="211a2-107">To get started with Azure Database for MySQL, see [Use Java to connect and query data](/azure/mysql/connect-java).</span></span>

## <a name="client-jbdc-driver"></a><span data-ttu-id="211a2-108">客户端 JBDC 驱动程序</span><span class="sxs-lookup"><span data-stu-id="211a2-108">Client JBDC driver</span></span>

<span data-ttu-id="211a2-109">使用开源 [MySQL JDBC 驱动程序](https://dev.mysql.com/downloads/connector/j/)从应用程序连接到 Azure Database for MySQL。</span><span class="sxs-lookup"><span data-stu-id="211a2-109">Connect to Azure Database for MySQL from your applications using the open-source [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).</span></span> <span data-ttu-id="211a2-110">可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接连接到数据库，或使用通过 JDBC 与数据库交互的数据访问框架（例如 [Hibernate](http://hibernate.org/)）。</span><span class="sxs-lookup"><span data-stu-id="211a2-110">You can use the [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) to directly connect to the database or use data access frameworks that interact with the database through JDBC such as [Hibernate](http://hibernate.org/).</span></span>

<span data-ttu-id="211a2-111">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端 JDBC 驱动程序。</span><span class="sxs-lookup"><span data-stu-id="211a2-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client JDBC driver in your project.</span></span>  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="211a2-112">示例</span><span class="sxs-lookup"><span data-stu-id="211a2-112">Example</span></span>

<span data-ttu-id="211a2-113">使用 JDBC 连接到 Azure Database for MySQL，并选择销售表中的所有记录。</span><span class="sxs-lookup"><span data-stu-id="211a2-113">Connect to Azure Database for MySQL using JDBC and select all records in the sales table.</span></span> <span data-ttu-id="211a2-114">可以从 Azure 门户获取数据库的 JDBC 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="211a2-114">You can get the JDBC connection string for the database from the Azure Portal.</span></span>

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a><span data-ttu-id="211a2-115">示例</span><span class="sxs-lookup"><span data-stu-id="211a2-115">Samples</span></span>

<span data-ttu-id="211a2-116">[构建 Java 和 MySQL Web 应用](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span><span class="sxs-lookup"><span data-stu-id="211a2-116">[Build a Java and MySQL web app](/azure/app-service-web/app-service-web-tutorial-java-mysql) </span></span>  
[<span data-ttu-id="211a2-117">使用 Azure CLI 设计 MySQL 数据库</span><span class="sxs-lookup"><span data-stu-id="211a2-117">Design a MySQL database using the Azure CLI</span></span>](/azure/mysql/tutorial-design-database-using-cli)   

<span data-ttu-id="211a2-118">详细了解可在应用中使用的 [Azure Database for MySQL 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)。</span><span class="sxs-lookup"><span data-stu-id="211a2-118">Explore more [sample Java code for Azure Database for MySQL](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql) you can use in your apps.</span></span>
