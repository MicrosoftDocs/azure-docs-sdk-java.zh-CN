---
title: 使用 Java 管理 Azure 存储帐户 | Microsoft Docs
description: 使用用于 Java 的 Azure SDK 管理 Azure 存储帐户的示例代码
author: rloutlaw
manager: douge
ms.assetid: 49be8b66-3b56-4c10-8f14-9d326d815cb4
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 5945164b2b04e1fa9169590a71f6c5f9f45842d6
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931053"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a><span data-ttu-id="1b08b-103">从 Java 应用程序管理 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-103">Manage Azure storage accounts from your Java applications</span></span>

<span data-ttu-id="1b08b-104">[本示例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)通过 [Java 管理库](https://github.com/Azure/azure-sdk-for-java)创建一个 [Azure 存储](https://docs.microsoft.com/azure/storage/storage-introduction)帐户并使用帐户访问密钥。</span><span class="sxs-lookup"><span data-stu-id="1b08b-104">[This sample](https://github.com/Azure-Samples/storage-java-manage-storage-accounts) creates an [Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction) account and works with the account access keys using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="1b08b-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="1b08b-105">Run the sample</span></span>

<span data-ttu-id="1b08b-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="1b08b-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="1b08b-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="1b08b-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

<span data-ttu-id="1b08b-108">查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)。</span><span class="sxs-lookup"><span data-stu-id="1b08b-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="1b08b-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="1b08b-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a><span data-ttu-id="1b08b-110">创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-110">Create a storage account</span></span>

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

<span data-ttu-id="1b08b-111">提供的存储名称必须在 Azure 中的所有名称之间保持唯一，且仅包含小写字母和数字。</span><span class="sxs-lookup"><span data-stu-id="1b08b-111">The storage name provided must be unique across all names in Azure and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="1b08b-112">用于此帐户的默认性能和复制配置文件为 [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage)。</span><span class="sxs-lookup"><span data-stu-id="1b08b-112">The default performance and replication profile used for this account is [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage).</span></span>

## <a name="list-keys-in-a-storage-account"></a><span data-ttu-id="1b08b-113">列出存储帐户中的密钥</span><span class="sxs-lookup"><span data-stu-id="1b08b-113">List keys in a storage account</span></span>
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

<span data-ttu-id="1b08b-114">每个 Azure 存储帐户中提供了两个密钥，以便可以重新生成其中的一个密钥，同时使用另一个密钥访问存储。</span><span class="sxs-lookup"><span data-stu-id="1b08b-114">Two keys are provided in each Azure storage account so that you can regenerate one key while still allowing access to storage using the other key.</span></span>

## <a name="regenerate-a-key-in-a-storage-account"></a><span data-ttu-id="1b08b-115">重新生成存储帐户中的密钥</span><span class="sxs-lookup"><span data-stu-id="1b08b-115">Regenerate a key in a storage account</span></span>

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

<span data-ttu-id="1b08b-116">生成密钥后，必须使用新密钥更新所有 Azure 资源和应用程序。</span><span class="sxs-lookup"><span data-stu-id="1b08b-116">You must update all Azure resources and applications with the new key after generating a new one.</span></span>

## <a name="list-all-storage-accounts-in-a-resource-group"></a><span data-ttu-id="1b08b-117">列出资源组中的所有存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-117">List all storage accounts in a resource group</span></span>
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

<span data-ttu-id="1b08b-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) 提供了一组有用的方法来检查存储帐户的配置。</span><span class="sxs-lookup"><span data-stu-id="1b08b-118">[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) provides a set of useful methods to inspect the configuration of a storage account.</span></span>

## <a name="delete-a-storage-account"></a><span data-ttu-id="1b08b-119">删除存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-119">Delete a storage account</span></span>
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> <span data-ttu-id="1b08b-120">使用这些方法无法删除包含已连接到虚拟机的使用中磁盘映像或者由其他项目使用的磁盘的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="1b08b-120">Storage accounts with in-use disk images connected to virtual machines or disks in use by other artifacts may not remove with these methods.</span></span> <span data-ttu-id="1b08b-121">在删除帐户之前，请从这些资源中分离存储。</span><span class="sxs-lookup"><span data-stu-id="1b08b-121">Detach the storage from these resources before removing the account.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="1b08b-122">示例解释</span><span class="sxs-lookup"><span data-stu-id="1b08b-122">Sample explanation</span></span>

<span data-ttu-id="1b08b-123">[GitHub 上的示例代码](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)：</span><span class="sxs-lookup"><span data-stu-id="1b08b-123">[The sample code on GitHub](https://github.com/Azure-Samples/storage-java-manage-storage-accounts):</span></span>

- <span data-ttu-id="1b08b-124">创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-124">creates a storage account</span></span>
- <span data-ttu-id="1b08b-125">读取和重新生成访问密钥</span><span class="sxs-lookup"><span data-stu-id="1b08b-125">reads and regenerates access keys</span></span>
- <span data-ttu-id="1b08b-126">列出资源组中的所有存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-126">lists all storage accounts in a resource group</span></span>
- <span data-ttu-id="1b08b-127">删除存储帐户</span><span class="sxs-lookup"><span data-stu-id="1b08b-127">deletes the storage account</span></span> 

| <span data-ttu-id="1b08b-128">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="1b08b-128">Class used in sample</span></span> | <span data-ttu-id="1b08b-129">说明</span><span class="sxs-lookup"><span data-stu-id="1b08b-129">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="1b08b-130">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="1b08b-130">StorageAccount</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | <span data-ttu-id="1b08b-131">Azure 存储帐户的表示形式。</span><span class="sxs-lookup"><span data-stu-id="1b08b-131">Representation of an Azure storage account.</span></span> <span data-ttu-id="1b08b-132">使用类中的方法获取有关存储帐户的信息。</span><span class="sxs-lookup"><span data-stu-id="1b08b-132">Use the methods in the class to get information about the storage account.</span></span>
| [<span data-ttu-id="1b08b-133">StorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="1b08b-133">StorageAccountKey</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | <span data-ttu-id="1b08b-134">`StorageAccount.getKeys()` 返回存储帐户密钥。</span><span class="sxs-lookup"><span data-stu-id="1b08b-134">`StorageAccount.getKeys()` returns the storage account keys.</span></span> <span data-ttu-id="1b08b-135">使用 `StorageAccount` 中的 `regenerateKey` 方法来更新密钥。</span><span class="sxs-lookup"><span data-stu-id="1b08b-135">Use the `regenerateKey` methods in `StorageAccount` to update the keys.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b08b-136">后续步骤</span><span class="sxs-lookup"><span data-stu-id="1b08b-136">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]