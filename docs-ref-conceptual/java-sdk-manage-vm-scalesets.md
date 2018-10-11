---
title: 使用 Java 管理虚拟机规模集 | Microsoft Docs
description: 使用用于 Java 的 Azure SDK 管理 Azure 虚拟机规模集的示例代码
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 4653726b387369c18942b6c11392f15b9f0351f3
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893488"
---
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a>从 Java 应用程序管理 Azure 虚拟机规模集

[本示例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)使用 [Java 管理库](https://github.com/Azure/azure-sdk-for-java)创建[虚拟机规模集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)。 

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a>为规模集创建虚拟网络

```java
Network network = azure.networks().define(vnetName)
                    .withRegion(region)
                    .withNewResourceGroup(rgName)
                    .withAddressSpace("172.16.0.0/16")
                    .defineSubnet("Front-end")
                    .withAddressPrefix("172.16.1.0/24")
                    .attach()
                    .create();
```

在创建规模集定义之前设置虚拟网络和负载均衡器。 规模集为其初始配置使用这些资源。

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a>创建负载均衡器以便在规模集中分配负载

```java
LoadBalancer loadBalancer1 = azure.loadBalancers().define(loadBalancerName1)
                    .withRegion(region)
                    .withExistingResourceGroup(rgName)
                    // assign a public IP address to the load balancer
                    .definePublicFrontend(frontendName)
                        .withExistingPublicIPAddress(publicIPAddress)
                        .attach()
                    // Add two backend address pools
                    .defineBackend(backendPoolName1)
                        .attach()
                    .defineBackend(backendPoolName2)
                        .attach()
                    // Add two health probes on 80 and 443
                    .defineHttpProbe(httpProbe)
                        .withRequestPath("/")
                        .withPort(80)
                        .attach()
                    .defineHttpProbe(httpsProbe)
                        .withRequestPath("/")
                        .withPort(443)
                        .attach()

                    // balance HTTP and HTTPs traffic
                    .defineLoadBalancingRule(httpLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(80)
                        .withProbe(httpProbe)
                        .withBackend(backendPoolName1)
                        .attach()
                    .defineLoadBalancingRule(httpsLoadBalancingRule)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPort(443)
                        .withProbe(httpsProbe)
                        .withBackend(backendPoolName2)
                        .attach()

                    // Add NAT definitions to enable SSH and telnet to the VMs 
                    .defineInboundNatPool(natPool50XXto22)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(5000, 5099)
                        .withBackendPort(22)
                        .attach()
                    .defineInboundNatPool(natPool60XXto23)
                        .withProtocol(TransportProtocol.TCP)
                        .withFrontend(frontendName)
                        .withFrontendPortRange(6000, 6099)
                        .withBackendPort(23)
                        .attach()
                    .create();
```

 负载均衡器定义两个后端网络地址池 - 一个用于均衡 HTTP 负载 (`backendPoolName1`)，另一个用于均衡 HTTPS 负载 (`backendPoolName2`)。  `defineHttpProbe()` 方法在负载均衡器上设置[运行状况探测终结点](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)。 NAT 规则公开规模集虚拟机上的端口 22 和 23 以进行 telnet 和 SSH 访问。

## <a name="create-a-scale-set"></a>创建规模集
 
```java
 // Create a virtual machine scale set with three virtual machines
 // And, install Apache Web servers on them
VirtualMachineScaleSet virtualMachineScaleSet = azure.virtualMachineScaleSets()
       .define(vmssName)
                .withRegion(region)
                .withExistingResourceGroup(rgName)
                .withSku(VirtualMachineScaleSetSkuTypes.STANDARD_D3_V2)
                .withExistingPrimaryNetworkSubnet(network, "Front-end")
                .withExistingPrimaryInternetFacingLoadBalancer(loadBalancer1)
                .withPrimaryInternetFacingLoadBalancerBackends(backendPoolName1, backendPoolName2)
                .withPrimaryInternetFacingLoadBalancerInboundNatPools(natPool50XXto22, natPool60XXto23)
                .withoutPrimaryInternalLoadBalancer()
                .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                .withRootUsername(userName)
                .withSsh(sshKey)
                .withNewDataDisk(100)
                .withNewDataDisk(100, 1, CachingTypes.READ_WRITE)
                .withNewDataDisk(100, 2, 
                     CachingTypes.READ_WRITE, StorageAccountTypes.STANDARD_LRS)
                .withCapacity(3)
                // Use a VM extension to install Apache Web servers
                .defineNewExtension("CustomScriptForLinux")
                    .withPublisher("Microsoft.OSTCExtensions")
                    .withType("CustomScriptForLinux")
                    .withVersion("1.4")
                    .withMinorVersionAutoUpgrade()
                    .withPublicSetting("fileUris", fileUris)
                    .withPublicSetting("commandToExecute", installCommand)
                    .attach()
                .create();
```

使用上一步骤中创建的虚拟网络定义和负载均衡器定义创建包含三个 Linux 实例的规模集 (`withCapacity(3)`)，其中每个实例包含三个 100GB 数据磁盘。 `defineNewExtension()` 方法链在每个 VM 上安装 Apache Web 服务器。

## <a name="list-virtual-machine-scale-set-network-interfaces"></a>列出虚拟机规模集网络接口

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a>获取每个规模集虚拟机的 SSH 连接字符串

```java
for (VirtualMachineScaleSetVM instance : virtualMachineScaleSet.virtualMachines().list()) {
    System.out.println("Scale set virtual machine instance #" + instance.instanceId());
    System.out.println(instance.id());
    PagedList<VirtualMachineScaleSetNetworkInterface> networkInterfaces = 
         instance.listNetworkInterfaces();
    // Pick the first NIC on the instance and use its primary IP address
    VirtualMachineScaleSetNetworkInterface networkInterface = networkInterfaces.get(0);
    for (VirtualMachineScaleSetNicIPConfiguration ipConfig : networkInterface.ipConfigurations().values()) {
        if (ipConfig.isPrimary()) {
            List<LoadBalancerInboundNatRule> natRules = ipConfig.listAssociatedLoadBalancerInboundNatRules();
            for (LoadBalancerInboundNatRule natRule : natRules) {
                // find rule matching the inbound SSH port on the backend for the IP address
                // and return the SSH connection string to that port on the load balancer
                if (natRule.backendPort() == 22) {
                    System.out.println("SSH connection string: " + userName + 
                        "@" + publicIPAddress.fqdn() + ":" + natRule.frontendPort());
                break;
                }
            }
            break;
        }
    }
}
```

前面创建的 NAT 池已将虚拟机上的 SSH 和 telnet 端口（分别为 22 和 23）映射到负载均衡器上的端口。 此代码为每个虚拟机生成 SSH 连接字符串。

## <a name="stop-the-virtual-machine-scale-set"></a>停止虚拟机规模集

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

已停止的虚拟机会继续消耗保留的资源。 使用 `deallocate()` 可停止虚拟机上的操作系统并释放虚拟机的计算资源。

## <a name="deallocate-the-virtual-machine-scale-set"></a>解除分配虚拟机规模集

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

Deallocate() 可关闭虚拟机上的操作系统，并释放规模集实例使用的计算和网络资源（例如 IP 地址）。 附加到虚拟机的所有磁盘（包括 OS 磁盘）仍会产生存储费用。

## <a name="start-a-virtual-machine-scale-set"></a>启动虚拟机规模集

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a>更新规模集中的虚拟机实例数目
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

使用 `withCapacity()` 缩放规模集中的虚拟机数目，使用 `withSku()` 缩放每个虚拟机的容量。

## <a name="sample-explanation"></a>示例解释

[示例代码](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)首先为规模集创建一个虚拟网络用于通信，并创建一个负载均衡器用于在虚拟机之间分配流量。 `azure.virtualMachineScaleSets().define()...create()` 方法链创建包含三个 Linux 实例的、运行 Apache Web 服务器的规模集。    
   
| 示例中使用的类 | 说明
|-------|-------|
| [VirtualMachineScaleSet](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | 查询、启动、停止、更新和删除规模集中的所有虚拟机。
| [VirtualMachineScaleSetVM](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | 从 `virtualMachineScaleSet.virtualMachines().get()` 或 `list()` 检索，用于查询、启动、停止、配置和删除规模集中的虚拟机。
| [VirtualMachineScaleSetNetworkInterface](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | 从 `virtualMachineScaleSet.listNetworkInterfaces()` 返回，规模集中虚拟机上的网络接口的只读表示形式。
| [VirtualMachineScaleSetSkuTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | 静态字段的类，用于设置[虚拟机规模集层](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)来定义规模集成员可以消耗的资源量。
| [VirtualMachineScaleSetNicIpConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | 用于查询与规模集虚拟机上的网络接口关联的 IP 配置。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]