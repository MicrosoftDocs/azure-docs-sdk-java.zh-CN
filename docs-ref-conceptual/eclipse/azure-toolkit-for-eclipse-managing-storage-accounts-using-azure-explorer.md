---
title: 使用用于 Eclipse 的 Azure 资源管理器管理存储帐户
description: 了解如何使用用于 Eclipse 的 Azure 资源管理器管理 Azure 存储帐户。
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
ms.openlocfilehash: 310d95436189af09f794154f4c9f0e71c47d88c8
ms.sourcegitcommit: 3d3460289ab6b9165c2cf6a3dd56eafd0692501e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2018
ms.locfileid: "34283011"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>使用用于 Eclipse 的 Azure 资源管理器管理存储帐户

Azure 资源管理器是用于 Eclipse 的 Azure 工具包的一部分，它为 Java 开发人员提供易用的解决方案，用于从 Eclipse 集成开发环境 (IDE) 内部管理其 Azure 帐户中的存储帐户。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a>在 Eclipse 中创建存储帐户

若要使用 Azure 资源管理器创建存储帐户，请执行以下操作：

1. 按照[用于 Eclipse 的 Azure 工具包的登录说明](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)中的步骤登录到 Azure 帐户。

1. 在“Azure 资源管理器”视图中，展开 Azure 节点，右键单击“存储帐户”，然后单击“创建存储帐户”。

   ![“创建存储帐户”命令][CS01]

1. 在“创建存储帐户”对话框中，指定以下选项：

   ![“创建新存储帐户”对话框][CS02]

   * **名称**：指定要用于新存储帐户的名称。

   * **订阅**：指定要用于新存储帐户的 Azure 订阅。

   * **资源组**：指定虚拟机的资源组。 选择以下选项之一：
      * **新建**：指定要创建新的资源组。
      * **使用现有**：指定将从与 Azure 帐户关联的资源组列表中进行选择。

   * **区域**：指定将创建存储帐户的位置（例如“美国西部”）。

   * **帐户类型**：指定要创建的存储帐户的类型（例如“Blob 存储”）。 有关详细信息，请参阅[关于 Azure 存储帐户]。

   * **性能**：指定要从所选发布者使用哪种存储帐户产品/服务（例如，“高级”）。 有关详细信息，请参阅 [Azure 存储可伸缩性和性能目标]。

   * **复制**：指定存储帐户的复制（例如“区域冗余”）。 有关详细信息，请参阅 [Azure 存储复制]。

1. 指定了上述所有选项后，单击“创建”。

## <a name="create-a-storage-container-in-eclipse"></a>在 Eclipse 中创建存储容器

若要使用 Azure 资源管理器创建存储容器，请执行以下操作：

1. 在 **Azure 资源管理器**视图中，右键单击要在其中创建容器的存储帐户，并单击“创建 Blob 容器”。

   ![“创建 blob 容器”命令][CC01]

1. 在“创建 Blob 容器”对话框中，指定容器的名称，并单击“确定”。 有关命名存储容器的详细信息，请参阅[命名和引用容器、Blob 和元数据]。

   ![“创建 blob 容器”对话框][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a>删除 Eclipse 中的存储容器

若要使用 Azure 资源管理器删除存储容器，请执行以下操作：

1. 在 **Azure 资源管理器**视图中，右键单击存储容器，并单击“删除”。

   ![“删除存储容器”命令][DC01]

1. 在确认窗口中，单击“确定”。

   ![删除存储容器确认窗口][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a>删除 Eclipse 中的存储帐户

若要使用 Azure 资源管理器删除存储帐户，请执行以下操作：

1. 在 **Azure 资源管理器**视图中，右键单击存储帐户，并单击“删除”。

   ![“删除存储帐户”命令][DS01]

1. 在确认窗口中，单击“确定”。

   ![删除存储帐户确认窗口][DS02]

## <a name="next-steps"></a>后续步骤

有关 Azure 存储帐户大小和定价的详细信息，请参阅以下资源：

* [Microsoft Azure 存储简介]
* [关于 Azure 存储帐户]
* Azure 存储帐户大小
  * [Azure 中的 Windows 存储帐户的大小]
  * [Azure 中的 Linux 存储帐户的大小]
* Azure 存储帐户定价
  * [Windows 存储帐户定价]
  * [Linux 存储帐户定价]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Microsoft Azure 存储简介]: /azure/storage/storage-introduction
[关于 Azure 存储帐户]: /azure/storage/storage-create-storage-account
[Azure 存储复制]: /azure/storage/storage-redundancy
[Azure 存储可伸缩性和性能目标]: /azure/storage/storage-scalability-targets
[命名和引用容器、Blob 和元数据]: http://go.microsoft.com/fwlink/?LinkId=255555

[Azure 中的 Windows 存储帐户的大小]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure 中的 Linux 存储帐户的大小]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows 存储帐户定价]: /pricing/details/virtual-machines/windows/
[Linux 存储帐户定价]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
