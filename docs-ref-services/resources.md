---
title: "用于 Java 的 Azure 资源管理器库"
description: "Java 资源管理器库的参考文档"
keywords: "Azure, Java, SDK, API, 资源组, arm, 资源管理器"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 56199b87fa64e9cbf0a14716a58c01f11f0e433b
ms.sourcegitcommit: 4b63ecd2c92a9115dfae018618e4e4046b061b3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a><span data-ttu-id="cc93c-104">用于 Java 的 Azure 资源管理器库</span><span class="sxs-lookup"><span data-stu-id="cc93c-104">Azure Resource Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="cc93c-105">概述</span><span class="sxs-lookup"><span data-stu-id="cc93c-105">Overview</span></span>

<span data-ttu-id="cc93c-106">使用 [Azure 资源管理器](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)部署、监视和管理组中的资源。</span><span class="sxs-lookup"><span data-stu-id="cc93c-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="cc93c-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="cc93c-107">Management API</span></span>

<span data-ttu-id="cc93c-108">使用管理 API 通过模板创建资源组和部署资源。</span><span class="sxs-lookup"><span data-stu-id="cc93c-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

<span data-ttu-id="cc93c-109">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="cc93c-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="cc93c-110">示例</span><span class="sxs-lookup"><span data-stu-id="cc93c-110">Example</span></span>

<span data-ttu-id="cc93c-111">在 Azure 美国东部区域中创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="cc93c-111">Create a new resource group in the Azure Eastern US region.</span></span>

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="cc93c-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="cc93c-112">Explore the Management APIs</span></span>](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a><span data-ttu-id="cc93c-113">示例</span><span class="sxs-lookup"><span data-stu-id="cc93c-113">Samples</span></span>

<span data-ttu-id="cc93c-114">[使用 Java 管理 Azure 资源组][1] 
[使用 ARM 模板部署资源][2]</span><span class="sxs-lookup"><span data-stu-id="cc93c-114">[Manage Azure Resource Groups with Java][1] 
[Deploy resources using an ARM template][2]</span></span>

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

<span data-ttu-id="cc93c-115">查看 Azure 资源管理器示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)。</span><span class="sxs-lookup"><span data-stu-id="cc93c-115">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=resource) of Azure Resource Manager samples.</span></span>
