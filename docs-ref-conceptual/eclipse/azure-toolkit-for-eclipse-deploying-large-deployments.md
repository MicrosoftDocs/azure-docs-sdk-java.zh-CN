---
title: "实施大型部署"
description: "了解如何部署使用 Azure Toolkit for Eclipse 的大型部署。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a>实施大型部署
如果你的部署太大，从而无法包含在默认 approot 文件夹，你可以使用本地存储资源的部署根文件夹作为为 JDK 和应用程序服务器。

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>若要使用作为部署根文件夹的本地存储资源，对于大型部署
1. 创建一个新的本地存储资源。 资源的名称并不重要。 存储资源在角色级别定义。 若要访问本地存储配置对话框中，你可以从中创建新的本地存储资源，最快捷方式是通过执行以下步骤： 用鼠标右键单击中的角色**项目资源管理器**视图 （展开你的 Azure 项目节点，如果你看不到角色，） 单击**Azure**，然后单击**本地存储**。 在**本地存储**对话框中，单击**添加**若要创建新的本地存储资源。

2. 将所需的大小设置为至少 2048 MB （任何内容不太可能会导致相同的文件大小问题将遇到在 approot 中）。

3. 确保**回收角色实例时清除内容**已选中; 这将有助于防止部署的启动逻辑与资源中预先存在的文件运行到冲突，回收角色实例时。

4. 确保**部署后存储资源的目录路径的环境变量**值设置为字符串**DEPLOYROOT**。 你的本地存储资源对话框看起来类似于以下。

   ![][ic667943]

或者，如果你使用**DEPLOYROOT**作为*名称*的本地资源和你不要更改自动生成的环境变量名称 (它将设置为**DEPLOYROOT_PATH**在这种情况下)，这样也适用于你的应用程序。

有关创建本地存储资源的其他信息可以在找到[本地存储属性][Local storage properties]。

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
