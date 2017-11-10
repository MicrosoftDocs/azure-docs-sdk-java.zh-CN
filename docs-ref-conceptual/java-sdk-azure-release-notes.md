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
ms.openlocfilehash: 924ccf9bdaad4bc635f133adbcfcc8f797d06644
ms.sourcegitcommit: acc83bb537d77568b2a5427479d6354d6ae30885
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2017
---
# <a name="release-notes"></a><span data-ttu-id="480c0-104">发行说明</span><span class="sxs-lookup"><span data-stu-id="480c0-104">Release Notes</span></span> 

## <a name="october-5-2017---130"></a><span data-ttu-id="480c0-105">2017 年 10 月 5 日 - 1.3.0</span><span class="sxs-lookup"><span data-stu-id="480c0-105">October 5, 2017 - 1.3.0</span></span> 

<span data-ttu-id="480c0-106">版本 1.3.0 与先前版本中已达到正式版（稳定版）发布阶段的服务和功能向后兼容。</span><span class="sxs-lookup"><span data-stu-id="480c0-106">Version 1.3.0 is backwards compatible with previous versions for services and features use that reached the general availability (stable) stage in previous releases.</span></span>

<span data-ttu-id="480c0-107">预览版中这些服务的任何重大更改带有 @Beta 批注。</span><span class="sxs-lookup"><span data-stu-id="480c0-107">Any breaking changes from Preview versions for those services are marked with the @Beta annotation.</span></span>

<span data-ttu-id="480c0-108">若要将代码迁移到 1.3.0，可以参考[这些说明](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md)准备在 1.3 版本中使用现有代码。</span><span class="sxs-lookup"><span data-stu-id="480c0-108">If you are migrating your code to 1.3.0, you can use [these notes](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) to prepare your existing code for the 1.3 version.</span></span>

### <a name="generally-availabile-in-v13"></a><span data-ttu-id="480c0-109">V1.3 中的正式版</span><span class="sxs-lookup"><span data-stu-id="480c0-109">Generally availabile in V1.3</span></span>

<span data-ttu-id="480c0-110">先前版本中仍处于 Beta 版状态的某些 API 现已推出正式版，具体而言：</span><span class="sxs-lookup"><span data-stu-id="480c0-110">Some of the APIs that were still in Beta in previous releases are now GA, in particular:</span></span>

- <span data-ttu-id="480c0-111">异步方法</span><span class="sxs-lookup"><span data-stu-id="480c0-111">async methods</span></span>
- <span data-ttu-id="480c0-112">CDN 中以前处于 Beta 版状态的所有方法</span><span class="sxs-lookup"><span data-stu-id="480c0-112">all methods in CDN that were previously in Beta</span></span>
- <span data-ttu-id="480c0-113">应用程序网关中以前处于 Beta 版状态的所有方法和接口</span><span class="sxs-lookup"><span data-stu-id="480c0-113">all methods and interfaces in Application Gateways that were previously in Beta</span></span>

 <span data-ttu-id="480c0-114">库的某些部件仍处于预览版状态。</span><span class="sxs-lookup"><span data-stu-id="480c0-114">Some parts of the library are still in Preview.</span></span> <span data-ttu-id="480c0-115">请参阅下表了解库的当前状态：</span><span class="sxs-lookup"><span data-stu-id="480c0-115">Refer to the table below for the current state of the libraries:</span></span>

<span data-ttu-id="480c0-116">服务或功能</span><span class="sxs-lookup"><span data-stu-id="480c0-116">Service or feature</span></span> | <span data-ttu-id="480c0-117">以正式版提供</span><span class="sxs-lookup"><span data-stu-id="480c0-117">Available as GA</span></span> | <span data-ttu-id="480c0-118">以预览版提供</span><span class="sxs-lookup"><span data-stu-id="480c0-118">Available as Preview</span></span> 
---------|---------|---------|-
<span data-ttu-id="480c0-119">计算</span><span class="sxs-lookup"><span data-stu-id="480c0-119">Compute</span></span>  | <span data-ttu-id="480c0-120">虚拟机和 VM 扩展、虚拟机规模集、托管磁盘</span><span class="sxs-lookup"><span data-stu-id="480c0-120">Virtual machines and VM extensions, Virtual machine scale sets, managed disks</span></span>   | <span data-ttu-id="480c0-121">Azure 容器服务、Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="480c0-121">Azure container service, Azure container registry</span></span> 
<span data-ttu-id="480c0-122">存储</span><span class="sxs-lookup"><span data-stu-id="480c0-122">Storage</span></span>   |  <span data-ttu-id="480c0-123">存储帐户</span><span class="sxs-lookup"><span data-stu-id="480c0-123">Storage accounts</span></span>       |    <span data-ttu-id="480c0-124">加密</span><span class="sxs-lookup"><span data-stu-id="480c0-124">Encryption</span></span>     
<span data-ttu-id="480c0-125">SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="480c0-125">SQL Database</span></span>  | <span data-ttu-id="480c0-126">数据库、防火墙、弹性池</span><span class="sxs-lookup"><span data-stu-id="480c0-126">Databases, firewalls, elastic pools</span></span>              
<span data-ttu-id="480c0-127">联网</span><span class="sxs-lookup"><span data-stu-id="480c0-127">Networking</span></span>    |  <span data-ttu-id="480c0-128">虚拟网络、网络接口、IP 地址、路由表、网络安全组、DNS、流量管理器、应用程序网关</span><span class="sxs-lookup"><span data-stu-id="480c0-128">Virtual networks , network interfaces , IP addresses ,routing tables, network security groups , DNS, Traffic managers, Application gateways</span></span>  |    <span data-ttu-id="480c0-129">负载均衡器、网络对等互连、虚拟网络网关、网络观察程序</span><span class="sxs-lookup"><span data-stu-id="480c0-129">Load balancers, Network peering, Virtual Network Gateway, Network watchers</span></span> 
<span data-ttu-id="480c0-130">其他服务</span><span class="sxs-lookup"><span data-stu-id="480c0-130">More services</span></span>    |  <span data-ttu-id="480c0-131">资源管理器、Key Vault、Redis、CDN、Batch</span><span class="sxs-lookup"><span data-stu-id="480c0-131">Resource Manager, Key Vault, Redis,  CDN, Batch</span></span>       |  <span data-ttu-id="480c0-132">Web 应用、函数应用、服务总线、Graph RBAC、Cosmos DB、搜索</span><span class="sxs-lookup"><span data-stu-id="480c0-132">Web apps, Function apps, Service Bus, Graph RBAC, Cosmos DB, Search</span></span>  
<span data-ttu-id="480c0-133">基本</span><span class="sxs-lookup"><span data-stu-id="480c0-133">Fundamentals</span></span>     |   <span data-ttu-id="480c0-134">身份验证 - 核心、异步方法、托管服务标识</span><span class="sxs-lookup"><span data-stu-id="480c0-134">Authentication - core , Async methods , Managed Service Identity</span></span>      |      |

> <span data-ttu-id="480c0-135">预览版功能在库中的类、接口或方法级别带有 `@Beta` 批注。</span><span class="sxs-lookup"><span data-stu-id="480c0-135">Preview features are marked with a `@Beta` annotation at the class or interface or method level in libraries.</span></span> <span data-ttu-id="480c0-136">这些功能随时会变化。</span><span class="sxs-lookup"><span data-stu-id="480c0-136">These features are subject to change.</span></span> <span data-ttu-id="480c0-137">将来可能会以任何方式对其进行修改甚至删除。</span><span class="sxs-lookup"><span data-stu-id="480c0-137">They can be modified in any way, or even removed, in the future.</span></span>

### <a name="import-with-maven"></a><span data-ttu-id="480c0-138">使用 Maven 导入</span><span class="sxs-lookup"><span data-stu-id="480c0-138">Import with Maven</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a><span data-ttu-id="480c0-139">获取帮助和提供反馈</span><span class="sxs-lookup"><span data-stu-id="480c0-139">Get help and give feedback</span></span>

<span data-ttu-id="480c0-140">请查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社区文章来帮助自己在代码中使用库。</span><span class="sxs-lookup"><span data-stu-id="480c0-140">Check out the [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) community for help using the libraries in your own code.</span></span> <span data-ttu-id="480c0-141">如果遇到任何 bug 或者有改进这些库的建议，请通过 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出问题。</span><span class="sxs-lookup"><span data-stu-id="480c0-141">If you encounter any bugs or have suggestions to improve these libraries, please file issues via [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).</span></span>


