---
title: 使用 Java 管理 Azure 虚拟机 | Microsoft Docs
description: 使用用于 Java 的 Azure SDK 管理 Azure 虚拟机的示例代码
author: rloutlaw
manager: douge
ms.assetid: 88629aee-6279-433e-a08b-4f8e290446d0
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 388b68bfb0fdac70efed5ad5d0f7c957ce915449
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592562"
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a><span data-ttu-id="933b2-103">从 Java 应用程序管理 Azure 虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-103">Manage Azure virtual machines from your Java applications</span></span>

<span data-ttu-id="933b2-104">[本示例](https://github.com/Azure-Samples/compute-java-manage-vm/)通过[用于 Java 的 Azure 管理库](https://github.com/Azure/azure-sdk-for-java)来创建和使用 Azure 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-vm/) uses the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java) to create and work with Azure virtual machines.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="933b2-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="933b2-105">Run the sample</span></span>

<span data-ttu-id="933b2-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="933b2-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="933b2-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="933b2-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

<span data-ttu-id="933b2-108">查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)。</span><span class="sxs-lookup"><span data-stu-id="933b2-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="933b2-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="933b2-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a><span data-ttu-id="933b2-110">创建 Windows 虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-110">Create a Windows virtual machine</span></span>

```java
// Prepare a data disk for VM
Disk dataDisk = azure.disks().define(SdkContext.randomResourceName("dsk", 30))
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withData()
            .withSizeInGB(50)
            .create();

// create the windows virtual machine with the data disk            
VirtualMachine windowsVM = azure.virtualMachines().define(windowsVmName)
            .withRegion(region)
            .withNewResourceGroup(rgName)
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularWindowsImage(KnownWindowsVirtualMachineImage.WINDOWS_SERVER_2012_R2_DATACENTER)
            .withAdminUsername(userName)
            .withAdminPassword(password)
            .withNewDataDisk(10)
            .withNewDataDisk(dataDiskCreatable)
            .withExistingDataDisk(dataDisk)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

<span data-ttu-id="933b2-111">此代码：</span><span class="sxs-lookup"><span data-stu-id="933b2-111">This code:</span></span>   

0. <span data-ttu-id="933b2-112">定义大小为 50GB 的 `Disk` 可创建对象，以及用于虚拟机的随机名称。</span><span class="sxs-lookup"><span data-stu-id="933b2-112">Defines a `Disk` Creatable with a 50GB size and random name for use with a virtual machine.</span></span>
0. <span data-ttu-id="933b2-113">使用 `azure.virtualMachines().define()..create()` 链创建 Windows Server 2012 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-113">Uses the `azure.virtualMachines().define()..create()` chain to create the Windows Server 2012 virtual machine.</span></span> <span data-ttu-id="933b2-114">API 创建上一步骤中定义的 `Disk`（与虚拟机同时创建）。</span><span class="sxs-lookup"><span data-stu-id="933b2-114">The API creates the `Disk` defined in the previous step the same time as the virtual machine.</span></span> <span data-ttu-id="933b2-115">同时，通过 `withNewDataDisk(10)` 将 10GB 数据磁盘附加到虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-115">A 10GB data disk is also attached to the virtual machine through `withNewDataDisk(10)`.</span></span>

<span data-ttu-id="933b2-116">详细了解如何使用 [Creatable<T> 对象](java-sdk-azure-concepts.md#Creatables)（可创建对象）来定义资源的本地表示形式，以及仅在其他 Azure 资源有需要时才创建资源。</span><span class="sxs-lookup"><span data-stu-id="933b2-116">Learn more about using [Creatable<T> objects](java-sdk-azure-concepts.md#Creatables) to define local representations of resources and create them just as other Azure resources need them.</span></span>

## <a name="stop-start-and-restart-a-virtual-machine"></a><span data-ttu-id="933b2-117">停止、启动和重启虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-117">Stop, start, and restart a virtual machine</span></span>

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

<span data-ttu-id="933b2-118">`powerOff()` 停止虚拟机操作系统，但不解除分配其资源。</span><span class="sxs-lookup"><span data-stu-id="933b2-118">`powerOff()` stops the virtual machine operating system but does not deallocate its resources.</span></span>

## <a name="add-a-virtual-machine-to-an-existing-network"></a><span data-ttu-id="933b2-119">将虚拟机添加到现有网络</span><span class="sxs-lookup"><span data-stu-id="933b2-119">Add a virtual machine to an existing network</span></span>

```java
// Get the virtual network the current virtual machine is using
Network network = windowsVM.getPrimaryNetworkInterface().primaryIPConfiguration().getNetwork();

// Create a Linux VM in the same subnet
VirtualMachine linuxVM = azure.virtualMachines().define(linuxVmName)
           .withRegion(region)
           .withExistingResourceGroup(rgName)
           .withExistingPrimaryNetwork(network)
           .withSubnet("subnet1") // default subnet name when no name specified at creation
           .withPrimaryPrivateIPAddressDynamic()
           .withoutPrimaryPublicIPAddress()
           .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
           .withRootUsername(userName)
           .withRootPassword(password)
           .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
           .create();
```

<span data-ttu-id="933b2-120">使用 `withPopularLinuxImage` 可定义 Linux VM，而不能定义 Windows VM。</span><span class="sxs-lookup"><span data-stu-id="933b2-120">Use `withPopularLinuxImage` to define a Linux VM instead of a Windows one.</span></span>


## <a name="list-virtual-machines"></a><span data-ttu-id="933b2-121">列出虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-121">List virtual machines</span></span>

```java
// get a list of VMs in the same resource group as an existing VM
String resourceGroupName = windowsVM.resourceGroupName();
PagedList<VirtualMachine> resourceGroupVMs = azure.virtualMachines()
        .listByResourceGroup(resourceGroupName); 

// for each vitual machine in the resource group, log their name and plan
for (VirtualMachine virtualMachine : azure.virtualMachines().listByResourceGroup(resourceGroupName)) {
    System.out.println("VM " + virtualMachine.computerName() + 
        " has plan " + virtualMachine.plan());
}
```

<span data-ttu-id="933b2-122">使用 `azure.virtualMachines().list()` 列出订阅的所有虚拟机，并循环访问 `tags()` 返回的映射，以管理各资源组中虚拟机的有标记集合。</span><span class="sxs-lookup"><span data-stu-id="933b2-122">List all virtual machines for a subscription using `azure.virtualMachines().list()` and iterate through the Map returned by `tags()` to manage tagged collections of virtual machines across resource groups.</span></span>

## <a name="update-a-virtual-machine"></a><span data-ttu-id="933b2-123">更新虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-123">Update a virtual machine</span></span>

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

<span data-ttu-id="933b2-124">使用 `update()...apply()` 以及用于配置通过 `define()...create()` 创建的虚拟机的相同方法来更新虚拟机配置。</span><span class="sxs-lookup"><span data-stu-id="933b2-124">Update the virtual machine configuration using `update()...apply()` and the same methods used to configure the virtual machine when created through `define()...create()`.</span></span>

## <a name="delete-a-virtual-machine"></a><span data-ttu-id="933b2-125">删除虚拟机</span><span class="sxs-lookup"><span data-stu-id="933b2-125">Delete a virtual machine</span></span>

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a><span data-ttu-id="933b2-126">示例解释</span><span class="sxs-lookup"><span data-stu-id="933b2-126">Sample explanation</span></span>

<span data-ttu-id="933b2-127">[本示例代码](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)创建具有 50GB 数据磁盘的 Windows 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-127">[The sample code](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java) creates a Windows virtual machine with a 50GB data disk.</span></span> <span data-ttu-id="933b2-128">接下来，本示例创建第二个 10GB 数据磁盘并将其附加到此 Windows 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-128">The sample then creates a second 10GB data disk and attaches it to this Windows virtual machine.</span></span>
<span data-ttu-id="933b2-129">然后，本示例在 Windows 虚拟机所在的同一虚拟网络中创建 Linux 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-129">Then the sample creates a Linux virtual machine in the same virtual network as the Windows virtual machine.</span></span>

<span data-ttu-id="933b2-130">本示例会记录有关上述两个虚拟机的信息，并在完成之前删除这些虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-130">The sample logs information about both virtual machines and deletes them both before completing.</span></span>

| <span data-ttu-id="933b2-131">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="933b2-131">Class used in sample</span></span> | <span data-ttu-id="933b2-132">说明</span><span class="sxs-lookup"><span data-stu-id="933b2-132">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="933b2-133">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="933b2-133">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="933b2-134">查询属性并管理虚拟机的状态。</span><span class="sxs-lookup"><span data-stu-id="933b2-134">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="933b2-135">在使用 `azure.virtualMachines().list()` 返回的，或者按名称或 ID 执行 `azure.virtualMachines().getByResourceGroup()` 后返回的列表表单中检索</span><span class="sxs-lookup"><span data-stu-id="933b2-135">Retrieved in list form  with`azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="933b2-136">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="933b2-136">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="933b2-137">包含映射到[虚拟机大小选项](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)的静态值的类，`withSize()` 方法使用它来定义分配给 VM 的资源。</span><span class="sxs-lookup"><span data-stu-id="933b2-137">Class with static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), used by the `withSize()` method to define the resources allocated to the VM.</span></span>
| [<span data-ttu-id="933b2-138">磁盘</span><span class="sxs-lookup"><span data-stu-id="933b2-138">Disk</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | <span data-ttu-id="933b2-139">定义磁盘时，使用 `withData()` 创建一个用于存储数据的磁盘，或使用相应的 `withLinux` 或 `withWindows` 方法创建用于存储操作系统映像的磁盘。</span><span class="sxs-lookup"><span data-stu-id="933b2-139">Create a disk to store data using `withData()` or operating system image using the appropriate `withLinux` or `withWindows` method when defining the disk.</span></span> <span data-ttu-id="933b2-140">在创建时（`using withNewDataDisk` 或 `withExistingDataDisk`）或者在 VirtualMachine 对象上通过 `update()..apply()` 进行创建后将磁盘添加到虚拟机。</span><span class="sxs-lookup"><span data-stu-id="933b2-140">Attach disks to virtual machines either at the time of creation (`using withNewDataDisk` or `withExistingDataDisk`) or after creation by `update()..apply()` on the VirtualMachine object.</span></span>
| [<span data-ttu-id="933b2-141">DiskSkuTypes</span><span class="sxs-lookup"><span data-stu-id="933b2-141">DiskSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | <span data-ttu-id="933b2-142">包含静态值的类，用于定义采用标准或[高级](https://docs.microsoft.com/azure/storage/storage-premium-storage)存储计划的磁盘。</span><span class="sxs-lookup"><span data-stu-id="933b2-142">Class with static values to define a disk with a standard or [premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) storage plan.</span></span>
| [<span data-ttu-id="933b2-143">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="933b2-143">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="933b2-144">包含一组 Linux 虚拟机选项的类，在定义虚拟机时与 `withPopularLinuxImage()` 方法结合使用。</span><span class="sxs-lookup"><span data-stu-id="933b2-144">Class with a set of Linux virtual machine options for use with the `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="933b2-145">KnownWindowsVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="933b2-145">KnownWindowsVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | <span data-ttu-id="933b2-146">包含一组 Windows 虚拟机映像选项的类，在定义虚拟机时与 `withPopularWindowsImage()` 方法结合使用。</span><span class="sxs-lookup"><span data-stu-id="933b2-146">Class with a set of Windows virtual machine image options for use with the `withPopularWindowsImage()` method when defining a virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="933b2-147">后续步骤</span><span class="sxs-lookup"><span data-stu-id="933b2-147">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]