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
# <a name="azure-cdn-libraries-for-java"></a><span data-ttu-id="55b1b-104">用于 Java 的 Azure CDN 库</span><span class="sxs-lookup"><span data-stu-id="55b1b-104">Azure CDN libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="55b1b-105">概述</span><span class="sxs-lookup"><span data-stu-id="55b1b-105">Overview</span></span>

<span data-ttu-id="55b1b-106">使用 [Azure 内容交付网络](/azure/cdn/cdn-overview) (CDN) 将静态 Web 内容缓存在按特定策略布置好的位置，以便为用户提供最大的吞吐量。</span><span class="sxs-lookup"><span data-stu-id="55b1b-106">Cache static web content at strategically placed locations to provide maximum throughput for users with [Azure Content Delivery Network](/azure/cdn/cdn-overview) (CDN).</span></span>

<span data-ttu-id="55b1b-107">若要开始使用 Azure CDN，请参阅 [Azure CDN 入门](/azure/cdn/cdn-create-new-endpoint)。</span><span class="sxs-lookup"><span data-stu-id="55b1b-107">To get started with Azure CDN, see [Getting started with Azure CDN](/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-api"></a><span data-ttu-id="55b1b-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="55b1b-108">Management API</span></span>

<span data-ttu-id="55b1b-109">使用管理 API 创建 CDN 配置文件、定义终结点，并将内容添加到 CDN。</span><span class="sxs-lookup"><span data-stu-id="55b1b-109">Create CDN profiles, define endpoints, and add content to the CDN using the management API.</span></span>

<span data-ttu-id="55b1b-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="55b1b-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-cdn</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="55b1b-111">示例</span><span class="sxs-lookup"><span data-stu-id="55b1b-111">Example</span></span>

<span data-ttu-id="55b1b-112">创建 CDN 配置文件、分配终结点，并将内容载入 CDN。</span><span class="sxs-lookup"><span data-stu-id="55b1b-112">Create a CDN profile, assign endpoints, and load content into the CDN.</span></span>

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
> [<span data-ttu-id="55b1b-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="55b1b-113">Explore the Management APIs</span></span>](/java/api/overview/azure/cdn/managementapi)

## <a name="samples"></a><span data-ttu-id="55b1b-114">示例</span><span class="sxs-lookup"><span data-stu-id="55b1b-114">Samples</span></span>

[<span data-ttu-id="55b1b-115">使用 Java 管理 CDN</span><span class="sxs-lookup"><span data-stu-id="55b1b-115">Manage CDNs with Java</span></span>](https://github.com/Azure-Samples/cdn-java-manage-cdn)

<span data-ttu-id="55b1b-116">详细了解可在应用中使用的 [Azure CDN 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn)。</span><span class="sxs-lookup"><span data-stu-id="55b1b-116">Explore more [sample Java code for Azure CDN](https://azure.microsoft.com/resources/samples/?platform=java&term=cdn) you can use in your apps.</span></span>