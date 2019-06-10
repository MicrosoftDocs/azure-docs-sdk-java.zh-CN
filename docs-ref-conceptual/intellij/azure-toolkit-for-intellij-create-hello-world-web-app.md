---
title: 使用 IntelliJ 创建适用于 Azure 应用服务的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for IntelliJ 创建 Azure 的 Hello World Web 应用。
services: app-service
keywords: java, intellij, web 应用, azure 应用服务, hello world, 快速入门
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ae0749ce1ddab971f1a83e2e5e58492fd8ccb287
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626104"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>使用 IntelliJ 创建适用于 Azure 应用服务的 Hello World Web 应用

使用开源的 [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 插件可以快速创建一个基本的 Hello World 应用程序并将其作为 Web 应用部署到 Azure 应用服务。

> [!NOTE]
>
> 如果你偏爱使用 Eclipse，请查看[适用于 Eclipse 的类似教程][eclipse-hello-world]。
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> 请勿忘记在完成本教程后清理资源。 在这种情况下，运行本指南不会超出免费帐户配额。
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>安装和登录

1. 在 IntelliJ IDEA 的“设置/首选项”对话框中 (Ctrl+Alt+S) 中，选择“插件”。  然后，在“市场”中找到“Azure Toolkit for IntelliJ”并单击“安装”。    安装后，单击“重启”以激活该插件。  

   ![市场中的 Azure Toolkit for IntelliJ 插件][marketplace]

2. 若要登录到你的 Azure 帐户，请打开边栏中的“Azure 资源管理器”，然后单击顶部栏中的“Azure 登录”图标（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 登录”）。   

   ![“IntelliJ Azure 登录”命令][I01]

3. 在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](azure-toolkit-for-intellij-sign-in-instructions.md)）。   

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][I03]

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][I04]

6. 在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。  

   ![“选择订阅”对话框][I05]

## <a name="creating-web-app-project"></a>创建 Web 应用项目

1. 在 IntelliJ 中，依次单击“文件”菜单、“新建”、“项目”。   

   ![创建新项目][file-new-project]

2. 在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”     。

   ![选择 Maven archetype Webapp][maven-archetype-webapp]

3. 为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”    。

   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

4. 自定义任何 Maven 设置或接受默认设置，然后单击“下一步”  。

   ![指定 Maven 设置][maven-options]

5. 指定项目名称和位置，并单击“完成”  。

   ![指定项目名称][project-name]

6. 在“项目资源管理器”视图下，按如下所示打开并编辑文件 **src/main/webapp/index.jsp**，然后**保存更改**：

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![编辑索引页面][edit-index-page]

## <a name="deploying-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在“项目资源管理器”视图下右键单击你的项目，展开“Azure”，然后单击“部署到 Azure”。  

   ![“部署到 Azure”菜单][deploy-to-azure-menu]

1. 在“部署到 Azure”对话框中，如果已有现有的 Tomcat Web 应用，则可将该应用程序直接部署到该 Web 应用，否则应该先创建一个 Web 应用。
   1. 单击“没有可用的 Web 应用，单击此处以新建一个”链接以创建新的 Web 应用；如果订阅中已有现有的 Web 应用，可以从“Web 应用”下拉列表中选择“创建新的 Web 应用”。  

      ![“部署到 Azure”对话框][deploy-to-azure-dialog]

   1. 在弹出对话框中，选择“TOMCAT 8.5-jre8”作为 Web 容器，指定其他所需信息，然后单击“确定”创建 Web 应用。  

      ![创建新 Web 应用][create-new-web-app-dialog]

   1. 从“Web 应用”下拉列表中选择 Web 应用，然后单击“运行”。（若要部署到现有的 Web 应用，则可以从此处开始） 

      ![部署到现有的 Web 应用][deploy-to-existing-webapp]

1. 成功部署 Web 应用后，工具包会显示一条状态消息，以及成功部署的 Web 应用的 URL。

   ![成功部署][successfully-deployed]

1. 可使用状态消息中提供的链接转到 Web 应用。

   ![转到你的 Web 应用][browse-web-app]

## <a name="managing-deploy-configurations"></a>管理部署配置

1. 发布 Web 应用后，你的设置将保存为默认设置。可以通过单击工具栏上的绿色箭头图标来运行部署。 可通过单击 Web 应用的下拉菜单来修改设置，然后单击“编辑配置”  。

   ![“编辑配置”菜单][edit-configuration-menu]

1. 出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”   。

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a>清理资源

1. 在 Azure 资源管理器中删除 Web 应用

     ![清理资源][clean-resources]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-intellij-create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->
[marketplace]:./media/azure-toolkit-for-intellij-create-hello-world-web-app/marketplace.png
[file-new-project]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/maven-options.png
[project-name]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/project-name.png
[open-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/open-index-page.png
[edit-index-page]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/clean-resource.png
[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png
