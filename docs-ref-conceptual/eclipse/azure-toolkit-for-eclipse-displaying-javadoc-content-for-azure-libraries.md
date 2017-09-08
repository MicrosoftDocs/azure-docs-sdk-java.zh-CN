---
title: "Azure Libraries for Java 包显示在 Eclipse 中的 Javadoc 内容"
description: "如何为 Azure Libraries 在 Eclipse 中显示的 Javadoc 内容。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Azure Libraries for Java 包显示在 Eclipse 中的 Javadoc 内容
通过将关联到 Azure Libraries for Java 的 Javadoc 内容，可以在 Eclipse 环境中查看有关 Azure Libraries for Java 的 Javadoc 内容。 以下步骤演示了如何使用 Eclipse 中的此功能。

此过程假定你已添加到生成路径用于 Java 的 Azure 库。

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>在 Eclipse 中的 Javadoc 内容显示为 Azure Libraries for Java
* 在 Eclipse 的项目资源管理器中**引用库**部分的你的项目，打开上下文菜单 Azure library for Java JAR。 例如， **microsoft windowsazure api 0.1.0.jar** （的版本号可能不同，依赖于已安装的版本）。

* 单击“属性” 。

* 在**属性**对话框中的，在左侧窗格中，单击**Javadoc 位置**。 **Javadoc 位置**显示对话框。

* 你可以指定**Javadoc URL**，或**存档中的 Javadoc**。

   * 如果你选择指定**Javadoc URL**，如使用 Url **http://dl.windowsazure.com/javadoc**或**http://dl.windowsazure.com/storage/javadoc**。

   * 如果你选择使用**存档中的 Javadoc**，你可以指定外部文件或工作区文件。

   做出选择，并浏览/验证根据需要。 下面的示例将 Azure Libraries for Java 关联到名为的文件夹已本地下载的相应 Javadoc JAR 与**c:\MyAzureJARs**。

   ![][ic553487]

* *可选步骤*： 单击**验证**。 此处可能会显示 Javadoc JAR 的潜在问题。

* 单击" **确定**"。

一旦与以下库关联，Javadoc 内容应显示在 Eclipse IDE。 例如，如果`blob`类型的定义`CloudBlockBlob`在代码中，以下是当你键入时显示的 Javadoc 内容的示例`blob.acquireLease`在代码中：

![][ic553488]

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

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
