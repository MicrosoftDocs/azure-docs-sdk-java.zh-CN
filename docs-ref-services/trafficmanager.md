---
title: "用于 Java 的 Azure 流量管理器库"
description: "Java 流量管理器管理库的参考文档"
keywords: "Azure, Java, SDK, API, 负载均衡, 负载分配, 网络, 流量管理器"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: traffic-manager
ms.openlocfilehash: a38db9a0d92d7dedc5ff49b83ad2658aa61729dd
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="980d3-104">用于 Java 的 Azure 流量管理器库</span><span class="sxs-lookup"><span data-stu-id="980d3-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="980d3-105">概述</span><span class="sxs-lookup"><span data-stu-id="980d3-105">Overview</span></span>

<span data-ttu-id="980d3-106">使用 [Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)控制不同数据中心内服务终结点的用户流量分配。</span><span class="sxs-lookup"><span data-stu-id="980d3-106">Control the distribution of user traffic for service endpoints in different datacenters with [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview).</span></span>

<span data-ttu-id="980d3-107">若要开始使用 Azure 流量管理器，请参阅[创建流量管理器配置文件](/azure/traffic-manager/traffic-manager-create-profile)。</span><span class="sxs-lookup"><span data-stu-id="980d3-107">To get started with Azure Traffic Manager, see [Create a Traffic Manager profile](/azure/traffic-manager/traffic-manager-create-profile).</span></span>

## <a name="management-api"></a><span data-ttu-id="980d3-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="980d3-108">Management API</span></span>

<span data-ttu-id="980d3-109">使用管理 API 创建流量管理器配置文件、定义终结点和更改路由方法。</span><span class="sxs-lookup"><span data-stu-id="980d3-109">Create Traffic Manager profiles, define endpoints, and change the routing method with the management API.</span></span> 

<span data-ttu-id="980d3-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="980d3-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.2.1</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="980d3-111">示例</span><span class="sxs-lookup"><span data-stu-id="980d3-111">Example</span></span>

<span data-ttu-id="980d3-112">创建流量管理器配置文件并分配单个终结点。</span><span class="sxs-lookup"><span data-stu-id="980d3-112">Create a Traffic Manager profile and assign a single endpoint.</span></span>

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
> [<span data-ttu-id="980d3-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="980d3-113">Explore the Management APIs</span></span>](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a><span data-ttu-id="980d3-114">示例</span><span class="sxs-lookup"><span data-stu-id="980d3-114">Samples</span></span>

[<span data-ttu-id="980d3-115">跨多个区域均衡 Web 应用流量</span><span class="sxs-lookup"><span data-stu-id="980d3-115">Balance web app traffic across multiple regions</span></span>](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

<span data-ttu-id="980d3-116">详细了解可在应用中使用的 [Azure 流量管理器示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)。</span><span class="sxs-lookup"><span data-stu-id="980d3-116">Explore more [sample Java code for Azure Traffic Manager](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic) you can use in your apps.</span></span>
