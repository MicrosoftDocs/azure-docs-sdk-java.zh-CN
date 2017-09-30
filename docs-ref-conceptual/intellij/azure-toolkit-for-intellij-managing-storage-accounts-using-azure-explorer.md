---
title: "使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户"
description: "了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure 存储帐户。"
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
ms.openlocfilehash: 44d2ade8a0900b60222f06dfb9f93c1df17228c8
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="6970c-103">使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户</span><span class="sxs-lookup"><span data-stu-id="6970c-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="6970c-104">Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 IntelliJ 集成开发环境 (IDE) 内部管理其 Azure 帐户中的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="6970c-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="6970c-105">在 IntelliJ 中创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="6970c-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="6970c-106">若要使用 Azure 资源管理器创建存储帐户，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6970c-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="6970c-107">按照[用于 IntelliJ 的 Azure 工具包的登录说明]中的步骤登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="6970c-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for IntelliJ].</span></span> 

2. <span data-ttu-id="6970c-108">在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“存储帐户”，并单击“创建存储帐户”。</span><span class="sxs-lookup"><span data-stu-id="6970c-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![“创建存储帐户”命令][CS01]

3. <span data-ttu-id="6970c-110">在“创建存储帐户”对话框中，指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="6970c-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![“创建新存储帐户”对话框][CS02]

   * <span data-ttu-id="6970c-112">**名称**：指定要用于新存储帐户的名称。</span><span class="sxs-lookup"><span data-stu-id="6970c-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="6970c-113">**帐户类型**：指定要创建的存储帐户的类型（例如“Blob 存储”）。</span><span class="sxs-lookup"><span data-stu-id="6970c-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="6970c-114">有关详细信息，请参阅[关于 Azure 存储帐户]。</span><span class="sxs-lookup"><span data-stu-id="6970c-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="6970c-115">**性能**：指定要从所选发布者使用哪种存储帐户产品/服务（例如，“高级”）。</span><span class="sxs-lookup"><span data-stu-id="6970c-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="6970c-116">有关详细信息，请参阅 [Azure 存储可伸缩性和性能目标]。</span><span class="sxs-lookup"><span data-stu-id="6970c-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="6970c-117">**复制**：指定存储帐户的复制（例如“区域冗余”）。</span><span class="sxs-lookup"><span data-stu-id="6970c-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="6970c-118">有关详细信息，请参阅 [Azure 存储复制]。</span><span class="sxs-lookup"><span data-stu-id="6970c-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="6970c-119">**订阅**：指定要用于新存储帐户的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="6970c-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="6970c-120">**位置**：指定将创建存储帐户的位置（例如“美国西部”）。</span><span class="sxs-lookup"><span data-stu-id="6970c-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="6970c-121">**资源组**：指定虚拟机的资源组。</span><span class="sxs-lookup"><span data-stu-id="6970c-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="6970c-122">选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="6970c-122">Select one of the following options:</span></span>
      * <span data-ttu-id="6970c-123">**新建**：指定要创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="6970c-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="6970c-124">**使用现有**：指定将从与 Azure 帐户关联的资源组列表中进行选择。</span><span class="sxs-lookup"><span data-stu-id="6970c-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="6970c-125">指定了上述所有选项后，单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="6970c-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="6970c-126">在 IntelliJ 中创建存储容器</span><span class="sxs-lookup"><span data-stu-id="6970c-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="6970c-127">若要使用 Azure 资源管理器创建存储容器，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6970c-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="6970c-128">在 Azure 资源管理器视图中，右键单击要在其中创建容器的存储帐户，并单击“创建 Blob 容器”。</span><span class="sxs-lookup"><span data-stu-id="6970c-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![“创建 blob 容器”命令][CC01]

2. <span data-ttu-id="6970c-130">在“创建 Blob 容器”对话框中，指定容器的名称，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="6970c-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="6970c-131">有关命名存储容器的详细信息，请参阅[命名和引用容器、Blob 和元数据]。</span><span class="sxs-lookup"><span data-stu-id="6970c-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![“创建存储容器”对话框][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="6970c-133">删除 IntelliJ 中的存储容器</span><span class="sxs-lookup"><span data-stu-id="6970c-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="6970c-134">若要使用 Azure 资源管理器删除存储容器，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6970c-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="6970c-135">在 Azure 资源管理器视图中，右键单击存储容器，并单击“删除”。</span><span class="sxs-lookup"><span data-stu-id="6970c-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![“删除存储容器”命令][DC01]

2. <span data-ttu-id="6970c-137">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="6970c-137">In the confirmation window, click **Yes**.</span></span>

   ![删除存储容器确认窗口][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="6970c-139">删除 IntelliJ 中的存储帐户</span><span class="sxs-lookup"><span data-stu-id="6970c-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="6970c-140">若要使用 Azure 资源管理器删除存储帐户，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6970c-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="6970c-141">在“Azure 资源管理器”视图中，右键单击存储帐户，并选择“删除”。</span><span class="sxs-lookup"><span data-stu-id="6970c-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![“删除存储帐户”菜单][DS01]

2. <span data-ttu-id="6970c-143">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="6970c-143">In the confirmation window, click **Yes**.</span></span>

   ![删除存储帐户确认窗口][DS02]

## <a name="next-steps"></a><span data-ttu-id="6970c-145">后续步骤</span><span class="sxs-lookup"><span data-stu-id="6970c-145">Next steps</span></span>

<span data-ttu-id="6970c-146">有关 Azure 存储帐户大小和定价的详细信息，请参阅以下资源：</span><span class="sxs-lookup"><span data-stu-id="6970c-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="6970c-147">[Microsoft Azure 存储空间简介]</span><span class="sxs-lookup"><span data-stu-id="6970c-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="6970c-148">[关于 Azure 存储帐户]</span><span class="sxs-lookup"><span data-stu-id="6970c-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="6970c-149">Azure 存储帐户大小</span><span class="sxs-lookup"><span data-stu-id="6970c-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="6970c-150">[Azure 中的 Windows 存储帐户的大小]</span><span class="sxs-lookup"><span data-stu-id="6970c-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="6970c-151">[Azure 中的 Linux 存储帐户的大小]</span><span class="sxs-lookup"><span data-stu-id="6970c-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="6970c-152">Azure 存储帐户定价</span><span class="sxs-lookup"><span data-stu-id="6970c-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="6970c-153">[Windows 存储帐户定价]</span><span class="sxs-lookup"><span data-stu-id="6970c-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="6970c-154">[Linux 存储帐户定价]</span><span class="sxs-lookup"><span data-stu-id="6970c-154">[Linux storage-account pricing]</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包的登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Microsoft Azure 存储空间简介]: /azure/storage/storage-introduction
[关于 Azure 存储帐户]: /azure/storage/storage-create-storage-account
[Azure 存储复制]: /azure/storage/storage-redundancy
[Azure 存储可伸缩性和性能目标]: /azure/storage/storage-scalability-targets
[命名和引用容器、Blob 和元数据]: http://go.microsoft.com/fwlink/?LinkId=255555

[Azure 中的 Windows 存储帐户的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 存储帐户的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 存储帐户定价]: /pricing/details/virtual-machines/windows/
[Linux 存储帐户定价]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
