---
title: 使用用于 Eclipse 的 Azure 资源管理器管理虚拟机
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure 虚拟机。
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
ms.openlocfilehash: c04f5225f0bb99898f69b26a4782aa57d75c4f22
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38074562"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="1d73c-103">使用用于 Eclipse 的 Azure 资源管理器管理虚拟机</span><span class="sxs-lookup"><span data-stu-id="1d73c-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="1d73c-104">Azure 资源管理器是用于 Eclipse 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse 集成开发环境 (IDE) 内部管理其 Azure 帐户中的虚拟机。</span><span class="sxs-lookup"><span data-stu-id="1d73c-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d73c-105">在 Eclipse 中创建虚拟机</span><span class="sxs-lookup"><span data-stu-id="1d73c-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d73c-106">若要使用 Azure 资源管理器创建虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1d73c-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="1d73c-107">按照[用于 Eclipse 的 Azure 工具包的登录说明](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)中的步骤登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="1d73c-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).</span></span>

2. <span data-ttu-id="1d73c-108">在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“虚拟机”，并单击“创建 VM”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   ![“创建 VM”命令][CR01]  

   <span data-ttu-id="1d73c-110">此时会打开“新建虚拟机”向导。</span><span class="sxs-lookup"><span data-stu-id="1d73c-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="1d73c-111">在“选择订阅”窗口中选择订阅，并单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![“选择订阅”窗口][CR02]

4. <span data-ttu-id="1d73c-113">在“选择虚拟机映像”窗口中输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1d73c-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="1d73c-114">**位置**：指定将创建虚拟机的位置（例如“美国西部”）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="1d73c-115">发布者：指定创建了用于创建虚拟机的映像的发布者（例如“Microsoft”）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="1d73c-116">**产品/服务**： 指定所选发布者提供的可以使用的虚拟机产品/服务（例如 *JDK* ）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="1d73c-117">Sku：从所选产品/服务中指定要使用的库存单位 (SKU)（例如“JDK_8”）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="1d73c-118">版本号：从所选 SKU 中指定要使用哪个版本。</span><span class="sxs-lookup"><span data-stu-id="1d73c-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![“选择虚拟机映像”窗口][CR03]

5. <span data-ttu-id="1d73c-120">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-120">Click **Next**.</span></span>

6. <span data-ttu-id="1d73c-121">在“虚拟机基本设置”窗口中输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1d73c-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="1d73c-122">**虚拟机名称**：指定新虚拟机的名称，该名称必须以字母开头并仅包含字母、数字和连字符。</span><span class="sxs-lookup"><span data-stu-id="1d73c-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="1d73c-123">**大小**：指定要为虚拟机分配的内核数和内存。</span><span class="sxs-lookup"><span data-stu-id="1d73c-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="1d73c-124">**用户名**：指定要创建的用于管理虚拟机的管理员帐户。</span><span class="sxs-lookup"><span data-stu-id="1d73c-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="1d73c-125">**密码**和**确认**：指定管理员帐户的密码。</span><span class="sxs-lookup"><span data-stu-id="1d73c-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![“虚拟机基本设置”窗口][CR04]

7. <span data-ttu-id="1d73c-127">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-127">Click **Next**.</span></span>

8. <span data-ttu-id="1d73c-128">在“创建新存储帐户”窗口输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1d73c-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="1d73c-129">资源组：指定虚拟机的资源组。</span><span class="sxs-lookup"><span data-stu-id="1d73c-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="1d73c-130">选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="1d73c-130">Select one of the following options:</span></span>
     * <span data-ttu-id="1d73c-131">新建：指定要创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="1d73c-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
     * <span data-ttu-id="1d73c-132">使用现有资源：指定选择已与 Azure 帐户关联的资源组。</span><span class="sxs-lookup"><span data-stu-id="1d73c-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

       ![“创建新存储帐户”对话框][CR05]

   * <span data-ttu-id="1d73c-134">存储帐户：指定用于存储虚拟机的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="1d73c-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="1d73c-135">可使用现有存储帐户，也可以创建新帐户。</span><span class="sxs-lookup"><span data-stu-id="1d73c-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="1d73c-136">虚拟网络和子网：指定虚拟机将连接到的虚拟网络和子网。</span><span class="sxs-lookup"><span data-stu-id="1d73c-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="1d73c-137">可使用现有网络和子网，也可以创建新网络和子网。</span><span class="sxs-lookup"><span data-stu-id="1d73c-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="1d73c-138">如果选择“新建”，会显示以下对话框：</span><span class="sxs-lookup"><span data-stu-id="1d73c-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![“新建虚拟网络”对话框][CR06]

9. <span data-ttu-id="1d73c-140">在“关联的资源”窗口输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="1d73c-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="1d73c-141">公共 IP 地址：为虚拟机指定面向外部的 IP 地址。</span><span class="sxs-lookup"><span data-stu-id="1d73c-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="1d73c-142">可选择创建新 IP 地址，也可以选择“(无)”（如果虚拟机将不具有公共 IP 地址）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="1d73c-143">网络安全组：指定虚拟机的可选网络防火墙。</span><span class="sxs-lookup"><span data-stu-id="1d73c-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="1d73c-144">可以选择现有防火墙，也可以选择“(无)”（如果虚拟机不使用网络防火墙）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="1d73c-145">**可用性集**：指定虚拟机可能属于的可选可用性集。</span><span class="sxs-lookup"><span data-stu-id="1d73c-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="1d73c-146">可选择现有可用性集，或创建新可用性集，也可选择“(无)”（如果虚拟机将不属于可用性集）。</span><span class="sxs-lookup"><span data-stu-id="1d73c-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![“关联的资源”窗口][CR07]

10. <span data-ttu-id="1d73c-148">单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-148">Click **Finish**.</span></span>  

    <span data-ttu-id="1d73c-149">新虚拟机显示在“Azure 资源管理器”工具窗口中。</span><span class="sxs-lookup"><span data-stu-id="1d73c-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

    ![新建虚拟机][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d73c-151">在 Eclipse 中重启虚拟机</span><span class="sxs-lookup"><span data-stu-id="1d73c-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d73c-152">若要在 Eclipse 中使用 Azure 资源管理器重启虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1d73c-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="1d73c-153">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“重新启动”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![“重新启动虚拟机”命令][RE01]

1. <span data-ttu-id="1d73c-155">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-155">In the confirmation window, click **Yes**.</span></span>

   ![“重启”确认窗口][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d73c-157">在 Eclipse 中关闭虚拟机</span><span class="sxs-lookup"><span data-stu-id="1d73c-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d73c-158">若要在 Eclipse 中使用 Azure 资源管理器关闭正在运行的虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1d73c-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="1d73c-159">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“关闭”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![“关闭虚拟机”命令][SH01]

1. <span data-ttu-id="1d73c-161">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-161">In the confirmation window, click **Yes**.</span></span>

   ![“关闭虚拟机”确认窗口][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d73c-163">在 Eclipse 中删除虚拟机</span><span class="sxs-lookup"><span data-stu-id="1d73c-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d73c-164">若要在 Eclipse 中使用 Azure 资源管理器删除虚拟机，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="1d73c-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="1d73c-165">在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“删除”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![“删除虚拟机”命令][DE01]

1. <span data-ttu-id="1d73c-167">在确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="1d73c-167">In the confirmation window, click **Yes**.</span></span>

   ![“删除虚拟机”确认窗口][DE02]

## <a name="next-steps"></a><span data-ttu-id="1d73c-169">后续步骤</span><span class="sxs-lookup"><span data-stu-id="1d73c-169">Next steps</span></span>

<span data-ttu-id="1d73c-170">有关 Azure 虚拟机大小和定价的详细信息，请参阅以下资源：</span><span class="sxs-lookup"><span data-stu-id="1d73c-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="1d73c-171">Azure 虚拟机大小</span><span class="sxs-lookup"><span data-stu-id="1d73c-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="1d73c-172">[Azure 中 Windows 虚拟机的大小]</span><span class="sxs-lookup"><span data-stu-id="1d73c-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="1d73c-173">[Azure 中 Linux 虚拟机的大小]</span><span class="sxs-lookup"><span data-stu-id="1d73c-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="1d73c-174">Azure 虚拟机定价</span><span class="sxs-lookup"><span data-stu-id="1d73c-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="1d73c-175">[Windows 虚拟机定价]</span><span class="sxs-lookup"><span data-stu-id="1d73c-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="1d73c-176">[Linux 虚拟机定价]</span><span class="sxs-lookup"><span data-stu-id="1d73c-176">[Linux virtual-machine pricing]</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure 中 Windows 虚拟机的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中 Linux 虚拟机的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 虚拟机定价]: /pricing/details/virtual-machines/windows/
[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/
[Linux 虚拟机定价]: /pricing/details/virtual-machines/linux/
[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
