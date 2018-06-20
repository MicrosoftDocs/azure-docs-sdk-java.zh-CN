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
ms.openlocfilehash: e20feb555c3a360eceae60c1569af9a00a5cd027
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931193"
---
# <a name="create-virtual-machines-across-multiple-regions-from-your-java-applications"></a>通过 Java 应用程序跨多个地区创建虚拟机

[本示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)使用[用于 Java 的 Azure 管理库](https://github.com/Azure/azure-sdk-for-java)跨不同的 Azure 区域并行创建虚拟机。

> [!IMPORTANT]
> 本示例在四个区域总共创建 48 个运行 Ubuntu 16.04 LTS、[大小为 STANDARD_DS3_V2](http://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes) 的 VM。 在退出之前，示例代码会删除这些虚拟机。 在针对默认的 VM 数目运行本示例之前，请务必[检查服务限制和配额](http://docs.microsoft.com/azure/azure-subscription-service-limits)。

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel.git
cd compute-java-create-virtual-machines-across-regions-in-parallel
mvn clean compile exec:java
```

查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/CreateVirtualMachinesInParallel.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="set-locations-and-counts-for-the-virtual-machines"></a>设置虚拟机的位置和计数

```java
// use a Map to define how where and how many VMs to create 
Map<Region, Integer> virtualMachinesByLocation = new HashMap<Region, Integer>();

// create 12 virtual machines in four regions
virtualMachinesByLocation.put(Region.US_EAST, 12);
virtualMachinesByLocation.put(Region.US_SOUTH_CENTRAL, 12);
virtualMachinesByLocation.put(Region.US_WEST, 12);
virtualMachinesByLocation.put(Region.US_NORTH_CENTRAL, 12);
```

此 `Map` 稍后在本示例中用于设置 VM 的全球分布。

## <a name="create-a-resource-group"></a>创建资源组 

```java
// logically associate the resources in the sample into a randomly named resource group
final String rgName = SdkContext.randomResourceName("rgCOPD", 24);
ResourceGroup resourceGroup = azure.resourceGroups().define(rgName)
                .withRegion(Region.US_EAST)
                .create();
```

本示例中的每个资源由此资源组管理。 这样，稍后便可通过删除资源组来轻松清理资源。

## <a name="define-the-virtual-machines"></a>定义虚拟机
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

上面所示的外部 `for` 循环会循环访问每个区域，并定义虚拟网络可创建对象和存储帐户可创建对象供该区域中的所有虚拟机使用。 详细了解在使用管理库时，如何使用[可创建对象](java-sdk-azure-concepts.md#Creatables)来做到仅有必要时才创建资源。

内部 `for` 循环获取虚拟机的公共 IP 地址可创建对象，然后使用前面定义的虚拟网络、存储帐户和公共 IP 地址的可创建对象来定义虚拟机可创建对象。  然后，此虚拟机可创建对象会添加到 `creatableVirtualMachines` 列表中。

## <a name="create-the-virtual-machines"></a>创建虚拟机

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

`azure.virtualMachines().create(creatableVirtualMachines)` 调用跨区域并行创建 `creatableVirtualMachines` 列表中定义的所有虚拟机。

使用返回的 `CreatedResources<VirtualMachine>` 对象可访问执行 `create()` 方法期间在 Azure 订阅中创建的所有资源，而不仅仅是返回的 `VirtualMachine` 类型。 将 `createdRelatedResources()` 中的返回值强制转换为正确的类型。 

在[库概念文章](java-sdk-azure-concepts.md)中详细了解 `Creatable<T>` 和 `CreatedResources` 的用法。

## <a name="delete-the-resource-group"></a>删除资源组 

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

此块在示例退出之前，删除示例中创建的资源。

## <a name="sample-explanation"></a>示例解释

查看 [Github](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) 上的完整示例代码。

本示例使用 `Creatable` 对象为托管虚拟机的每个区域定义虚拟网络和存储帐户。 然后，为每个虚拟机的公共 IP 地址定义 `Creatable` 对象。 本示例使用这些 `Creatable` 对象定义虚拟机，示例会将 VM 定义添加到 `virtualMachineCreatable` 列表。

代码将每个虚拟机定义添加到列表之后，`azure.virtualMachines().create(creatableVirtualMachines)` 在 Azure 中并行创建每个虚拟机。

然后，示例代码从返回的 CreatedResources 对象获取所有已创建的虚拟机的 IP 地址，以创建[流量管理器](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)在虚拟机之间分配负载。 

即使出错，`finally` 块也会从 Azure 订阅中删除资源。

| 示例中使用的类 | 说明
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | 查询属性并管理虚拟机的状态。 在从 `azure.virtualMachines().list()` 返回的，或者按名称或 ID 执行 `azure.virtualMachines().getByResourceGroup()` 后返回的列表表单中检索
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | 映射到[虚拟机大小选项](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)的静态值，定义虚拟机时用作 `withSize()` 的参数。
| [PublicIpAddress](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._public_i_p_address) | 通过 `azure.publicIpAddresses().define()` 为每个虚拟机定义，但不会立即创建该类。 存储每个 `Creatable` 的键，稍后可通过 `createdRelatedResource()` 检索
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | Linux 虚拟机选项集，在定义虚拟机时用作 `withPopularLinuxImage()` 方法的参数。
| [网络](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | 本示例通过 `azure.networks().define()` 为每个区域定义一个虚拟网络。 

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]