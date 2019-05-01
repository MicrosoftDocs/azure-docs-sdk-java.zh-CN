---
title: 使用 Java 管理 Azure 虚拟网络 | Microsoft Docs
description: 在 Java 代码中管理 Azure 虚拟网络的示例代码
author: rloutlaw
manager: douge
ms.assetid: 92736911-3df6-46e7-b751-25bb36bf89b9
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 3/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 6989c5184d09ac011cb39eb21ad8d96db3f0c107
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592552"
---
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a><span data-ttu-id="5beaa-103">通过 Java 应用创建和管理 Azure 虚拟网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-103">Create and manage Azure virtual networks from your Java apps</span></span>

<span data-ttu-id="5beaa-104">[本示例](https://github.com/Azure-Samples/network-java-manage-virtual-network)创建一个[虚拟网络](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)来隔离你所控制的网段上的 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="5beaa-104">[This sample](https://github.com/Azure-Samples/network-java-manage-virtual-network) creates a [virtual network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) to isolate your Azure resources on network segment you control.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="5beaa-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="5beaa-105">Run the sample</span></span>

<span data-ttu-id="5beaa-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="5beaa-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="5beaa-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="5beaa-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

<span data-ttu-id="5beaa-108">查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java)。</span><span class="sxs-lookup"><span data-stu-id="5beaa-108">View the [complete code sample on GitHub](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="5beaa-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="5beaa-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a><span data-ttu-id="5beaa-110">创建网络安全组来阻止 Internet 流量</span><span class="sxs-lookup"><span data-stu-id="5beaa-110">Create a network security group to block Internet traffic</span></span>

```java
// this NSG definition block traffic to and from the public Internet
NetworkSecurityGroup backEndSubnetNsg = azure.networkSecurityGroups()
              .define(vnet1BackEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup(rgName)
                    .defineRule("DenyInternetInComing")
                        .denyInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

<span data-ttu-id="5beaa-111">此[网络安全规则](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)阻止入站和出站公共 Internet 流量。</span><span class="sxs-lookup"><span data-stu-id="5beaa-111">This [network security rule](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) blocks both inbound and outbound public Internet traffic.</span></span> <span data-ttu-id="5beaa-112">只有在应用到虚拟网络中的子网之后，此网络安全组才起作用。</span><span class="sxs-lookup"><span data-stu-id="5beaa-112">This network security group will not have an effect until applied to a subnet in your virtual network.</span></span>

## <a name="create-a-virtual-network-with-two-subnets"></a><span data-ttu-id="5beaa-113">创建包含两个子网的虚拟网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-113">Create a virtual network with two subnets</span></span>

```java
// create the a virtual network with two subnets
// assign the backend subnet a rule blocking public internet traffic
Network virtualNetwork1 = azure.networks().define(vnetName1)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withAddressSpace("192.168.0.0/16")
                    .withSubnet(vnet1FrontEndSubnetName, "192.168.1.0/24")
                    .defineSubnet(vnet1BackEndSubnetName)
                        .withAddressPrefix("192.168.2.0/24")
                        .withExistingNetworkSecurityGroup(backEndSubnetNsg)
                        .attach()
                    .create();
```

<span data-ttu-id="5beaa-114">后端子网遵循网络安全组中设置的规则来拒绝 Internet 访问。</span><span class="sxs-lookup"><span data-stu-id="5beaa-114">The backend subnet denies Internet access usingfollowing the rules set in the network security group.</span></span> <span data-ttu-id="5beaa-115">前端子网使用允许向 Internet 发送出站流量的[默认规则](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)。</span><span class="sxs-lookup"><span data-stu-id="5beaa-115">The front end subnet uses the [default rules](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) which allow outbound traffic to the Internet.</span></span>

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a><span data-ttu-id="5beaa-116">创建网络安全组来允许入站 HTTP 流量</span><span class="sxs-lookup"><span data-stu-id="5beaa-116">Create a network security group to allow inbound HTTP traffic</span></span>
```java
// create a rule that allows inbound HTTP and blocks outbound Internet traffic
NetworkSecurityGroup frontEndSubnetNsg = azure.networkSecurityGroups()
               .define(vnet1FrontEndSubnetNsgName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .defineRule("AllowHttpInComing")
                        .allowInbound()
                        .fromAddress("INTERNET")
                        .fromAnyPort()
                        .toAnyAddress()
                        .toPort(80)
                        .withProtocol(SecurityRuleProtocol.TCP)
                        .attach()
                    .defineRule("DenyInternetOutGoing")
                        .denyOutbound()
                        .fromAnyAddress()
                        .fromAnyPort()
                        .toAddress("INTERNET")
                        .toAnyPort()
                        .withAnyProtocol()
                        .attach()
                    .create();
```

<span data-ttu-id="5beaa-117">此网络安全规则在端口 80 上打开来自公共 Internet 的入站流量，并阻止从网络内部发往公共 Internet 的所有出站流量。</span><span class="sxs-lookup"><span data-stu-id="5beaa-117">This network security rule opens up inbound traffic on port 80 from the public Internet, and blocks all outbound traffic from inside the network to the public Internet.</span></span> 

## <a name="update-a-virtual-network"></a><span data-ttu-id="5beaa-118">更新虚拟网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-118">Update a virtual network</span></span>
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

<span data-ttu-id="5beaa-119">更新前端子网，以使用上一步骤中创建的网络安全规则允许入站 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="5beaa-119">Update the front end subnet to allow inbound HTTP traffic using the network security rule created in the previous step.</span></span>

## <a name="create-a-virtual-machine-on-a-subnet"></a><span data-ttu-id="5beaa-120">在子网中创建虚拟机</span><span class="sxs-lookup"><span data-stu-id="5beaa-120">Create a virtual machine on a subnet</span></span>
```java
// attach the new VM to the front end subnet on the virtual network
VirtualMachine frontEndVM = azure.virtualMachines().define(frontEndVmName)
                    .withRegion(Region.US_EAST)
                    .withExistingResourceGroup(rgName)
                    .withExistingPrimaryNetwork(virtualNetwork1) 
                    .withSubnet(vnet1FrontEndSubnetName)
                    .withPrimaryPrivateIpAddressDynamic()
                    .withNewPrimaryPublicIpAddress(publicIpAddressLeafDnsForFrontEndVm)
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();
```

<span data-ttu-id="5beaa-121">`withExistingPrimaryNetwork()` 和 `withSubnet()` 将虚拟机配置为使用前面步骤中创建的虚拟网络上的前端子网。</span><span class="sxs-lookup"><span data-stu-id="5beaa-121">`withExistingPrimaryNetwork()` and `withSubnet()` configure the virtual machine to use the front-end subnet on the virtual network created in the previous steps.</span></span>

## <a name="list-virtual-networks-in-a-resource-group"></a><span data-ttu-id="5beaa-122">列出资源组中的虚拟网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-122">List virtual networks in a resource group</span></span>
```java
// iterate over every virtual network in the resource group 
for (Network virtualNetwork : azure.networks().listByResourceGroup(rgName)) {
    // for each subnet on the virtual network, log the network address prefix 
    for (Map.Entry<String, Subnet> entry : virtualNetwork.subnets().entrySet()) {
        String subnetName = entry.getKey();
        Subnet subnet = entry.getValue();
        System.out.println("Address prefix for subnet " + subnetName + 
             " is " + subnet.addressPrefix());
    }
}
```       

<span data-ttu-id="5beaa-123">可以使用外部集合列出并检查 `Network` 对象，或者按本示例中所示，使用嵌套的 for-each 循环来循环访问每个网络的每个子资源。</span><span class="sxs-lookup"><span data-stu-id="5beaa-123">You can list and inspect `Network` object using the outer collection or iterate through each child resource for each network using the nested for-each loop as seen in this example.</span></span>

## <a name="delete-a-virtual-network"></a><span data-ttu-id="5beaa-124">删除虚拟网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-124">Delete a virtual network</span></span>
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

<span data-ttu-id="5beaa-125">删除某个虚拟网络会删除该网络中的子网，但不会删除应用到子网的网络安全组规则。</span><span class="sxs-lookup"><span data-stu-id="5beaa-125">Removing a virtual network deletes the subnets on the network but does not delete the network security group rules applied to the subnets.</span></span> <span data-ttu-id="5beaa-126">可将这些定义重新应用到其他子网。</span><span class="sxs-lookup"><span data-stu-id="5beaa-126">Those definitions can be reapplied to other subnets.</span></span>

## <a name="sample-explanation"></a><span data-ttu-id="5beaa-127">示例解释</span><span class="sxs-lookup"><span data-stu-id="5beaa-127">Sample explanation</span></span>

<span data-ttu-id="5beaa-128">本示例创建包含两个子网的虚拟网络，其中每个子网包含一个虚拟机。</span><span class="sxs-lookup"><span data-stu-id="5beaa-128">This sample creates a virtual network with two subnets and with one virtual machine on each subnet.</span></span> <span data-ttu-id="5beaa-129">后端子网已从公共 Internet 断开。</span><span class="sxs-lookup"><span data-stu-id="5beaa-129">The back subnet is cut off from the public Internet.</span></span> <span data-ttu-id="5beaa-130">面向前端的子网接受来自 Internet 的入站 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="5beaa-130">The front-facing subnet accepts inbound HTTP traffic from the Internet.</span></span> <span data-ttu-id="5beaa-131">虚拟网络中的两个虚拟机通过默认网络安全组规则相互通信。</span><span class="sxs-lookup"><span data-stu-id="5beaa-131">Both virtual machines in the virtual network communicate with each other through the default network security group rules.</span></span>

| <span data-ttu-id="5beaa-132">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="5beaa-132">Class used in sample</span></span> | <span data-ttu-id="5beaa-133">说明</span><span class="sxs-lookup"><span data-stu-id="5beaa-133">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="5beaa-134">网络</span><span class="sxs-lookup"><span data-stu-id="5beaa-134">Network</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | <span data-ttu-id="5beaa-135">从 `azure.networks().define()...create()` 创建的虚拟网络的本地对象表示形式。</span><span class="sxs-lookup"><span data-stu-id="5beaa-135">Local object representation of the virtual network created from `azure.networks().define()...create()` .</span></span> <span data-ttu-id="5beaa-136">使用 `update()...apply()` Fluent 链更新现有的虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="5beaa-136">Use the `update()...apply()` fluent chain to update an existing virtual network.</span></span>
| [<span data-ttu-id="5beaa-137">子网</span><span class="sxs-lookup"><span data-stu-id="5beaa-137">Subnet</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | <span data-ttu-id="5beaa-138">使用 `withSubnet()` 定义或更新网络时，在虚拟网络中创建子网。</span><span class="sxs-lookup"><span data-stu-id="5beaa-138">Create subnets on the virtual network when defining or updating the network using `withSubnet()`.</span></span> <span data-ttu-id="5beaa-139">通过 `Network.subnets().get()` 或 `Network.subnets().entrySet()` 获取子网的对象表示形式。</span><span class="sxs-lookup"><span data-stu-id="5beaa-139">Get object representations of a subnet from `Network.subnets().get()` or `Network.subnets().entrySet()`.</span></span> <span data-ttu-id="5beaa-140">这些对象提供用于查询子网属性的方法。</span><span class="sxs-lookup"><span data-stu-id="5beaa-140">These objects have methods to query subnet properties.</span></span>
| [<span data-ttu-id="5beaa-141">NetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="5beaa-141">NetworkSecurityGroup</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | <span data-ttu-id="5beaa-142">使用 `azure.networkSecurityGroups().define()...create()` Fluent 链创建，然后通过在虚拟网络中更新或创建子网应用到子网。</span><span class="sxs-lookup"><span data-stu-id="5beaa-142">Created using the `azure.networkSecurityGroups().define()...create()` fluent chain and then applied to subnets through the updating or creating subnets in a virtual network.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5beaa-143">后续步骤</span><span class="sxs-lookup"><span data-stu-id="5beaa-143">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]