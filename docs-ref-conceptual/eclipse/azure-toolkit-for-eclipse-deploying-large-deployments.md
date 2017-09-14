---
title: "实施大型部署"
description: "了解如何使用 Azure Toolkit for Eclipse 实施大型部署。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 0e367e74d038043f1626dbf19aab87db6451bc02
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="deploying-large-deployments"></a>实施大型部署

如果部署太大，从而无法包含在默认 approot 文件夹中，可以使用本地存储资源作为 JDK 和应用程序服务器的部署根文件夹。

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>使用本地存储资源作为大型部署的部署根文件夹

1. 创建新的本地存储资源。 资源的名称并不重要。 存储资源在角色级别定义。 访问本地存储配置对话框（可从中创建新的本地存储资源）的最快方式是使用以下步骤：在“项目资源管理器”视图中右键单击角色（如果看不到该角色，请展开 Azure 项目节点），单击“Azure”，并单击“本地存储”。 在“本地存储”对话框中，单击“添加”创建新的本地存储资源。

1. 将所需大小设置为至少 2048 MB（指定更小的值可能会导致你在 approot 中遇到的相同文件大小问题）。

1. 确保已选中“回收角色实例时清除内容”；这有助于在回收角色实例时，防止部署启动逻辑与资源中的现有文件发生冲突。

1. 确保将“部署后存储资源目录路径的环境变量”值设置为字符串 **DEPLOYROOT**。 本地存储资源对话框如下所示。

   ![][ic667943]

或者，如果使用 **DEPLOYROOT** 作为本地资源的*名称*，并且不更改自动生成的环境变量名称（在此情况下将设置为 **DEPLOYROOT_PATH**），则这也适用于应用程序。

有关创建本地存储资源的其他信息可以在[本地存储属性][Local storage properties]中找到。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
