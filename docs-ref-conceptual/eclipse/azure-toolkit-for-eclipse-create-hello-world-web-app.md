---
title: 使用 Eclipse 创建适用于 Azure 应用服务的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for Eclipse 创建适用于 Azure 的 Hello World Web 应用。
services: app-service
keywords: java, eclipse, web 应用, azure 应用服务, hello world, 快速入门
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.author: robmcm;asirveda
ms.date: 02/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 7e88298afaf0b4601d85d6063b7096c79e677421
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65625873"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>使用 Eclipse 创建适用于 Azure 应用服务的 Hello World Web 应用

使用开源的 [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) 插件可以快速创建一个基本的 Hello World 应用程序并将其作为 Web 应用部署到 Azure 应用服务。

> [!NOTE]
>
> 如果你偏爱使用 IntelliJ IDEA，请查看[适用于 IntelliJ 的类似教程][intellij-hello-world]。
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> 请勿忘记在完成本教程后清理资源。 在这种情况下，运行本指南不会超出免费帐户配额。
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>安装和登录

1. 将以下按钮拖到正在运行的 Eclipse 工作区，以便安装用于 Eclipse 插件的 Azure 工具包（[其他安装选项](azure-toolkit-for-eclipse-installation.md)）。

    [![拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端")

1. 若要登录到 Azure 帐户，请依次单击“工具”、“Azure”、“登录”。   
   ![用于 Azure 登录的 Eclipse 菜单][I01]

1. 在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](azure-toolkit-for-eclipse-sign-in-instructions.md)）。   

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

1. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][I03]

1. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][I04]

1. 最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  

   ![“选择订阅”对话框][I05]

## <a name="creating-web-app-project"></a>创建 Web 应用项目

1. 依次单击“文件”、“新建”和“动态 Web 项目”。    （如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，并单击“下一步”。）         

   ![创建新的动态 Web 项目][file-new-dynamic-web-project]

2. 在本教程中，项目命名为 **MyWebApp**。 屏幕应与下图中所示类似：
   
   ![“新建动态 Web 项目”属性][dynamic-web-project-properties]

3. 单击“完成”  。

4. 在 Eclipse 的项目资源管理器视图中，展开“MyWebApp”。  右键单击“WebContent”，单击“新建”，并单击“JSP 文件”。   

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

## <a name="deploying-web-app-to-azure"></a>将 Web 应用部署到 Azure

1. 在 Eclipse 的“项目资源管理器”视图中，右键单击项目，选择“Azure”，然后选择“发布为 Azure Web 应用”   。
   
   ![发布为 Azure Web 应用][publish-as-azure-web-app]

1. 当“部署 Web 应用”对话框显示时，可选择以下某个选项： 

   * 选择现有的 Web 应用（如果存在）。

      ![选择应用服务][select-app-service]

   * 单击“创建新 Web 应用”  。

      ![创建应用服务][create-app-service]

      在“创建应用服务”对话框中为 Web 应用指定必要信息，然后单击“创建”   。

      在这里可以配置运行时环境、应用设置、服务计划和资源组。

      ![“创建应用服务”对话框][create-app-service-dialog]

1. 选择 Web 应用，然后单击“部署”  。

   ![部署应用服务][deploy-app-service]

1. 工具包在成功部署 Web 应用后会在“Azure 活动日志”选项卡下显示“已发布”状态   ，这是已部署 Web 应用的 URL 超链接。

   ![发布状态][publish-status]

1. 可使用状态消息中提供的链接转到 Web 应用。

   ![转到你的 Web 应用][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a>清理资源

1. 将 Web 应用发布到 Azure 以后，即可对其进行管理，方法是在 Azure 资源管理器中右键单击，然后在上下文菜单中选择一个选项。 例如，可以**删除**此处的 Web 应用，以便清理本教程的资源。

   ![管理应用服务][manage-app-service]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version.md

<!-- IMG List -->
[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
