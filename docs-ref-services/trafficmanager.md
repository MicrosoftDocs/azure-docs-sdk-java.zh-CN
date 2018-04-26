---
title: 用于 Java 的 Azure 流量管理器库
description: Java 流量管理器管理库的参考文档
keywords: Azure, Java, SDK, API, 负载均衡, 负载分配, 网络, 流量管理器
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: fd78402f50df16ad7d57c0ca67815bfad5b18d51
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="e6f3e-104">用于 Java 的 Azure 流量管理器库</span><span class="sxs-lookup"><span data-stu-id="e6f3e-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="e6f3e-105">概述</span><span class="sxs-lookup"><span data-stu-id="e6f3e-105">Overview</span></span>

<span data-ttu-id="e6f3e-106">使用 [Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)控制不同数据中心内服务终结点的用户流量分配。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="e6f3e-107">若要开始使用 Azure 流量管理器，请参阅[创建流量管理器配置文件](/azure/traffic-manager/traffic-manager-create-profile)。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="e6f3e-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="e6f3e-108">Management API</span></span>

<span data-ttu-id="e6f3e-109">使用管理 API 创建流量管理器配置文件、定义终结点和更改路由方法。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="e6f3e-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.3.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="e6f3e-111">示例</span><span class="sxs-lookup"><span data-stu-id="e6f3e-111">Example</span></span>

<span data-ttu-id="e6f3e-112">创建流量管理器配置文件并分配单个终结点。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

```java
TrafficManagerProfile tmProfile = azure.trafficManagerProfiles().define("testTMProfile")
        .withNewResourceGroup(Region.US_EAST)
        .withLeafDomainLabel("testTMProfile")
        .withPriorityBasedRouting()
        .defineAzureTargetEndpoint("endpoint")
        .toResourceId(webAppId)
        .withRoutingPriority(1)
        .attach()
        .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e6f3e-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="e6f3e-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/management)

## <a name="samples"></a><span data-ttu-id="e6f3e-114">示例</span><span class="sxs-lookup"><span data-stu-id="e6f3e-114">Samples</span></span>

[<span data-ttu-id="e6f3e-115">跨多个区域均衡 Web 应用流量</span><span class="sxs-lookup"><span data-stu-id="e6f3e-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="e6f3e-116">详细了解可在应用中使用的 [Azure 流量管理器示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)。</span><span class="sxs-lookup"><span data-stu-id="e6f3e-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
