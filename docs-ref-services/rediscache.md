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
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="redis-cache-libraries-for-java"></a><span data-ttu-id="e9526-104">用于 Java 的 Redis 缓存库</span><span class="sxs-lookup"><span data-stu-id="e9526-104">Redis Cache libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e9526-105">概述</span><span class="sxs-lookup"><span data-stu-id="e9526-105">Overview</span></span>

<span data-ttu-id="e9526-106">Azure Redis 缓存是基于流行开源 [Redis](https://redis.io/) 缓存的安全分布式键值存储。</span><span class="sxs-lookup"><span data-stu-id="e9526-106">Azure Redis Cache is a secure, distributed key-value store based on the popular open source [Redis](https://redis.io/) cache.</span></span> 

<span data-ttu-id="e9526-107">若要开始使用 Azure Redis 缓存，请参阅[如何将 Azure Redis 缓存与 Java 配合使用](/azure/redis-cache/cache-java-get-started)。</span><span class="sxs-lookup"><span data-stu-id="e9526-107">To get started with Azure Redis Cache, see [How to use Azure Redis Cache with Java](/azure/redis-cache/cache-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="e9526-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="e9526-108">Client library</span></span>

<span data-ttu-id="e9526-109">使用开源 [Jedis](https://github.com/xetorthio/jedis) 客户端连接到 Azure Redis 缓存并在缓存中存储和检索值。</span><span class="sxs-lookup"><span data-stu-id="e9526-109">Connect to Azure Redis Cache and store and retrieve values from the cache using the open-source [Jedis](https://github.com/xetorthio/jedis) client.</span></span>  

<span data-ttu-id="e9526-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="e9526-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

## <a name="example"></a><span data-ttu-id="e9526-111">示例</span><span class="sxs-lookup"><span data-stu-id="e9526-111">Example</span></span>

<span data-ttu-id="e9526-112">连接到 Azure Redis 并在缓存中插入字符串。</span><span class="sxs-lookup"><span data-stu-id="e9526-112">Connect to Azure Redis and insert a string into the cache.</span></span>

```java
JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */
    Jedis jedis = new Jedis(shardInfo);
    jedis.set("foo", "bar");
```

## <a name="management-api"></a><span data-ttu-id="e9526-113">管理 API</span><span class="sxs-lookup"><span data-stu-id="e9526-113">Management API</span></span>

<span data-ttu-id="e9526-114">使用管理 API 创建和缩放 Azure Redis 资源及管理访问密钥。</span><span class="sxs-lookup"><span data-stu-id="e9526-114">Create and scale Azure Redis resources and manage access keys to with the management API.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-redis</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="e9526-115">示例</span><span class="sxs-lookup"><span data-stu-id="e9526-115">Example</span></span>

<span data-ttu-id="e9526-116">在[双节点标准层](https://azure.microsoft.com/services/cache/)中创建新的 Azure Redis 缓存。</span><span class="sxs-lookup"><span data-stu-id="e9526-116">Create a new Azure Redis Cache in the [two-node standard tier](https://azure.microsoft.com/services/cache/).</span></span> 

```java
RedisCache cache = azure.redisCaches().define(redisCacheName1)
    .withRegion(Region.US_CENTRAL)
    .withNewResourceGroup(rgName)
    .withStandardSku();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9526-117">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="e9526-117">Explore the Management APIs</span></span>](/java/api/overview/azure/rediscache/management)

## <a name="samples"></a><span data-ttu-id="e9526-118">示例</span><span class="sxs-lookup"><span data-stu-id="e9526-118">Samples</span></span>

[<span data-ttu-id="e9526-119">管理 Azure Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="e9526-119">Manage Azure Redis Cache</span></span>](https://github.com/Azure-Samples/redis-java-manage-cache)   

<span data-ttu-id="e9526-120">详细了解可在应用中使用的 [Azure Redis 缓存示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=redis)。</span><span class="sxs-lookup"><span data-stu-id="e9526-120">Explore more [sample Java code for Azure Redis Cache](https://azure.microsoft.com/resources/samples/?platform=java&term=redis) you can use in your apps.</span></span>
