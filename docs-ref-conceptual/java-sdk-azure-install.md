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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892528"
---
# <a name="azure-libraries-for-java"></a>用于 Java 的 Azure 库

借助 Azure 库，可以通过本机接口使用 Java 应用中的 Azure 服务。 每个库是独立的，可与其他库分开使用。

| | | | |
|:-------------:|:----------:|:----:|:---:|
| [Azure 存储](#azure-storage) | [SQL 数据库](#sql-database)  | [Redis 缓存](#redis-cache)   | [Azure Cosmos DB](#cosmos-db) |
| [服务总线](#servicebus)  | [Azure Active Directory](#azuread) | [Key Vault](#keyvault)  | [事件中心](#eventhub)
| [IoT 服务](#iotservice) | [IoT 设备](#iotdevice) | [Data Lake](#datalake)  | [AppInsights](#appinsights) | 
| [批处理](#batch) | [管理 Azure 资源](#management) |

## <a name="install-with-maven"></a>连同 Maven 一起安装

在 `pom.xml` 中添加一个依赖项条目，用于将库导入到 [Maven](https://maven.apache.org) 项目。

例如，若要包含最新版本的[用于 Java 的 Azure 管理库](#management)：

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

支持其他 Java 生成工具（例如 Gradle），但本文未提供相关的安装步骤。 请查看生成工具的文档，了解如何使用 Maven 导入功能。

## <a name="azure-service-libraries"></a>Azure 服务库

集成 Azure 服务，以使用这些库将功能添加到应用。 在 [Java 开发人员中心](https://azure.microsoft.com/develop/java)详细了解如何使用 Azure 服务生成应用。

<a name="azure-storage"></a>

### <a name="azure-storageazurestoragestorage-introduction"></a>[Azure 存储](/azure/storage/storage-introduction)  

应用程序的数据存储和消息传送。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.4.0</version>
</dependency>
```   

[示例](https://github.com/Azure/azure-storage-java/tree/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage) | [参考](/java/api/overview/azure/storage) | [GitHub](https://github.com/Azure/azure-storage-java)  | [发行说明](https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt)

<a name="sql-database"></a>

### <a name="sql-databaseazuresql-databasesql-database-technical-overview"></a>[SQL 数据库](/azure/sql-database/sql-database-technical-overview)

适用于 Azure SQL 数据库的 JDBC 驱动程序。

```XML
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[示例](/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java) | [参考](/java/api/overview/azure/sql) | [GitHub](https://github.com/Microsoft/mssql-jdbc)  | [发行说明](https://github.com/Microsoft/mssql-jdbc/blob/master/CHANGELOG.md)

<a name="redis-cache"></a>

### <a name="redis-cachehttpsazuremicrosoftcomservicescache"></a>[Redis 缓存](https://azure.microsoft.com/services/cache/)

低延迟、高性能的键值存储。

```XML
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```   

[示例](/azure/redis-cache/cache-java-get-started) | [参考](http://xetorthio.github.io/jedis)  | [GitHub](https://github.com/xetorthio/jedis)  | [发行说明](https://github.com/xetorthio/jedis/releases)  

<a name="cosmos-db"></a>

### <a name="azure-cosmos-dbazurecosmos-dbintroduction"></a>[Azure Cosmos DB](/azure/cosmos-db/introduction)

包含 JSON 文档并采用 SQL 或 JavaScript 查询语法的可缩放 NoSQL 数据库。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>1.12.0</version>
</dependency>
```

[示例](/azure/cosmos-db/sql-api-java-application) | [参考](http://azure.github.io/azure-documentdb-java/) | [GitHub](https://github.com/Azure/azure-documentdb-java)   | [发行说明](https://github.com/Azure/azure-documentdb-java/blob/master/changelog.md)

<a name="servicebus"></a>
 
 ### <a name="servicebusazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](/azure/service-bus-messaging/service-bus-messaging-overview) 
    
 服务总线是企业级的事务消息传送平台服务。
 
 ```XML
 <dependency> 
     <groupId>com.microsoft.azure</groupId> 
     <artifactId>azure-servicebus</artifactId> 
     <version>1.0.0</version> 
 </dependency>   
 ```
 
 [示例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [参考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [发行说明](https://github.com/Azure/azure-service-bus-java)   
  
<a name="azuread"></a>

### <a name="azure-active-directoryazureactive-directoryactive-directory-whatis"></a>[Azure Active Directory](/azure/active-directory/active-directory-whatis)   

应用程序的标识管理和安全登录。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```
   
[示例](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=active%20directory%20&type=&language=java) | [参考](/java/api/overview/azure/activedirectory) | [GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) | [发行说明](https://github.com/AzureAD/azure-activedirectory-library-for-javaT-)
 
<a name="keyvault"></a>

### <a name="key-vaultazurekey-vault"></a>[Key Vault](/azure/key-vault) 

从应用程序安全访问密钥和机密。 

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```

[示例](https://github.com/Azure-Samples/key-vault-java-manage-key-vaults) | [参考](/java/api/overview/azure/keyvault) | [GitHub](https://github.com/Azure/azure-keyvault-java) | [发行说明](https://github.com/Azure/azure-keyvault-java) 

<a name="eventhub"></a>

### <a name="event-hubazureevent-hubsevent-hubs-what-is-event-hubs"></a>[事件中心](/azure/event-hubs/event-hubs-what-is-event-hubs) 
   
用于检测或 IoT 方案的高吞吐量事件和遥测处理。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-eventhubs</artifactId> 
    <version>0.14.4</version> 
</dependency>   
```

[示例](https://github.com/Azure/azure-event-hubs/tree/master/samples#java) | [参考](/java/api/overview/azure/eventhub) | [GitHub](https://github.com/azure/azure-event-hubs-java)  | [发行说明](https://github.com/Azure/azure-event-hubs-java)

<a name="iotservice"></a> 

### <a name="iot-serviceazureiot-hub"></a>[IoT 服务](/azure/iot-hub/)

从注册到 IoT 中心的设备管理标识、发送消息和获取反馈。

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.7.23</version>
</dependency>
```   
   
[示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples) | [参考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [发行说明](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="iotdevice"></a> 

### <a name="iot-deviceazureiot-hubiot-hub-devguide"></a>[IoT 设备](/azure/iot-hub/iot-hub-devguide)

将消息从设备发送到 IoT 中心。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.32</version>
</dependency>
```  

[示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples) | [参考](/java/api/overview/azure/iot) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [发行说明](https://github.com/Azure/azure-iot-sdk-java/blob/master/readme.md)

<a name="datalake"></a> 

### <a name="data-lake-storeazuredata-lake-storedata-lake-store-overview"></a>[Data Lake Store](/azure/data-lake-store/data-lake-store-overview)   
   
将任何大小和形式的数据捕获到单个位置以执行分析。    

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

[示例](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started) | [参考](/java/api/overview/azure/datalakestore) | [GitHub](https://github.com/Azure/azure-data-lake-store-java) | [发行说明](https://github.com/Azure/azure-data-lake-store-java/blob/master/CHANGES.md)

<a name="appinsights"></a> 

### <a name="appinsightsazureapplication-insightsapp-insights-overview"></a>[AppInsights](/azure/application-insights/app-insights-overview)

跟踪使用情况、添加遥测和监视 Web 应用。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>
    <version>1.0.8</version>
</dependency>
```

[示例](/azure/application-insights/app-insights-java-get-started) | [参考](/java/api/overview/azure/appinsights) | [GitHub](https://github.com/Microsoft/ApplicationInsights-Java) | [发行说明](https://github.com/Microsoft/ApplicationInsights-Java#to-upgrade-to-the-latest-sdk)

<a name="batch"></a>

### <a name="batchazurebatch"></a>[批处理](/azure/batch)

在云中有效运行大规模并行和高性能计算应用程序。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```

[示例](https://github.com/azure/azure-batch-samples) | [参考](/java/api/overview/azure/batch) | [GitHub](https://github.com/azure/azure-batch-sdk-for-java) | [发行说明](https://github.com/Azure/azure-batch-sdk-for-java/blob/master/README.md)

<a name="management"></a> 

## <a name="manage-azure-resources"></a>管理 Azure 资源

通过应用程序代码创建、更新和删除 Azure 资源。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

[示例](https://github.com/Azure/azure-sdk-for-java#sample-code) | [参考](https://docs.microsoft.com/java/api/overview/azure/) | [GitHub](https://github.com/Azure/azure-sdk-for-java) | [发行说明](java-sdk-azure-release-notes.md)

<a name="servicebus"></a>

### <a name="servicebushttpsdocsmicrosoftcomazureservice-bus-messagingservice-bus-messaging-overview"></a>[ServiceBus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) 
   
服务总线是企业级的事务消息传送平台服务。

```XML
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>azure-servicebus</artifactId> 
    <version>1.0.0</version> 
</dependency>   
```

[示例](https://github.com/Azure/azure-service-bus/tree/master/samples/Java) | [参考](https://docs.microsoft.com/java/api/overview/azure/servicebus) | [GitHub](https://github.com/azure/azure-service-bus-java)  | [发行说明](https://github.com/Azure/azure-service-bus-java)

