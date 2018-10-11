---
title: 使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用
description: 本教程说明如何使用 Azure Toolkit for IntelliJ 创建 Azure 的 Hello World Web 应用。
services: app-service
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
ms.openlocfilehash: cc68e16a6940a1f0f2b08f0b63c90c58ec6dbc4e
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892858"
---
# <a name="create-a-hello-world-web-app-for-azure-using-intellij"></a><span data-ttu-id="faa30-103">使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="faa30-103">Create a Hello World web app for Azure using IntelliJ</span></span>

<span data-ttu-id="faa30-104">本教程介绍如何使用[用于 IntelliJ 的 Azure 工具包]创建基本 Hello World 应用程序，并将其作为 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="faa30-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="faa30-105">如需使用[用于 Eclipse 的 Azure 工具]的本文版本，请参阅[使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用][eclipse-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="faa30-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="faa30-106">用于 IntelliJ 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。</span><span class="sxs-lookup"><span data-stu-id="faa30-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="faa30-107">本文详述如何使用用于 IntelliJ 的 Azure 工具包 3.0.7（或更高版本）创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="faa30-107">This article illustrates creating a Hello World web app by using version 3.0.7 (or later) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="faa30-108">如果使用的是工具包 3.0.6（或更低版本），则需执行[使用旧工具包在 IntelliJ 中创建适用于 Azure 的 Hello World Web 应用][Legacy Version]中的步骤。</span><span class="sxs-lookup"><span data-stu-id="faa30-108">If you are using the version 3.0.6 (or earlier) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ using the legacy toolkit][Legacy Version].</span></span>
> 

<span data-ttu-id="faa30-109">完成本教程后，应用程序会在 Web 浏览器中如下图所示：</span><span class="sxs-lookup"><span data-stu-id="faa30-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 应用预览][browse-web-app]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="faa30-111">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="faa30-111">Create a new web app project</span></span>

1. <span data-ttu-id="faa30-112">启动 IntelliJ，然后根据[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明][intelliJ-sign-in-instructions]一文中的说明登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="faa30-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="faa30-113">依次单击“文件”菜单、“新建”、“项目”。</span><span class="sxs-lookup"><span data-stu-id="faa30-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![创建新项目][file-new-project]

1. <span data-ttu-id="faa30-115">在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="faa30-115">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="faa30-117">为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="faa30-117">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="faa30-119">自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="faa30-119">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 设置][maven-options]

1. <span data-ttu-id="faa30-121">指定项目名称和位置，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="faa30-121">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定项目名称][project-name]

1. <span data-ttu-id="faa30-123">在 IntelliJ 的项目资源管理器视图中，依次展开 src、main、webapp，然后双击 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="faa30-123">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![打开索引页面][open-index-page]

1. <span data-ttu-id="faa30-125">在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="faa30-125">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="faa30-126">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="faa30-126">within the existing `<body>` element.</span></span> <span data-ttu-id="faa30-127">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="faa30-127">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![编辑索引页面][edit-index-page]

1. <span data-ttu-id="faa30-129">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="faa30-129">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="faa30-130">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="faa30-130">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="faa30-131">在 IntelliJ 的项目资源管理器视图中，右键单击项目，选择“Azure”，然后选择“在 Web 应用上运行”。</span><span class="sxs-lookup"><span data-stu-id="faa30-131">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![“在 Web 应用上运行”菜单][run-on-web-app-menu]

1. <span data-ttu-id="faa30-133">在“在 Web 应用上运行”对话框中，可选择以下任一选项：</span><span class="sxs-lookup"><span data-stu-id="faa30-133">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="faa30-134">选择现有 Web 应用（如果有），然后单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="faa30-134">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![“在 Web 应用上运行”对话框][run-on-web-app-dialog]

   * <span data-ttu-id="faa30-136">单击“创建新 Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="faa30-136">Click **Create New Web App**.</span></span> <span data-ttu-id="faa30-137">如果选择创建新 Web 应用，请为 Web 应用指定必要信息，然后单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="faa30-137">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![创建新 Web 应用][create-new-web-app-dialog]

1. <span data-ttu-id="faa30-139">成功部署 Web 应用后，该工具包会显示一条状态消息，其中还含有所部署 Web 应用的 URL。</span><span class="sxs-lookup"><span data-stu-id="faa30-139">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![成功部署][successfully-deployed]

1. <span data-ttu-id="faa30-141">可使用状态消息中提供的链接转到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="faa30-141">You can browse to your web app using the link provided in the status message.</span></span>

   ![转到你的 Web 应用][browse-web-app]

1. <span data-ttu-id="faa30-143">发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="faa30-143">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="faa30-144">可通过单击 Web 应用的下拉菜单来修改设置，然后单击“编辑配置”。</span><span class="sxs-lookup"><span data-stu-id="faa30-144">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![“编辑配置”菜单][edit-configuration-menu]

1. <span data-ttu-id="faa30-146">出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="faa30-146">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="faa30-148">后续步骤</span><span class="sxs-lookup"><span data-stu-id="faa30-148">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="faa30-149">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="faa30-149">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包]: azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[用于 Eclipse 的 Azure 工具]: ../eclipse/azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
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
