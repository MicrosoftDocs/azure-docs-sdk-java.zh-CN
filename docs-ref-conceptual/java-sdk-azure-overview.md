---
title: "用于 Java 的 Azure 库"
description: "用于 Java 的 Azure 管理和服务库概述"
keywords: Azure, Java, SDK, API
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 9aaf22a2-382a-4b13-a8e3-0e467d607207
ms.openlocfilehash: 22a337e928085475e905a271f991147729d97671
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-libraries-for-java"></a>用于 Java 的 Azure 库

借助用于 Java 的 Azure 库可以通过应用程序代码管理 Azure 资源及连接到服务。 这些库以 [Maven 导入](java-sdk-azure-install.md)的形式提供，方便在 Java 项目中使用。 

## <a name="manage-azure-resources"></a>管理 Azure 资源

使用[用于 Java 的 Azure 管理库](java-sdk-azure-get-started.md)通过 Java 应用程序创建和管理 Azure 资源。 使用这些库可生成自己的 Azure 自动化工具和服务。 

例如，若要创建 Linux VM，可编写以下代码：

```java
VirtualMachine linuxVM = azure.virtualMachines().define("myAzureVM")
           .withRegion(region)
           .withExistingResourceGroup("sampleResourceGroup")
           .withNewPrimaryNetwork("10.0.0.0/28")
           .withPrimaryPrivateIpAddressDynamic()
           .withoutPrimaryPublicIpAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withSsh(key)
           .withUnmanagedStorage()
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
 ```

查看[安装说明](java-sdk-azure-install.md)了解库的完整列表以及如何将其导入项目，然后阅读[入门文章](java-sdk-azure-get-started.md)来设置身份验证并针对自己的 Azure 订阅运行示例代码。 

## <a name="connect-to-azure-services"></a>连接到 Azure 服务

使用 Java 库除了可以在 Azure 中创建和管理资源以外，还可以连接并使用应用中的资源。 例如，可以更新表 SQL 数据库或者在 Azure 存储中存储文件。 从[库的完整列表](java-sdk-azure-install.md)中选择特定服务所需的库，并访问 [Java 开发人员中心](https://azure.microsoft.com/develop/java/)获取教程和示例代码来帮助自己在应用中使用这些库。

例如，若要输出 Azure 存储容器中每个 Blob 的内容：

```java
// get the container from the blob client
CloudBlobContainer container = blobClient.getContainerReference("blobcontainer");

// For each item in the container
for (ListBlobItem blobItem : container.listBlobs()) {
    // If the item is a blob, not a virtual directory
    if (blobItem instanceof CloudBlockBlob) {
        // Download the text
        CloudBlockBlob retrievedBlob = (CloudBlockBlob) blobItem;
        System.out.println(retrievedBlob.downloadText());
    }
}
```

## <a name="sample-code-and-reference"></a>代码示例和参考

以下示例涵盖可以通过用于 Java 的 Azure 管理库完成的常见自动化任务，并提供可在自己应用中使用的现成代码：

- [虚拟机](java-sdk-azure-virtual-machine-samples.md)
- [Web 应用](java-sdk-azure-web-apps-samples.md)
- [SQL 数据库](java-sdk-azure-sql-database-samples.md)
   
我们针对服务和管理库中的所有包提供了[参考](https://docs.microsoft.com/java/api)文档。 [发行说明](java-sdk-azure-release-notes.md)中介绍了新增功能、重大更改，并提供了有关如何从以前的版本迁移的说明。