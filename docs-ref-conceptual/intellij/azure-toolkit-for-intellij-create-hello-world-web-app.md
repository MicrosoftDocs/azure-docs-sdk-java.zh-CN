---
title: "使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用"
description: "本教程说明如何使用 Azure Toolkit for IntelliJ 创建 Azure 的 Hello World Web 应用。"
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/15/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 900e64667bcc2eed79fe80954c118d54a9ef957f
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a>使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用

本教程介绍如何使用[用于 IntelliJ 的 Azure 工具包]创建基本 Hello World 应用程序，并将其作为 Web 应用部署到 Azure。

> [!NOTE]
>
> 如需使用[用于 Eclipse 的 Azure 工具包]的本文版本，请参阅[使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用][eclipse-hello-world]。
>

> [!IMPORTANT]
> 
> 用于 IntelliJ 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。 本文详述如何使用用于 IntelliJ 的 Azure 工具包 3.0.7（或更高版本）创建 Hello World Web 应用。 如果使用的是工具包 3.0.6（或更低版本），则需执行[使用旧工具包在 IntelliJ 中创建适用于 Azure 的 Hello World Web 应用][Legacy Version]中的步骤。
> 

完成本教程后，应用程序会在 Web 浏览器中如下图所示：

![Hello World 应用预览][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a>创建新 Web 应用项目

1. 启动 IntelliJ，然后根据[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明][intelliJ-sign-in-instructions]一文中的说明登录到 Azure 帐户。

1. 依次单击“文件”菜单、“新建”、“项目”。
   
   ![创建新项目][file-new-project]

1. 在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. 为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. 自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。
   
   ![指定 Maven 设置][maven-options]

1. 指定项目名称和位置，并单击“完成”。
   
   ![指定项目名称][project-name]

1. 在 IntelliJ 的项目资源管理器视图中，依次展开 src、main、webapp，然后双击 index.jsp。
   
   ![打开索引页面][open-index-page]

1. 在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示 在现有 `<body>` 元素中。 更新后的 `<body>` 内容应类似于以下示例：
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![编辑索引页面][edit-index-page]

1. 保存 index.jsp。

## <a name="deploy-your-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在 IntelliJ 的项目资源管理器视图中，右键单击项目，选择“Azure”，然后选择“在 Web 应用上运行”。
   
   ![“在 Web 应用上运行”菜单][run-on-web-app-menu]

1. 在“在 Web 应用上运行”对话框中，可选择以下任一选项：

   * 选择现有 Web 应用（如果有），然后单击“运行”。

      ![“在 Web 应用上运行”对话框][run-on-web-app-dialog]

   * 单击“创建新 Web 应用”。 如果选择创建新 Web 应用，请为 Web 应用指定必要信息，然后单击“运行”。

      ![创建新 Web 应用][create-new-web-app-dialog]

1. 成功部署 Web 应用后，该工具包会显示一条状态消息，其中还含有所部署 Web 应用的 URL。

   ![成功部署][successfully-deployed]

1. 可使用状态消息中提供的链接转到 Web 应用。

   ![转到你的 Web 应用][browse-web-app]

1. 发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。 可通过单击 Web 应用的下拉菜单来修改设置，然后单击“编辑配置”。

   ![“编辑配置”菜单][edit-configuration-menu]

1. 出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。

   ![“编辑配置”对话框][edit-configuration-dialog]

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
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[run-on-web-app-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[run-on-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
