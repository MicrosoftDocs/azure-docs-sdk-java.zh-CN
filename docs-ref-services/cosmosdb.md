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
ms.openlocfilehash: d8748dc6fcc0c839a8b2d2f817c0477bcac5baa2
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799883"
---
# <a name="azure-cosmos-db-libraries-for-java"></a>用于 Java 的 Azure Cosmos DB 库

## <a name="overview"></a>概述

使用 [Azure Cosmos DB](/azure/cosmos-db/introduction) 在全球分布式数据库中存储和查询键值、JSON 文档、图形和列数据。

若要开始使用 Azure Cosmos DB，请参阅 [Azure Cosmos DB：使用 Java 和 Azure 门户构建 API 应用](/azure/cosmos-db/create-sql-api-java)。

## <a name="client-library"></a>客户端库

使用 [SQL API](/azure/cosmos-db/sql-api-introduction) 客户端库连接到 Azure Cosmos DB，以使用 [SQL 查询语法](/azure/cosmos-db/sql-api-sql-query)处理 JSON 数据。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用 Cosmos DB 客户端库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

### <a name="example"></a>示例

使用 SQL 查询语法选择 Cosmos DB 中的匹配 JSON 文档。

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
> [了解客户端 API](/java/api/overview/azure/cosmosdb/client)


## <a name="samples"></a>示例

[使用 Azure Cosmos DB MongoDB API 开发 Java 应用][2]   
[使用 Azure Cosmos DB 图形 API 开发 Java 应用][3]   
[使用 Azure Cosmos DB SQL API 开发 Java 应用][4]        

详细了解可在应用中使用的 [Azure Cosmos DB 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=cosmos)。

[2]: https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started
[3]: https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started
[4]: https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started
