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
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a><span data-ttu-id="28dbe-104">使用 IntelliJ 创建适用于 Azure 应用服务的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="28dbe-104">Create a Hello World web app for Azure App Service using IntelliJ</span></span>

<span data-ttu-id="28dbe-105">使用开源的 [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 插件可以快速创建一个基本的 Hello World 应用程序并将其作为 Web 应用部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="28dbe-105">Using open sourced [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) plugin, creating and deploying a basic Hello World application to Azure App Service as a web app can be done in a few minutes.</span></span>

> [!NOTE]
>
> <span data-ttu-id="28dbe-106">如果你偏爱使用 Eclipse，请查看[适用于 Eclipse 的类似教程][eclipse-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="28dbe-106">If you prefer using Eclipse, check out our [similar tutorial for Eclipse][eclipse-hello-world].</span></span>
>
>[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]
>
> <span data-ttu-id="28dbe-107">请勿忘记在完成本教程后清理资源。</span><span class="sxs-lookup"><span data-stu-id="28dbe-107">Don't forget to clean up the resources after you complete this tutorial.</span></span> <span data-ttu-id="28dbe-108">在这种情况下，运行本指南不会超出免费帐户配额。</span><span class="sxs-lookup"><span data-stu-id="28dbe-108">In that case, running this guide will not exceed your free account quota.</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a><span data-ttu-id="28dbe-109">安装和登录</span><span class="sxs-lookup"><span data-stu-id="28dbe-109">Installation and Sign-in</span></span>

1. <span data-ttu-id="28dbe-110">在 IntelliJ IDEA 的“设置/首选项”对话框中 (Ctrl+Alt+S) 中，选择“插件”。 </span><span class="sxs-lookup"><span data-stu-id="28dbe-110">In IntelliJ IDEA's Settings/Preferences dialog (Ctrl+Alt+S), select **Plugins**.</span></span> <span data-ttu-id="28dbe-111">然后，在“市场”中找到“Azure Toolkit for IntelliJ”并单击“安装”。   </span><span class="sxs-lookup"><span data-stu-id="28dbe-111">Then, find the **Azure Toolkit for IntelliJ** in the **Marketplace** and click **Install**.</span></span> <span data-ttu-id="28dbe-112">安装后，单击“重启”以激活该插件。 </span><span class="sxs-lookup"><span data-stu-id="28dbe-112">After installed, click **Restart** to activate the plugin.</span></span> 

   ![市场中的 Azure Toolkit for IntelliJ 插件][marketplace]

2. <span data-ttu-id="28dbe-114">若要登录到你的 Azure 帐户，请打开边栏中的“Azure 资源管理器”，然后单击顶部栏中的“Azure 登录”图标（或者在 IDEA 菜单中选择“工具”>“Azure”>“Azure 登录”）。   </span><span class="sxs-lookup"><span data-stu-id="28dbe-114">To sign in to your Azure account, open sidebar **Azure Explorer**, and then click the **Azure Sign In** icon in the bar on top (or from IDEA menu **Tools/Azure/Azure Sign in**).</span></span>

   ![“IntelliJ Azure 登录”命令][I01]

3. <span data-ttu-id="28dbe-116">在“Azure 登录”窗口中选择“设备登录”，然后单击“登录”（[其他登录选项](azure-toolkit-for-intellij-sign-in-instructions.md)）。   </span><span class="sxs-lookup"><span data-stu-id="28dbe-116">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in** ([other sign in options](azure-toolkit-for-intellij-sign-in-instructions.md)).</span></span>

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. <span data-ttu-id="28dbe-118">在“Azure 设备登录”对话框中单击“复制并打开”。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-118">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![“Azure 登录”对话框窗口][I03]

5. <span data-ttu-id="28dbe-120">在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-120">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![设备登录浏览器][I04]

6. <span data-ttu-id="28dbe-122">在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-122">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][I05]

## <a name="creating-web-app-project"></a><span data-ttu-id="28dbe-124">创建 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="28dbe-124">Creating web app project</span></span>

1. <span data-ttu-id="28dbe-125">在 IntelliJ 中，依次单击“文件”菜单、“新建”、“项目”。   </span><span class="sxs-lookup"><span data-stu-id="28dbe-125">In IntelliJ, click the **File** menu, then click **New**, and then click **Project**.</span></span>

   ![创建新项目][file-new-project]

2. <span data-ttu-id="28dbe-127">在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”     。</span><span class="sxs-lookup"><span data-stu-id="28dbe-127">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>

   ![选择 Maven archetype Webapp][maven-archetype-webapp]

3. <span data-ttu-id="28dbe-129">为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”    。</span><span class="sxs-lookup"><span data-stu-id="28dbe-129">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>

   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

4. <span data-ttu-id="28dbe-131">自定义任何 Maven 设置或接受默认设置，然后单击“下一步”  。</span><span class="sxs-lookup"><span data-stu-id="28dbe-131">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>

   ![指定 Maven 设置][maven-options]

5. <span data-ttu-id="28dbe-133">指定项目名称和位置，并单击“完成”  。</span><span class="sxs-lookup"><span data-stu-id="28dbe-133">Specify your project name and location, and then click **Finish**.</span></span>

   ![指定项目名称][project-name]

6. <span data-ttu-id="28dbe-135">在“项目资源管理器”视图下，按如下所示打开并编辑文件 **src/main/webapp/index.jsp**，然后**保存更改**：</span><span class="sxs-lookup"><span data-stu-id="28dbe-135">Under Project Explorer view, open and edit the file **src/main/webapp/index.jsp** as following and **save the changes**:</span></span>

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```

   ![编辑索引页面][edit-index-page]

## <a name="deploying-web-app-to-azure"></a><span data-ttu-id="28dbe-137">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="28dbe-137">Deploying web app to Azure</span></span>

1. <span data-ttu-id="28dbe-138">在“项目资源管理器”视图下右键单击你的项目，展开“Azure”，然后单击“部署到 Azure”。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-138">Under Project Explorer view, right-click your project, expand **Azure**, then click **Deploy to Azure**.</span></span>

   ![“部署到 Azure”菜单][deploy-to-azure-menu]

1. <span data-ttu-id="28dbe-140">在“部署到 Azure”对话框中，如果已有现有的 Tomcat Web 应用，则可将该应用程序直接部署到该 Web 应用，否则应该先创建一个 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="28dbe-140">In the Deploy to Azure dialog box, you can directly deploy the application to an existing Tomcat webapp if you already have one, otherwise you should create a new one first.</span></span>
   1. <span data-ttu-id="28dbe-141">单击“没有可用的 Web 应用，单击此处以新建一个”链接以创建新的 Web 应用；如果订阅中已有现有的 Web 应用，可以从“Web 应用”下拉列表中选择“创建新的 Web 应用”。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-141">Click the link **No Available webapp, click to create a new one** to crete a new web app, you could choose **Create New WebApp** from WebApp dropdown if there are existing webapps in your subscription.</span></span>

      ![“部署到 Azure”对话框][deploy-to-azure-dialog]

   1. <span data-ttu-id="28dbe-143">在弹出对话框中，选择“TOMCAT 8.5-jre8”作为 Web 容器，指定其他所需信息，然后单击“确定”创建 Web 应用。  </span><span class="sxs-lookup"><span data-stu-id="28dbe-143">In the pop-up dialog box, chose **TOMCAT 8.5-jre8** as Web Container and specify other required information, then click **OK** to create the webapp.</span></span>

      ![创建新 Web 应用][create-new-web-app-dialog]

   1. <span data-ttu-id="28dbe-145">从“Web 应用”下拉列表中选择 Web 应用，然后单击“运行”。（若要部署到现有的 Web 应用，则可以从此处开始） </span><span class="sxs-lookup"><span data-stu-id="28dbe-145">Choose the web app from WebApp drop down, and then click **Run**.(You could start from here if you want deploy to an existing webapp)</span></span>

      ![部署到现有的 Web 应用][deploy-to-existing-webapp]

1. <span data-ttu-id="28dbe-147">成功部署 Web 应用后，工具包会显示一条状态消息，以及成功部署的 Web 应用的 URL。</span><span class="sxs-lookup"><span data-stu-id="28dbe-147">The toolkit will display a status message when it has successfully deployed your web app, along with the URL of your deployed web app if succeed.</span></span>

   ![成功部署][successfully-deployed]

1. <span data-ttu-id="28dbe-149">可使用状态消息中提供的链接转到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="28dbe-149">You can browse to your web app using the link provided in the status message.</span></span>

   ![转到你的 Web 应用][browse-web-app]

## <a name="managing-deploy-configurations"></a><span data-ttu-id="28dbe-151">管理部署配置</span><span class="sxs-lookup"><span data-stu-id="28dbe-151">Managing deploy configurations</span></span>

1. <span data-ttu-id="28dbe-152">发布 Web 应用后，你的设置将保存为默认设置。可以通过单击工具栏上的绿色箭头图标来运行部署。</span><span class="sxs-lookup"><span data-stu-id="28dbe-152">After you have published your web app, your settings will be saved as the default, and you can run the deployment by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="28dbe-153">可通过单击 Web 应用的下拉菜单来修改设置，然后单击“编辑配置”  。</span><span class="sxs-lookup"><span data-stu-id="28dbe-153">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![“编辑配置”菜单][edit-configuration-menu]

1. <span data-ttu-id="28dbe-155">出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”   。</span><span class="sxs-lookup"><span data-stu-id="28dbe-155">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="cleaning-up-resources"></a><span data-ttu-id="28dbe-157">清理资源</span><span class="sxs-lookup"><span data-stu-id="28dbe-157">Cleaning up resources</span></span>

1. <span data-ttu-id="28dbe-158">在 Azure 资源管理器中删除 Web 应用</span><span class="sxs-lookup"><span data-stu-id="28dbe-158">Deleting Web Apps in Azure Explorer</span></span>

     ![清理资源][clean-resources]

## <a name="next-steps"></a><span data-ttu-id="28dbe-160">后续步骤</span><span class="sxs-lookup"><span data-stu-id="28dbe-160">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="28dbe-161">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="28dbe-161">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
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
