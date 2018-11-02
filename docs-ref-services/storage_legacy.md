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
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="bc03e-103">用于 Java 的 Azure 存储库</span><span class="sxs-lookup"><span data-stu-id="bc03e-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="bc03e-104">概述</span><span class="sxs-lookup"><span data-stu-id="bc03e-104">Overview</span></span>

<span data-ttu-id="bc03e-105">使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入 Blob（对象）数据、文件和消息。</span><span class="sxs-lookup"><span data-stu-id="bc03e-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="bc03e-106">若要开始使用 Azure 存储，请参阅[如何通过 Java SDK v7 使用 Blob 存储](/azure/storage/blobs/storage-quickstart-blobs-java)。</span><span class="sxs-lookup"><span data-stu-id="bc03e-106">To get started with Azure Storage, see [How to use Blob storage from Java SDK v7](/azure/storage/blobs/storage-quickstart-blobs-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="bc03e-107">客户端库</span><span class="sxs-lookup"><span data-stu-id="bc03e-107">Client library</span></span>

<span data-ttu-id="bc03e-108">使用 Azure Active Directory 中的共享密钥、SAS 令牌或 OAuth 令牌通过 Azure 存储服务进行授权。</span><span class="sxs-lookup"><span data-stu-id="bc03e-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="bc03e-109">然后通过客户端库的类和方法来使用 Blob、文件或队列存储。</span><span class="sxs-lookup"><span data-stu-id="bc03e-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="bc03e-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="bc03e-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="bc03e-111">**旧版 Azure 存储 SDK v7 的依赖项**：</span><span class="sxs-lookup"><span data-stu-id="bc03e-111">**Dependency for the legacy Azure Storage SDK v7**:</span></span>
```XML
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>8.0.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="bc03e-112">示例</span><span class="sxs-lookup"><span data-stu-id="bc03e-112">Example</span></span>

<span data-ttu-id="bc03e-113">将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。</span><span class="sxs-lookup"><span data-stu-id="bc03e-113">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


```java
// Parse the connection string and create a blob client to interact with Blob storage
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();
CloudBlobContainer container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());         
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="bc03e-114">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="bc03e-114">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="bc03e-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="bc03e-115">Management API</span></span>

<span data-ttu-id="bc03e-116">使用管理 API 创建和管理 Azure 存储帐户与连接密钥。</span><span class="sxs-lookup"><span data-stu-id="bc03e-116">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="bc03e-117">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="bc03e-117">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="bc03e-118">示例</span><span class="sxs-lookup"><span data-stu-id="bc03e-118">Example</span></span>

<span data-ttu-id="bc03e-119">在订阅中创建新的 Azure 存储帐户并检索其访问密钥。</span><span class="sxs-lookup"><span data-stu-id="bc03e-119">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="bc03e-120">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="bc03e-120">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="bc03e-121">示例</span><span class="sxs-lookup"><span data-stu-id="bc03e-121">Samples</span></span>

<span data-ttu-id="bc03e-122">[用于 Java 的 Azure 存储 SDK](https://github.com/azure/azure-storage-java)
[在 Blob 存储中读取和写入对象](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="bc03e-122">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="bc03e-123">使用队列读取和写入消息</span><span class="sxs-lookup"><span data-stu-id="bc03e-123">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
