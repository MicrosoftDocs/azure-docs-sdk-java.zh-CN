---
title: "用于 Java 的 Azure Database for PostgreSQL 库"
description: "用于 Azure Database for PostgreSQL 的 Java 客户端库的参考文档"
keywords: "Azure, Java, SDK, API, SQL, 数据库, PostGres, PostgreSQL"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: postgresql
ms.openlocfilehash: d6fa2acb9a2f44b157ae52ba6e41f6777a43b574
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-database-for-postgresql-libraries-for-java"></a>用于 Java 的 Azure Database for PostgreSQL 库

## <a name="overview"></a>概述

[Azure Database for PostgreSQL](/azure/sql-database/sql-database-technical-overview) 是 Azure 中基于开源 [PostgreSQL](https://www.postgresql.org/) 数据库引擎的社区版本、为开发人员构建的关系型数据库服务。

若要开始使用 Azure Database for PostgreSQL，请参阅[使用 Java 连接和查询数据](/azure/postgresql/connect-java)。

## <a name="client-jdbc-driver"></a>客户端 JDBC 驱动程序

使用开源 [PostgreSQL JDBC 驱动程序](https://jdbc.postgresql.org/)从应用程序连接到 Azure Database for PostgreSQL。 可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接连接到数据库，或使用通过 JDBC 与数据库交互的数据访问框架（例如 [Hibernate](http://hibernate.org/)）。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端 JDBC 驱动程序。  

```XML
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.1.1</version>
</dependency>
```   

## <a name="example"></a>示例

使用 JDBC 连接到 Azure Database for PostgreSQL，并选择销售表中的所有记录。 可以从 Azure 门户获取数据库的 JDBC 连接字符串。

```java
String url = String.format("jdbc:postgresql://mypostgresdb.postgres.database.azure.com:5432/mydb?user=frank@mypostgresdb&password=AbCdEfGhIjK&ssl=true");
Connection connection = null;
try {
    connection = DriverManager.getConnection(url);
    String selectSql = "SELECT * FROM SALES";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a>示例

[使用 Azure CLI 设计 PostgreSQL 数据库](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-cli) 

详细了解可在应用中使用的 [Azure Database for PostgreSQL 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=postgres)。
