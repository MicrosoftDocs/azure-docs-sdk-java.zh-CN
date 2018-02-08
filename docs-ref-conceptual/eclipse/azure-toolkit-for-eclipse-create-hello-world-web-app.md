---
title: "使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用"
description: "本教程说明如何使用 Azure Toolkit for Eclipse 创建适用于 Azure 的 Hello World Web 应用。"
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: bec94e65951330c933e0173fd580c3578e759c18
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a>使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用

本教程说明如何使用[用于 Eclipse 的 Azure 工具包]创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。

> [!NOTE]
>
> 如需使用[用于 IntelliJ 的 Azure 工具包]的本文版本，请参阅[使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用][intellij-hello-world]。
>

> [!IMPORTANT]
> 
> 用于 Eclipse 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。 本文详述如何使用用于 Eclipse 的 Azure 工具包 3.0.7（或更高版本）创建 Hello World Web 应用。 如果使用的是工具包 3.0.6（或更低版本），则需执行[使用旧工具包在 Eclipse 中创建适用于 Azure 的 Hello World Web 应用][Legacy Version]中的步骤。
> 

完成本教程后，应用程序会在 Web 浏览器中如下图所示：

![Hello World 应用预览][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>创建新 Web 应用项目

1. 启动 Eclipse，然后根据[用于 Eclipse 的 Azure 工具包的 Azure 登录说明][eclipse-sign-in-instructions]一文中的说明登录到 Azure 帐户。

1. 依次单击“文件”、“新建”和“动态 Web 项目”。 （如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，并单击“下一步”。）

   ![创建新的动态 Web 项目][file-new-dynamic-web-project]

2. 在本教程中，项目命名为 **MyWebApp**。 屏幕应与下图中所示类似：
   
   ![“新建动态 Web 项目”属性][dynamic-web-project-properties]

3. 单击“完成” 。

4. 在 Eclipse 的项目资源管理器视图中，展开“MyWebApp”。 右键单击“WebContent”，单击“新建”，并单击“JSP 文件”。

   ![新建 JSP 文件][create-new-jsp-file]

5. 在“新建 JSP 文件”对话框中，将文件命名为“index.jsp”，将父文件夹保留为“MyWebApp/WebContent”，然后单击“下一步”。

   ![“新建 JSP 文件”对话框][new-jsp-file-dialog]

6. 基于本教程的目的，在“选择 JSP 模板”对话框中选择“新建 JSP 文件(html)”，并单击“完成”。

   ![选择 JSP 模板][select-jsp-template]

7. 在 Eclipse 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示 在现有 `<body>` 元素中。 更新后的 `<body>` 内容应类似于以下示例：
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. 保存 index.jsp。

## <a name="deploy-your-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在 Eclipse 的“项目资源管理器”视图中，右键单击项目，选择“Azure”，然后选择“发布为 Azure Web 应用”。
   
   ![发布为 Azure Web 应用][publish-as-azure-web-app]

1. 当“部署 Web 应用”对话框显示时，可选择以下某个选项：

   * 选择现有的 Web 应用（如果存在）。

      ![选择应用服务][select-app-service]

   * 单击“创建新 Web 应用”。

      ![创建应用服务][create-app-service]

      在“创建应用服务”对话框中为 Web 应用指定必要信息，然后单击“创建”。

      ![“创建应用服务”对话框][create-app-service-dialog]

1. 选择 Web 应用，然后单击“部署”。

   ![部署应用服务][deploy-app-service]

1. 工具包在成功部署 Web 应用后会显示“已发布”状态的 **Azure 活动日志**，这是已部署 Web 应用的 URL 超链接。

   ![发布状态][publish-status]

1. 可使用状态消息中提供的链接转到 Web 应用。

   ![转到你的 Web 应用][browse-web-app]

1. 将 Web 发布到 Azure 以后，即可对应用进行管理，方法是右键单击该应用，然后在上下文菜单中选择一个选项。 例如，可以**启动**、**停止**或**删除** Web 应用。

   ![管理应用服务][manage-app-service]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[用于 Eclipse 的 Azure 工具包]: azure-toolkit-for-eclipse.md
[用于 IntelliJ 的 Azure 工具包]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->

[browse-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/browse-web-app.png
[file-new-dynamic-web-project]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/file-new-dynamic-web-project.png
[dynamic-web-project-properties]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/dynamic-web-project-properties.png
[create-new-jsp-file]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-new-jsp-file.png
[new-jsp-file-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/new-jsp-file-dialog.png
[select-jsp-template]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-jsp-template.png
[publish-as-azure-web-app]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-as-azure-web-app.png
[deploy-web-app-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-web-app-dialog.png
[select-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/select-app-service.png
[create-app-service-dialog]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service-dialog.png
[publish-status]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/publish-status.png
[create-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/create-app-service.png
[deploy-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/deploy-app-service.png
[manage-app-service]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app/manage-app-service.png
