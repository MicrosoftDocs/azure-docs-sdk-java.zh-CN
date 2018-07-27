---
title: 用于 Java 的 Azure Database for MySQL 库
description: 用于 Azure Database for MySQL 的 Java 客户端库的参考文档
keywords: Azure, Java, SDK, API, SQL, 数据库, PostGres, MySQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: mysql
ms.openlocfilehash: 72c94ef4bdad7adeae63da2efda93b52a9afef53
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931013"
---
# <a name="azure-database-for-mysql-libraries-for-java"></a>用于 Java 的 Azure Database for MySQL 库

## <a name="overview"></a>概述

[Azure Database for MySQL](/azure/sql-database/sql-database-technical-overview) 是基于开源 [MySQL](https://www.mysql.com/) Server 引擎的关系型数据库服务。 

若要开始使用 Azure Database for MySQL，请参阅[使用 Java 连接和查询数据](/azure/mysql/connect-java)。

## <a name="client-jbdc-driver"></a>客户端 JBDC 驱动程序

使用开源 [MySQL JDBC 驱动程序](https://dev.mysql.com/downloads/connector/j/)从应用程序连接到 Azure Database for MySQL。 可以使用 [Java JDBC API](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 直接连接到数据库，或使用通过 JDBC 与数据库交互的数据访问框架（例如 [Hibernate](http://hibernate.org/)）。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端 JDBC 驱动程序。  

```XML
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.42</version>
</dependency>
```   

## <a name="example"></a>示例

使用 JDBC 连接到 Azure Database for MySQL，并选择销售表中的所有记录。 可以从 Azure 门户获取数据库的 JDBC 连接字符串。

```java
String url = String.format("jdbc:mysql://fabrikamysql.mysql.database.azure.com:3306/fabrikamdb?verifyServerCertificate=true&useSSL=true&requireSSL=false");
try {
    Connection conn = DriverManager.getConnection(url, "frank@fabrikamysql", "aBcDeFgHiJkL");
    String selectSql = "SELECT * FROM SALES";
    Statement statement = conn.createStatement();
    ResultSet resultSet = statement.executeQuery(selectSql);
}
```

## <a name="samples"></a>示例

[构建 Java 和 MySQL Web 应用](/azure/app-service-web/app-service-web-tutorial-java-mysql)   
[使用 Azure CLI 设计 MySQL 数据库](/azure/mysql/tutorial-design-database-using-cli)   

详细了解可在应用中使用的 [Azure Database for MySQL 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=mysql)。
