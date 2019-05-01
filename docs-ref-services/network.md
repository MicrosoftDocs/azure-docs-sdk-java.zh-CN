---
title: 用于 Java 的 Azure 网络库
description: Java Azure 网络管理库的参考文档
keywords: Azure, Java, SDK, API, 网络, 负载均衡, vnet, 子网
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: networking
ms.openlocfilehash: 4eb4f583da686508db9fb0c4346f2525502f669d
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593322"
---
# <a name="azure-network-libraries-for-java"></a>用于 Java 的 Azure 网络库

## <a name="overview"></a>概述

使用 [Azure 网络](/azure/networking/networking-overview)连接 Azure 资源、筛选和均衡流量，以及管理路由。

若要开始使用 Azure 网络，请参阅[创建第一个虚拟网络](/azure/virtual-network/virtual-network-get-started-vnet-subnet)。

## <a name="management-api"></a>管理 API

使用管理 API 创建和管理 Azure [虚拟网络](/azure/virtual-network/virtual-networks-overview)、[ExpressRoutes](/azure/expressroute/) 和[应用程序网关](/azure/application-gateway/)。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-network</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>示例

创建包含单个子网的新虚拟网络。

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
> [了解管理 API](/java/api/overview/azure/networking/management)

## <a name="samples"></a>示例

[管理虚拟网络](https://github.com/Azure-Samples/network-java-manage-virtual-network)   
[管理网络接口](https://github.com/Azure-Samples/network-java-manage-network-interface)   
[管理应用程序网关](https://github.com/Azure-Samples/application-gateway-java-manage-simple-application-gateways)   
[管理面向 Internet 的负载均衡器](https://github.com/Azure-Samples/network-java-manage-internet-facing-load-balancers)   

详细了解可在应用中使用的 [Azure 网络示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=network)。
