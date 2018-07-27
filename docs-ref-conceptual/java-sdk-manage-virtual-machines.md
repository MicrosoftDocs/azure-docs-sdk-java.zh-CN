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
ms.openlocfilehash: e3048b3317477f4b1fb8edf93e4bebad6b7fafce
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
ms.locfileid: "21931163"
---
# <a name="manage-azure-virtual-machines-from-your-java-applications"></a>从 Java 应用程序管理 Azure 虚拟机

[本示例](https://github.com/Azure-Samples/compute-java-manage-vm/)通过[用于 Java 的 Azure 管理库](https://github.com/Azure/azure-sdk-for-java)来创建和使用 Azure 虚拟机。

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/compute-java-manage-vm.git
cd compute-java-manage-vm
mvn clean compile exec:java
```

查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-windows-virtual-machine"></a>创建 Windows 虚拟机

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

此代码：   

0. 定义大小为 50GB 的 `Disk` 可创建对象，以及用于虚拟机的随机名称。
0. 使用 `azure.virtualMachines().define()..create()` 链创建 Windows Server 2012 虚拟机。 API 创建上一步骤中定义的 `Disk`（与虚拟机同时创建）。 同时，通过 `withNewDataDisk(10)` 将 10GB 数据磁盘附加到虚拟机。

详细了解如何使用 [Creatable<T> 对象](java-sdk-azure-concepts.md#Creatables)（可创建对象）来定义资源的本地表示形式，以及仅在其他 Azure 资源有需要时才创建资源。

## <a name="stop-start-and-restart-a-virtual-machine"></a>停止、启动和重启虚拟机

```java
// look up a virtual machine by its ID and then restart, stop, and start it
azureVM = azure.getVirtualMachine.getById(windowsVM.id());

azureVM.restart();
azureVM.powerOff();
azureVM.start();
```

`powerOff()` 停止虚拟机操作系统，但不解除分配其资源。

## <a name="add-a-virtual-machine-to-an-existing-network"></a>将虚拟机添加到现有网络

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

使用 `withPopularLinuxImage` 可定义 Linux VM，而不能定义 Windows VM。


## <a name="list-virtual-machines"></a>列出虚拟机

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

使用 `azure.virtualMachines().list()` 列出订阅的所有虚拟机，并循环访问 `tags()` 返回的映射，以管理各资源组中虚拟机的有标记集合。

## <a name="update-a-virtual-machine"></a>更新虚拟机

```java
// add a 10GB data disk to the virtual machine
windowsVM.update()
     .withNewDataDisk(10)
     .apply();
```

使用 `update()...apply()` 以及用于配置通过 `define()...create()` 创建的虚拟机的相同方法来更新虚拟机配置。

## <a name="delete-a-virtual-machine"></a>删除虚拟机

```java
// delete by ID if you already are working with the VM object
azure.virtualMachines().deleteById(windowsVM.id());

// delete by resource group and name
azure.virtualMachines().deleteByResourceGroup(rgName,windowsVmName);
```

## <a name="sample-explanation"></a>示例解释

[本示例代码](https://github.com/Azure-Samples/compute-java-manage-vm/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachine.java)创建具有 50GB 数据磁盘的 Windows 虚拟机。 接下来，本示例创建第二个 10GB 数据磁盘并将其附加到此 Windows 虚拟机。
然后，本示例在 Windows 虚拟机所在的同一虚拟网络中创建 Linux 虚拟机。

本示例会记录有关上述两个虚拟机的信息，并在完成之前删除这些虚拟机。

| 示例中使用的类 | 说明
|-------|-------|
| [VirtualMachine](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine) | 查询属性并管理虚拟机的状态。 在使用 `azure.virtualMachines().list()` 返回的，或者按名称或 ID 执行 `azure.virtualMachines().getByResourceGroup()` 后返回的列表表单中检索
| [VirtualMachineSizeTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_size_types) | 包含映射到[虚拟机大小选项](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)的静态值的类，`withSize()` 方法使用它来定义分配给 VM 的资源。
| [磁盘](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk) | 定义磁盘时，使用 `withData()` 创建一个用于存储数据的磁盘，或使用相应的 `withLinux` 或 `withWindows` 方法创建用于存储操作系统映像的磁盘。 在创建时（`using withNewDataDisk` 或 `withExistingDataDisk`）或者在 VirtualMachine 对象上通过 `update()..apply()` 进行创建后将磁盘添加到虚拟机。
| [DiskSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._disk_sku_types) | 包含静态值的类，用于定义采用标准或[高级](https://docs.microsoft.com/azure/storage/storage-premium-storage)存储计划的磁盘。
| [KnownLinuxVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_linux_virtual_machine_image) | 包含一组 Linux 虚拟机选项的类，在定义虚拟机时与 `withPopularLinuxImage()` 方法结合使用。
| [KnownWindowsVirtualMachineImage](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._known_windows_virtual_machine_image) | 包含一组 Windows 虚拟机映像选项的类，在定义虚拟机时与 `withPopularWindowsImage()` 方法结合使用。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]