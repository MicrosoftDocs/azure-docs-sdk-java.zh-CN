---
title: 使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机
description: 了解如何使用用于 IntelliJ 的 Azure 资源管理器管理 Azure 虚拟机。
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/13/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 57441a9cbdf0805e08f303c1f05049ce7f668ac0
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338661"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="1239a-103">使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机</span><span class="sxs-lookup"><span data-stu-id="1239a-103">Manage virtual machines by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="1239a-104">Azure 资源管理器是用于 IntelliJ 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 IntelliJ 集成开发环境 (IDE) 内部管理其 Azure 帐户中的虚拟机。</span><span class="sxs-lookup"><span data-stu-id="1239a-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="1239a-105">在 IntelliJ 中创建虚拟机</span><span class="sxs-lookup"><span data-stu-id="1239a-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="1239a-106">若要使用 Azure 资源管理器创建虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1239a-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span> 

1. <span data-ttu-id="1239a-107">按照[用于 IntelliJ 的 Azure 工具包的登录说明]一文中的步骤登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="1239a-107">Sign in to your Azure account by using the steps in the [Sign-in instructions for the Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="1239a-108">在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“虚拟机”，并单击“创建 VM”。</span><span class="sxs-lookup"><span data-stu-id="1239a-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="1239a-109">创建 VM 命令![][CR01]</span><span class="sxs-lookup"><span data-stu-id="1239a-109">![The Create VM command][CR01]</span></span>  
    <span data-ttu-id="1239a-110">此时会打开“新建虚拟机”向导。</span><span class="sxs-lookup"><span data-stu-id="1239a-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="1239a-111">在“选择订阅”窗口中选择订阅，并单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1239a-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![“选择订阅”窗口][CR02]

4. <span data-ttu-id="1239a-113">在“选择虚拟机映像”窗口中输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1239a-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="1239a-114">**位置**：指定将创建虚拟机的位置（例如“美国西部”）。</span><span class="sxs-lookup"><span data-stu-id="1239a-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="1239a-115">**建议的映像**：指定要从常用映像的简化列表中选择映像。</span><span class="sxs-lookup"><span data-stu-id="1239a-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="1239a-116">**自定义映像**：指定要选择自定义映像，为此将需要提供以下信息：</span><span class="sxs-lookup"><span data-stu-id="1239a-116">**Custom image**: Specifies that you will choose a custom image by providing the following information:</span></span>

      * <span data-ttu-id="1239a-117">**发布者**：指定创建了用于创建虚拟机的映像的发布者（例如“Microsoft”）。</span><span class="sxs-lookup"><span data-stu-id="1239a-117">**Publisher**: Specifies the publisher that created the image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="1239a-118">**产品/服务**： 指定所选发布者提供的可以使用的虚拟机产品/服务（例如 *JDK* ）。</span><span class="sxs-lookup"><span data-stu-id="1239a-118">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="1239a-119">Sku：从所选产品/服务中指定要使用的库存单位 (SKU)（例如“JDK_8”）。</span><span class="sxs-lookup"><span data-stu-id="1239a-119">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="1239a-120">版本号：从所选 SKU 中指定要使用哪个版本。</span><span class="sxs-lookup"><span data-stu-id="1239a-120">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![“选择虚拟机映像”窗口][CR03]

5. <span data-ttu-id="1239a-122">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1239a-122">Click **Next**.</span></span> 

6. <span data-ttu-id="1239a-123">在“虚拟机基本设置”窗口中输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1239a-123">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="1239a-124">**虚拟机名称**：指定新虚拟机的名称，该名称必须以字母开头并仅包含字母、数字和连字符。</span><span class="sxs-lookup"><span data-stu-id="1239a-124">**Virtual machine name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="1239a-125">**大小**：指定要为虚拟机分配的内核数和内存。</span><span class="sxs-lookup"><span data-stu-id="1239a-125">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="1239a-126">**用户名**：指定要创建的用于管理虚拟机的管理员帐户。</span><span class="sxs-lookup"><span data-stu-id="1239a-126">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="1239a-127">**密码**和**确认**：指定管理员帐户的密码。</span><span class="sxs-lookup"><span data-stu-id="1239a-127">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![“虚拟机基本设置”窗口][CR04]

7. <span data-ttu-id="1239a-129">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1239a-129">Click **Next**.</span></span> 

8. <span data-ttu-id="1239a-130">在“关联的资源”窗口中，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1239a-130">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="1239a-131">**资源组**：指定虚拟机的资源组。</span><span class="sxs-lookup"><span data-stu-id="1239a-131">**Resource group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="1239a-132">选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="1239a-132">Select one of the following options:</span></span>
      * <span data-ttu-id="1239a-133">新建：指定要创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="1239a-133">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="1239a-134">**使用现有**：指定要从与 Azure 帐户关联的资源组列表中进行选择。</span><span class="sxs-lookup"><span data-stu-id="1239a-134">**Use existing**: Specifies that you want to select from a list of resource groups that are associated with your Azure account.</span></span>

       ![“关联的资源”窗口][CR07]

   * <span data-ttu-id="1239a-136">**存储帐户**：指定用于存储虚拟机的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="1239a-136">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="1239a-137">可以选择现有存储帐户，也可以创建新的帐户。</span><span class="sxs-lookup"><span data-stu-id="1239a-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="1239a-138">如果选择“新建”，会显示以下对话框：</span><span class="sxs-lookup"><span data-stu-id="1239a-138">If you choose **Create New**, the following dialog box appears:</span></span>

      ![“创建存储帐户”对话框][CR05]

   * <span data-ttu-id="1239a-140">虚拟网络和子网：指定虚拟机将连接到的虚拟网络和子网。</span><span class="sxs-lookup"><span data-stu-id="1239a-140">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="1239a-141">可以使用现有网络和子网，也可以创建新的网络和子网。</span><span class="sxs-lookup"><span data-stu-id="1239a-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="1239a-142">如果选择“新建”，会显示以下对话框：</span><span class="sxs-lookup"><span data-stu-id="1239a-142">If you select **Create new**, the following dialog box appears:</span></span>

      ![“创建虚拟网络”对话框][CR06]

   * <span data-ttu-id="1239a-144">**公共 IP 地址**：为虚拟机指定面向外部的 IP 地址。</span><span class="sxs-lookup"><span data-stu-id="1239a-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="1239a-145">可选择创建新 IP 地址，也可以选择“(无)”（如果虚拟机将不具有公共 IP 地址）。</span><span class="sxs-lookup"><span data-stu-id="1239a-145">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="1239a-146">网络安全组：指定虚拟机的可选网络防火墙。</span><span class="sxs-lookup"><span data-stu-id="1239a-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="1239a-147">可以选择现有防火墙，也可以选择“(无)”（如果虚拟机不使用网络防火墙）。</span><span class="sxs-lookup"><span data-stu-id="1239a-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="1239a-148">**可用性集**：指定虚拟机可能属于的可选可用性集。</span><span class="sxs-lookup"><span data-stu-id="1239a-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="1239a-149">可以选择现有的可用性集，也可以创建新的可用性集，或选择“(无)”（如果虚拟机将不属于可用性集）。</span><span class="sxs-lookup"><span data-stu-id="1239a-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong to an availability set, select **(None)**.</span></span>

9. <span data-ttu-id="1239a-150">单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="1239a-150">Click **Finish**.</span></span>  
    <span data-ttu-id="1239a-151">新虚拟机随即显示在“Azure 资源管理器”工具窗口中。</span><span class="sxs-lookup"><span data-stu-id="1239a-151">Your new virtual machine appears in the Azure Explorer tool window.</span></span> 

   ![Azure 资源管理器视图中的新虚拟机][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="1239a-153">在 IntelliJ 中重新启动虚拟机</span><span class="sxs-lookup"><span data-stu-id="1239a-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="1239a-154">若要在 IntelliJ 中使用 Azure 资源管理器重新启动虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1239a-154">To restart a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="1239a-155">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“重新启动”。</span><span class="sxs-lookup"><span data-stu-id="1239a-155">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![“重新启动虚拟机”命令][RE01]

2. <span data-ttu-id="1239a-157">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1239a-157">In the confirmation window, click **Yes**.</span></span> 

   ![确认重新启动虚拟机窗口][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="1239a-159">在 IntelliJ 中关闭虚拟机</span><span class="sxs-lookup"><span data-stu-id="1239a-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="1239a-160">若要在 IntelliJ 中使用 Azure 资源管理器关闭正在运行的虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1239a-160">To shut down a running virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="1239a-161">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“关闭”。</span><span class="sxs-lookup"><span data-stu-id="1239a-161">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![“关闭虚拟机”命令][SH01]

2. <span data-ttu-id="1239a-163">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1239a-163">In the confirmation window, click **Yes**.</span></span> 

   ![确认关闭虚拟机窗口][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="1239a-165">在 IntelliJ 中删除虚拟机</span><span class="sxs-lookup"><span data-stu-id="1239a-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="1239a-166">若要在 IntelliJ 中使用 Azure 资源管理器重新删除虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1239a-166">To delete a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="1239a-167">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“删除”。</span><span class="sxs-lookup"><span data-stu-id="1239a-167">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![“删除虚拟机”命令][DE01]

2. <span data-ttu-id="1239a-169">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1239a-169">In the confirmation window, click **Yes**.</span></span> 

   ![确认删除虚拟机窗口][DE02]

## <a name="next-steps"></a><span data-ttu-id="1239a-171">后续步骤</span><span class="sxs-lookup"><span data-stu-id="1239a-171">Next steps</span></span>

<span data-ttu-id="1239a-172">有关 Azure 虚拟机大小和定价的详细信息，请参阅以下资源：</span><span class="sxs-lookup"><span data-stu-id="1239a-172">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="1239a-173">Azure 虚拟机大小</span><span class="sxs-lookup"><span data-stu-id="1239a-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="1239a-174">[Azure 中 Windows 虚拟机的大小]</span><span class="sxs-lookup"><span data-stu-id="1239a-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="1239a-175">[Azure 中 Linux 虚拟机的大小]</span><span class="sxs-lookup"><span data-stu-id="1239a-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="1239a-176">Azure 虚拟机定价</span><span class="sxs-lookup"><span data-stu-id="1239a-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="1239a-177">[Windows 虚拟机定价]</span><span class="sxs-lookup"><span data-stu-id="1239a-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="1239a-178">[Linux 虚拟机定价]</span><span class="sxs-lookup"><span data-stu-id="1239a-178">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包的登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Azure 中 Windows 虚拟机的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中 Linux 虚拟机的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 虚拟机定价]: /pricing/details/virtual-machines/windows/
[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/
[Linux 虚拟机定价]: /pricing/details/virtual-machines/linux/
[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
