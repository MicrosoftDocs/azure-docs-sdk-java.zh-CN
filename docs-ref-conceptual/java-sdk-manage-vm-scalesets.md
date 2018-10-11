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
# <a name="manage-azure-virtual-machine-scale-sets-from-your-java-applications"></a><span data-ttu-id="e8b89-103">从 Java 应用程序管理 Azure 虚拟机规模集</span><span class="sxs-lookup"><span data-stu-id="e8b89-103">Manage Azure virtual machine scale sets from your Java applications</span></span>

<span data-ttu-id="e8b89-104">[本示例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets)使用 [Java 管理库](https://github.com/Azure/azure-sdk-for-java)创建[虚拟机规模集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)。</span><span class="sxs-lookup"><span data-stu-id="e8b89-104">[This sample](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets) creates a  [virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) using the [Java management libraries](https://github.com/Azure/azure-sdk-for-java).</span></span> 

## <a name="run-the-sample"></a><span data-ttu-id="e8b89-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="e8b89-105">Run the sample</span></span>

<span data-ttu-id="e8b89-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="e8b89-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="e8b89-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="e8b89-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets.git
cd compute-java-manage-virtual-machine-scale-sets
mvn clean compile exec:java
```

<span data-ttu-id="e8b89-108">查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)。</span><span class="sxs-lookup"><span data-stu-id="e8b89-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="e8b89-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="e8b89-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-virtual-network-for-the-scale-set"></a><span data-ttu-id="e8b89-110">为规模集创建虚拟网络</span><span class="sxs-lookup"><span data-stu-id="e8b89-110">Create a virtual network for the scale set</span></span>

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

<span data-ttu-id="e8b89-111">在创建规模集定义之前设置虚拟网络和负载均衡器。</span><span class="sxs-lookup"><span data-stu-id="e8b89-111">Set up a virtual network and load balancer before creating the scale set definition.</span></span> <span data-ttu-id="e8b89-112">规模集为其初始配置使用这些资源。</span><span class="sxs-lookup"><span data-stu-id="e8b89-112">The scale set uses these resources for its initial configuration.</span></span>

## <a name="create-a-load-balancer-to-distribute-load-across-the-scale-set"></a><span data-ttu-id="e8b89-113">创建负载均衡器以便在规模集中分配负载</span><span class="sxs-lookup"><span data-stu-id="e8b89-113">Create a load balancer to distribute load across the scale set</span></span>

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

 <span data-ttu-id="e8b89-114">负载均衡器定义两个后端网络地址池 - 一个用于均衡 HTTP 负载 (`backendPoolName1`)，另一个用于均衡 HTTPS 负载 (`backendPoolName2`)。</span><span class="sxs-lookup"><span data-stu-id="e8b89-114">The load balancer defines two backend network address pools-one to balance load across HTTP (`backendPoolName1`) and the other to balance load across HTTPS (`backendPoolName2`).</span></span>  <span data-ttu-id="e8b89-115">`defineHttpProbe()` 方法在负载均衡器上设置[运行状况探测终结点](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)。</span><span class="sxs-lookup"><span data-stu-id="e8b89-115">The `defineHttpProbe()` methods set up [health probe endpoints](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) on the load balancers.</span></span> <span data-ttu-id="e8b89-116">NAT 规则公开规模集虚拟机上的端口 22 和 23 以进行 telnet 和 SSH 访问。</span><span class="sxs-lookup"><span data-stu-id="e8b89-116">NAT rules expose ports 22 and 23 on the scale set virtual machines for telnet and SSH access.</span></span>

## <a name="create-a-scale-set"></a><span data-ttu-id="e8b89-117">创建规模集</span><span class="sxs-lookup"><span data-stu-id="e8b89-117">Create a scale set</span></span>
 
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

<span data-ttu-id="e8b89-118">使用上一步骤中创建的虚拟网络定义和负载均衡器定义创建包含三个 Linux 实例的规模集 (`withCapacity(3)`)，其中每个实例包含三个 100GB 数据磁盘。</span><span class="sxs-lookup"><span data-stu-id="e8b89-118">Use the virtual network definition and load balancer definitions created in the previous step to create a scale set with three Linux instances (`withCapacity(3)`) and three 100GB data disks each.</span></span> <span data-ttu-id="e8b89-119">`defineNewExtension()` 方法链在每个 VM 上安装 Apache Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="e8b89-119">The `defineNewExtension()` method chain installs the Apache web server on each VM.</span></span>

## <a name="list-virtual-machine-scale-set-network-interfaces"></a><span data-ttu-id="e8b89-120">列出虚拟机规模集网络接口</span><span class="sxs-lookup"><span data-stu-id="e8b89-120">List virtual machine scale set network interfaces</span></span>

```java
// List network interfaces on the scale set and iterate through them
PagedList<VirtualMachineScaleSetNetworkInterface> vmssNics = 
     virtualMachineScaleSet.listNetworkInterfaces();
for (VirtualMachineScaleSetNetworkInterface vmssNic : vmssNics) {
    System.out.println(vmssNic.id());
}
```

## <a name="get-ssh-connection-strings-for-each-scale-set-virtual-machine"></a><span data-ttu-id="e8b89-121">获取每个规模集虚拟机的 SSH 连接字符串</span><span class="sxs-lookup"><span data-stu-id="e8b89-121">Get SSH connection strings for each scale set virtual machine</span></span>

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

<span data-ttu-id="e8b89-122">前面创建的 NAT 池已将虚拟机上的 SSH 和 telnet 端口（分别为 22 和 23）映射到负载均衡器上的端口。</span><span class="sxs-lookup"><span data-stu-id="e8b89-122">The NAT pool created earlier mapped the SSH and telnet ports (22 and 23, respectively) on the virtual machines to ports on the load balancer.</span></span> <span data-ttu-id="e8b89-123">此代码为每个虚拟机生成 SSH 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="e8b89-123">This code builds the SSH connection string for each virtual machine.</span></span>

## <a name="stop-the-virtual-machine-scale-set"></a><span data-ttu-id="e8b89-124">停止虚拟机规模集</span><span class="sxs-lookup"><span data-stu-id="e8b89-124">Stop the virtual machine scale set</span></span>

```java
// stop (not deallocate) all scale set instances
virtualMachineScaleSet.powerOff();
```

<span data-ttu-id="e8b89-125">已停止的虚拟机会继续消耗保留的资源。</span><span class="sxs-lookup"><span data-stu-id="e8b89-125">Stopped virtual machines continue to consume reserved resources.</span></span> <span data-ttu-id="e8b89-126">使用 `deallocate()` 可停止虚拟机上的操作系统并释放虚拟机的计算资源。</span><span class="sxs-lookup"><span data-stu-id="e8b89-126">Use `deallocate()` to stop the operating system on the virtual machines and release their compute resources.</span></span>

## <a name="deallocate-the-virtual-machine-scale-set"></a><span data-ttu-id="e8b89-127">解除分配虚拟机规模集</span><span class="sxs-lookup"><span data-stu-id="e8b89-127">Deallocate the virtual machine scale set</span></span>

```java
// deallocate the virtual machine scale set
virtualMachineScaleSet.deallocate();
```

<span data-ttu-id="e8b89-128">Deallocate() 可关闭虚拟机上的操作系统，并释放规模集实例使用的计算和网络资源（例如 IP 地址）。</span><span class="sxs-lookup"><span data-stu-id="e8b89-128">Deallocate() shuts down the operating system on the virtual machines and frees up the compute and network resources (such as IP addresses) used by the scale set instances.</span></span> <span data-ttu-id="e8b89-129">附加到虚拟机的所有磁盘（包括 OS 磁盘）仍会产生存储费用。</span><span class="sxs-lookup"><span data-stu-id="e8b89-129">You continue to accrue storage charges for any disks (including the OS) attached to the virtual machines.</span></span>

## <a name="start-a-virtual-machine-scale-set"></a><span data-ttu-id="e8b89-130">启动虚拟机规模集</span><span class="sxs-lookup"><span data-stu-id="e8b89-130">Start a virtual machine scale set</span></span>

```java
// start a deallocated or stopped virtual machine scale set
virtualMachineScaleSet.start();
```

## <a name="update-the-number-of-virtual-machines-instances-in-the-scale-set"></a><span data-ttu-id="e8b89-131">更新规模集中的虚拟机实例数目</span><span class="sxs-lookup"><span data-stu-id="e8b89-131">Update the number of virtual machines instances in the scale set</span></span>
```java
// increase the number of virtual machine scale set instances from three to six
virtualMachineScaleSet.update()
                    .withCapacity(6)
                    .apply();
```

<span data-ttu-id="e8b89-132">使用 `withCapacity()` 缩放规模集中的虚拟机数目，使用 `withSku()` 缩放每个虚拟机的容量。</span><span class="sxs-lookup"><span data-stu-id="e8b89-132">Scale the number of virtual machines in the scale set using `withCapacity()` and scale the capacity of each virtual machine using `withSku()`.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="e8b89-133">示例解释</span><span class="sxs-lookup"><span data-stu-id="e8b89-133">Sample explanation</span></span>

<span data-ttu-id="e8b89-134">[示例代码](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java)首先为规模集创建一个虚拟网络用于通信，并创建一个负载均衡器用于在虚拟机之间分配流量。</span><span class="sxs-lookup"><span data-stu-id="e8b89-134">[The sample code](https://github.com/Azure-Samples/compute-java-manage-virtual-machine-scale-sets/blob/master/src/main/java/com/microsoft/azure/management/compute/samples/ManageVirtualMachineScaleSet.java) first creates a virtual network for the scale set to communicate across and a load balancer to distribute traffic across the virtual machines.</span></span> <span data-ttu-id="e8b89-135">`azure.virtualMachineScaleSets().define()...create()` 方法链创建包含三个 Linux 实例的、运行 Apache Web 服务器的规模集。</span><span class="sxs-lookup"><span data-stu-id="e8b89-135">The `azure.virtualMachineScaleSets().define()...create()` method chain creates the scale set with three Linux instances running the Apache web server.</span></span>    
   
| <span data-ttu-id="e8b89-136">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="e8b89-136">Class used in sample</span></span> | <span data-ttu-id="e8b89-137">说明</span><span class="sxs-lookup"><span data-stu-id="e8b89-137">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="e8b89-138">VirtualMachineScaleSet</span><span class="sxs-lookup"><span data-stu-id="e8b89-138">VirtualMachineScaleSet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set) | <span data-ttu-id="e8b89-139">查询、启动、停止、更新和删除规模集中的所有虚拟机。</span><span class="sxs-lookup"><span data-stu-id="e8b89-139">Query, start, stop, update and delete all virtual machines in the scale set.</span></span>
| [<span data-ttu-id="e8b89-140">VirtualMachineScaleSetVM</span><span class="sxs-lookup"><span data-stu-id="e8b89-140">VirtualMachineScaleSetVM</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_v_m) | <span data-ttu-id="e8b89-141">从 `virtualMachineScaleSet.virtualMachines().get()` 或 `list()` 检索，用于查询、启动、停止、配置和删除规模集中的虚拟机。</span><span class="sxs-lookup"><span data-stu-id="e8b89-141">Retrieved from `virtualMachineScaleSet.virtualMachines().get()` or `list()`, allows you to query, start, stop, configure and delete virtual machines in the scale set.</span></span>
| [<span data-ttu-id="e8b89-142">VirtualMachineScaleSetNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="e8b89-142">VirtualMachineScaleSetNetworkInterface</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_network_interface) | <span data-ttu-id="e8b89-143">从 `virtualMachineScaleSet.listNetworkInterfaces()` 返回，规模集中虚拟机上的网络接口的只读表示形式。</span><span class="sxs-lookup"><span data-stu-id="e8b89-143">Returned from `virtualMachineScaleSet.listNetworkInterfaces()`, read-only representation of a network interface on a virtual machine in a scale set.</span></span>
| [<span data-ttu-id="e8b89-144">VirtualMachineScaleSetSkuTypes</span><span class="sxs-lookup"><span data-stu-id="e8b89-144">VirtualMachineScaleSetSkuTypes</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.compute._virtual_machine_scale_set_sku_types) | <span data-ttu-id="e8b89-145">静态字段的类，用于设置[虚拟机规模集层](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/)来定义规模集成员可以消耗的资源量。</span><span class="sxs-lookup"><span data-stu-id="e8b89-145">Class of static fields used to set the [virtual machine scale set tier](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) used to define how much resources scale set members can consume.</span></span>
| [<span data-ttu-id="e8b89-146">VirtualMachineScaleSetNicIpConfiguration</span><span class="sxs-lookup"><span data-stu-id="e8b89-146">VirtualMachineScaleSetNicIpConfiguration</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._virtual_machine_scale_set_nic_i_p_configuration) | <span data-ttu-id="e8b89-147">用于查询与规模集虚拟机上的网络接口关联的 IP 配置。</span><span class="sxs-lookup"><span data-stu-id="e8b89-147">Used to query the IP configuration associated with a network interface on a scale set virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8b89-148">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e8b89-148">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]