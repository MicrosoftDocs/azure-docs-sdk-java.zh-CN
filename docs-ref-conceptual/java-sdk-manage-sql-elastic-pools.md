---
title: "使用 Java 管理 SQL 数据库弹性池 | Microsoft Docs"
description: "使用用于 Java 的 Azure SDK 创建和配置 Azure SQL 数据库的示例代码"
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
---
# <a name="manage-azure-sql-databases-in-elastic-pools-from-your-java-applications"></a><span data-ttu-id="75c4b-103">通过 Java 应用程序管理弹性池中的 Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="75c4b-103">Manage Azure SQL databases in elastic pools from your Java applications</span></span>

<span data-ttu-id="75c4b-104">[本示例](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)创建一个包含[弹性池](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)的 SQL 数据库服务器，以便在单个计划中管理和缩放多个数据库的资源。</span><span class="sxs-lookup"><span data-stu-id="75c4b-104">[This sample](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool) creates a SQL database server with an [elastic pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to manage and scale resources for mulitple databases in a single plan.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="75c4b-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="75c4b-105">Run the sample</span></span>

<span data-ttu-id="75c4b-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="75c4b-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="75c4b-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="75c4b-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool
cd sql-database-java-manage-sql-dbs-in-elastic-pool
mvn clean compile exec:java
```

[<span data-ttu-id="75c4b-108">查看 GitHub 上的完整代码示例</span><span class="sxs-lookup"><span data-stu-id="75c4b-108">View the complete code sample on GitHub</span></span>](https://github.com/Azure-Samples/sql-database-java-manage-sql-dbs-in-elastic-pool)

## <a name="authenticate-with-azure"></a><span data-ttu-id="75c4b-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="75c4b-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-sql-database-server-with-an-elastic-pool"></a><span data-ttu-id="75c4b-110">创建包含弹性池的 SQL 数据库服务器</span><span class="sxs-lookup"><span data-stu-id="75c4b-110">Create a SQL database server with an elastic pool</span></span>

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

<span data-ttu-id="75c4b-111">请参阅 [ElasticPoolEditions 类参考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions)获取当前版本值。</span><span class="sxs-lookup"><span data-stu-id="75c4b-111">See the [ElasticPoolEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) for current edition values.</span></span> <span data-ttu-id="75c4b-112">查看 [SQL 数据库弹性池文档](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)来比较版本资源的特征。</span><span class="sxs-lookup"><span data-stu-id="75c4b-112">Review the [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) to compare edition resource characteristics.</span></span> 

## <a name="change-database-transaction-unit-dtu-settings-in-an-elastic-pool"></a><span data-ttu-id="75c4b-113">更改弹性池中的数据库事务单位 (DTU) 设置</span><span class="sxs-lookup"><span data-stu-id="75c4b-113">Change Database Transaction Unit (DTU) settings in an elastic pool</span></span>

```java
// Set an pool of 200 eDTUs to share between the databases
// and ensure that a database always has 10 DTUs available but never uses more than 50
elasticPool = elasticPool.update()
                    .withDtu(200)
                    .withDatabaseDtuMin(10)
                    .withDatabaseDtuMax(50)
                    .apply();
```

<span data-ttu-id="75c4b-114">查看 [DTU 和 eDTU 文档](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)来详细了解如何将资源分配到 SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="75c4b-114">Review the [DTUs and eDTUs documentation](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) to learn more about allocating resources to SQL databases.</span></span>

## <a name="create-a-new-database-and-add-it-to-an-elastic-pool"></a><span data-ttu-id="75c4b-115">创建新数据库并将其添加到弹性池</span><span class="sxs-lookup"><span data-stu-id="75c4b-115">Create a new database and add it to an elastic pool</span></span>

```java
// Add a newly created database to the elastic pool
SqlDatabase anotherDatabase = sqlServer.databases().define(anotherDatabaseName).create();
elasticPool.update().withExistingDatabase(anotherDatabase).apply();            
```

<span data-ttu-id="75c4b-116">API 的第一个语句会在 [S0 层](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)中创建 `anotherDatabase`。</span><span class="sxs-lookup"><span data-stu-id="75c4b-116">The API creates `anotherDatabase` at [S0 tier](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) in the first statement.</span></span> <span data-ttu-id="75c4b-117">将 `anotherDatabase` 移到弹性池会根据池的设置来分配数据库资源。</span><span class="sxs-lookup"><span data-stu-id="75c4b-117">Moving `anotherDatabase` to the elastic pool assigns the database resources based on the pool settings.</span></span>

## <a name="remove-a-database-from-an-elastic-pool"></a><span data-ttu-id="75c4b-118">从弹性池中删除数据库</span><span class="sxs-lookup"><span data-stu-id="75c4b-118">Remove a database from an elastic pool</span></span>
```java
// Assign an edition since the database resources are no longer managed in the pool 
anotherDatabase = anotherDatabase.update()
                     .withoutElasticPool()
                     .withEdition(DatabaseEditions.STANDARD)
                     .apply();
```

<span data-ttu-id="75c4b-119">请参阅 [DatabaseEditions 类参考](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions)来了解要传递给 `withEdition()` 的值。</span><span class="sxs-lookup"><span data-stu-id="75c4b-119">See the [DatabaseEditions class reference](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) for values to pass to `withEdition()`.</span></span>

## <a name="list-current-database-activities-in-an-elastic-pool"></a><span data-ttu-id="75c4b-120">列出弹性池中的当前数据库活动</span><span class="sxs-lookup"><span data-stu-id="75c4b-120">List current database activities in an elastic pool</span></span>
```java
// get a list of in-flight elastic operations on databases in the pool and log them 
for (ElasticPoolDatabaseActivity databaseActivity : elasticPool.listDatabaseActivities()) {
    System.out.println("Database " + databaseActivity.databaseName() 
        + " performing operation " + databaseActivity.operation() + 
        " with objective " + databaseActivity.requestedServiceObjective());
}
```

<span data-ttu-id="75c4b-121">数据库活动包括将数据库移入或移出弹性池，以及更新弹性池中的数据库。</span><span class="sxs-lookup"><span data-stu-id="75c4b-121">Database activities include moving a database in or out of an elastic pool and updating databases in an elastic pool.</span></span>


## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="75c4b-122">列出弹性池中的数据库</span><span class="sxs-lookup"><span data-stu-id="75c4b-122">List databases in an elastic pool</span></span>
```java
// Log a list of databases in the elastic pool 
for (SqlDatabase databaseInServer : elasticPool.listDatabases()) {
    System.out.println(databaseInServer.name());
}
```

<span data-ttu-id="75c4b-123">查看 [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) 中的方法，以便更详细地查询数据库。</span><span class="sxs-lookup"><span data-stu-id="75c4b-123">Review the methods in [com.microsoft.azure.management.sql.SqlDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) to query the databases in more detail.</span></span>

## <a name="delete-an-elastic-pool"></a><span data-ttu-id="75c4b-124">删除弹性池</span><span class="sxs-lookup"><span data-stu-id="75c4b-124">Delete an elastic pool</span></span>
```java
sqlServer.elasticPools().delete(elasticPoolName);
```

<span data-ttu-id="75c4b-125">要删除的弹性池必须是空的。</span><span class="sxs-lookup"><span data-stu-id="75c4b-125">The elastic pool must be empty before deleting it.</span></span>

## <a name="sample-code-summary"></a><span data-ttu-id="75c4b-126">示例代码摘要。</span><span class="sxs-lookup"><span data-stu-id="75c4b-126">Sample code summary.</span></span>

<span data-ttu-id="75c4b-127">本示例创建一个 SQL 服务器，该服务器包含在单个弹性池中管理的两个数据库。</span><span class="sxs-lookup"><span data-stu-id="75c4b-127">The sample creates a SQL server with two databases managed in a single elasic pool.</span></span> <span data-ttu-id="75c4b-128">会更新弹性池资源限制，再将其他数据库添加到池中。</span><span class="sxs-lookup"><span data-stu-id="75c4b-128">The elastic pool resource limits are updated, then additional databases are added to the pool.</span></span> <span data-ttu-id="75c4b-129">然后，代码会读取弹性池的配置和状态。</span><span class="sxs-lookup"><span data-stu-id="75c4b-129">The code then reads the elastic pool's configuration and state.</span></span> 

<span data-ttu-id="75c4b-130">本示例在退出之前会删除它所创建的所有资源。</span><span class="sxs-lookup"><span data-stu-id="75c4b-130">The sample deletes all resources it created before exiting.</span></span>

| <span data-ttu-id="75c4b-131">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="75c4b-131">Class used in sample</span></span> | <span data-ttu-id="75c4b-132">说明</span><span class="sxs-lookup"><span data-stu-id="75c4b-132">Notes</span></span> |
|-------|-------|
| [<span data-ttu-id="75c4b-133">SqlServer</span><span class="sxs-lookup"><span data-stu-id="75c4b-133">SqlServer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_server) | <span data-ttu-id="75c4b-134">`azure.sqlServers().define()...create()` Fluent 链在 Azure 中创建的 SQL 数据库服务器。</span><span class="sxs-lookup"><span data-stu-id="75c4b-134">SQL DB server in Azure created by `azure.sqlServers().define()...create()` fluent chain.</span></span> <span data-ttu-id="75c4b-135">提供方法来创建和使用弹性池与数据库。</span><span class="sxs-lookup"><span data-stu-id="75c4b-135">Provides methods to create and work with elastic pools and databases.</span></span> 
| [<span data-ttu-id="75c4b-136">SqlDatabase</span><span class="sxs-lookup"><span data-stu-id="75c4b-136">SqlDatabase</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_database) | <span data-ttu-id="75c4b-137">表示 SQL 数据库的客户端对象。</span><span class="sxs-lookup"><span data-stu-id="75c4b-137">Client side object representing a SQL database.</span></span> <span data-ttu-id="75c4b-138">通过 `sqlServer().define()...create()` 创建。</span><span class="sxs-lookup"><span data-stu-id="75c4b-138">Created through `sqlServer().define()...create()`.</span></span> 
| [<span data-ttu-id="75c4b-139">DatabaseEditions</span><span class="sxs-lookup"><span data-stu-id="75c4b-139">DatabaseEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._database_editions) | <span data-ttu-id="75c4b-140">在弹性池外部创建数据库或者将数据库移出弹性池时，用于设置数据库资源的常量静态字段</span><span class="sxs-lookup"><span data-stu-id="75c4b-140">Constant static fields used to set database resources when creating a database outside of an elastic pool or when moving a database out of an elastic pool</span></span>  
| [<span data-ttu-id="75c4b-141">SqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="75c4b-141">SqlElasticPool</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._sql_elastic_pool) | <span data-ttu-id="75c4b-142">通过在 Azure 中创建 SqlServer 的 Fluent 链的 `withNewElasticPool()` 部分创建。</span><span class="sxs-lookup"><span data-stu-id="75c4b-142">Created from the `withNewElasticPool()` section of the fluent chain that created the SqlServer in Azure.</span></span> <span data-ttu-id="75c4b-143">提供方法用于针对弹性池中运行的数据库以及弹性池本身设置资源限制。</span><span class="sxs-lookup"><span data-stu-id="75c4b-143">Provides methods to set resource limits for databases running in the elastic pool and for the elastic pool itself.</span></span> 
| [<span data-ttu-id="75c4b-144">ElasticPoolEditions</span><span class="sxs-lookup"><span data-stu-id="75c4b-144">ElasticPoolEditions</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_editions) | <span data-ttu-id="75c4b-145">定义弹性池可用资源的常量字段的类。</span><span class="sxs-lookup"><span data-stu-id="75c4b-145">Class of constant fields defining the resources available to an elastic pool.</span></span> <span data-ttu-id="75c4b-146">请参阅 [SQL 数据库弹性池文档](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)了解有关层的详细信息。</span><span class="sxs-lookup"><span data-stu-id="75c4b-146">See [SQL database elastic pool documentation](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) for tier details.</span></span> 
| [<span data-ttu-id="75c4b-147">ElasticPoolDatabaseActivity</span><span class="sxs-lookup"><span data-stu-id="75c4b-147">ElasticPoolDatabaseActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_database_activity) | <span data-ttu-id="75c4b-148">通过 `SqlElasticPool.listDatabaseActivities()` 检索。</span><span class="sxs-lookup"><span data-stu-id="75c4b-148">Retreived from `SqlElasticPool.listDatabaseActivities()`.</span></span> <span data-ttu-id="75c4b-149">此类型的每个对象表示针对弹性池中的数据库执行的活动。</span><span class="sxs-lookup"><span data-stu-id="75c4b-149">Each object of this type represents an activity performed on a database in the elastic pool.</span></span>
| [<span data-ttu-id="75c4b-150">ElasticPoolActivity</span><span class="sxs-lookup"><span data-stu-id="75c4b-150">ElasticPoolActivity</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.sql._elastic_pool_activity) | <span data-ttu-id="75c4b-151">通过 `SqlElasticPool.listActivities()` 在列表中检索。</span><span class="sxs-lookup"><span data-stu-id="75c4b-151">Retrieved in a List from `SqlElasticPool.listActivities()`.</span></span> <span data-ttu-id="75c4b-152">列表中的每个对象表示针对弹性池（不是弹性池中的数据库）执行的活动。</span><span class="sxs-lookup"><span data-stu-id="75c4b-152">Each of object in the list represents an activity performed on the elastic pool (not the databases in the elastic pool).</span></span>

## <a name="next-steps"></a><span data-ttu-id="75c4b-153">后续步骤</span><span class="sxs-lookup"><span data-stu-id="75c4b-153">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]