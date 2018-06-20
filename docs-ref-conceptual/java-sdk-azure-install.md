---
title: 面向 Java 开发人员的 Azure | Microsoft Docs
description: 适用于 Azure 的 Java SDK 和 API 参考
keywords: Azure Java, Azure Java API 参考, Azure Java 类库, Azure SDK
author: routlaw
manager: douge
ms.assetid: 7b92e776-959b-4632-8b1d-047ce1417616
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: 5c8bb4b81080461285551573eefc0d76b47b2d3d
ms.sourcegitcommit: 61030d025614b084e897809e603b2ec79900ec8d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2018
ms.locfileid: "30302545"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="1ecb4-104">用于 Java 的 Azure 库</span><span class="sxs-lookup"><span data-stu-id="1ecb4-104">Azure libraries for Java</span></span>

<span data-ttu-id="1ecb4-105">借助 Azure 库，可以通过本机接口使用 Java 应用中的 Azure 服务。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-105">Azure libraries help you consume Azure services in your Java apps using native interfaces.</span></span> <span data-ttu-id="1ecb4-106">每个库是独立的，可与其他库分开使用。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-106">Each library is independent and can be used separately from the others another.</span></span>

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [<span data-ttu-id="1ecb4-107">Azure 存储</span><span class="sxs-lookup"><span data-stu-id="1ecb4-107">Azure Storage</span></span>](#azure-storage) | [<span data-ttu-id="1ecb4-108">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="1ecb4-108">SQL Database</span></span>](#sql-database)  | [<span data-ttu-id="1ecb4-109">Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="1ecb4-109">Redis Cache</span></span>](#redis-cache)   | [<span data-ttu-id="1ecb4-110">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1ecb4-110">Azure Cosmos DB</span></span>](#cosmos-db) |
| [<span data-ttu-id="1ecb4-111">服务总线</span><span class="sxs-lookup"><span data-stu-id="1ecb4-111">Service Bus</span></span>](#servicebus)  | [<span data-ttu-id="1ecb4-112">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1ecb4-112">Azure Active Directory</span></span>](#azuread) | [<span data-ttu-id="1ecb4-113">Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ecb4-113">Key Vault</span></span>](#keyvault)  | [<span data-ttu-id="1ecb4-114">事件中心</span><span class="sxs-lookup"><span data-stu-id="1ecb4-114">Event Hub</span></span>](#eventhub)
| [<span data-ttu-id="1ecb4-115">IoT 服务</span><span class="sxs-lookup"><span data-stu-id="1ecb4-115">IoT Service</span></span>](#iotservice) | [<span data-ttu-id="1ecb4-116">IoT 设备</span><span class="sxs-lookup"><span data-stu-id="1ecb4-116">IoT Device</span></span>](#iotdevice) | [<span data-ttu-id="1ecb4-117">Data Lake</span><span class="sxs-lookup"><span data-stu-id="1ecb4-117">Data Lake</span></span>](#datalake)  | [<span data-ttu-id="1ecb4-118">AppInsights</span><span class="sxs-lookup"><span data-stu-id="1ecb4-118">AppInsights</span></span>](#appinsights) | 
| [<span data-ttu-id="1ecb4-119">批处理</span><span class="sxs-lookup"><span data-stu-id="1ecb4-119">Batch</span></span>](#batch) | [<span data-ttu-id="1ecb4-120">管理 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="1ecb4-120">Manage Azure resources</span></span>](#management) |

## <a name="install-with-maven"></a><span data-ttu-id="1ecb4-121">连同 Maven 一起安装</span><span class="sxs-lookup"><span data-stu-id="1ecb4-121">Install with Maven</span></span>

<span data-ttu-id="1ecb4-122">在 `pom.xml` 中添加一个依赖项条目，用于将库导入到 [Maven](https://maven.apache.org) 项目。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-122">Add a dependency entry in your `pom.xml` to import a library into your [Maven](https://maven.apache.org) project.</span></span>

<span data-ttu-id="1ecb4-123">例如，若要包含最新版本的[用于 Java 的 Azure 管理库](#management)：</span><span class="sxs-lookup"><span data-stu-id="1ecb4-123">For example, to include the latest version of the [Azure management libraries for Java](#management):</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

<span data-ttu-id="1ecb4-124">支持其他 Java 生成工具（例如 Gradle），但本文未提供相关的安装步骤。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-124">Other Java build tools like Gradle are supported but the install steps are not provided in this article.</span></span> <span data-ttu-id="1ecb4-125">请查看生成工具的文档，了解如何使用 Maven 导入功能。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-125">Review the documentation for your build tool on how to consume Maven imports.</span></span>

## <a name="azure-service-libraries"></a><span data-ttu-id="1ecb4-126">Azure 服务库</span><span class="sxs-lookup"><span data-stu-id="1ecb4-126">Azure service libraries</span></span>

<span data-ttu-id="1ecb4-127">集成 Azure 服务，以使用这些库将功能添加到应用。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-127">Integrate Azure services to add functionality to your apps using these libraries.</span></span> <span data-ttu-id="1ecb4-128">在 [Java 开发人员中心](https://azure.microsoft.com/develop/java)详细了解如何使用 Azure 服务生成应用。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-128">Learn more about building apps with Azure services at the [Java developer center](https://azure.microsoft.com/develop/java).</span></span>

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[<span data-ttu-id="1ecb4-129">Azure 存储</span><span class="sxs-lookup"><span data-stu-id="1ecb4-129">Azure Storage</span></span>](/azure/storage/storage-introduction)  

<span data-ttu-id="1ecb4-130">应用程序的数据存储和消息传送。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-130">Data storage and messaging for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

<span data-ttu-id="1ecb4-131">[示例](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [参考](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [发行说明](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-131">[Samples](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [Reference](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [Release Notes](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)</span></span>

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[<span data-ttu-id="1ecb4-132">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="1ecb4-132">SQL Database</span></span>](/azure/sql-database/sql-database-technical-overview)

<span data-ttu-id="1ecb4-133">适用于 Azure SQL 数据库的 JDBC 驱动程序。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-133">JDBC driver for Azure SQL Database.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

<span data-ttu-id="1ecb4-134">[示例](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [参考](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [发行说明](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-134">[Samples](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [Reference](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [Release Notes](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)</span></span>

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[<span data-ttu-id="1ecb4-135">Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="1ecb4-135">Redis Cache</span></span>](https://azure.microsoft.com/services/cache/)

<span data-ttu-id="1ecb4-136">低延迟、高性能的键值存储。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-136">Low-latency, high-performance key-value store.</span></span>

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

<span data-ttu-id="1ecb4-137">[示例](/azure/redis-cache/cache-java-get-started) | [参考](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [发行说明](https://github.com/xetorthio/jedis/releases)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-137">[Samples](/azure/redis-cache/cache-java-get-started) | [Reference](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [Release Notes](https://github.com/xetorthio/jedis/releases)</span></span>  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[<span data-ttu-id="1ecb4-138">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1ecb4-138">Azure Cosmos DB</span></span>](/azure/cosmos-db/introduction)

<span data-ttu-id="1ecb4-139">包含 JSON 文档并采用 SQL 或 JavaScript 查询语法的可缩放 NoSQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-139">Scalable NoSQL database with JSON documents and a SQL or JavaScript query syntax.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

<span data-ttu-id="1ecb4-140">[示例](/azure/cosmos-db/sql-api-java-application) | [参考](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [发行说明](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-140">[Samples](/azure/cosmos-db/sql-api-java-application) | [Reference](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [Release Notes](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)</span></span>

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="1ecb4-141">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1ecb4-141">ServiceBus</span></span>](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 <span data-ttu-id="1ecb4-142">服务总线是企业级的事务消息传送平台服务。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-142">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 <span data-ttu-id="1ecb4-143">[示例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [参考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [发行说明](https://github.com/Azure/azure-service-bus-java)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-143">[Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes](https://github.com/Azure/azure-service-bus-java)</span></span>   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[<span data-ttu-id="1ecb4-144">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1ecb4-144">Azure Active Directory</span></span>](/azure/active-directory/active-directory-whatis)   

<span data-ttu-id="1ecb4-145">应用程序的标识管理和安全登录。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-145">Identity management and secure sign-in for your applications.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
<span data-ttu-id="1ecb4-146">[示例](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [参考](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [发行说明](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-146">[Samples](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [Reference](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [Release Notes](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)</span></span>
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[<span data-ttu-id="1ecb4-147">Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ecb4-147">Key Vault</span></span>](/azure/key-vault) 

<span data-ttu-id="1ecb4-148">从应用程序安全访问密钥和机密。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-148">Safely access keys and secrets from your applications.</span></span> 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

<span data-ttu-id="1ecb4-149">[示例](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [参考](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [发行说明](https://github.com/Azure/azure-keyvault-java)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-149">[Samples](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [Reference](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [Release Notes](https://github.com/Azure/azure-keyvault-java)</span></span> 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[<span data-ttu-id="1ecb4-150">事件中心</span><span class="sxs-lookup"><span data-stu-id="1ecb4-150">Event Hub</span></span>](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
<span data-ttu-id="1ecb4-151">用于检测或 IoT 方案的高吞吐量事件和遥测处理。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-151">High-throughput event and telemetry handling for your instrumentation or IoT scenarios.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

<span data-ttu-id="1ecb4-152">[示例](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [参考](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [发行说明](https://github.com/Azure/azure-event-hubs-java)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-152">[Samples](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [Reference](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [Release Notes](https://github.com/Azure/azure-event-hubs-java)</span></span>

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[<span data-ttu-id="1ecb4-153">IoT 服务</span><span class="sxs-lookup"><span data-stu-id="1ecb4-153">IoT Service</span></span>](/azure/iot-hub/)

<span data-ttu-id="1ecb4-154">从注册到 IoT 中心的设备管理标识、发送消息和获取反馈。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-154">Manage identities, send messages, and get feedback from devices registered with your IoT hub.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
<span data-ttu-id="1ecb4-155">[示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [参考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [发行说明](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-155">[Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span></span>

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[<span data-ttu-id="1ecb4-156">IoT 设备</span><span class="sxs-lookup"><span data-stu-id="1ecb4-156">IoT Device</span></span>](/azure/iot-hub/iot-hub-devguide)

<span data-ttu-id="1ecb4-157">将消息从设备发送到 IoT 中心。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-157">Send a message to an IoT hub from your device.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

<span data-ttu-id="1ecb4-158">[示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [参考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [发行说明](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-158">[Samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [Reference](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Release Notes](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)</span></span>

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[<span data-ttu-id="1ecb4-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1ecb4-159">Data Lake Store</span></span>](/azure/data-lake-store/data-lake-store-overview)   
   
<span data-ttu-id="1ecb4-160">将任何大小和形式的数据捕获到单个位置以执行分析。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-160">Capture data of any size and shape into a single location for performing analytics.</span></span>    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

<span data-ttu-id="1ecb4-161">[示例](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [参考](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [发行说明](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-161">[Samples](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [Reference](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [Release Notes](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)</span></span>

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[<span data-ttu-id="1ecb4-162">AppInsights</span><span class="sxs-lookup"><span data-stu-id="1ecb4-162">AppInsights</span></span>](/azure/application-insights/app-insights-overview)

<span data-ttu-id="1ecb4-163">跟踪使用情况、添加遥测和监视 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-163">Track usage, add telemetry, and monitor your web apps.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

<span data-ttu-id="1ecb4-164">[示例](/azure/application-insights/app-insights-java-get-started) | [参考](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [发行说明](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-164">[Samples](/azure/application-insights/app-insights-java-get-started) | [Reference](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [Release Notes](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)</span></span>

<a name="batch"></a>

### <a name="batchazurebatch"></a>[<span data-ttu-id="1ecb4-165">批处理</span><span class="sxs-lookup"><span data-stu-id="1ecb4-165">Batch</span></span>](/azure/batch)

<span data-ttu-id="1ecb4-166">在云中有效运行大规模并行和高性能计算应用程序。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-166">Run large-scale parallel and high-performance computing applications efficiently in the cloud.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

<span data-ttu-id="1ecb4-167">[示例](https://github.com/azure/azure-batch-samples) | [参考](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [发行说明](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-167">[Samples](https://github.com/azure/azure-batch-samples) | [Reference](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [Release Notes](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)</span></span>

<a name="management"></a> 

## <a name="manage-azure-resources"></a><span data-ttu-id="1ecb4-168">管理 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="1ecb4-168">Manage Azure resources</span></span>

<span data-ttu-id="1ecb4-169">通过应用程序代码创建、更新和删除 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-169">Create, update, and delete Azure resources from your application code.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

<span data-ttu-id="1ecb4-170">[示例](https://github.com/Azure/azure-sdk-for-java#sample-code) | [参考](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [发行说明](java-sdk-azure-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-170">[Samples](https://github.com/Azure/azure-sdk-for-java#sample-code) | [Reference](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [Release Notes](java-sdk-azure-release-notes.md)</span></span>

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[<span data-ttu-id="1ecb4-171">ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1ecb4-171">ServiceBus</span></span>](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
<span data-ttu-id="1ecb4-172">服务总线是企业级的事务消息传送平台服务。</span><span class="sxs-lookup"><span data-stu-id="1ecb4-172">Service Bus is an enterprise-class, transactional messaging platform service.</span></span>

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

<span data-ttu-id="1ecb4-173">[示例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [参考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [发行说明](https://github.com/Azure/azure-service-bus-java)</span><span class="sxs-lookup"><span data-stu-id="1ecb4-173">[Samples](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [Reference](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [Release Notes](https://github.com/Azure/azure-service-bus-java)</span></span>

