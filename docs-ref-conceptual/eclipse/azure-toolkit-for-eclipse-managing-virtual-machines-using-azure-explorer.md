---
title: 使用用于 Eclipse 的 Azure 资源管理器管理虚拟机
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure 虚拟机。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: c04f5225f0bb99898f69b26a4782aa57d75c4f22
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090655"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>使用用于 Eclipse 的 Azure 资源管理器管理虚拟机

Azure 资源管理器是用于 Eclipse 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse 集成开发环境 (IDE) 内部管理其 Azure 帐户中的虚拟机。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a>在 Eclipse 中创建虚拟机

若要使用 Azure 资源管理器创建虚拟机，请执行以下操作：

1. 按照[用于 Eclipse 的 Azure 工具包的登录说明](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)中的步骤登录到 Azure 帐户。

2. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“虚拟机”，并单击“创建 VM”。

   ![“创建 VM”命令][CR01]  

   此时会打开“新建虚拟机”向导。

3. 在“选择订阅”窗口中选择订阅，并单击“下一步”。

   ![“选择订阅”窗口][CR02]

4. 在“选择虚拟机映像”窗口中输入以下信息：

   * **位置**：指定将创建虚拟机的位置（例如“美国西部”）。

   * 发布者：指定创建了用于创建虚拟机的映像的发布者（例如“Microsoft”）。

   * 
  **
  **产品/服务：指定所选发布者提供的可以使用的虚拟机产品/服务（例如“JDK”）。**

   * Sku：从所选产品/服务中指定要使用的库存单位 (SKU)（例如“JDK_8”）。

   * 版本号：从所选 SKU 中指定要使用哪个版本。

   ![“选择虚拟机映像”窗口][CR03]

5. 单击“下一步”。

6. 在“虚拟机基本设置”窗口中输入以下信息：

   * **虚拟机名称**：指定新虚拟机的名称，该名称必须以字母开头并仅包含字母、数字和连字符。

   * **大小**：指定要为虚拟机分配的内核数和内存。

   * **用户名**：指定要创建的用于管理虚拟机的管理员帐户。

   * **密码**和**确认**：指定管理员帐户的密码。

   ![“虚拟机基本设置”窗口][CR04]

7. 单击“下一步”。

8. 在“创建新存储帐户”窗口输入以下信息：

   * 资源组：指定虚拟机的资源组。 选择以下选项之一：
     * 新建：指定要创建新的资源组。
     * 使用现有资源：指定选择已与 Azure 帐户关联的资源组。

       ![“创建新存储帐户”对话框][CR05]

   * 存储帐户：指定用于存储虚拟机的存储帐户。 可使用现有存储帐户，也可以创建新帐户。

   * 虚拟网络和子网：指定虚拟机将连接到的虚拟网络和子网。 可使用现有网络和子网，也可以创建新网络和子网。 如果选择“新建”，会显示以下对话框：

      ![“新建虚拟网络”对话框][CR06]

9. 在“关联的资源”窗口输入以下信息：

   * 公共 IP 地址：为虚拟机指定面向外部的 IP 地址。 可选择创建新 IP 地址，也可以选择“(无)”（如果虚拟机将不具有公共 IP 地址）。

   * 网络安全组：指定虚拟机的可选网络防火墙。 可以选择现有防火墙，也可以选择“(无)”（如果虚拟机不使用网络防火墙）。

   * **可用性集**：指定虚拟机可能属于的可选可用性集。 可选择现有可用性集，或创建新可用性集，也可选择“(无)”（如果虚拟机将不属于可用性集）。

   ![“关联的资源”窗口][CR07]

10. 单击“完成”。  

    新虚拟机显示在“Azure 资源管理器”工具窗口中。

    ![新建虚拟机][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a>在 Eclipse 中重启虚拟机

若要在 Eclipse 中使用 Azure 资源管理器重启虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“重新启动”。

   ![“重新启动虚拟机”命令][RE01]

1. 在确认窗口中，单击“是”。

   ![“重启”确认窗口][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a>在 Eclipse 中关闭虚拟机

若要在 Eclipse 中使用 Azure 资源管理器关闭正在运行的虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“关闭”。

   ![“关闭虚拟机”命令][SH01]

1. 在确认窗口中，单击“是”。

   ![“关闭虚拟机”确认窗口][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a>在 Eclipse 中删除虚拟机

若要在 Eclipse 中使用 Azure 资源管理器删除虚拟机，请执行以下操作：

1. 在“Azure 资源管理器”视图中，右键单击虚拟机，并选择“删除”。

   ![“删除虚拟机”命令][DE01]

1. 在确认窗口中，单击“是”。

   ![“删除虚拟机”确认窗口][DE02]

## <a name="next-steps"></a>后续步骤

有关 Azure 虚拟机大小和定价的详细信息，请参阅以下资源：

* Azure 虚拟机大小
  * [Azure 中 Windows 虚拟机的大小]
  * [Azure 中 Linux 虚拟机的大小]
* Azure 虚拟机定价
  * [Windows 虚拟机定价]
  * [Linux 虚拟机定价]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure 中 Windows 虚拟机的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中 Linux 虚拟机的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 虚拟机定价]: /pricing/details/virtual-machines/windows/
[Linux 虚拟机定价]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
