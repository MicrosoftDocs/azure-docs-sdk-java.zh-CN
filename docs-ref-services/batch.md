---
title: 用于 Java 的 Azure Batch 库
description: Java Batch 库的参考文档
keywords: Azure, Java, SDK, API, Batch, 处理, 计划, 长时间运行
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: batch
ms.openlocfilehash: d8e7a6969bf35d98f03c5d3e335fbaf2f6b3a51d
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324354"
---
# <a name="azure-batch-libraries-for-java"></a><span data-ttu-id="1b0ca-104">用于 Java 的 Azure Batch 库</span><span class="sxs-lookup"><span data-stu-id="1b0ca-104">Azure Batch libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="1b0ca-105">概述</span><span class="sxs-lookup"><span data-stu-id="1b0ca-105">Overview</span></span>

<span data-ttu-id="1b0ca-106">使用 [Azure Batch](/azure/batch/batch-technical-overview) 在云中有效运行大规模并行和高性能计算应用程序。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>   

<span data-ttu-id="1b0ca-107">若要开始使用 Azure Batch，请参阅[使用 Azure 门户创建 Batch 帐户](/azure/batch/batch-account-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="client-library"></a><span data-ttu-id="1b0ca-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="1b0ca-108">Client library</span></span>

<span data-ttu-id="1b0ca-109">使用 Azure Batch 客户端库可以配置计算节点和池、定义任务并将其配置为在作业中运行，以及设置一个作业管理器来控制和监视作业执行。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-109">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="1b0ca-110">[详细了解](/azure/batch/batch-api-basics)如何使用这些对象来运行大规模并行计算解决方案。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-110">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

<span data-ttu-id="1b0ca-111">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-111">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span> <span data-ttu-id="1b0ca-112">可在 [Github](https://github.com/Azure/azure-batch-sdk-for-java) 中找到客户端库源代码。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-112">The client library source code can be found in [Github](https://github.com/Azure/azure-batch-sdk-for-java).</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>4.0.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="1b0ca-113">示例</span><span class="sxs-lookup"><span data-stu-id="1b0ca-113">Example</span></span>

<span data-ttu-id="1b0ca-114">在 Batch 帐户中设置 Linux 计算节点池：</span><span class="sxs-lookup"><span data-stu-id="1b0ca-114">Set up a pool of Linux compute nodes in a batch account:</span></span>

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="1b0ca-115">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="1b0ca-115">Explore the Client APIs</span></span>](/java/api/overview/azure/batch/client)


## <a name="management-api"></a><span data-ttu-id="1b0ca-116">管理 API</span><span class="sxs-lookup"><span data-stu-id="1b0ca-116">Management API</span></span>

<span data-ttu-id="1b0ca-117">使用 Azure Batch 管理库可以创建和删除 Batch 帐户、读取和重新生成 Batch 帐户密钥，以及管理 Batch 帐户存储。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-117">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

<span data-ttu-id="1b0ca-118">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="1b0ca-119">示例</span><span class="sxs-lookup"><span data-stu-id="1b0ca-119">Example</span></span>

<span data-ttu-id="1b0ca-120">创建 Azure Batch 帐户并为其配置新的应用程序和 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-120">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```java
BatchAccount batchAccount = azure.batchAccounts().define("newBatchAcct")
    .withRegion(Region.US_EAST)
    .withNewResourceGroup("myResourceGroup")
    .defineNewApplication("batchAppName")
        .defineNewApplicationPackage(applicationPackageName)
        .withAllowUpdates(true)
        .withDisplayName(applicationDisplayName)
        .attach()
    .withNewStorageAccount("batchStorageAcct")
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="1b0ca-121">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="1b0ca-121">Explore the Management APIs</span></span>](/java/api/overview/azure/batch/management)


## <a name="samples"></a><span data-ttu-id="1b0ca-122">示例</span><span class="sxs-lookup"><span data-stu-id="1b0ca-122">Samples</span></span>

<span data-ttu-id="1b0ca-123">[管理 Batch 帐户][1]</span><span class="sxs-lookup"><span data-stu-id="1b0ca-123">[Manage Batch accounts][1]</span></span>   

<span data-ttu-id="1b0ca-124">详细了解可在应用中使用的 [Azure Batch 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)。</span><span class="sxs-lookup"><span data-stu-id="1b0ca-124">Explore more [sample Java code for Azure Batch](https://azure.microsoft.com/resources/samples/?platform=java&term=batch) you can use in your apps.</span></span>

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts
