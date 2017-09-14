---
title: "使用用于 IntelliJ 的 Azure 资源管理器管理 Redis 缓存"
description: "了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure redis 缓存。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: f8b2bc7adce71a27f1a059ef8ef691e52f66157e
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="cc60a-103">使用用于 IntelliJ 的 Azure 资源管理器管理 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="cc60a-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="cc60a-104">Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易于使用的解决方案，用于从 IntelliJ IDE 内部管理其 Azure 帐户中的 redis 缓存。</span><span class="sxs-lookup"><span data-stu-id="cc60a-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="cc60a-105">使用 IntelliJ 创建 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="cc60a-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="cc60a-106">以下步骤将引导完成使用 Azure 资源管理器创建 redis 缓存的步骤。</span><span class="sxs-lookup"><span data-stu-id="cc60a-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="cc60a-107">按照[用于 IntelliJ 的 Azure 工具包的登录说明]一文中的步骤登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="cc60a-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="cc60a-108">在“Azure 资源管理器”工具窗口中，展开“Azure”节点，右键单击“Redis 缓存”，然后单击“创建 Redis 缓存”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![创建 Redis 缓存菜单][CR01]

1. <span data-ttu-id="cc60a-110">当“新建 Redis 缓存”对话框出现时，请指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="cc60a-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![“新建 Redis 缓存”对话框][CR02]

   <span data-ttu-id="cc60a-112">a.</span><span class="sxs-lookup"><span data-stu-id="cc60a-112">a.</span></span> <span data-ttu-id="cc60a-113">DNS 名称：为新的 redis 缓存指定 DNS 子域，预设为“redis.cache.windows.net”，例如：wingtiptoys.redis.cache.windows.net。</span><span class="sxs-lookup"><span data-stu-id="cc60a-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="cc60a-114">b.</span><span class="sxs-lookup"><span data-stu-id="cc60a-114">b.</span></span> <span data-ttu-id="cc60a-115">订阅：指定要用于新的 redis 缓存的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="cc60a-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="cc60a-116">c.</span><span class="sxs-lookup"><span data-stu-id="cc60a-116">c.</span></span> <span data-ttu-id="cc60a-117">资源组：为 redis 缓存指定资源组；需要选择以下一种选项：</span><span class="sxs-lookup"><span data-stu-id="cc60a-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span> 
      * <span data-ttu-id="cc60a-118">**新建**：指定要创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="cc60a-118">**Create New**: Specifies that you want to create a new resource group.</span></span> 
      * <span data-ttu-id="cc60a-119">使用现有资源组：指定将从与 Azure 帐户关联的资源组列表中选择。</span><span class="sxs-lookup"><span data-stu-id="cc60a-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span> 

   <span data-ttu-id="cc60a-120">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-120">d.</span></span> <span data-ttu-id="cc60a-121">位置：指定创建 redis 缓存的位置，例如：美国西部。</span><span class="sxs-lookup"><span data-stu-id="cc60a-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="cc60a-122">e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-122">e.</span></span> <span data-ttu-id="cc60a-123">定价层： 指定 redis 缓存使用的定价层；此设置将确定客户端连接数。</span><span class="sxs-lookup"><span data-stu-id="cc60a-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="cc60a-124">（有关详细信息，请参阅 [Redis 缓存定价]。）</span><span class="sxs-lookup"><span data-stu-id="cc60a-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="cc60a-125">f.</span><span class="sxs-lookup"><span data-stu-id="cc60a-125">f.</span></span> <span data-ttu-id="cc60a-126">非 SSL 端口：指定 redis 缓存是否允许非 SSL 连接；默认情况下，仅允许 SSL 连接。</span><span class="sxs-lookup"><span data-stu-id="cc60a-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="cc60a-127">指定 redis 缓存的所有设置后，单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="cc60a-128">redis 缓存创建完成后，将显示在 Azure 资源管理器中。</span><span class="sxs-lookup"><span data-stu-id="cc60a-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Azure 资源管理器中的 Redis 缓存][CR03]

> [!NOTE]
>
> <span data-ttu-id="cc60a-130">有关配置 Azure redis 缓存设置的详细信息，请参阅[如何配置 Azure Redis 缓存]。</span><span class="sxs-lookup"><span data-stu-id="cc60a-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="cc60a-131">在 IntelliJ 中显示 Redis 缓存属性</span><span class="sxs-lookup"><span data-stu-id="cc60a-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="cc60a-132">在 Azure 资源管理器中，右键单击 redis 缓存，然后单击“显示属性”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![用于显示 redis 缓存属性的 Azure 资源管理器上下文菜单][SP01]

1. <span data-ttu-id="cc60a-134">Azure 资源管理器会显示 redis 缓存的属性。</span><span class="sxs-lookup"><span data-stu-id="cc60a-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Redis 缓存属性][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="cc60a-136">使用 IntelliJ 删除 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="cc60a-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="cc60a-137">在 Azure 资源管理器中，右键单击 redis 缓存，然后单击“删除”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![使用 Azure 资源管理器上下文菜单删除 redis 缓存][DE01]

1. <span data-ttu-id="cc60a-139">系统提示删除 redis 缓存时，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="cc60a-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![删除 redis 缓存提示][DE02]

## <a name="next-steps"></a><span data-ttu-id="cc60a-141">后续步骤</span><span class="sxs-lookup"><span data-stu-id="cc60a-141">Next steps</span></span>

<span data-ttu-id="cc60a-142">有关 Azure redis 缓存、配置设置和定价的详细信息，请参阅以下链接：</span><span class="sxs-lookup"><span data-stu-id="cc60a-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="cc60a-143">[Azure Redis 缓存]</span><span class="sxs-lookup"><span data-stu-id="cc60a-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="cc60a-144">[Redis 缓存文档]</span><span class="sxs-lookup"><span data-stu-id="cc60a-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="cc60a-145">[Redis 缓存定价]</span><span class="sxs-lookup"><span data-stu-id="cc60a-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="cc60a-146">[如何配置 Azure Redis 缓存]</span><span class="sxs-lookup"><span data-stu-id="cc60a-146">[How to configure Azure Redis Cache]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Redis 缓存定价]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis 缓存]: https://azure.microsoft.com/services/cache/
[Redis 缓存文档]: /azure/redis-cache
[如何配置 Azure Redis 缓存]: /azure/redis-cache/cache-configure
[用于 IntelliJ 的 Azure 工具包的登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
