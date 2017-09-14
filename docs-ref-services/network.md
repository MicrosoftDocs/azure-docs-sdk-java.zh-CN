---
title: "用于 Java 的 Azure 网络库"
description: "Java Azure 网络管理库的参考文档"
keywords: "Azure, Java, SDK, API, 网络, 负载均衡, vnet, 子网"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: de03cf7c073c3a0dd636e23a4554987e7cad11f7
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="azure-network-libraries-for-java"></a><span data-ttu-id="86281-104">用于 Java 的 Azure 网络库</span><span class="sxs-lookup"><span data-stu-id="86281-104">Azure Network libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="86281-105">概述</span><span class="sxs-lookup"><span data-stu-id="86281-105">Overview</span></span>

<span data-ttu-id="86281-106">使用 [Azure 网络](/azure/networking/networking-overview)连接 Azure 资源、筛选和均衡流量，以及管理路由。</span><span class="sxs-lookup"><span data-stu-id="86281-106">Connect Azure resources, filter and balance traffic, and manage routing with [Azure Networking](/azure/networking/networking-overview).</span></span>

<span data-ttu-id="86281-107">若要开始使用 Azure 网络，请参阅[创建第一个虚拟网络](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。</span><span class="sxs-lookup"><span data-stu-id="86281-107">To get started with Azure Networking, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-api"></a><span data-ttu-id="86281-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="86281-108">Management API</span></span>

<span data-ttu-id="86281-109">使用管理 API 创建和管理 Azure [虚拟网络](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/) 和[应用程序网关](/azure/application-gateway/)。</span><span class="sxs-lookup"><span data-stu-id="86281-109">Create and manage Azure [virtual networks](/azure/virtual-network/virtual-networks-overview) , [ExpressRoutes](/azure/expressroute/) , and [Application Gateways](/azure/application-gateway/) with the management API.</span></span>

<span data-ttu-id="86281-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="86281-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.2.1</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="86281-111">示例</span><span class="sxs-lookup"><span data-stu-id="86281-111">Example</span></span>

<span data-ttu-id="86281-112">创建包含单个子网的新虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="86281-112">Create a new virtual network with a single subnet.</span></span>

```java
Network virtualNetwork1 = azure.networks().define(vnetName1)
        .withRegion(Region.US_EAST)
        .withExistingResourceGroup("myResourceGroup")
        .withAddressSpace("192.168.0.0/16")
        .defineSubnet("myVirtualNetwork")
            .withAddressPrefix("192.168.2.0/24")
            .attach()
        .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="86281-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="86281-113">Explore the Management APIs</span></span>](/java/api/overview/azure/networking/managementapi)

## <a name="samples"></a><span data-ttu-id="86281-114">示例</span><span class="sxs-lookup"><span data-stu-id="86281-114">Samples</span></span>

<span data-ttu-id="86281-115">[管理虚拟网络](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span><span class="sxs-lookup"><span data-stu-id="86281-115">[Manage virtual networks](https://github.com/Azure-Samples/network-java-manage-virtual-network) </span></span>  
<span data-ttu-id="86281-116">[管理网络接口](https://github.com/Azure-Samples/network-java-manage-network-interface) </span><span class="sxs-lookup"><span data-stu-id="86281-116">[Manage network interfaces](https://github.com/Azure-Samples/network-java-manage-network-interface) </span></span>  
<span data-ttu-id="86281-117">[管理应用程序网关](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span><span class="sxs-lookup"><span data-stu-id="86281-117">[Manage Application Gateways](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways) </span></span>  
[<span data-ttu-id="86281-118">管理面向 Internet 的负载均衡器</span><span class="sxs-lookup"><span data-stu-id="86281-118">Manage internet facing load balancers</span></span>](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

<span data-ttu-id="86281-119">详细了解可在应用中使用的 [Azure 网络示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=network)。</span><span class="sxs-lookup"><span data-stu-id="86281-119">Explore more [sample Java code for Azure Networking](https://azure.microsoft.com/resources/samples/?platform=java&term=network) you can use in your apps.</span></span>
