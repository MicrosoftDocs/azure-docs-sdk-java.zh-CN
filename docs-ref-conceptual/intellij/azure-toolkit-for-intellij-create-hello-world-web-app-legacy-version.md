---
title: "使用用于 IntelliJ 的旧工具包创建适用于 Azure 的 Hello World Web 应用"
description: "本教程说明如何使用用于 IntelliJ 的 Azure 工具包 3.0.6（或更低版本）创建适用于 Azure 的 Hello World Web 应用。"
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ebe98a604b52dc9a4b5a47cbf65a4c68a5c86fe3
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-intellij"></a>使用用于 IntelliJ 的旧工具包创建适用于 Azure 的 Hello World Web 应用

本教程说明如何使用[用于 IntelliJ 的 Azure 工具包] 3.0.6（或更低版本）创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。

> [!NOTE]
>
> 如需使用[用于 Eclipse 的 Azure 工具包]的本文版本，请参阅[使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用][eclipse-hello-world]。
>

> [!IMPORTANT]
> 
> 用于 IntelliJ 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。 本文详述如何使用用于 IntelliJ 的 Azure 工具包 3.0.6（或更低版本）创建 Hello World Web 应用。 如果使用的是工具包 3.0.7（或更高版本），则需执行[在 IntelliJ 中创建适用于 Azure 的 Hello World Web 应用][Updated Version]中的步骤。
>

完成本教程后，应用程序会在 Web 浏览器中如下图所示：

![Hello World 应用预览][01]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>创建新 Web 应用项目

1. 启动 IntelliJ，然后根据[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明][intelliJ-sign-in-instructions]一文中的说明登录到 Azure 帐户。

1. 依次单击“文件”菜单、“新建”、“项目”。
   
   ![文件新建项目][02]

2. 在“新建项目”对话框中，依次选择 Java 和“Web 应用程序”，并单击“新建”以添加项目 SDK。
   
   ![“新建项目”对话框][03a]
   
3. 在“选择 JDK 的主目录”对话框中，选择要安装 JDK 的文件夹，并单击“确定”。 在“新建项目”对话框中单击“下一步”以继续。
   
   ![指定 JDK 主目录][03b]

4. 基于本教程的目的，将项目命名为 **Java-Web-App-On-Azure**，然后单击“完成”。
   
   ![“新建项目”对话框][04]

5. 在 IntelliJ 的项目资源管理器视图中，依次展开“Java-Web-App-On-Azure”和“Web”，并双击“index.jsp”。
   
   ![打开索引页面][05c]

6. 在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示 在现有 `<body>` 元素中。 更新后的 `<body>` 内容应类似于以下示例：
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. 保存 index.jsp。

## <a name="deploy-your-web-app-to-azure"></a>将 Web 应用部署到 Azure

有多种方式可将 Java Web 应用部署到 Azure。 本教程说明其中一个最简单的方式：将应用程序部署到 Azure Web 应用容器，无需特殊的项目类型或额外的工具。 Azure 为提供 JDK 及 Web 容器软件，因此不需要自己上传；只需要 Java Web 应用。 这样，应用程序的发布过程只需数秒，连一分钟都不用。

在发布应用程序之前，需要先配置模块设置。 为此，请按照以下步骤操作：

1. 在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。 出现上下文菜单时，单击“打开模块设置”。

   ![打开模块设置][05a]

2. “项目结构”对话框出现后：

   a. 单击“项目设置”列表中的“项目”。

   b. 更改“名称”框中的项目名称，使其不包含空格或特殊字符；这是必要步骤，因为该名称会在统一资源标识符 (URI) 中使用。

   c. 将“类型”更改为“Web 应用程序: 存档”。

   d.单击“下一步”。 单击“确定”关闭“项目结构”对话框。

   ![打开模块设置][05b]

配置模块设置后，可以通过执行以下步骤将应用程序发布到 Azure：

1. 在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。 出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”
   
   ![Azure 发布上下文菜单][06]

2. 如果尚未从 IntelliJ 登录到 Azure，系统会提示登录 Azure 帐户。 （如果有多个 Azure 帐户，部分提示会在登录过程中显示多次，即使这些提示看起来是相同的。 发生此情况时，请继续遵循登录指示进行操作。）
   
   ![对话框中的 Azure 日志][07]

3. 成功登录 Azure 帐户后，“管理订阅”对话框会显示与凭据关联的订阅的列表。 （如果列出了多个订阅，而你只想使用其中几个订阅，可选择取消选中不想使用的订阅。）选择订阅后，单击“关闭”。
   
   ![管理订阅][08]

4. 当“部署到 Azure Web 应用容器”对话框出现时，它会显示前面创建的所有 Web 应用容器；如果尚未创建任何容器，列表将是空白的。
   
   ![应用容器][09]

5. 如果前面尚未创建 Azure Web 应用容器，或你想要将应用程序发布到新的容器，请使用以下步骤。 否则，请选择现有的 Web 应用容器，并跳到下面的步骤 6。
   
   a. 单击 + 号。
      
      ![添加应用容器][10]

   b. 此时会显示“新建 Web 应用容器”对话框，该对话框将用来进行接下来的几个步骤。
      
      ![新建应用容器][11a]
   
   c. 为 Web 应用容器输入“DNS 标签”；这是在 Azure 中的 Web 应用程序构成主机 URL 的叶 DNS 标签。 请注意该名称必须是可用的，且符合 Azure Web 应用命名要求。

   d.单击“下一步”。 在“Web 容器”下拉菜单中，为应用程序选择适当的软件。
      
      当前，可以从 Tomcat 8、Tomcat 7 或 Jetty 9 中选择。 Azure 将提供所选软件的最新分发版，并且该版本将基于由 JDK 8 创建并由 Azure 提供的 JDK 最新分发版运行。

   e. 在“订阅”下拉菜单中，选择要用于此部署的订阅。

   f. 在“资源组”下拉菜单中，选择要与 Web 应用关联的资源组。 （使用 Azure 资源组可以将相关资源组织在一起，以便于将它们一起删除。）
      
      可以选择现有资源组（如果有）并跳到下面的步骤 g，或者按照以下步骤创建新的资源组：
      
      * 在“资源组”下拉菜单中选择“&lt;&lt;新建资源组&gt;&gt;”。
      * 此时会显示“新建资源组”对话框：
        
         ![新建资源组][12]

      * 在“名称”文本框中，为新的资源组指定名称。
      * 在“区域”下拉菜单中，为资源组选择适当的 Azure 数据中心位置。
      * 单击“确定”。

   g. “应用服务计划”下拉菜单列出了与选定资源组关联的应用服务计划。 （应用服务计划指定了 Web 应用的位置、定价层以及计算实例大小等信息。 单个应用服务计划可用于多个 Web 应用，这也是从特定 Web 应用部署中单独维护它的原因。）
      
      可以选择现有的应用服务计划（如果有）并跳到下面的步骤 h，或者按照以下步骤创建新的应用服务计划：
      
      * 在“应用服务计划”下拉菜单中选择“&lt;&lt;创建新的应用服务计划&gt;&gt;”。
      * 此时会显示“新建应用服务计划”对话框：
        
         ![新建应用服务计划][13]

      * 在“名称”文本框中，为新的应用服务计划指定名称。
      * 在“位置”下拉菜单中，为计划选择适当的 Azure 数据中心位置。
      * 在“定价层”下拉菜单中，为计划选择适当的定价。 对于测试，可以选择“免费”。
      * 在“实例大小”下拉菜单中，为计划选择适当的实例大小。 对于测试，可以选择“小”。
      * 单击“确定”。

   h. （可选）默认情况下，Azure 自动将最新的 Java 8 分发版作为 JVM 部署到 Web 应用容器。 但是，可指定 JVM 的其他版本和分发版。 为此，请按照以下步骤操作：
      
      * 在“新建 Web 应用容器”对话框中单击“JDK”。
      * 可以选择以下选项之一：
        
         * 部署 Azure 提供的默认 JDK
         * 可从 Azure 上提供的其他 JDK 下拉列表中部署第三方 JDK
         * 部署自定义 JDK，必须将其打包为 ZIP 文件，且该 JDK 公开可用或位于 Azure 存储帐户中
        
      ![“新建应用容器 JDK”选项卡][11b]

   i. 完成所有上述步骤之后，“新建 Web 应用容器”对话框看起来应如下图所示：
      
      ![新建应用容器][14]
   
   j. 单击“确定”完成创建新的 Web 应用容器。
       
      等待 Web 应用容器列表刷新，这需要几秒，然后，新创建的 Web 应用容器应在列表中处于选中状态。

6. 现已准备好完成 Web 应用到 Azure 的初始部署；单击“确定”将 Java 应用程序部署到选定的 Web 应用容器。 默认情况下，应用程序部署为应用程序服务器的子目录。 如果想要将其部署为根应用程序，请选中“部署到根”复选框，然后单击“确定”。
   
   ![部署到 Azure][15]

7. 接下来，应会看到“Azure 活动日志”视图，其中指示了 Web 应用的部署状态。
   
   ![进度指示器][16]
   
   将 Web 应用部署到 Azure 的过程只需几秒钟即可完成。 应用程序准备就绪后，可在“状态”列中看到名为“已发布”的链接。 单击该链接时，它将转到已部署 Web 应用的主页；你也可以使用下一节中的步骤浏览到 Web 应用。

## <a name="browsing-to-your-web-app-on-azure"></a>浏览到 Azure 上的 Web 应用

若要浏览到 Azure 上的 Web 应用，可以使用“Azure 资源管理器”视图。

如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。 如果事先未尚未登录，系统会提示登录。

显示“Azure 资源管理器”视图后，按以下步骤浏览到 Web 应用： 

1. 展开“Azure”节点。
2. 展开“Web 应用”节点。 
3. 右键单击所需的 Web 应用。
4. 出现上下文菜单时，单击“在浏览器中打开”。
   
   ![浏览 Web 应用][17]

## <a name="updating-your-web-app"></a>更新 Web 应用

更新现有运行中的 Azure Web 应用是一个快速而简单的过程，可以使用两个更新选项：

* 可以更新现有 Java Web 应用的部署。
* 可以将其他 Java 应用程序发布到同一个 Web 应用容器。

对于任一情况，过程都是相同的，只需几秒钟即可完成：

1. 在 IntelliJ 项目资源管理器中，右键单击要更新或添加到现有 Web 应用容器的 Java 应用程序。
2. 出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”
3. 由于前面已登录，因此将看到现有 Web 应用容器的列表。 选择要对其发布或重新发布 Java 应用程序的 Web 应用容器，并单击“确定”。

几秒钟后，“Azure 活动日志”视图会将已更新的部署显示为“已发布”，可以在 Web 浏览器中验证已更新的应用程序。

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>启动、停止或重启现有的 Web 应用

若要启动或停止现有的 Azure Web 应用容器（包括其中所有已部署的 Java 应用程序），可以使用“Azure 资源管理器”视图。

如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。 如果事先未尚未登录，系统会提示登录。

显示“Azure 资源管理器”视图后，按以下步骤启动或停止 Web 应用： 

1. 展开“Azure”节点。
2. 展开“Web 应用”节点。 
3. 右键单击所需的 Web 应用。
4. 出现上下文菜单时，单击“启动”、“停止”或“重启”。 请注意，菜单选项可识别上下文，因此，只能停止正在运行的 Web 应用，或启动当前未运行的 Web 应用。
   
   ![停止 Web 应用][18]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包]: azure-toolkit-for-intellij.md
[用于 Eclipse 的 Azure 工具包]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Updated Version]: azure-toolkit-for-intellij-create-hello-world-web-app.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version/18-Stop-Web-App.png
