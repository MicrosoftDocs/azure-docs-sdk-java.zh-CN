---
title: "Azure 存储帐户列表"
description: "管理你使用 Azure Toolkit for Eclipse 的存储帐户设置"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a>Azure 存储帐户列表
Azure 存储帐户启用用于 JDK、 应用程序服务器和任意组件，以及用于存储状态，使用缓存时要使用的下载位置。 Eclipse 维护已知的存储帐户可供 Eclipse 工作区中的项目的列表。 若要打开**存储帐户**对话框中，它用于管理该列表中的，在 Eclipse 中，单击**窗口**，单击**首选项**，展开**Azure**，然后单击**存储帐户**。

如下所示**存储帐户**对话框。

![][ic719496]

此对话框还可以打开从**帐户**使用存储帐户，如下所示的对话框上的链接：

* **JDK**选项卡**服务器配置**对话框。
* **服务器**选项卡**服务器配置**对话框。
* **添加组件**对话框。
* **Caching**属性对话框。

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>使用发布设置文件导入你的存储帐户
1. 在**存储帐户**对话框中，单击**从发布设置文件导入**。

2. （如果跳过此步骤已，你已将发布设置文件保存到本地计算机。）在**导入订阅信息**对话框中，单击**下载发布设置文件**。 如果你尚未登录到你的 Azure 帐户，将提示您登录。 然后系统将提示你保存 Azure 发布设置文件。 （可以忽略登录页的显示的最终说明它们由 Azure 门户和适用于 Visual Studio 用户。）将其保存到本地计算机。

3. 仍在**导入订阅信息**对话框中，单击**浏览**按钮，选择你先前在本地保存的发布设置文件，然后单击**打开**。

4. 单击**确定**关闭**导入订阅信息**对话框。

## <a name="to-create-a-new-storage-account"></a>若要创建新的存储帐户
1. 在**存储帐户**对话框中，单击**添加**。

2. 在**添加存储帐户**对话框中，单击**新建**。

3. 在**新的存储帐户**对话框中，指定以下设置的值：

   * 存储帐户名称。

   * 存储帐户的位置。

   * 存储帐户的说明。

   * 存储帐户属于订阅。

4. 单击**确定**关闭**新的存储帐户**对话框。

可能需要几分钟时间你要创建的存储帐户。 创建后，单击**确定**关闭**添加存储帐户**对话框中和新的存储帐户将添加到可用存储帐户的列表。

## <a name="to-add-an-existing-storage-account-to-the-list"></a>若要将现有的存储帐户添加到列表
1. 如果你还没有 Azure 存储帐户，创建一个按照中列出的步骤**若要创建新的存储帐户部分**上面。 (或者，可以创建新的存储帐户在[Azure 管理门户][Azure Management Portal]。)

2. 在**存储帐户**对话框中，单击**添加**。

3. 在**添加存储帐户**对话框中，输入值**名称**和**访问密钥**。 帐户名称和访问密钥必须是现有 Azure 存储帐户。 使用**存储**部分[Azure 管理门户][ Azure Management Portal]若要查看你的存储帐户名称和密钥。 你**添加存储帐户**对话框看起来类似于以下。
   
   ![][ic719497]

4. 单击**确定**关闭**添加存储帐户**对话框。

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>修改存储帐户以使用新的访问密钥
1. 在**存储帐户**对话框中，单击该你想要编辑的存储帐户，然后单击**编辑**。

2. 在**编辑存储帐户访问密钥**对话框中，修改**访问密钥**值。

3. 单击**确定**关闭**编辑存储帐户访问密钥**对话框。

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>若要从在 Eclipse 中维护的列表中删除存储帐户
1. 在**存储帐户**对话框中，单击该你想要编辑的存储帐户，然后单击**删除**。

2. 单击**确定**当系统提示您删除该存储帐户。

> [!NOTE]
> 删除存储帐户通过**存储帐户**对话框仅删除它从 Eclipse 中可查看的存储帐户的列表。 它不从你的 Azure 订阅中删除存储帐户。 此外，存储帐户可能会再次出现在列表中后 Eclipse 重新加载你的订阅的详细信息。
> 
> 

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
