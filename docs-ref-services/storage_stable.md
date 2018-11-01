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
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235218"
---
# <a name="azure-storage-libraries-for-java"></a><span data-ttu-id="406d0-103">用于 Java 的 Azure 存储库</span><span class="sxs-lookup"><span data-stu-id="406d0-103">Azure Storage libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="406d0-104">概述</span><span class="sxs-lookup"><span data-stu-id="406d0-104">Overview</span></span>

<span data-ttu-id="406d0-105">使用 [Azure 存储](/azure/storage/storage-introduction)在 Java 应用程序中读取和写入 Blob（对象）数据、文件和消息。</span><span class="sxs-lookup"><span data-stu-id="406d0-105">Read and write blob (object) data, files, and messages from your Java applications with [Azure Storage](/azure/storage/storage-introduction).</span></span>

<span data-ttu-id="406d0-106">若要开始使用 Azure 存储，请参阅[如何通过 Java 使用 Blob 存储](/azure/storage/blobs/storage-quickstart-blobs-java-v10)。</span><span class="sxs-lookup"><span data-stu-id="406d0-106">To get started with Azure Storage, see [How to use Blob storage from Java](/azure/storage/blobs/storage-quickstart-blobs-java-v10).</span></span>

## <a name="client-library"></a><span data-ttu-id="406d0-107">客户端库</span><span class="sxs-lookup"><span data-stu-id="406d0-107">Client library</span></span>

<span data-ttu-id="406d0-108">使用 Azure Active Directory 中的共享密钥、SAS 令牌或 OAuth 令牌通过 Azure 存储服务进行授权。</span><span class="sxs-lookup"><span data-stu-id="406d0-108">Use a Shared Key, SAS token or an OAuth token from the Azure Active Directory to authorize with Azure Storage services.</span></span> <span data-ttu-id="406d0-109">然后通过客户端库的类和方法来使用 Blob、文件或队列存储。</span><span class="sxs-lookup"><span data-stu-id="406d0-109">Then use the client libraries' classes and methods to work with blob, file, or queue storage.</span></span> 

<span data-ttu-id="406d0-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="406d0-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

<span data-ttu-id="406d0-111">**Blob 服务的依赖项**：</span><span class="sxs-lookup"><span data-stu-id="406d0-111">**Dependency for Blob service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>10.1.0</version>
</dependency>
```

<span data-ttu-id="406d0-112">**队列服务的依赖项**：</span><span class="sxs-lookup"><span data-stu-id="406d0-112">**Dependency for Queue service**:</span></span>
```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage-queue</artifactId>
    <version>10.0.0-Preview</version>
</dependency>
```


### <a name="example"></a><span data-ttu-id="406d0-113">示例</span><span class="sxs-lookup"><span data-stu-id="406d0-113">Example</span></span>

<span data-ttu-id="406d0-114">将本地文件系统中的映像文件写入现有 Azure 存储 Blob 容器中的新 Blob。</span><span class="sxs-lookup"><span data-stu-id="406d0-114">Write an image file from the local file system into a new blob in an existing Azure Storage blob container.</span></span>


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
> [<span data-ttu-id="406d0-115">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="406d0-115">Explore the Client APIs</span></span>](/java/api/overview/azure/storage/client)

## <a name="management-api"></a><span data-ttu-id="406d0-116">管理 API</span><span class="sxs-lookup"><span data-stu-id="406d0-116">Management API</span></span>

<span data-ttu-id="406d0-117">使用管理 API 创建和管理 Azure 存储帐户与连接密钥。</span><span class="sxs-lookup"><span data-stu-id="406d0-117">Crete and manage Azure Storage accounts and connection keys with the management API.</span></span>

<span data-ttu-id="406d0-118">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="406d0-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-storage</artifactId>
    <version>1.3.0</version>
</dependency
```   

### <a name="example"></a><span data-ttu-id="406d0-119">示例</span><span class="sxs-lookup"><span data-stu-id="406d0-119">Example</span></span>

<span data-ttu-id="406d0-120">在订阅中创建新的 Azure 存储帐户并检索其访问密钥。</span><span class="sxs-lookup"><span data-stu-id="406d0-120">Create a new Azure Storage account in your subscription and retrieve its access keys.</span></span>

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
> [<span data-ttu-id="406d0-121">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="406d0-121">Explore the Management APIs</span></span>](/java/api/overview/azure/storage/management)


## <a name="samples"></a><span data-ttu-id="406d0-122">示例</span><span class="sxs-lookup"><span data-stu-id="406d0-122">Samples</span></span>

<span data-ttu-id="406d0-123">[用于 Java 的 Azure 存储 SDK](https://github.com/azure/azure-storage-java)
[在 Blob 存储中读取和写入对象](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span><span class="sxs-lookup"><span data-stu-id="406d0-123">[Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
[Read and write objects to blob storage](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) </span></span>  
[<span data-ttu-id="406d0-124">使用队列读取和写入消息</span><span class="sxs-lookup"><span data-stu-id="406d0-124">Read and write messages with queues</span></span>](https://github.com/Azure-Samples/storage-queue-java-getting-started)   
