---
ms.openlocfilehash: ab31ee32ea940db2d7bcfa2fe36475d8a648bfc9
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592604"
---
| <span data-ttu-id="fa134-101">**创建虚拟机**</span><span class="sxs-lookup"><span data-stu-id="fa134-101">**Create virtual machines**</span></span> || 
|---|---|
| <span data-ttu-id="fa134-102">[管理虚拟机][1]</span><span class="sxs-lookup"><span data-stu-id="fa134-102">[Manage virtual machines][1]</span></span> | <span data-ttu-id="fa134-103">创建、修改、启动、停止和删除虚拟机。</span><span class="sxs-lookup"><span data-stu-id="fa134-103">Create, modify, start, stop, and delete virtual machines.</span></span> |
| <span data-ttu-id="fa134-104">[从自定义映像创建虚拟机][2]</span><span class="sxs-lookup"><span data-stu-id="fa134-104">[Create a virtual machine from a custom image][2]</span></span> | <span data-ttu-id="fa134-105">创建自定义虚拟机映像并使用它创建新的虚拟机。</span><span class="sxs-lookup"><span data-stu-id="fa134-105">Create a custom virtual machine image and use it to create new virtual machines.</span></span> | 
| <span data-ttu-id="fa134-106">[通过快照使用专用 VHD 创建虚拟机][3]</span><span class="sxs-lookup"><span data-stu-id="fa134-106">[Create a virtual machine using specialized VHD from a snapshot][3]</span></span> | <span data-ttu-id="fa134-107">通过虚拟机的 OS 和数据磁盘创建快照，通过快照创建托管磁盘，然后通过附加托管磁盘来创建虚拟机。</span><span class="sxs-lookup"><span data-stu-id="fa134-107">Create snapshot from the virtual machine's OS and data disks, create managed disks from the snapshots, and then create a virtual machine by attaching the managed disks.</span></span> |  
| <span data-ttu-id="fa134-108">[在同一网络中并行创建虚拟机][4]</span><span class="sxs-lookup"><span data-stu-id="fa134-108">[Create virtual machines in parallel in the same network][4]</span></span> | <span data-ttu-id="fa134-109">在同一区域中包含两个子网的同一虚拟网络上并行创建虚拟机。</span><span class="sxs-lookup"><span data-stu-id="fa134-109">Create virtual machines in the same region on the same virtual network with two subnets in parallel.</span></span> |
| <span data-ttu-id="fa134-110">[跨区域并行创建虚拟机][5]</span><span class="sxs-lookup"><span data-stu-id="fa134-110">[Create virtual machines across regions in parallel][5]</span></span> | <span data-ttu-id="fa134-111">跨多个 Azure 区域创建并负载均衡一组虚拟机。</span><span class="sxs-lookup"><span data-stu-id="fa134-111">Create and load-balance a set of virtual machines across multiple Azure regions.</span></span> |
| <span data-ttu-id="fa134-112">**网络虚拟机**</span><span class="sxs-lookup"><span data-stu-id="fa134-112">**Network virtual machines**</span></span> || 
| <span data-ttu-id="fa134-113">[管理虚拟网络][6]</span><span class="sxs-lookup"><span data-stu-id="fa134-113">[Manage virtual networks][6]</span></span> | <span data-ttu-id="fa134-114">设置包含两个子网的虚拟网络并限制对其进行 Internet 访问。</span><span class="sxs-lookup"><span data-stu-id="fa134-114">Set up a virtual network with two subnets and restrict Internet access to them.</span></span> |
| <span data-ttu-id="fa134-115">**创建规模集**</span><span class="sxs-lookup"><span data-stu-id="fa134-115">**Create scale sets**</span></span> ||
| <span data-ttu-id="fa134-116">[创建包含负载均衡器的虚拟机规模集][7]</span><span class="sxs-lookup"><span data-stu-id="fa134-116">[Create a virtual machine scale set with a load balancer][7]</span></span> | <span data-ttu-id="fa134-117">创建 VM 缩放集、设置负载均衡器，并获取规模集 VM 的 SSH 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="fa134-117">Create a VM scale set, set up a load balancer, and get SSH connection strings to the scale set VMs.</span></span> |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md