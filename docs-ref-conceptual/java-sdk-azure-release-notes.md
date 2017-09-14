---
title: "用于 Java 的 Azure 管理库发行说明 | Microsoft Docs"
description: "了解用于 Java 的 Azure 管理库的新增功能，并留意其中的重大更改"
keywords: "Azure,  Java, API, 参考, 说明, 更新, 弃用"
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: c0d5c4b3702d3bee4e93de51cec36e72aeaf598f
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="release-notes"></a><span data-ttu-id="43480-104">发行说明</span><span class="sxs-lookup"><span data-stu-id="43480-104">Release Notes</span></span> 

## <a name="june-30-2017---110"></a><span data-ttu-id="43480-105">2017 年 6 月 30 日 - 1.1.0</span><span class="sxs-lookup"><span data-stu-id="43480-105">June 30, 2017 - 1.1.0</span></span> 

<span data-ttu-id="43480-106">V1.1 与 V1.0 中已达到正式版（稳定版）发行阶段的、旨在供大众使用的 API 向后兼容。</span><span class="sxs-lookup"><span data-stu-id="43480-106">V1.1 is backwards compatible with V1.0 in the APIs intended for public use that reached the general availability (stable) stage in V1.0.</span></span>

<span data-ttu-id="43480-107">V.0 中标有 @Beta 注释的 API 引入了一些重大更改</span><span class="sxs-lookup"><span data-stu-id="43480-107">Some breaking changes were introduced in APIs marked with the @Beta annotation in V.0</span></span>

<span data-ttu-id="43480-108">若要将代码迁移到 1.1.0，可以参考[这些说明](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)准备将代码从 1.0.0 迁移到 1.1.0。</span><span class="sxs-lookup"><span data-stu-id="43480-108">If you are migrating your code to 1.1.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md) for preparing your code for 1.1.0 from 1.0.0.</span></span>

### <a name="generally-availabile-in-v11"></a><span data-ttu-id="43480-109">V1.1 中的正式版</span><span class="sxs-lookup"><span data-stu-id="43480-109">Generally availabile in V1.1</span></span>

<span data-ttu-id="43480-110">V1.0 中仍处于 Beta 版状态的某些 API 在 V1.1 中现已推出正式版，具体而言：</span><span class="sxs-lookup"><span data-stu-id="43480-110">Some of the APIs that were still in Beta in V1.0 are now GA in V1.1, in particular:</span></span>

- <span data-ttu-id="43480-111">异步方法</span><span class="sxs-lookup"><span data-stu-id="43480-111">async methods</span></span>
- <span data-ttu-id="43480-112">CDN 中以前处于 Beta 版状态的所有方法</span><span class="sxs-lookup"><span data-stu-id="43480-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="43480-113">应用程序网关中以前处于 Beta 版状态的所有方法和接口</span><span class="sxs-lookup"><span data-stu-id="43480-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="43480-114">库的某些部件仍处于预览版状态。</span><span class="sxs-lookup"><span data-stu-id="43480-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="43480-115">请参阅下表了解库的当前状态：</span><span class="sxs-lookup"><span data-stu-id="43480-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="43480-116">服务或功能</span><span class="sxs-lookup"><span data-stu-id="43480-116">Service or feature</span></span> | <span data-ttu-id="43480-117">以正式版提供</span><span class="sxs-lookup"><span data-stu-id="43480-117">Available as GA</span></span> | <span data-ttu-id="43480-118">以预览版提供</span><span class="sxs-lookup"><span data-stu-id="43480-118">Available as Preview</span></span>  | <span data-ttu-id="43480-119">即将支持</span><span class="sxs-lookup"><span data-stu-id="43480-119">Coming soon</span></span> |
---------|---------|---------|---------|
<span data-ttu-id="43480-120">计算</span><span class="sxs-lookup"><span data-stu-id="43480-120">Compute</span></span>  | <span data-ttu-id="43480-121">虚拟机和 VM 扩展、虚拟机规模集、托管磁盘</span><span class="sxs-lookup"><span data-stu-id="43480-121">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="43480-122">Azure 容器服务、Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="43480-122">Azure container service, Azure container registry</span></span> |    |
<span data-ttu-id="43480-123">存储</span><span class="sxs-lookup"><span data-stu-id="43480-123">Storage</span></span>   |  <span data-ttu-id="43480-124">存储帐户</span><span class="sxs-lookup"><span data-stu-id="43480-124">Storage accounts</span></span>       |         |   <span data-ttu-id="43480-125">加密</span><span class="sxs-lookup"><span data-stu-id="43480-125">Encryption</span></span>      |
<span data-ttu-id="43480-126">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="43480-126">SQL Database</span></span>  | <span data-ttu-id="43480-127">数据库、防火墙、弹性池</span><span class="sxs-lookup"><span data-stu-id="43480-127">Databases, firewalls, elastic pools</span></span>        |         |   <span data-ttu-id="43480-128">其他功能</span><span class="sxs-lookup"><span data-stu-id="43480-128">More features</span></span>      |
<span data-ttu-id="43480-129">联网</span><span class="sxs-lookup"><span data-stu-id="43480-129">Networking</span></span>    |  <span data-ttu-id="43480-130">虚拟网络、网络接口、IP 地址、路由表、网络安全组、DNS、流量管理器、应用程序网关</span><span class="sxs-lookup"><span data-stu-id="43480-130">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="43480-131">负载均衡器</span><span class="sxs-lookup"><span data-stu-id="43480-131">Load balancers</span></span>     |   <span data-ttu-id="43480-132">VPN、网络观察程序</span><span class="sxs-lookup"><span data-stu-id="43480-132">VPN, Network watchers</span></span>   |
<span data-ttu-id="43480-133">其他服务</span><span class="sxs-lookup"><span data-stu-id="43480-133">More services</span></span>    |  <span data-ttu-id="43480-134">资源管理器、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="43480-134">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="43480-135">Web 应用、函数应用、服务总线、Graph RBAC、Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="43480-135">Web apps, Function apps, Service Bus, Graph RBAC, DocumentDB</span></span>   | <span data-ttu-id="43480-136">Monitor、计划程序、函数管理、搜索、其他 Graph RBAC 功能</span><span class="sxs-lookup"><span data-stu-id="43480-136">Monitor ,Scheduler, Functions management, Search, more Graph RBAC features</span></span>        |
<span data-ttu-id="43480-137">基本</span><span class="sxs-lookup"><span data-stu-id="43480-137">Fundamentals</span></span>     |   <span data-ttu-id="43480-138">身份验证 - 核心身份验证、异步方法</span><span class="sxs-lookup"><span data-stu-id="43480-138">Authentication - core , Async methods</span></span>       |      |         |

### <a name="import-with-maven"></a><span data-ttu-id="43480-139">使用 Maven 导入</span><span class="sxs-lookup"><span data-stu-id="43480-139">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="43480-140">获取帮助和提供反馈</span><span class="sxs-lookup"><span data-stu-id="43480-140">Get help and give feedback</span></span>

<span data-ttu-id="43480-141">请查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社区文章来帮助自己在代码中使用库。</span><span class="sxs-lookup"><span data-stu-id="43480-141">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="43480-142">如果遇到任何 bug 或者有改进这些库的建议，请通过 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出问题。</span><span class="sxs-lookup"><span data-stu-id="43480-142">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>

### <a name="migrate-from-previous-releases"></a><span data-ttu-id="43480-143">从以前的版本迁移</span><span class="sxs-lookup"><span data-stu-id="43480-143">Migrate from previous releases</span></span>

<span data-ttu-id="43480-144">[从 1.0.0-beta5 迁移](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)[从 1.1.0 迁移](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)</span><span class="sxs-lookup"><span data-stu-id="43480-144">[Migrate from 1.0.0-beta5](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)  [Migrate from 1.1.0](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)</span></span>


