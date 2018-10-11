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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893058"
---
# <a name="manage-azure-storage-accounts-from-your-java-applications"></a>从 Java 应用程序管理 Azure 存储帐户

[本示例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)通过 [Java 管理库](https://github.com/Azure/azure-sdk-for-java)创建一个 [Azure 存储](https://docs.microsoft.com/azure/storage/storage-introduction)帐户并使用帐户访问密钥。 

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/storage-java-manage-storage-accounts.git
cd storage-java-manage-storage-accounts
mvn clean compile exec:java
```

查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)] 

## <a name="create-a-storage-account"></a>创建存储帐户

```java
// create a new storage account
StorageAccount storageAccount = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .create();
```

提供的存储名称必须在 Azure 中的所有名称之间保持唯一，且仅包含小写字母和数字。 用于此帐户的默认性能和复制配置文件为 [Standard_GRS](https://docs.microsoft.com/azure/storage/storage-redundancy#geo-redundant-storage)。

## <a name="list-keys-in-a-storage-account"></a>列出存储帐户中的密钥
```java
// list the name and value for each access key in the storage account
List<StorageAccountKey> storageAccountKeys = storageAccount.getKeys();
for(StorageAccountKey key : storageAccountKeys)    {
    System.out.println("Key name: " + key.keyName() + " with value "+ key.value());
}
```

每个 Azure 存储帐户中提供了两个密钥，以便可以重新生成其中的一个密钥，同时使用另一个密钥访问存储。

## <a name="regenerate-a-key-in-a-storage-account"></a>重新生成存储帐户中的密钥

```java
// regenerate the first key in a storage account and return an updated list of keys 
List<StorageAccountKey> updatedStorageAccountKeys =
    storageAccount.regenerateKey(storageAccountKeys.get(0).keyName());
```

生成密钥后，必须使用新密钥更新所有 Azure 资源和应用程序。

## <a name="list-all-storage-accounts-in-a-resource-group"></a>列出资源组中的所有存储帐户
```java
// get a list of accounts in a resource group , log info about each one
List<StorageAccount> accounts = azure.storageAccounts().listByResourceGroup(rgName);
for (StorageAccount sa : accounts) {
    System.out.println("Storage Account " + sa.name() + " created @ " + sa.creationTime());
}
```

[com.microsoft.azure.management.storage.StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account) 提供了一组有用的方法来检查存储帐户的配置。

## <a name="delete-a-storage-account"></a>删除存储帐户
```java
// delete by ID when you already have a storage account object
azure.storageAccounts().deleteById(storageAccount.id());

// delete by resource group and account name if you don't have an account object
azure.storageAccounts().deleteByResourceGroup(rgName,accountName);
```

> [!NOTE]
> 使用这些方法无法删除包含已连接到虚拟机的使用中磁盘映像或者由其他项目使用的磁盘的存储帐户。 在删除帐户之前，请从这些资源中分离存储。

## <a name="sample-explanation"></a>示例解释

[GitHub 上的示例代码](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)：

- 创建存储帐户
- 读取和重新生成访问密钥
- 列出资源组中的所有存储帐户
- 删除存储帐户 

| 示例中使用的类 | 说明
|-------|-------|
| [StorageAccount](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account)  | Azure 存储帐户的表示形式。 使用类中的方法获取有关存储帐户的信息。
| [StorageAccountKey](https://docs.microsoft.com/java/api/com.microsoft.azure.management.storage._storage_account_key) | `StorageAccount.getKeys()` 返回存储帐户密钥。 使用 `StorageAccount` 中的 `regenerateKey` 方法来更新密钥。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]