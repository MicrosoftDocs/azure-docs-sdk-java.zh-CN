---
title: 用于 Java 的 Azure 库
description: 用于 Java 的 Azure 管理和服务库概述
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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892728"
---
# <a name="azure-libraries-for-java"></a><span data-ttu-id="545c6-104">用于 Java 的 Azure 库</span><span class="sxs-lookup"><span data-stu-id="545c6-104">Azure libraries for Java</span></span>

<span data-ttu-id="545c6-105">借助用于 Java 的 Azure 库可以通过应用程序代码管理 Azure 资源及连接到服务。</span><span class="sxs-lookup"><span data-stu-id="545c6-105">The Azure libraries for Java help you manage Azure resources and connect to services from your application code.</span></span> <span data-ttu-id="545c6-106">这些库以 [Maven 导入](java-sdk-azure-install.md)的形式提供，方便在 Java 项目中使用。</span><span class="sxs-lookup"><span data-stu-id="545c6-106">The libraries are available as [Maven imports](java-sdk-azure-install.md) for use in your Java projects.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="545c6-107">管理 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="545c6-107">Manage Azure resources</span></span>

<span data-ttu-id="545c6-108">使用[用于 Java 的 Azure 管理库](java-sdk-azure-get-started.md)通过 Java 应用程序创建和管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="545c6-108">Create and manage Azure resources from your Java applications using the [Azure management libraries for Java](java-sdk-azure-get-started.md).</span></span> <span data-ttu-id="545c6-109">使用这些库可生成自己的 Azure 自动化工具和服务。</span><span class="sxs-lookup"><span data-stu-id="545c6-109">Use these libraries to build your own Azure automation tools and services.</span></span> 

<span data-ttu-id="545c6-110">例如，若要创建 Linux VM，可编写以下代码：</span><span class="sxs-lookup"><span data-stu-id="545c6-110">For example, to create a Linux VM you would write the following code:</span></span>

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

<span data-ttu-id="545c6-111">查看[安装说明](java-sdk-azure-install.md)了解库的完整列表以及如何将其导入项目，然后阅读[入门文章](java-sdk-azure-get-started.md)来设置身份验证并针对自己的 Azure 订阅运行示例代码。</span><span class="sxs-lookup"><span data-stu-id="545c6-111">Review the [install instructions](java-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](java-sdk-azure-get-started.md) to set up your authentication and run sample code against your own Azure subscription.</span></span> 

## <a name="connect-to-azure-services"></a><span data-ttu-id="545c6-112">连接到 Azure 服务</span><span class="sxs-lookup"><span data-stu-id="545c6-112">Connect to Azure services</span></span>

<span data-ttu-id="545c6-113">使用 Java 库除了可以在 Azure 中创建和管理资源以外，还可以连接并使用应用中的资源。</span><span class="sxs-lookup"><span data-stu-id="545c6-113">In addition to using Java libraries to create and manage resources within Azure, you can also use Java libraries to connect  and use those resources in your apps.</span></span> <span data-ttu-id="545c6-114">例如，可以更新表 SQL 数据库或者在 Azure 存储中存储文件。</span><span class="sxs-lookup"><span data-stu-id="545c6-114">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="545c6-115">从[库的完整列表](java-sdk-azure-install.md)中选择特定服务所需的库，并访问 [Java 开发人员中心](https://azure.microsoft.com/develop/java/)获取教程和示例代码来帮助自己在应用中使用这些库。</span><span class="sxs-lookup"><span data-stu-id="545c6-115">Select the library you need for a particular service from the [complete list of libraries](java-sdk-azure-install.md) and visit the [Java developer center](https://azure.microsoft.com/develop/java/) for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="545c6-116">例如，若要输出 Azure 存储容器中每个 Blob 的内容：</span><span class="sxs-lookup"><span data-stu-id="545c6-116">For example, to print out the contents of every blob in an Azure storage container:</span></span>

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

## <a name="sample-code-and-reference"></a><span data-ttu-id="545c6-117">代码示例和参考</span><span class="sxs-lookup"><span data-stu-id="545c6-117">Sample code and reference</span></span>

<span data-ttu-id="545c6-118">以下示例涵盖可以通过用于 Java 的 Azure 管理库完成的常见自动化任务，并提供可在自己应用中使用的现成代码：</span><span class="sxs-lookup"><span data-stu-id="545c6-118">The following samples cover common automation tasks with the Azure management libraries for Java and have code ready to use in your own apps:</span></span>

- [<span data-ttu-id="545c6-119">虚拟机</span><span class="sxs-lookup"><span data-stu-id="545c6-119">Virtual machines</span></span>](java-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="545c6-120">Web 应用</span><span class="sxs-lookup"><span data-stu-id="545c6-120">Web apps</span></span>](java-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="545c6-121">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="545c6-121">SQL Database</span></span>](java-sdk-azure-sql-database-samples.md)
   
<span data-ttu-id="545c6-122">我们针对服务和管理库中的所有包提供了[参考](https://docs.microsoft.com/java/api)文档。</span><span class="sxs-lookup"><span data-stu-id="545c6-122">A [reference](https://docs.microsoft.com/java/api) is available for all packages in both the service and management libraries.</span></span> <span data-ttu-id="545c6-123">[发行说明](java-sdk-azure-release-notes.md)中介绍了新增功能、重大更改，并提供了有关如何从以前的版本迁移的说明。</span><span class="sxs-lookup"><span data-stu-id="545c6-123">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](java-sdk-azure-release-notes.md).</span></span>