---
title: 使用 Java 管理 SQL 数据库弹性池 | Microsoft Docs
description: 使用用于 Java 的 Azure SDK 创建和配置 Azure SQL 数据库的示例代码
author: rloutlaw
manager: douge
ms.assetid: 9b461de8-46bc-4650-8e9e-59531f4e2a53
ms.topic: article
ms.service: Azure
ms.devlang: java
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 9ec0cf3083b8659fa850b798ca0ecf18b2757234
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931113"
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a>通过 Java 应用程序管理弹性池中的 Azure SQL 数据库

[本示例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)创建一个包含[弹性池](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)的 SQL 数据库服务器，以便在单个计划中管理和缩放多个数据库的资源。

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[查看 GitHub 上的完整代码示例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a>创建包含弹性池的 SQL 数据库服务器

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .withAdministratorLogin(administratorLogin)
                    .withAdministratorPassword(administratorPassword)
                    // Use ElasticPoolEditions.STANDARD as the edition
                    // and create two databases
                    .withNewElasticPool(elasticPoolName, ElasticPoolEditions.STANDARD, 
                        database1Name, database2Name)
                    .create();
```

请参阅 [ElasticPoolEditions 类参考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)获取当前版本值。 查看 [SQL 数据库弹性池文档](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)来比较版本资源的特征。 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a>更改弹性池中的数据库事务单位 (DTU) 设置

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

查看 [DTU 和 eDTU 文档](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)来详细了解如何将资源分配到 SQL 数据库。

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a>创建新数据库并将其添加到弹性池

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

API 的第一个语句会在 [S0 层](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)中创建 `anotherDatabase`。 将 `anotherDatabase` 移到弹性池会根据池的设置来分配数据库资源。

## <a name="remove-a-database-from-an-elastic-pool"></a>从弹性池中删除数据库
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

请参阅 [DatabaseEditions 类参考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)来了解要传递给 `withEdition()` 的值。

## <a name="list-current-database-activities-in-an-elastic-pool"></a>列出弹性池中的当前数据库活动
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

数据库活动包括将数据库移入或移出弹性池，以及更新弹性池中的数据库。


## <a name="list-databases-in-an-elastic-pool"></a>列出弹性池中的数据库
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

查看 [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) 中的方法，以便更详细地查询数据库。

## <a name="delete-an-elastic-pool"></a>删除弹性池
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

要删除的弹性池必须是空的。

## <a name="sample-code-summary"></a>示例代码摘要。

本示例创建一个 SQL 服务器，该服务器包含在单个弹性池中管理的两个数据库。 会更新弹性池资源限制，再将其他数据库添加到池中。 然后，代码会读取弹性池的配置和状态。 

本示例在退出之前会删除它所创建的所有资源。

| 示例中使用的类 | 说明 |
|-------|-------|
| [SqlServer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | `azure.sqlServers().define()...create()` Fluent 链在 Azure 中创建的 SQL 数据库服务器。 提供方法来创建和使用弹性池与数据库。 
| [SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | 表示 SQL 数据库的客户端对象。 通过 `sqlServer().define()...create()` 创建。 
| [DatabaseEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | 在弹性池外部创建数据库或者将数据库移出弹性池时，用于设置数据库资源的常量静态字段  
| [SqlElasticPool](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | 通过在 Azure 中创建 SqlServer 的 Fluent 链的 `withNewElasticPool()` 部分创建。 提供方法用于针对弹性池中运行的数据库以及弹性池本身设置资源限制。 
| [ElasticPoolEditions](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | 定义弹性池可用资源的常量字段的类。 请参阅 [SQL 数据库弹性池文档](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)了解有关层的详细信息。 
| [ElasticPoolDatabaseActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | 通过 `SqlElasticPool.listDatabaseActivities()` 检索。 此类型的每个对象表示针对弹性池中的数据库执行的活动。
| [ElasticPoolActivity](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | 通过 `SqlElasticPool.listActivities()` 在列表中检索。 列表中的每个对象表示针对弹性池（不是弹性池中的数据库）执行的活动。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]