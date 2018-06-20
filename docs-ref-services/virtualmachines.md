---
title: 用于 Java 的 Azure 虚拟机库
description: ''
keywords: Azure, Java, SDK, API, 计算, 虚拟机
author: douge
ms.author: douge
manager: douge
ms.date: 05/17/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: compute
ms.openlocfilehash: a54bc40e1d28ba6ee1d8b0638cb259adbb69d78d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823750"
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="ccfda-103">Azure 虚拟机库</span><span class="sxs-lookup"><span data-stu-id="ccfda-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="ccfda-104">概述</span><span class="sxs-lookup"><span data-stu-id="ccfda-104">Overview</span></span>

<span data-ttu-id="ccfda-105">运行 Linux 或 Windows 的按需可缩放计算资源。</span><span class="sxs-lookup"><span data-stu-id="ccfda-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="ccfda-106">若要开始使用 Azure 虚拟机，请参阅[使用 Azure 门户创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-portal)。</span><span class="sxs-lookup"><span data-stu-id="ccfda-106">To get started with Azure virtual machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="ccfda-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="ccfda-107">Management API</span></span>

<span data-ttu-id="ccfda-108">使用管理 API 通过代码在 Azure 中创建、配置和横向扩展 Windows 与 Linux 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="ccfda-108">Create, configure, and scale out Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="ccfda-109">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="ccfda-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-compute</artifactId>
    <version>1.3.0</version>
</dependency>
```   


## <a name="example"></a><span data-ttu-id="ccfda-110">示例</span><span class="sxs-lookup"><span data-stu-id="ccfda-110">Example</span></span>

<span data-ttu-id="ccfda-111">在新的 Azure 资源组中创建新的 Linux 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="ccfda-111">Create a new Linux virtual machine in a new Azure resource group.</span></span>

```java
VirtualMachine newLinuxVm = azure.virtualMachines().define(linuxVmName)
            .withRegion(Region.US_EAST)
            .withNewResourceGroup("myResourceGroup")
            .withNewPrimaryNetwork("10.0.0.0/28")
            .withPrimaryPrivateIpAddressDynamic()
            .withoutPrimaryPublicIpAddress()
            .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
            .withRootUsername(userName)
            .withSshKey(key)
            .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
            .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ccfda-112">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="ccfda-112">Explore the Management APIs</span></span>](/java/api/overview/azure/virtualmachines/management)


## <a name="samples"></a><span data-ttu-id="ccfda-113">示例</span><span class="sxs-lookup"><span data-stu-id="ccfda-113">Samples</span></span>

<span data-ttu-id="ccfda-114">[管理虚拟机][1] </span><span class="sxs-lookup"><span data-stu-id="ccfda-114">[Manage virtual machines][1] </span></span>  
<span data-ttu-id="ccfda-115">[管理虚拟网络][6] </span><span class="sxs-lookup"><span data-stu-id="ccfda-115">[Manage virtual networks][6] </span></span>  
<span data-ttu-id="ccfda-116">[从自定义映像创建虚拟机][2] </span><span class="sxs-lookup"><span data-stu-id="ccfda-116">[Create a virtual machine from a custom image][2] </span></span>  
<span data-ttu-id="ccfda-117">[跨区域并行创建虚拟机][5]  </span><span class="sxs-lookup"><span data-stu-id="ccfda-117">[Create virtual machines across regions in parallel][5]  </span></span>  
<span data-ttu-id="ccfda-118">[创建包含负载均衡器的虚拟机规模集][7]</span><span class="sxs-lookup"><span data-stu-id="ccfda-118">[Create a virtual machine scale set with a load balancer][7]</span></span>    

[1]: ../docs-ref-conceptual/java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[5]: ../docs-ref-conceptual/java-sdk-virtual-machines-in-parallel.md
[6]: ../docs-ref-conceptual/java-sdk-manage-virtual-networks.md
[7]: ../docs-ref-conceptual/java-sdk-manage-vm-scalesets.md

<span data-ttu-id="ccfda-119">详细了解可在应用中使用的 [Azure 虚拟机示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=VM)。</span><span class="sxs-lookup"><span data-stu-id="ccfda-119">Explore more [sample Java code for Azure virtual machines](https://azure.microsoft.com/resources/samples/?platform=java&term=VM) you can use in your apps.</span></span>