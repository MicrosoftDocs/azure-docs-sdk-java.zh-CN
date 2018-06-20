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
ms.locfileid: "31823650"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="c2f0c-103">用于 Java 的 Azure 存储库</span><span class="sxs-lookup"><span data-stu-id="c2f0c-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="c2f0c-104">概述</span><span class="sxs-lookup"><span data-stu-id="c2f0c-104">Overview</span></span>

<span data-ttu-id="c2f0c-105">使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入文件、Blob（对象）数据、键值对和消息。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-105">Read and write files, blob (object) data, key-value pairs, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="c2f0c-106">若要开始使用 Azure 存储，请参阅[如何通过 Java 使用 Blob 存储](/azure/storage/storage-java-how-to-use-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/storage-java-how-to-use-blob-storage).</span></span>

## <a name="client-library"></a><span data-ttu-id="c2f0c-107">客户端库</span><span class="sxs-lookup"><span data-stu-id="c2f0c-107">Client library</span></span>

<span data-ttu-id="c2f0c-108">使用[连接字符串](/azure/storage/storage-create-storage-account#manage-your-storage-account)连接到 Azure 存储帐户，然后通过客户端库的类和方法来使用 Blob、表、文件或队列存储。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-108">Use [connection strings](/azure/storage/storage-create-storage-account#manage-your-storage-account) to connect to an Azure Storage account, then use the client libraries' classes and methods to work with blob, table, file, or queue storage.</span></span> 

<span data-ttu-id="c2f0c-109">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>7.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="c2f0c-110">示例</span><span class="sxs-lookup"><span data-stu-id="c2f0c-110">Example</span></span>

<span data-ttu-id="c2f0c-111">将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-111">Write a image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


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
> [<span data-ttu-id="c2f0c-112">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="c2f0c-112">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="c2f0c-113">管理 API</span><span class="sxs-lookup"><span data-stu-id="c2f0c-113">Management API</span></span>

<span data-ttu-id="c2f0c-114">使用管理 API 创建和管理 Azure 存储帐户与连接密钥。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-114">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="c2f0c-115">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-115">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="c2f0c-116">示例</span><span class="sxs-lookup"><span data-stu-id="c2f0c-116">Example</span></span>

<span data-ttu-id="c2f0c-117">在订阅中创建新的 Azure 存储帐户并检索其访问密钥。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-117">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="c2f0c-118">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="c2f0c-118">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="c2f0c-119">示例</span><span class="sxs-lookup"><span data-stu-id="c2f0c-119">Samples</span></span>

<span data-ttu-id="c2f0c-120">[管理 Azure 存储帐户](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span><span class="sxs-lookup"><span data-stu-id="c2f0c-120">[Manage Azure Storage accounts](../docs-ref-conceptual/java-sdk-manage-storage-accounts.md)  </span></span>  
<span data-ttu-id="c2f0c-121">[在 Blob 存储中读取和写入对象](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="c2f0c-121">[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blob-java-getting-started) </span></span>  
<span data-ttu-id="c2f0c-122">[使用队列读取和写入消息](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span><span class="sxs-lookup"><span data-stu-id="c2f0c-122">[Read and write messages with queues](https://github.com/Azure-Samples/storage-queue-java-getting-started) </span></span>  
[<span data-ttu-id="c2f0c-123">从 Web 应用中的 Blob 存储读取文件</span><span class="sxs-lookup"><span data-stu-id="c2f0c-123">Read files from blob storage in a web app</span></span>](https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps-on-linux)

<span data-ttu-id="c2f0c-124">详细了解可在应用中使用的 [Azure 存储示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=storage)。</span><span class="sxs-lookup"><span data-stu-id="c2f0c-124">Explore more [sample Java code for Azure Storage](https://azure.microsoft.com/resources/samples/?platform=java&term=storage) you can use in your apps.</span></span>
