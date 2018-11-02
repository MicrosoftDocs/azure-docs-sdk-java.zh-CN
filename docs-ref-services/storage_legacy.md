---
title: 用于 Java 的 Azure 存储库
description: ''
keywords: Azure, Java, SDK, API, 存储
author: douge
ms.author: seguler
manager: dineshm
ms.date: 10/29/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: 68a5fdbf8e2fbfe83f9ea09112d64488e0c11d08
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235222"
---
# <a name="azure-storage-libraries-for-java"></a>用于 Java 的 Azure 存储库

## <a name="overview"></a>概述

使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入 Blob（对象）数据、文件和消息。

若要开始使用 Azure 存储，请参阅[如何通过 Java SDK v7 使用 Blob 存储](/azure/storage/blobs/storage-quickstart-blobs-java)。

## <a name="client-library"></a>客户端库

使用 Azure Active Directory 中的共享密钥、SAS 令牌或 OAuth 令牌通过 Azure 存储服务进行授权。 然后通过客户端库的类和方法来使用 Blob、文件或队列存储。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。   

**旧版 Azure 存储 SDK v7 的依赖项**：
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a>示例

将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
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

[用于 Java 的 Azure 存储 SDK](https://github.com/azure/azure-storage-java)
[在 Blob 存储中读取和写入对象](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart)   
[使用队列读取和写入消息](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
