---
title: 用于 Java 的 Redis 缓存库
description: 用于 Redis 缓存的 Java 客户端和管理库的参考文档
keywords: Azure, Java, SDK, API, 缓存, redis, web 缓存, 键值, 内存中
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: redis-cache
ms.openlocfilehash: dd03825d9ae7cba32087f92262d5ef213cf3af0b
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892768"
---
# <a name="redis-cache-libraries-for-java"></a>用于 Java 的 Redis 缓存库

## <a name="overview"></a>概述

Azure Redis 缓存是基于流行开源 [Redis](https://redis.io/) 缓存的安全分布式键值存储。 

若要开始使用 Azure Redis 缓存，请参阅[如何将 Azure Redis 缓存与 Java 配合使用](/azure/redis-cache/cache-java-get-started)。

## <a name="client-library"></a>客户端库

使用开源 [Jedis](https://github.com/xetorthio/jedis) 客户端连接到 Azure Redis 缓存并在缓存中存储和检索值。  

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a>示例

连接到 Azure Redis 并在缓存中插入字符串。

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a>管理 API

使用管理 API 创建和缩放 Azure Redis 资源及管理访问密钥。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>示例

在[双节点标准层](https://azure.microsoft.com/services/cache/)中创建新的 Azure Redis 缓存。 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/rediscache/management)

## <a name="samples"></a>示例

[管理 Azure Redis 缓存](https://github.com/Azure-Samples/redis-java-manage-cache)   

详细了解可在应用中使用的 [Azure Redis 缓存示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)。
