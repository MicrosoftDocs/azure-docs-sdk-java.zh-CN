---
title: 跨区域并行创建 VM | Microsoft Docs
description: 使用用于 Java 的 Azure SDK 跨不同的 Azure 区域并行创建虚拟机的示例代码
author: rloutlaw
manager: douge
ms.assetid: e5a36699-2d96-4571-84f9-a6af13f3c067
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 497ea5c4e2efd73335f05f90b700fb70e2809de6
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592499"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a><span data-ttu-id="361ba-103">通过 Java 应用程序跨多个地区创建虚拟机</span><span class="sxs-lookup"><span data-stu-id="361ba-103">Create virtual machines across multiple regions from your Java applications</span></span>

<span data-ttu-id="361ba-104">[本示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)使用[用于 Java 的 Azure 管理库](https://github.com/Azure/azure-sdk-for-java)跨不同的 Azure 区域并行创建虚拟机。</span><span class="sxs-lookup"><span data-stu-id="361ba-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) creates virtual machines in parallel across different Azure regions using the [Azure management libraries for Java](https://github.com/Azure/azure-sdk-for-java).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="361ba-105">本示例在四个区域总共创建 48 个运行 Ubuntu 16.04 LTS、[大小为 STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) 的 VM。</span><span class="sxs-lookup"><span data-stu-id="361ba-105">The sample creates a total of 48 VMs running Ubuntu 16.04 LTS of [size STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) across four regions.</span></span> <span data-ttu-id="361ba-106">在退出之前，示例代码会删除这些虚拟机。</span><span class="sxs-lookup"><span data-stu-id="361ba-106">The sample code deletes these virtual machines before exiting.</span></span> <span data-ttu-id="361ba-107">在针对默认的 VM 数目运行本示例之前，请务必[检查服务限制和配额](http://docs.microsoft.com/azure/azure-subscription-service-limits)。</span><span class="sxs-lookup"><span data-stu-id="361ba-107">Make sure to [check your service limits and quota](http://docs.microsoft.com/azure/azure-subscription-service-limits) before running this sample with the default number of VMs.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="361ba-108">运行示例</span><span class="sxs-lookup"><span data-stu-id="361ba-108">Run the sample</span></span>

<span data-ttu-id="361ba-109">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="361ba-109">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="361ba-110">然后运行：</span><span class="sxs-lookup"><span data-stu-id="361ba-110">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

<span data-ttu-id="361ba-111">查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java)。</span><span class="sxs-lookup"><span data-stu-id="361ba-111">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="361ba-112">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="361ba-112">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a><span data-ttu-id="361ba-113">设置虚拟机的位置和计数</span><span class="sxs-lookup"><span data-stu-id="361ba-113">Set locations and counts for the virtual machines</span></span>

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

<span data-ttu-id="361ba-114">此 `Map` 稍后在本示例中用于设置 VM 的全球分布。</span><span class="sxs-lookup"><span data-stu-id="361ba-114">This `Map` is used later in the sample to set the distrubtion of the VMs worldwide.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="361ba-115">创建资源组</span><span class="sxs-lookup"><span data-stu-id="361ba-115">Create a resource group</span></span> 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

<span data-ttu-id="361ba-116">本示例中的每个资源由此资源组管理。</span><span class="sxs-lookup"><span data-stu-id="361ba-116">Each resource in the sample is managed by this resource group.</span></span> <span data-ttu-id="361ba-117">这样，稍后便可通过删除资源组来轻松清理资源。</span><span class="sxs-lookup"><span data-stu-id="361ba-117">This makes the resources easy to clean up later by deleting the resource group.</span></span>

## <a name="define-the-virtual-machines"></a><span data-ttu-id="361ba-118">定义虚拟机</span><span class="sxs-lookup"><span data-stu-id="361ba-118">Define the virtual machines</span></span>
```java
// list to store the VirtualMachine definitions
List<Creatable<VirtualMachine>> creatableVirtualMachines = new ArrayList<>();
    
// outer loop: iterate through each region included in the map
for (Map.Entry<Region, Integer> entry : virtualMachinesByLocation.entrySet()) {
    Region region = entry.getKey();
    Integer vmCount = entry.getValue();

    // Define one virtual network Creatable per region for the VMs to share
    String networkName = SdkContext.randomResourceName("vnetCOPD-", 20);
    Creatable<Network> networkCreatable = azure.networks().define(networkName)
           .withRegion(region)
           .withExistingResourceGroup(resourceGroup)
           .withAddressSpace("172.16.0.0/16");

    // Define one storage account Creatable per region for storing VM disks
    String storageAccountName = SdkContext.randomResourceName("stgcopd", 20);
    Creatable<StorageAccount> storageAccountCreatable = azure.storageAccounts()
        .define(storageAccountName)
              .withRegion(region)
              .withExistingResourceGroup(resourceGroup);

    // generate a common prefix for every VM name
    String linuxVMNamePrefix = SdkContext.randomResourceName("vm-", 15);

    // inner loop: iterate once for every VM instance in the region
    for (int i = 1; i <= vmCount; i++) {

        // Create one public IP address Creatable for each VM
        Creatable<PublicIpAddress> publicIpAddressCreatable = azure.publicIpAddresses()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withLeafDomainLabel(SdkContext.randomResourceName("pip", 10));

        publicIpCreatableKeys.add(publicIpAddressCreatable.key());

        // Create one virtual machine Creatable 
        Creatable<VirtualMachine> virtualMachineCreatable = azure.virtualMachines()
             .define(String.format("%s-%d", linuxVMNamePrefix, i))
             .withRegion(region)
             .withExistingResourceGroup(resourceGroup)
             .withNewPrimaryNetwork(networkCreatable)
             .withPrimaryPrivateIpAddressDynamic()
             .withNewPrimaryPublicIpAddress(publicIpAddressCreatable)
             .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
             .withRootUsername(userName)
             .withSsh(sshKey)
             .withSize(VirtualMachineSizeTypes.STANDARD_DS3_V2)
             .withNewStorageAccount(storageAccountCreatable);
        // add the virtual machine Creatable to the list     
        creatableVirtualMachines.add(virtualMachineCreatable); 
     }
}
```

<span data-ttu-id="361ba-119">上面所示的外部 `for` 循环会循环访问每个区域，并定义虚拟网络可创建对象和存储帐户可创建对象供该区域中的所有虚拟机使用。</span><span class="sxs-lookup"><span data-stu-id="361ba-119">The outer `for` loop above iterates through each region, defining a virtual network Creatable and storage account Creatable for use by all virtual machines in that region.</span></span> <span data-ttu-id="361ba-120">详细了解在使用管理库时，如何使用[可创建对象](java-sdk-azure-concepts.md#Creatables)来做到仅有必要时才创建资源。</span><span class="sxs-lookup"><span data-stu-id="361ba-120">Learn more about using [Creatables](java-sdk-azure-concepts.md#Creatables) to create resources only as needed when using the management libraries.</span></span>

<span data-ttu-id="361ba-121">内部 `for` 循环获取虚拟机的公共 IP 地址可创建对象，然后使用前面定义的虚拟网络、存储帐户和公共 IP 地址的可创建对象来定义虚拟机可创建对象。</span><span class="sxs-lookup"><span data-stu-id="361ba-121">The inner `for` loop gets a public IP address Creatable for the virtual machine and then defines a virtual machine Creatable using the Creatables for the virtual network, storage account, and public IP address defined previously.</span></span>  <span data-ttu-id="361ba-122">然后，此虚拟机可创建对象会添加到 `creatableVirtualMachines` 列表中。</span><span class="sxs-lookup"><span data-stu-id="361ba-122">This VirtualMachine Creatable is then added to the `creatableVirtualMachines` list.</span></span>

## <a name="create-the-virtual-machines"></a><span data-ttu-id="361ba-123">创建虚拟机</span><span class="sxs-lookup"><span data-stu-id="361ba-123">Create the virtual machines</span></span>

```java
// create all virtual machines defined in the list, return all Creatable objects used
// including networks, public IP addresses, and storage accounts
CreatedResources<VirtualMachine> virtualMachines = azure.virtualMachines().create(creatableVirtualMachines);

// list the IDs of each virtual machine created 
for (VirtualMachine virtualMachine : virtualMachines.values()) {
    System.out.println(virtualMachine.id());
}

// call createdRelatedResource(key) to get the resources used to define the virtual machines. 
// Save the key at the time you define the Creatable to use CreatedResources like this
for (String publicIpCreatableKey : publicIpCreatableKeys) {
    PublicIPAddress pip = 
         (PublicIPAddress) virtualMachines.createdRelatedResource(publicIpCreatableKey);
}
```

<span data-ttu-id="361ba-124">`azure.virtualMachines().create(creatableVirtualMachines)` 调用跨区域并行创建 `creatableVirtualMachines` 列表中定义的所有虚拟机。</span><span class="sxs-lookup"><span data-stu-id="361ba-124">The `azure.virtualMachines().create(creatableVirtualMachines)` call creates all of the virtual machines defined in the `creatableVirtualMachines` List in parallel across the regions.</span></span>

<span data-ttu-id="361ba-125">使用返回的 `CreatedResources<VirtualMachine>` 对象可访问执行 `create()` 方法期间在 Azure 订阅中创建的所有资源，而不仅仅是返回的 `VirtualMachine` 类型。</span><span class="sxs-lookup"><span data-stu-id="361ba-125">Use the returned `CreatedResources<VirtualMachine>` object to access any resources created in the Azure subscription during the the `create()` method, not just the returned `VirtualMachine` type.</span></span> <span data-ttu-id="361ba-126">将 `createdRelatedResources()` 中的返回值强制转换为正确的类型。</span><span class="sxs-lookup"><span data-stu-id="361ba-126">Cast the returned value from `createdRelatedResources()` to the correct type.</span></span> 

<span data-ttu-id="361ba-127">在[库概念文章](java-sdk-azure-concepts.md)中详细了解 `Creatable<T>` 和 `CreatedResources` 的用法。</span><span class="sxs-lookup"><span data-stu-id="361ba-127">Learn more about working with `Creatable<T>` and `CreatedResources` in our [library concepts article](java-sdk-azure-concepts.md).</span></span>

## <a name="delete-the-resource-group"></a><span data-ttu-id="361ba-128">删除资源组</span><span class="sxs-lookup"><span data-stu-id="361ba-128">Delete the resource group</span></span> 

```java
// finally block deletes the resource group before the code exits
// deleting a resource group deletes all resources created in it
finally {
    try {
        System.out.println("Deleting Resource Group: " + rgName);
        azure.resourceGroups().deleteByName(rgName);
        System.out.println("Deleted Resource Group: " + rgName);
    } catch (NullPointerException npe) {
        System.out.println("Did not create any resources in Azure. No clean up is necessary");
    } catch (Exception g) {
        g.printStackTrace();
    }
}
```

<span data-ttu-id="361ba-129">此块在示例退出之前，删除示例中创建的资源。</span><span class="sxs-lookup"><span data-stu-id="361ba-129">This block deletes resources created in the sample before the sample exits.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="361ba-130">示例解释</span><span class="sxs-lookup"><span data-stu-id="361ba-130">Sample explanation</span></span>

<span data-ttu-id="361ba-131">查看 [Github](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) 上的完整示例代码。</span><span class="sxs-lookup"><span data-stu-id="361ba-131">View the complete sample code on [Github](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel).</span></span>

<span data-ttu-id="361ba-132">本示例使用 `Creatable` 对象为托管虚拟机的每个区域定义虚拟网络和存储帐户。</span><span class="sxs-lookup"><span data-stu-id="361ba-132">The sample uses `Creatable` objects to define a virtual network and storage account for each region hosting the virtual machines.</span></span> <span data-ttu-id="361ba-133">然后，为每个虚拟机的公共 IP 地址定义 `Creatable` 对象。</span><span class="sxs-lookup"><span data-stu-id="361ba-133">`Creatable` objects are then defined for the public IP address for each virtual machine.</span></span> <span data-ttu-id="361ba-134">本示例使用这些 `Creatable` 对象定义虚拟机，示例会将 VM 定义添加到 `virtualMachineCreatable` 列表。</span><span class="sxs-lookup"><span data-stu-id="361ba-134">The sample defines the virtual machines using these `Creatable` objects, and sample adds the VM definition to the `virtualMachineCreatable` list.</span></span>

<span data-ttu-id="361ba-135">代码将每个虚拟机定义添加到列表之后，`azure.virtualMachines().create(creatableVirtualMachines)` 在 Azure 中并行创建每个虚拟机。</span><span class="sxs-lookup"><span data-stu-id="361ba-135">After the code adds every virtual machine definition to the list, `azure.virtualMachines().create(creatableVirtualMachines)` creates each virtual machine in parallel in Azure.</span></span>

<span data-ttu-id="361ba-136">然后，示例代码从返回的 CreatedResources 对象获取所有已创建的虚拟机的 IP 地址，以创建[流量管理器](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)在虚拟机之间分配负载。</span><span class="sxs-lookup"><span data-stu-id="361ba-136">The sample code then gets the IP addresses for all of the created virtual machines from the returned CreatedResources object to create a [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) to distribute load across the virtual machines.</span></span> 

<span data-ttu-id="361ba-137">即使出错，`finally` 块也会从 Azure 订阅中删除资源。</span><span class="sxs-lookup"><span data-stu-id="361ba-137">The `finally` block deletes the resources from your Azure subscription even in the case of an error.</span></span>

| <span data-ttu-id="361ba-138">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="361ba-138">Class used in sample</span></span> | <span data-ttu-id="361ba-139">说明</span><span class="sxs-lookup"><span data-stu-id="361ba-139">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="361ba-140">VirtualMachine</span><span class="sxs-lookup"><span data-stu-id="361ba-140">VirtualMachine</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | <span data-ttu-id="361ba-141">查询属性并管理虚拟机的状态。</span><span class="sxs-lookup"><span data-stu-id="361ba-141">Query properties and manage state of virtual machines.</span></span> <span data-ttu-id="361ba-142">在从 `azure.virtualMachines().list()` 返回的，或者按名称或 ID 执行 `azure.virtualMachines().getByResourceGroup()` 后返回的列表表单中检索</span><span class="sxs-lookup"><span data-stu-id="361ba-142">Retrieved in list form from `azure.virtualMachines().list()` or by name or ID `azure.virtualMachines().getByResourceGroup()`</span></span>
| [<span data-ttu-id="361ba-143">VirtualMachineSizeTypes</span><span class="sxs-lookup"><span data-stu-id="361ba-143">VirtualMachineSizeTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | <span data-ttu-id="361ba-144">映射到[虚拟机大小选项](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)的静态值，定义虚拟机时用作 `withSize()` 的参数。</span><span class="sxs-lookup"><span data-stu-id="361ba-144">Static values that map to [virtual machine size options](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) for use as a parameter to `withSize()` when defining a virtual machine.</span></span>
| [<span data-ttu-id="361ba-145">PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="361ba-145">PublicIpAddress</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | <span data-ttu-id="361ba-146">通过 `azure.publicIpAddresses().define()` 为每个虚拟机定义，但不会立即创建该类。</span><span class="sxs-lookup"><span data-stu-id="361ba-146">Defined, but not immediately created, for each virtual machine through `azure.publicIpAddresses().define()`.</span></span> <span data-ttu-id="361ba-147">存储每个 `Creatable` 的键，稍后可通过 `createdRelatedResource()` 检索</span><span class="sxs-lookup"><span data-stu-id="361ba-147">Store the key for each `Creatable` and retrieve later through `createdRelatedResource()`</span></span>
| [<span data-ttu-id="361ba-148">KnownLinuxVirtualMachineImage</span><span class="sxs-lookup"><span data-stu-id="361ba-148">KnownLinuxVirtualMachineImage</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | <span data-ttu-id="361ba-149">Linux 虚拟机选项集，在定义虚拟机时用作 `withPopularLinuxImage()` 方法的参数。</span><span class="sxs-lookup"><span data-stu-id="361ba-149">Set of Linux virtual machine options used as a parameter to `withPopularLinuxImage()` method when defining a virtual machine.</span></span>
| [<span data-ttu-id="361ba-150">网络</span><span class="sxs-lookup"><span data-stu-id="361ba-150">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="361ba-151">本示例通过 `azure.networks().define()` 为每个区域定义一个虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="361ba-151">The sample defines one virtual network for each region through  `azure.networks().define()` .</span></span> 

## <a name="next-steps"></a><span data-ttu-id="361ba-152">后续步骤</span><span class="sxs-lookup"><span data-stu-id="361ba-152">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]