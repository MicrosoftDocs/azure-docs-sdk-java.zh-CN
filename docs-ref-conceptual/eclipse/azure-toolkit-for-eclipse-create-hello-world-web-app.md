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
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a><span data-ttu-id="313b5-104">使用 Eclipse 创建适用于 Azure 应用服务的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="313b5-104">Create a Hello World web app for Azure App Service using Eclipse</span></span>

<span data-ttu-id="313b5-105">使用开源的 [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) 插件可以快速创建一个基本的 Hello World 应用程序并将其作为 Web 应用部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="313b5-105">Using open sourced [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="313b5-106">如果你偏爱使用 IntelliJ IDEA，请查看[适用于 IntelliJ 的类似教程][intellij-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="313b5-106">If you prefer using IntelliJ IDEA, check out our [similar tutorial for IntelliJ][intellij-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="313b5-107">请勿忘记在完成本教程后清理资源。</span><span class="sxs-lookup"><span data-stu-id="313b5-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="313b5-108">在这种情况下，运行本指南不会超出免费帐户配额。</span><span class="sxs-lookup"><span data-stu-id="313b5-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="313b5-109">安装和登录</span><span class="sxs-lookup"><span data-stu-id="313b5-109">Installation and sign-in</span></span>

1. <span data-ttu-id="313b5-110">将以下按钮拖到正在运行的 Eclipse 工作区，以便安装用于 Eclipse 插件的 Azure 工具包（[其他安装选项](azure-toolkit-for-eclipse-installation.md)）。</span><span class="sxs-lookup"><span data-stu-id="313b5-110">Drag the following button to your running Eclipse workspace to install the Azure Toolkit for Eclipse plugin ([other installation options](azure-toolkit-for-eclipse-installation.md)).</span></span>

    <span data-ttu-id="313b5-111">[![拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端")</span><span class="sxs-lookup"><span data-stu-id="313b5-111">[![Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Drag to your running Eclipse* workspace. *Requires Eclipse Marketplace Client")</span></span>

1. <span data-ttu-id="313b5-112">若要登录到 Azure 帐户，请依次单击“工具”、“Azure”、“登录”。   </span><span class="sxs-lookup"><span data-stu-id="313b5-112">To sign in to your Azure account, click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="313b5-113">![用于 Azure 登录的 Eclipse 菜单][I01]</span><span class="sxs-lookup"><span data-stu-id="313b5-113">![Eclipse Menu for Azure Sign In][I01]</span></span>

1. <span data-ttu-id="313b5-114">在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](azure-toolkit-for-eclipse-sign-in-instructions.md)）。   </span><span class="sxs-lookup"><span data-stu-id="313b5-114">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign-in options](azure-toolkit-for-eclipse-sign-in-instructions.md)).</span></span>

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

1. <span data-ttu-id="313b5-116">在“Azure 设备登录”对话框中单击“复制并打开”。  </span><span class="sxs-lookup"><span data-stu-id="313b5-116">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![“Azure 登录”对话框窗口][I03]

1. <span data-ttu-id="313b5-118">在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  </span><span class="sxs-lookup"><span data-stu-id="313b5-118">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![设备登录浏览器][I04]

1. <span data-ttu-id="313b5-120">最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  </span><span class="sxs-lookup"><span data-stu-id="313b5-120">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![“选择订阅”对话框][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="313b5-122">创建 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="313b5-122">Creating web app project</span></span>

1. <span data-ttu-id="313b5-123">依次单击“文件”、“新建”和“动态 Web 项目”。   </span><span class="sxs-lookup"><span data-stu-id="313b5-123">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="313b5-124">（如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，并单击“下一步”。）         </span><span class="sxs-lookup"><span data-stu-id="313b5-124">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![创建新的动态 Web 项目][file-new-dynamic-web-project]

2. <span data-ttu-id="313b5-126">在本教程中，项目命名为 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="313b5-126">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="313b5-127">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="313b5-127">Your screen will appear similar to the following:</span></span>
   
   ![“新建动态 Web 项目”属性][dynamic-web-project-properties]

3. <span data-ttu-id="313b5-129">单击“完成”  。</span><span class="sxs-lookup"><span data-stu-id="313b5-129">Click **Finish**.</span></span>

4. <span data-ttu-id="313b5-130">在 Eclipse 的项目资源管理器视图中，展开“MyWebApp”。 </span><span class="sxs-lookup"><span data-stu-id="313b5-130">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="313b5-131">右键单击“WebContent”，单击“新建”，并单击“JSP 文件”。   </span><span class="sxs-lookup"><span data-stu-id="313b5-131">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![新建 JSP 文件][create-new-jsp-file]

5. <span data-ttu-id="313b5-133">在“新建 JSP 文件”对话框中，将文件命名为“index.jsp”，将父文件夹保留为“MyWebApp/WebContent”，然后单击“下一步”。    </span><span class="sxs-lookup"><span data-stu-id="313b5-133">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![“新建 JSP 文件”对话框][new-jsp-file-dialog]

6. <span data-ttu-id="313b5-135">基于本教程的目的，在“选择 JSP 模板”对话框中选择“新建 JSP 文件(html)”，并单击“完成”。   </span><span class="sxs-lookup"><span data-stu-id="313b5-135">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![选择 JSP 模板][select-jsp-template]

7. <span data-ttu-id="313b5-137">在 Eclipse 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="313b5-137">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="313b5-138">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="313b5-138">within the existing `<body>` element.</span></span> <span data-ttu-id="313b5-139">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="313b5-139">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="313b5-140">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="313b5-140">Save index.jsp.</span></span>

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="313b5-141">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="313b5-141">Deploying web app to Azure</span></span>

1. <span data-ttu-id="313b5-142">在 Eclipse 的“项目资源管理器”视图中，右键单击项目，选择“Azure”，然后选择“发布为 Azure Web 应用”   。</span><span class="sxs-lookup"><span data-stu-id="313b5-142">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![发布为 Azure Web 应用][publish-as-azure-web-app]

1. <span data-ttu-id="313b5-144">当“部署 Web 应用”对话框显示时，可选择以下某个选项： </span><span class="sxs-lookup"><span data-stu-id="313b5-144">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="313b5-145">选择现有的 Web 应用（如果存在）。</span><span class="sxs-lookup"><span data-stu-id="313b5-145">Select an existing web app if one exists.</span></span>

      ![选择应用服务][select-app-service]

   * <span data-ttu-id="313b5-147">单击“创建新 Web 应用”  。</span><span class="sxs-lookup"><span data-stu-id="313b5-147">Click **Create New Web App**.</span></span>

      ![创建应用服务][create-app-service]

      <span data-ttu-id="313b5-149">在“创建应用服务”对话框中为 Web 应用指定必要信息，然后单击“创建”   。</span><span class="sxs-lookup"><span data-stu-id="313b5-149">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      <span data-ttu-id="313b5-150">在这里可以配置运行时环境、应用设置、服务计划和资源组。</span><span class="sxs-lookup"><span data-stu-id="313b5-150">Here you can configure the runtime environment, app settings, service plan and resource group.</span></span>

      ![“创建应用服务”对话框][create-app-service-dialog]

1. <span data-ttu-id="313b5-152">选择 Web 应用，然后单击“部署”  。</span><span class="sxs-lookup"><span data-stu-id="313b5-152">Select your web app and then click **Deploy**.</span></span>

   ![部署应用服务][deploy-app-service]

1. <span data-ttu-id="313b5-154">工具包在成功部署 Web 应用后会在“Azure 活动日志”选项卡下显示“已发布”状态   ，这是已部署 Web 应用的 URL 超链接。</span><span class="sxs-lookup"><span data-stu-id="313b5-154">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![发布状态][publish-status]

1. <span data-ttu-id="313b5-156">可使用状态消息中提供的链接转到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="313b5-156">You can browse to your web app using the link provided in the status message.</span></span>

   ![转到你的 Web 应用][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="cleaning-up-resources"></a><span data-ttu-id="313b5-158">清理资源</span><span class="sxs-lookup"><span data-stu-id="313b5-158">Cleaning up resources</span></span>

1. <span data-ttu-id="313b5-159">将 Web 应用发布到 Azure 以后，即可对其进行管理，方法是在 Azure 资源管理器中右键单击，然后在上下文菜单中选择一个选项。</span><span class="sxs-lookup"><span data-stu-id="313b5-159">After you have published your web app to Azure, you can manage it by right-clicking in Azure Explorer and selecting one of the options in the context menu.</span></span> <span data-ttu-id="313b5-160">例如，可以**删除**此处的 Web 应用，以便清理本教程的资源。</span><span class="sxs-lookup"><span data-stu-id="313b5-160">For example, you can **Delete** your web app here to clean up the resource for this tutorial.</span></span>

   ![管理应用服务][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="313b5-162">后续步骤</span><span class="sxs-lookup"><span data-stu-id="313b5-162">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="313b5-163">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="313b5-163">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
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
