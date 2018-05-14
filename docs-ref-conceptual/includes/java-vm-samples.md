| **创建虚拟机** || 
|---|---|
| [管理虚拟机][1] | 创建、修改、启动、停止和删除虚拟机。 |
| [从自定义映像创建虚拟机][2] | 创建自定义虚拟机映像并使用它创建新的虚拟机。 | 
| [通过快照使用专用 VHD 创建虚拟机][3] | 通过虚拟机的 OS 和数据磁盘创建快照，通过快照创建托管磁盘，然后通过附加托管磁盘来创建虚拟机。 |  
| [在同一网络中并行创建虚拟机][4] | 在同一区域中包含两个子网的同一虚拟网络上并行创建虚拟机。 |
| [跨区域并行创建虚拟机][5] | 跨多个 Azure 区域创建并负载均衡一组虚拟机。 |
| **网络虚拟机** || 
| [管理虚拟网络][6] | 设置包含两个子网的虚拟网络并限制对其进行 Internet 访问。 |
| **创建规模集** ||
| [创建包含负载均衡器的虚拟机规模集][7] | 创建 VM 缩放集、设置负载均衡器，并获取规模集 VM 的 SSH 连接字符串。 |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md