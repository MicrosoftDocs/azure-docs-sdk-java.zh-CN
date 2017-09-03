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
ms.openlocfilehash: 3b9c8f0e3bd5e8feb5a819a7c2583ffbbcf53e38
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-traffic-manager-libraries-for-java"></a>用于 Java 的 Azure 流量管理器库

## <a name="overview"></a>概述

使用 [Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)控制不同数据中心内服务终结点的用户流量分配。

若要开始使用 Azure 流量管理器，请参阅[创建流量管理器配置文件](/azure/traffic-manager/traffic-manager-create-profile)。

## <a name="management-api"></a>管理 API

使用管理 API 创建流量管理器配置文件、定义终结点和更改路由方法。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-trafficmanager</artifactId>
    <version>1.1.2</version>
</dependency>
```   

## <a name="example"></a>示例

创建流量管理器配置文件并分配单个终结点。

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
> [了解管理 API](/java/api/overview/azure/trafficmanager/managementapi)

## <a name="samples"></a>示例

[跨多个区域均衡 Web 应用流量](https://github.com/Azure-Samples/traffic-manager-java-manage-profiles)

详细了解可在应用中使用的 [Azure 流量管理器示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=traffic)。
