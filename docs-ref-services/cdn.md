---
title: "用于 Java 的 Azure CDN 库"
description: "Java CDN 管理库的参考文档"
keywords: "Azure, Java, SDK, API, 内容, 分发, 网络, CDN"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: cdn
ms.openlocfilehash: 91df958d2d78fb4fd959c228b28c6ae003716be6
ms.sourcegitcommit: 634ab7578c73a219f8f3a2a6d43999d9d372cb43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2017
---
# <a name="azure-cdn-libraries-for-java"></a>用于 Java 的 Azure CDN 库

## <a name="overview"></a>概述

使用 [Azure 内容交付网络](/azure/cdn/cdn-overview) (CDN) 将静态 Web 内容缓存在按特定策略布置好的位置，以便为用户提供最大的吞吐量。

若要开始使用 Azure CDN，请参阅 [Azure CDN 入门](/azure/cdn/cdn-create-new-endpoint)。

## <a name="management-api"></a>管理 API

使用管理 API 创建 CDN 配置文件、定义终结点，并将内容添加到 CDN。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a>示例

创建 CDN 配置文件、分配终结点，并将内容载入 CDN。

```java
CdnProfile profile = azure.cdnProfiles().define("testCDN")
        .withRegion(Region.US_EAST)
        .withNewResourceGroup()
        .withStandardVerizonSku()
        .withNewEndpoint("webAppHostname1")
        .withNewEndpoint("webAppHostname2")
        .create();

List<String> contentList = new ArrayList<String>();
contentList.add("/client.js");
contentList.add("/media/fabrikam_logo.png");

for (CdnEndpoint endpoint : profile.endpoints().values()) {
    endpoint.loadContent(contentList);
}
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a>示例

[使用 Java 管理 CDN](https://github.com/Azure-Samples/cdn-java-manage-cdn)

详细了解可在应用中使用的 [Azure CDN 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)。