---
title: "在 Eclipse 中创建用于 Azure 的 Hello World 云服务"
description: "了解如何创建一个简单的 Hello World 应用程序使用 Azure Toolkit for Eclipse。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 7262e705-59d6-43ce-b888-29a21c8e0cb7
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 9b31f0faeb6ee7b5e7b8fe3a1f2827133d6188e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>在 Eclipse 中创建用于 Azure 的 Hello World 云服务
以下步骤演示如何创建和部署到 Azure 的基本 JSP 应用程序使用 Azure Toolkit for Eclipse。 为简单起见，将演示 JSP 示例，但就 Azure 部署而言，将适用于 Java servlet 步骤非常相似。

应用程序将类似于以下形式：

![][ic600360]

## <a name="prerequisites"></a>必备条件
* Java 开发人员工具包 (JDK)、 1.7 版或更高版本。
* Eclipse IDE for Java EE Developers，Indigo 或更高版本。 可以从下载此<http://www.eclipse.org/downloads/>。
* 基于 Java 的 web 服务器或应用程序服务器，如 Apache Tomcat、 GlassFish、 JBoss 应用程序服务器、 Jetty 或 IBM® WebSphere® Application Server Liberty Core 的分发。
* Azure 订阅，可以从获取<http://azure.microsoft.com/pricing/purchase-options/>。
* 使用 Azure Toolkit for Eclipse。 有关详细信息，请参阅[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]。

## <a name="to-create-a-hello-world-application"></a>若要创建 Hello World 应用程序
首先，我们将首先创建一个 Java 项目。

1. 启动 Eclipse，然后在菜单单击**文件**，单击**新建**，然后单击**动态 Web 项目**。 (如果看不到**动态 Web 项目**单击后作为可用项目列出**文件**，**新建**，然后执行以下操作： 单击**文件**，单击**新建**，单击**项目...**，展开**Web**，单击**动态 Web 项目**，然后单击**下一步**。)

1. 对于本教程，请将项目**MyHelloWorld**。 (确保使用此名称，本教程中的后续步骤预期 WAR 文件命名为 MyHelloWorld)。 你的屏幕将显示类似如下：

   ![][ic589576]

1. 单击 **“完成”**。

1. 在 Eclipse 的项目资源管理器视图中，展开**MyHelloWorld**。 右键单击**WebContent**，单击**新建**，然后单击**JSP 文件**。

1. 在**新建 JSP 文件**对话框中，将文件**index.jsp**。 将作为父文件夹保留**MyHelloWorld/WebContent**，如在下面的示例所示：

   ![][ic659262]

1. 在**选择 JSP 模板**对话框中的，对于此教程，请选择**新建 JSP 文件 (html)**单击**完成**。

1. 在 Eclipse 中打开 index.jsp 文件，添加文本以便动态显示**Hello World ！** 在现有`<body>`元素。 更新后`<body>`内容应如下所示：
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. 保存 index.jsp。

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>若要部署到 Azure，应用程序的快速而又简单的方法
只要有 Java web 应用程序准备好测试时，可以使用以下快捷键来直接在 Azure 云上试用一下。

1. 在 Eclipse 的项目资源管理器中，单击**MyHelloWorld**。

2. 在 Eclipse 工具栏中，单击**发布**下拉列表按钮，然后单击**发布为 Azure 云服务**

   ![][publishDropdownButton]

3. 如果你要发布到 Azure 的第一次此应用程序，并且不创建此应用程序，然后 Azure 部署项目，Azure 部署项目为你自动创建。 您应看到以下提示，它还列出了的 JDK 包和将会自动部署到运行你的应用程序的应用程序服务器。

   ![][ic789598]
   
   此快捷方法可快速轻松地在 Azure 中测试你的应用程序，而无需配置的特定服务器或不同于默认安装的 JDK。 如果您感到满意默认值，则可以单击**确定**以继续执行以下步骤。
   但是，如果你想要更改 JDK 或应用程序服务器要用于你的应用程序，执行该操作通过编辑已为你自动创建的 Azure 部署项目的更高版本，也可以单击**取消**现在和读取**关于 Azure 部署项目部分**本教程。

4. 在**发布到 Azure**对话框：

   1. 如果没有任何订阅可供选择中**订阅**列表，请按照这些步骤导入你的订阅信息：
      1. 单击**从发布设置文件导入**。
      2. 在**导入订阅信息**对话框中，单击**下载发布设置文件**。 如果你尚未登录到你的 Azure 帐户，将提示您登录。 然后系统将提示你保存 Azure 发布设置文件。 将其保存到本地计算机。
      3. 仍在**导入订阅信息**对话框中，单击**浏览**按钮，选择在上一步骤中，保存在本地的发布设置文件，然后单击**打开**。 你的屏幕应类似于以下形式：![][ic644267]
      4. 单击" **确定**"。
   2. 有关**订阅**，选择你想使用为你的部署的订阅。
   3. 有关**存储帐户**，选择你想要使用，或单击的存储帐户**新建**以创建新的存储帐户。
   4. 有关**服务名称**，选择你想要使用，或单击的云服务**新建**以便创建新的云服务。
   5. 有关**目标 OS**，选择你想要用于部署的操作系统版本。
   6. 有关**目标环境**，对于本教程中的，选择**过渡**。 (如果你已准备好部署到生产站点，你将更改到**生产**。)
   7. 可选： 确保**覆盖以前的部署**已选中是否你希望新部署来自动覆盖以前的部署。 在启用此选项时，发布到相同的位置时将不出现"409 冲突"问题。
      请注意，**发布到 Azure**对话框包含一个节，用于**远程访问**。 默认情况下，不启用远程访问，并且我们将为此示例不启用。 若要启用远程访问，将输入用户名和密码远程登录时使用。 有关远程访问的详细信息，请参阅[为在 Eclipse 中的 Azure 部署启用远程访问][Enabling Remote Access for Azure Deployments in Eclipse]。
      你**发布到 Azure**对话框将出现与下类似：![][ic719488]

5. 单击**发布**发布到过渡环境。

   当系统提示执行完全生成，则单击**是**。 这可能需要几分钟的第一次生成时间。
   **Azure 活动日志**将在你的 Eclipse 选项卡式视图部分中显示。
   ![][ic719489]你可以使用此日志，以及**控制台**视图，以查看你的部署的进度。 替代方法是登录到[Azure 管理门户][Azure Management Portal]，并使用**云服务**部分来监视的状态。

6. 已成功部署你的部署， **Azure 活动日志**将显示的状态**已发布**。 单击**已发布**下, 图中中, 所示，你的浏览器将打开你的部署的实例。

   ![][ic719490]

由于这是部署到过渡环境，DNS 名称将的窗体 http://&lt;*guid*&gt;。 cloudapp.net，并且 URL 将包含 DNS 名称和你的应用程序的后缀。 例如，http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld。 ( **MyHelloWorld**部分是区分大小写。)如果你单击部署名称在 Azure 平台管理门户中 （在管理门户的云服务部分中），您也可以看到 DNS 名称。

尽管本演练是针对部署到过渡环境，部署到生产环境中除遵循相同的步骤，**发布到 Azure**对话框中，选择**生产**而不是**暂存**为**目标环境**。 部署到生产环境会导致根据您的选择，而不是用于过渡的 GUID 的 DNS 名称的 URL。

> [!WARNING]
> 此时你已部署到云中 Azure 应用程序。 但是，在继续之前，请注意，部署的应用程序，即使未在运行，会继续累加订阅的计费时间。 因此，它是从你的 Azure 订阅中删除不需要的部署非常重要。
> 
> 

## <a name="about-azure-deployment-projects"></a>关于 Azure 部署项目
为了部署到 Azure 的一个或多个 Java 应用程序，需要使用 Azure 部署项目。 它充当你的应用程序需要在 Azure 上发布包装到的"包"的角色。

除了有关你的应用程序的信息，Azure 部署项目还包含有关其他关键组件的信息你的部署，最重要的是： 要运行中，web 应用的应用程序服务器容器和 Java 运行时上运行它。 Azure 支持大量可选择的 Java 运行时和可以从选择的 Java 应用程序服务器。

虽然此处使用的示例已极大地简化出于教育目的，Azure 部署项目还可以包含其他重要的配置信息，可用于与应用程序创建几乎任意复杂、 可伸缩、 高度可用、 多层云服务。 你可以启用**会话相关性 （"粘滞会话"）**，**快速缓存**， **SSL 卸载**，**防火墙/端口路由**，**远程访问**，以及大量的其他功能强大的功能。

如果你已完成本教程 （"应用程序部署到 Azure，快速而简单的方法"） 的上一节，你现在将看到新的 Azure 部署项目在项目资源管理器为你自动生成和名为"**MyHelloWorld_onAzure**"。

你无法具有还开始本教程通过首次自行创建空的 Azure 部署项目，然后添加你的应用程序到它。 这是较长过程中，但可让你更好地控制从开始处的初始配置。

若要从头开始创建新的 Azure 部署项目，请单击**新建 Azure 部署项目**按钮![][ic710876]。

无论是否要使用现有 Azure 部署项目，或创建一个从零开始，你将能够更改其配置设置和组件，如 JDK 或应用程序服务器中，同样轻松地在任何时间。

若要更改 JDK、 应用程序服务器或在现有 Azure 部署项目中的应用程序列表：

1. 展开的项目节点 (例如**MyHelloWorld_onAzure**) 在项目资源管理器

2. 右键单击**WorkerRole1**

3. 展开**Azure**上下文菜单中的子菜单

4. 单击**服务器配置**

无论是否你启动这些服务器配置步骤通过，如上所示编辑现有的 Azure 部署项目或创建一个新从零开始，你将看到相同类型的对话框，让你可以配置 JDK、 服务器和应用程序组件。 若要详细了解如何更改的设置在这些对话框中，例如，更改 JDK、 应用程序服务器、 添加或删除应用程序在部署中，请参阅[服务器配置属性][ Server configuration properties]文章。

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>仅限 Windows： 你将应用部署到计算仿真程序

> [!NOTE]
> Azure 仿真程序在 Windows 上才可用。 如果使用非 Windows 操作系统，跳过此部分。
> 
> 

如果你已创建新的 Azure 部署项目之前，即隐式地介绍的步骤通过发布应用程序以便对 Azure、 JDK 和应用程序服务器已配置云，而非本地模拟。 若要准备在本地模拟器中测试你的项目，请按照下列步骤：

1. 在 Eclipse 的项目资源管理器中，单击**MyHelloWorld_onAzure**。

2. 右键单击**WorkerRole1**。

3. 展开**Azure**上下文菜单中的子菜单。

4. 单击**服务器配置**。

5. 上**JDK**选项卡上，检查该工具包已预先配置了默认为你的本地 JDK。 如果没有，或者你想要更改假定的默认值，确保**使用此文件路径中的 JDK 进行本地测试**选中复选框并且指定你想要使用的 JDK 安装位置。 如果你想要对其进行更改，请单击**浏览**按钮，然后使用浏览控件选择要使用的 JDK 的目录位置。

6. 单击**服务器**选项卡。

7. 在**本地服务器路径**底部的对话框中，文本框中输入匹配的类型和选择顶部的对话框中下的服务器的主版本号的本地安装的服务器的路径**部署此类型的服务器**复选框。 如果你想要使用不同类型或应用程序服务器的主要版本，请首先更改该复选框下面选择。

8. 单击" **确定**"。

9. 在 Eclipse 工具栏中，单击**在 Azure 模拟器中运行**按钮， ![][ic710879]。 如果**在 Azure 模拟器中运行**按钮未启用时，请确保**MyHelloWorld_onAzure**在 Eclipse 的项目资源管理器中选择，并确保 Eclipse 的项目资源管理器在当前窗口具有焦点。 这首先将开始完全生成你的项目，然后启动 Java web 应用程序在计算模拟器中。 （请注意，具体取决于你的计算机的性能特征，第一次生成可能需要几秒钟到几分钟，但后续生成将变快。）第一个生成步骤完成后，将会提示你通过 Windows 用户帐户控制 (UAC) 以允许此命令到你的计算机进行更改。 单击“是”。

> [!IMPORTANT]
> 如果你未看到 UAC 提示，请检查 Windows 任务栏 UAC 图标，然后先单击该。 有时 UAC 提示未不显示为最顶端窗口，但只显示为任务栏图标。
> 
> 

1. 检查计算模拟器 UI，以确定是否有任何问题与您的项目的输出。 具体取决于你的部署内容时，可能需要应用程序，完全可以计算模拟器中启动的几分钟。

2. 启动浏览器并使用 URL`http://localhost:8080/MyHelloWorld`作为地址 ( `MyHelloWorld` URL 部分是区分大小写)。 你应看到 MyHelloWorld 应用程序 （index.jsp 的输出），与下图类似：

   ![][ic589579]

当你准备好停止应用程序运行在计算模拟器中，在 Eclipse 工具栏中，单击**重置 Azure 模拟器**按钮， ![][ic710880]。

## <a name="to-delete-your-deployment"></a>若要删除你的部署
若要删除你在 Azure Toolkit for Eclipse 的部署，请确保**MyHelloWorld_onAzure**是在 Eclipse 的项目资源管理器中选择，请确保 Eclipse 项目资源管理器具有当前窗口专注，，然后单击**取消发布**按钮， ![][ic710883]，在 Eclipse 工具栏中。 (你可以执行相同操作操作通过右键单击**MyHelloWorld_onAzure**在 Eclipse 的项目资源管理器，单击**Azure** ，然后单击**从 Azure 云取消部署后的再次**。)这将显示**取消发布 Azure 项目**对话框。

![][ic719491]

选择包含你的部署的订阅和云服务中，选择你想要删除，然后单击部署**取消发布**。

(使用工具包来删除部署的替代方法是使用**云服务**Azure 管理门户部分： 导航到你的部署，选择它，，然后单击**删除**按钮。 这将停止，然后删除该部署。 如果您只想要停止的部署，而不要删除它，请单击**停止**按钮而不是**删除**按钮，但如上所述，如果你未删除该部署，计费费用将继续针对该部署累加，即使它已停止)。

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

[什么是 Azure Toolkit for Eclipse 中的新增功能][What's New in the Azure Toolkit for Eclipse]

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->
