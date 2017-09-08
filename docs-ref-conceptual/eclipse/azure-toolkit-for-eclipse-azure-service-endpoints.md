---
title: "Azure 服务终结点"
description: "介绍在 Azure Toolkit for Eclipse 的 Azure 服务终结点设置。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a>Azure 服务终结点
Azure 服务终结点确定是否部署到你的应用程序，并将其管理的全球 Azure 平台，Azure 21vianet 在中国，还是私有 Azure 平台。 **服务终结点**对话框允许你指定你想要使用的服务终结点。 若要打开**服务终结点**对话框中的，在 Eclipse 中，单击**窗口**，单击**首选项**，展开**Azure**，然后单击**服务终结点**。 设置**活动集**字段确定哪些 Azure 服务终结点将用于你当前的工作区中的 Azure 项目。

如下所示**服务终结点**对话框。

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>设置服务终结点
在**服务终结点**对话框中，执行以下操作之一：

* 如果你想要使用全球 Azure 平台中，从**活动集**下拉列表中，选择**windowsazure.com**单击**确定**。

* 如果你想要使用从由中国 21Vianet 运营的 Azure**活动集**下拉列表中，选择**windowsazure.cn （中国）**单击**确定**。

* 如果你想要使用私有 Azure 平台：

  1. 单击“编辑” 。

  2. 会打开一个对话框，通知你，**服务终结点**将关闭对话框，并将打开首选项集文件。 单击" **确定**"。

  3. 在 preferencesets.xml 文件中，创建一个新`preferenceset`元素。 对于此新元素，创建`name`， `blob`， `management`，`portalURL`和`publishsettings`属性，并将为其对应的值添加到私有 Azure 平台。 你可以使用提供给现有值`preferenceset`元素作为模板。 **请注意**： 用于值`blob`属性必须包含文本"blob"在 URL 中。

  4. 保存并关闭 preferencesets.xml。

  5. 重新打开**服务终结点**对话框。

  6. 从**活动集**下拉列表中选择的活动集所创建的虚拟机或模板，单击**确定**。

  7. 创建私有 Azure 平台后`preferenceset`元素，你可以更改通过单击分配给它的值**编辑**按钮**服务终结点**对话框。 你还可以创建多个私有 Azure 平台`preferenceset`元素，如果你需要实现。

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
