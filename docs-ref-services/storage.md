---
title: 用于 Java 的 Azure 存储库
description: ''
keywords: Azure, Java, SDK, API, 存储
author: douge
ms.author: douge
manager: douge
ms.date: 02/07/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: ec06e79374176b5a4795d27c5fbbb2260e65cd8c
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="azure-storage-libraries-for-java"></a>用于 Java 的 Azure 存储库

## <a name="overview"></a>概述

使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入文件、Blob（对象）数据、键值对和消息。

若要开始使用 Azure 存储，请参阅[如何通过 Java 使用 Blob 存储](/azure/storage/storage-java-how-to-use-blob-storage)。

## <a name="client-library"></a>客户端库

使用[连接字符串](/azure/storage/storage-create-storage-account#manage-your-storage-account)连接到 Azure 存储帐户，然后通过客户端库的类和方法来使用 Blob、表、文件或队列存储。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a>示例

将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。


```java
String storageConnectionString = "DefaultEndpointsProtocol=https;" + 
"AccountName=fabrikamblobstorage;" + 
"AccountKey=keyvalue;EndpointSuffix=core.windows.net";

CloudStorageAccount account = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient serviceClient = account.createCloudBlobClient();
CloudBlobContainer container = serviceClient.getContainerReference(blobContainer);

// write a blob from a local filesystem path to the container as logo.png
CloudBlockBlob blob = container.getBlockBlobReference("logo.png");
blob.uploadFromFile("/Users/raisa/fabrikam.png");
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/storage/client)

## <a name="management-api"></a>管理 API

使用管理 API 创建和管理 Azure 存储帐户与连接密钥。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a>示例

在订阅中创建新的 Azure 存储帐户并检索其访问密钥。

```java
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
        .withRegion(Region.US_EAST)
        .withNewResourceGroup(rgName)
        .create();

// get a list of storage account keys related to the account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/storage/management)


## <a name="samples"></a>示例

[管理 Azure 存储帐户](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)    
[在 Blob 存储中读取和写入对象](https://github.com/Azure-Samples/storage-blob-java-getting-started)   
[使用队列读取和写入消息](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
[从 Web 应用中的 Blob 存储读取文件](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

详细了解可在应用中使用的 [Azure 存储示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)。
