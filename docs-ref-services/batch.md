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
ms.openlocfilehash: 67381d68d23f98579a472aefbebaa929af622b8d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823590"
---
# <a name="azure-batch-libraries-for-java"></a>用于 Java 的 Azure Batch 库

## <a name="overview"></a>概述

使用 [Azure Batch](/azure/batch/batch-technical-overview) 在云中有效运行大规模并行和高性能计算应用程序。   

若要开始使用 Azure Batch，请参阅[使用 Azure 门户创建 Batch 帐户](/azure/batch/batch-account-create-portal)。

## <a name="client-library"></a>客户端库

使用 Azure Batch 客户端库可以配置计算节点和池、定义任务并将其配置为在作业中运行，以及设置一个作业管理器来控制和监视作业执行。 [详细了解](/azure/batch/batch-api-basics)如何使用这些对象来运行大规模并行计算解决方案。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-batch</artifactId>
    <version>2.0.0</version>
</dependency>
```   

### <a name="example"></a>示例

在 Batch 帐户中设置 Linux 计算节点池：

```java
// create the batch client for an account using its URI and keys
BatchClient client = BatchClient.open(new BatchSharedKeyCredentials("https://fabrikambatch.eastus.batch.azure.com", "fabrikambatch", batchKey));

// configure a pool of VMs to use 
VirtualMachineConfiguration configuration = new VirtualMachineConfiguration();
configuration.withNodeAgentSKUId("batch.node.ubuntu 16.04");
client.poolOperations().createPool(poolId, poolVMSize, configuration, poolVMCount);
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/batch/client)


## <a name="management-api"></a>管理 API

使用 Azure Batch 管理库可以创建和删除 Batch 帐户、读取和重新生成 Batch 帐户密钥，以及管理 Batch 帐户存储。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-batch</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>示例

创建 Azure Batch 帐户并为其配置新的应用程序和 Azure 存储帐户。

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
> [了解管理 API](/java/api/overview/azure/batch/management)


## <a name="samples"></a>示例

[管理 Batch 帐户][1]   

详细了解可在应用中使用的 [Azure Batch 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=batch)。

[1]: https://github.com/Azure-Samples/batch-java-manage-batch-accounts