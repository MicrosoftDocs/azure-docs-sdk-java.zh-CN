---
title: 用于 Java 的 Azure 存储库
description: ''
keywords: Azure, Java, SDK, API, 存储
author: douge
ms.author: douge
manager: douge
ms.date: 10/19/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: storage
ms.openlocfilehash: fba48dfa04f223dce72a0ee54da967565ebd3687
ms.sourcegitcommit: 67b3542b174e8448f9ca3e7c9506f1216ea6a8fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285661"
---
# <a name="azure-storage-libraries-for-java"></a>用于 Java 的 Azure 存储库

## <a name="overview"></a>概述

使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入 Blob（对象）数据、文件和消息。

若要开始使用 Azure 存储，请参阅[如何通过 Java 使用 Blob 存储](/azure/storage/blobs/storage-quickstart-blobs-java-v10)。

## <a name="client-library"></a>客户端库

使用 Azure Active Directory 中的共享密钥、SAS 令牌或 OAuth 令牌通过 Azure 存储服务进行授权。 然后通过客户端库的类和方法来使用 Blob、文件或队列存储。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。   

**Blob 服务的依赖项**：
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

**队列服务的依赖项**：
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a>示例

将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。


```java
// Retrieve the credentials and initialize SharedKeyCredentials
String accountName = System.getenv("AZURE_STORAGE_ACCOUNT");
String accountKey = System.getenv("AZURE_STORAGE_ACCESS_KEY");

// Create a BlockBlobURL to run operations on Block Blobs. Alternatively create a ServiceURL, or ContainerURL for operations on Blob service, and Blob containers
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);

// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final BlockBlobURL blobURL = new BlockBlobURL(
    new URL("https://" + accountName + ".blob.core.windows.net/mycontainer/myimage.jpg"), 
        StorageURL.createPipeline(creds, new PipelineOptions())
);

AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(Paths.get("myimage.jpg"));
TransferManager.uploadFileToBlockBlob(fileChannel, blobURL,0, null).blockingGet();
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
