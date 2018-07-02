---
title: 用于 Java 的 Azure Cosmos DB 库
description: 用于 Azure Cosmos DB 的 Java 客户端库的参考文档
keywords: Azure, Java, SDK, API, SQL, 数据库, MongoDB, Cosmos DB, NoSQL
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/10/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cosmosdb
ms.openlocfilehash: 6fc9f90cb3c8130aa82b20554a94a8b5ab78c083
ms.sourcegitcommit: 33c70f921f1524c8bdb69ad5a1a3c1b4b1de97ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026789"
---
# <a name="azure-cosmos-db-libraries-for-java"></a><span data-ttu-id="5661a-104">用于 Java 的 Azure Cosmos DB 库</span><span class="sxs-lookup"><span data-stu-id="5661a-104">Azure Cosmos DB libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="5661a-105">概述</span><span class="sxs-lookup"><span data-stu-id="5661a-105">Overview</span></span>

<span data-ttu-id="5661a-106">使用 [Azure Cosmos DB](/azure/cosmos-db/introduction) 在全球分布式数据库中存储和查询键值、JSON 文档、图形和列数据。</span><span class="sxs-lookup"><span data-stu-id="5661a-106">Store and query key-value, JSON document, graph, and columnar data in a globally distributed database with [Azure Cosmos DB](/azure/cosmos-db/introduction).</span></span>

<span data-ttu-id="5661a-107">若要开始使用 Azure Cosmos DB，请参阅 [Azure Cosmos DB：使用 Java 和 Azure 门户构建 API 应用](/azure/cosmos-db/create-sql-api-java)。</span><span class="sxs-lookup"><span data-stu-id="5661a-107">To get started with Azure Cosmos DB, see [Azure Cosmos DB: Build an API app with Java and the Azure portal](/azure/cosmos-db/create-sql-api-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="5661a-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="5661a-108">Client library</span></span>

<span data-ttu-id="5661a-109">使用 [SQL API](/azure/cosmos-db/sql-api-introduction) 客户端库连接到 Azure Cosmos DB，以使用 [SQL 查询语法](/azure/cosmos-db/sql-api-sql-query)处理 JSON 数据。</span><span class="sxs-lookup"><span data-stu-id="5661a-109">Connect to Azure Cosmos DB using the [SQL API](/azure/cosmos-db/sql-api-introduction) client library to work with JSON data with [SQL query syntax](/azure/cosmos-db/sql-api-sql-query).</span></span>

<span data-ttu-id="5661a-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用 Cosmos DB 客户端库。</span><span class="sxs-lookup"><span data-stu-id="5661a-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the Cosmos DB client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="5661a-111">示例</span><span class="sxs-lookup"><span data-stu-id="5661a-111">Example</span></span>

<span data-ttu-id="5661a-112">使用 SQL 查询语法选择 Cosmos DB 中的匹配 JSON 文档。</span><span class="sxs-lookup"><span data-stu-id="5661a-112">Select matching JSON documents in Cosmos DB using SQL query syntax.</span></span>

```java
DocumentClient client = new DocumentClient("https://contoso.documents.azure.com:443",
                "contosoCosmosDBKey", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);

List<Document> results = client.queryDocuments("dbs/" + DATABASE_ID + "/colls/" + COLLECTION_ID,
        "SELECT * FROM myCollection WHERE myCollection.email = 'allen [at] contoso.com'",
        null)
    .getQueryIterable()
    .toList();

```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5661a-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="5661a-113">Explore the Client APIs</span></span>](/java/api/overview/azure/cosmosdb/client)


## <a name="samples"></a><span data-ttu-id="5661a-114">示例</span><span class="sxs-lookup"><span data-stu-id="5661a-114">Samples</span></span>

<span data-ttu-id="5661a-115">[使用 Azure Cosmos DB MongoDB API 开发 Java 应用][2] </span><span class="sxs-lookup"><span data-stu-id="5661a-115">[Develop a Java app using Azure Cosmos DB MongoDB API][2] </span></span>  
<span data-ttu-id="5661a-116">[使用 Azure Cosmos DB 图形 API 开发 Java 应用][3] </span><span class="sxs-lookup"><span data-stu-id="5661a-116">[Develop a Java app using Azure Cosmos DB Graph API][3] </span></span>  
<span data-ttu-id="5661a-117">[使用 Azure Cosmos DB SQL API 开发 Java 应用][4]</span><span class="sxs-lookup"><span data-stu-id="5661a-117">[Develop a Java app using Azure Cosmos DB SQL API][4]</span></span>        

<span data-ttu-id="5661a-118">详细了解可在应用中使用的 [Azure Cosmos DB 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)。</span><span class="sxs-lookup"><span data-stu-id="5661a-118">Explore more [sample Java code for Azure Cosmos DB](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos) you can use in your apps.</span></span>

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
