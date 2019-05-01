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
# <a name="create-and-manage-azure-virtual-networks-from-your-java-apps"></a>通过 Java 应用创建和管理 Azure 虚拟网络

[本示例](https://github.com/Azure-Samples/network-java-manage-virtual-network)创建一个[虚拟网络](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)来隔离你所控制的网段上的 Azure 资源。

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/network-java-manage-virtual-network.git
cd network-java-manage-virtual-network
mvn clean compile exec:java
```

查看 [GitHub 上的完整代码示例](https://github.com/Azure-Samples/network-java-manage-virtual-network/blob/master/src/main/java/com/microsoft/azure/management/network/samples/ManageVirtualNetwork.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-network-security-group-to-block-internet-traffic"></a>创建网络安全组来阻止 Internet 流量

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

此[网络安全规则](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)阻止入站和出站公共 Internet 流量。 只有在应用到虚拟网络中的子网之后，此网络安全组才起作用。

## <a name="create-a-virtual-network-with-two-subnets"></a>创建包含两个子网的虚拟网络

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

后端子网遵循网络安全组中设置的规则来拒绝 Internet 访问。 前端子网使用允许向 Internet 发送出站流量的[默认规则](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)。

## <a name="create-a-network-security-group-to-allow-inbound-http-traffic"></a>创建网络安全组来允许入站 HTTP 流量
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

此网络安全规则在端口 80 上打开来自公共 Internet 的入站流量，并阻止从网络内部发往公共 Internet 的所有出站流量。 

## <a name="update-a-virtual-network"></a>更新虚拟网络
```java
// update the front end subnet to use the rules in the new network security group
virtualNetwork1.update()
          .updateSubnet(vnet1FrontEndSubnetName)
          .withExistingNetworkSecurityGroup(frontEndSubnetNsg)
          .parent()
          .apply();
```

更新前端子网，以使用上一步骤中创建的网络安全规则允许入站 HTTP 流量。

## <a name="create-a-virtual-machine-on-a-subnet"></a>在子网中创建虚拟机
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

`withExistingPrimaryNetwork()` 和 `withSubnet()` 将虚拟机配置为使用前面步骤中创建的虚拟网络上的前端子网。

## <a name="list-virtual-networks-in-a-resource-group"></a>列出资源组中的虚拟网络
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

可以使用外部集合列出并检查 `Network` 对象，或者按本示例中所示，使用嵌套的 for-each 循环来循环访问每个网络的每个子资源。

## <a name="delete-a-virtual-network"></a>删除虚拟网络
```java
// if you already have the virtual network object it is easiest to delete by ID
azure.networks().deleteById(virtualNetwork1.id());

// Delete by resource group and name if you don't have the VirtualMachine object
azure.networks().deleteByResourceGroup(rgName,vnetName1);
```

删除某个虚拟网络会删除该网络中的子网，但不会删除应用到子网的网络安全组规则。 可将这些定义重新应用到其他子网。

## <a name="sample-explanation"></a>示例解释

本示例创建包含两个子网的虚拟网络，其中每个子网包含一个虚拟机。 后端子网已从公共 Internet 断开。 面向前端的子网接受来自 Internet 的入站 HTTP 流量。 虚拟网络中的两个虚拟机通过默认网络安全组规则相互通信。

| 示例中使用的类 | 说明
|-------|-------|
| [网络](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network) | 从 `azure.networks().define()...create()` 创建的虚拟网络的本地对象表示形式。 使用 `update()...apply()` Fluent 链更新现有的虚拟网络。
| [子网](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._subnet) | 使用 `withSubnet()` 定义或更新网络时，在虚拟网络中创建子网。 通过 `Network.subnets().get()` 或 `Network.subnets().entrySet()` 获取子网的对象表示形式。 这些对象提供用于查询子网属性的方法。
| [NetworkSecurityGroup](https://docs.microsoft.com/java/api/com.microsoft.azure.management.network._network_security_group) | 使用 `azure.networkSecurityGroups().define()...create()` Fluent 链创建，然后通过在虚拟网络中更新或创建子网应用到子网。 

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]