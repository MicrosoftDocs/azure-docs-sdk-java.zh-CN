---
title: 使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for Eclipse 创建适用于 Azure 的 Hello World Web 应用。
services: app-service
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
ms.openlocfilehash: 5e025c90c2619ec72ffddf5815fd49c3ac59c00f
ms.sourcegitcommit: 798f4d4199d3be9fc5c9f8bf7a754d7393de31ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-hello-world-web-app-for-azure-using-eclipse"></a><span data-ttu-id="b71d8-103">使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="b71d8-103">Create a Hello World web app for Azure using Eclipse</span></span>

<span data-ttu-id="b71d8-104">本教程说明如何使用[用于 Eclipse 的 Azure 工具包]创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b71d8-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="b71d8-105">如需使用[用于 IntelliJ 的 Azure 工具包]的本文版本，请参阅[使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用][intellij-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="b71d8-105">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="b71d8-106">用于 Eclipse 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。</span><span class="sxs-lookup"><span data-stu-id="b71d8-106">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="b71d8-107">本文详述如何使用用于 Eclipse 的 Azure 工具包 3.0.7（或更高版本）创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b71d8-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="b71d8-108">如果使用的是工具包 3.0.6（或更低版本），则需执行[使用旧工具包在 Eclipse 中创建适用于 Azure 的 Hello World Web 应用][Legacy Version]中的步骤。</span><span class="sxs-lookup"><span data-stu-id="b71d8-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="b71d8-109">完成本教程后，应用程序会在 Web 浏览器中如下图所示：</span><span class="sxs-lookup"><span data-stu-id="b71d8-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 应用预览][browse-web-app]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="b71d8-111">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="b71d8-111">Create a new web app project</span></span>

1. <span data-ttu-id="b71d8-112">启动 Eclipse，然后根据[用于 Eclipse 的 Azure 工具包的 Azure 登录说明][eclipse-sign-in-instructions]一文中的说明登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="b71d8-112">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="b71d8-113">依次单击“文件”、“新建”和“动态 Web 项目”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-113">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="b71d8-114">（如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，并单击“下一步”。）</span><span class="sxs-lookup"><span data-stu-id="b71d8-114">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

   ![创建新的动态 Web 项目][file-new-dynamic-web-project]

2. <span data-ttu-id="b71d8-116">在本教程中，项目命名为 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="b71d8-116">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="b71d8-117">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="b71d8-117">Your screen will appear similar to the following:</span></span>
   
   ![“新建动态 Web 项目”属性][dynamic-web-project-properties]

3. <span data-ttu-id="b71d8-119">单击“完成” 。</span><span class="sxs-lookup"><span data-stu-id="b71d8-119">Click **Finish**.</span></span>

4. <span data-ttu-id="b71d8-120">在 Eclipse 的项目资源管理器视图中，展开“MyWebApp”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-120">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="b71d8-121">右键单击“WebContent”，单击“新建”，并单击“JSP 文件”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-121">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

   ![新建 JSP 文件][create-new-jsp-file]

5. <span data-ttu-id="b71d8-123">在“新建 JSP 文件”对话框中，将文件命名为“index.jsp”，将父文件夹保留为“MyWebApp/WebContent”，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-123">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

   ![“新建 JSP 文件”对话框][new-jsp-file-dialog]

6. <span data-ttu-id="b71d8-125">基于本教程的目的，在“选择 JSP 模板”对话框中选择“新建 JSP 文件(html)”，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-125">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

   ![选择 JSP 模板][select-jsp-template]

7. <span data-ttu-id="b71d8-127">在 Eclipse 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="b71d8-127">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="b71d8-128">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="b71d8-128">within the existing `<body>` element.</span></span> <span data-ttu-id="b71d8-129">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="b71d8-129">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="b71d8-130">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="b71d8-130">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="b71d8-131">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="b71d8-131">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="b71d8-132">在 Eclipse 的“项目资源管理器”视图中，右键单击项目，选择“Azure”，然后选择“发布为 Azure Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-132">Within Eclipse's Project Explorer view, right-click your project, choose **Azure**, and then choose **Publish as Azure Web App**.</span></span>
   
   ![发布为 Azure Web 应用][publish-as-azure-web-app]

1. <span data-ttu-id="b71d8-134">当“部署 Web 应用”对话框显示时，可选择以下某个选项：</span><span class="sxs-lookup"><span data-stu-id="b71d8-134">When the **Deploy Web App** dialog box appears, you can choose one of the following options:</span></span>

   * <span data-ttu-id="b71d8-135">选择现有的 Web 应用（如果存在）。</span><span class="sxs-lookup"><span data-stu-id="b71d8-135">Select an existing web app if one exists.</span></span>

      ![选择应用服务][select-app-service]

   * <span data-ttu-id="b71d8-137">单击“创建新 Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-137">Click **Create New Web App**.</span></span>

      ![创建应用服务][create-app-service]

      <span data-ttu-id="b71d8-139">在“创建应用服务”对话框中为 Web 应用指定必要信息，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-139">Specify the requisite information for your web app in the **Create App Service** dialog box, and then click **Create**.</span></span>

      ![“创建应用服务”对话框][create-app-service-dialog]

1. <span data-ttu-id="b71d8-141">选择 Web 应用，然后单击“部署”。</span><span class="sxs-lookup"><span data-stu-id="b71d8-141">Select your web app and then click **Deploy**.</span></span>

   ![部署应用服务][deploy-app-service]

1. <span data-ttu-id="b71d8-143">工具包在成功部署 Web 应用后会在“Azure 活动日志”选项卡下显示“已发布”状态，这是已部署 Web 应用的 URL 超链接。</span><span class="sxs-lookup"><span data-stu-id="b71d8-143">The toolkit will display a **Published** status under the **Azure Activity Log** tab when it has successfully deployed your web app, which is a hyperlink for the URL of your deployed web app.</span></span>

   ![发布状态][publish-status]

1. <span data-ttu-id="b71d8-145">可使用状态消息中提供的链接转到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b71d8-145">You can browse to your web app using the link provided in the status message.</span></span>

   ![转到你的 Web 应用][browse-web-app]

1. <span data-ttu-id="b71d8-147">将 Web 发布到 Azure 以后，即可对应用进行管理，方法是右键单击该应用，然后在上下文菜单中选择一个选项。</span><span class="sxs-lookup"><span data-stu-id="b71d8-147">After you have published your web to Azure, you can manage your app by right-clicking on it and selecting one of the options on the context menu.</span></span> <span data-ttu-id="b71d8-148">例如，可以**启动**、**停止**或**删除** Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b71d8-148">For example, you can **Start**, **Stop**, or **Delete** your web app.</span></span>

   ![管理应用服务][manage-app-service]

## <a name="next-steps"></a><span data-ttu-id="b71d8-150">后续步骤</span><span class="sxs-lookup"><span data-stu-id="b71d8-150">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="b71d8-151">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="b71d8-151">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[用于 Eclipse 的 Azure 工具包]: azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: azure-toolkit-for-eclipse.md
[用于 IntelliJ 的 Azure 工具包]: ../intellij/azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: ../intellij/azure-toolkit-for-intellij.md
[intellij-hello-world]: ../intellij/azure-toolkit-for-intellij-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
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
