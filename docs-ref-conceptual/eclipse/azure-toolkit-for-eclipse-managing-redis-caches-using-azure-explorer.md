---
title: 使用用于 Eclipse 的 Azure 资源管理器管理 Redis 缓存
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure redis 缓存。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 2d3f2363bd0b41808cd409417327b924cb86d85b
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954818"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="eec22-103">使用用于 Eclipse 的 Azure 资源管理器管理 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="eec22-103">Managing Redis Caches using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="eec22-104">Azure 资源管理器是用于 Eclipse 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse IDE 内部管理其 Azure 帐户中的 Redis 缓存。</span><span class="sxs-lookup"><span data-stu-id="eec22-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the Eclipse IDE.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a><span data-ttu-id="eec22-105">使用 Eclipse 创建 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="eec22-105">Create a Redis Cache by using Eclipse</span></span>

<span data-ttu-id="eec22-106">以下步骤将引导你完成使用 Azure 资源管理器创建 Redis 缓存的步骤。</span><span class="sxs-lookup"><span data-stu-id="eec22-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="eec22-107">按照 [用于 Eclipse 的 Azure 工具包的登录说明] 一文中的步骤登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="eec22-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for Eclipse] article.</span></span>

1. <span data-ttu-id="eec22-108">在“Azure 资源管理器”工具窗口中，展开“Azure”节点，右键单击“Redis 缓存”，然后单击“创建 Redis 缓存”。</span><span class="sxs-lookup"><span data-stu-id="eec22-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![“创建 Redis 缓存”菜单][CR01]

1. <span data-ttu-id="eec22-110">当“新 Redis 缓存”对话框出现时，指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="eec22-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![“创建新的 Redis 缓存”对话框][CR02]

   <span data-ttu-id="eec22-112">a.</span><span class="sxs-lookup"><span data-stu-id="eec22-112">a.</span></span> <span data-ttu-id="eec22-113">**DNS 名称**：为新的 Redis 缓存指定 DNS 子域，该子域名称将添加到“redis.cache.windows.net”之前，例如：*wingtiptoys.redis.cache.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="eec22-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which is prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="eec22-114">b.</span><span class="sxs-lookup"><span data-stu-id="eec22-114">b.</span></span> <span data-ttu-id="eec22-115">订阅：指定要用于新的 redis 缓存的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="eec22-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="eec22-116">c.</span><span class="sxs-lookup"><span data-stu-id="eec22-116">c.</span></span> <span data-ttu-id="eec22-117">资源组：为 redis 缓存指定资源组；需要选择以下一种选项：</span><span class="sxs-lookup"><span data-stu-id="eec22-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span>
      * <span data-ttu-id="eec22-118">**新建**：指定要创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="eec22-118">**Create New**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="eec22-119">使用现有资源组：指定将从与 Azure 帐户关联的资源组列表中选择。</span><span class="sxs-lookup"><span data-stu-id="eec22-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="eec22-120">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="eec22-120">d.</span></span> <span data-ttu-id="eec22-121">位置：指定创建 redis 缓存的位置，例如：美国西部。</span><span class="sxs-lookup"><span data-stu-id="eec22-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="eec22-122">e.</span><span class="sxs-lookup"><span data-stu-id="eec22-122">e.</span></span> <span data-ttu-id="eec22-123">定价层： 指定 redis 缓存使用的定价层；此设置将确定客户端连接数。</span><span class="sxs-lookup"><span data-stu-id="eec22-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="eec22-124">（有关详细信息，请参阅 [Redis 缓存定价]。）</span><span class="sxs-lookup"><span data-stu-id="eec22-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="eec22-125">f.</span><span class="sxs-lookup"><span data-stu-id="eec22-125">f.</span></span> <span data-ttu-id="eec22-126">非 SSL 端口：指定 redis 缓存是否允许非 SSL 连接；默认情况下，仅允许 SSL 连接。</span><span class="sxs-lookup"><span data-stu-id="eec22-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="eec22-127">指定 redis 缓存的所有设置后，单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="eec22-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="eec22-128">redis 缓存创建完成后，将显示在 Azure 资源管理器中。</span><span class="sxs-lookup"><span data-stu-id="eec22-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Azure 资源管理器中的 Redis 缓存][CR03]

> [!NOTE]
>
> <span data-ttu-id="eec22-130">有关配置 Azure redis 缓存设置的详细信息，请参阅[如何配置 Azure Redis 缓存]。</span><span class="sxs-lookup"><span data-stu-id="eec22-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-eclipse"></a><span data-ttu-id="eec22-131">在 Eclipse 中显示 Redis 缓存的属性</span><span class="sxs-lookup"><span data-stu-id="eec22-131">Display the properties for your Redis Cache in Eclipse</span></span>

1. <span data-ttu-id="eec22-132">在 Azure 资源管理器中，右键单击 redis 缓存，然后单击“显示属性”。</span><span class="sxs-lookup"><span data-stu-id="eec22-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![用于显示 redis 缓存属性的 Azure 资源管理器上下文菜单][SP01]

1. <span data-ttu-id="eec22-134">Azure 资源管理器会显示 redis 缓存的属性。</span><span class="sxs-lookup"><span data-stu-id="eec22-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Redis 缓存属性][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a><span data-ttu-id="eec22-136">使用 Eclipse 删除 Redis 缓存</span><span class="sxs-lookup"><span data-stu-id="eec22-136">Delete your Redis Cache by using Eclipse</span></span>

1. <span data-ttu-id="eec22-137">在 Azure 资源管理器中，右键单击 redis 缓存，然后单击“删除”。</span><span class="sxs-lookup"><span data-stu-id="eec22-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![用于删除 redis 缓存属性的 Azure 资源管理器上下文菜单][DE01]

1. <span data-ttu-id="eec22-139">提示删除 redis 缓存时，单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="eec22-139">Click **OK** when prompted to delete your redis cache.</span></span>

   ![“删除 redis 缓存”提示][DE02]

## <a name="next-steps"></a><span data-ttu-id="eec22-141">后续步骤</span><span class="sxs-lookup"><span data-stu-id="eec22-141">Next steps</span></span>

<span data-ttu-id="eec22-142">有关 Azure redis 缓存、配置设置和定价的详细信息，请参阅以下链接：</span><span class="sxs-lookup"><span data-stu-id="eec22-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="eec22-143">[Azure Redis 缓存]</span><span class="sxs-lookup"><span data-stu-id="eec22-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="eec22-144">[Redis 缓存文档]</span><span class="sxs-lookup"><span data-stu-id="eec22-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="eec22-145">[Redis 缓存定价]</span><span class="sxs-lookup"><span data-stu-id="eec22-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="eec22-146">[如何配置 Azure Redis 缓存]</span><span class="sxs-lookup"><span data-stu-id="eec22-146">[How to configure Azure Redis Cache]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Redis 缓存定价]: https://azure.microsoft.com/pricing/details/cache/
[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis 缓存]: https://azure.microsoft.com/services/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis 缓存文档]: /azure/redis-cache/
[Redis Cache Documentation]: /azure/redis-cache/
[如何配置 Azure Redis 缓存]: /azure/redis-cache/cache-configure
[How to configure Azure Redis Cache]: /azure/redis-cache/cache-configure

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
