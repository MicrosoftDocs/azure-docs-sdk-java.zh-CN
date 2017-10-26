---
title: "在 IntelliJ 中创建基本的 Azure Web 应用"
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
ms.date: 10/19/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 323ce74f2d4dad10460a0c2bc0fb59f2bbdbbf3c
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="e38b0-103">在 IntelliJ 中创建基本的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-103">Create a basic Azure web app in IntelliJ</span></span>

<span data-ttu-id="e38b0-104">本教程介绍如何使用[用于 IntelliJ 的 Azure 工具包]创建基本 Hello World 应用程序，并将其作为 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="e38b0-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
> 
> <span data-ttu-id="e38b0-105">用于 IntelliJ 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同工作流。</span><span class="sxs-lookup"><span data-stu-id="e38b0-105">The Azure Toolkit for IntelliJ was updated in August 2017, with a different workflow.</span></span> <span data-ttu-id="e38b0-106">基于这一点，本文包含了两个不同的部分：</span><span class="sxs-lookup"><span data-stu-id="e38b0-106">With that in mind, this article contains two different sections:</span></span>
>
> * <span data-ttu-id="e38b0-107">第一部分详述如何通过用于 IntelliJ 的 Azure 工具包 2017 年 8 月版（或更高版本）来创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-107">The first section illustrates creating a Hello World web app by using the August 2017 (or later) version of the Azure Toolkit for IntelliJ.</span></span>
>
> * <span data-ttu-id="e38b0-108">本文的第二部分介绍如何通过 2017 年 4 月版（或更早版本）的工具包来创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-108">The second section of this article demonstrates creating a Hello World web app by using the April 2017 (or earlier) version of the toolkit.</span></span>
> 

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-hello-world-web-app-by-using-the-version-307-or-later-toolkit"></a><span data-ttu-id="e38b0-109">使用 3.0.7 版或更高版本的工具包创建 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-109">Create a Hello World web app by using the version 3.0.7 or later toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="e38b0-110">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="e38b0-110">Create a new web app project</span></span>

1. <span data-ttu-id="e38b0-111">启动 IntelliJ，并按照 [用于 IntelliJ 的 Azure 工具包的登录说明] 一文中的步骤登录 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="e38b0-111">Start IntelliJ and sign-in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="e38b0-112">依次单击“文件”菜单、“新建”、“项目”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-112">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![创建新项目][file-new-project]

1. <span data-ttu-id="e38b0-114">在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-114">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="e38b0-116">为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-116">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="e38b0-118">自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-118">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 设置][maven-options]

1. <span data-ttu-id="e38b0-120">指定项目名称和位置，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-120">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定项目名称][project-name]

1. <span data-ttu-id="e38b0-122">在 IntelliJ 的项目资源管理器视图中，依次展开 src、main、webapp，然后双击 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="e38b0-122">Within IntelliJ's Project Explorer view, expand **src**, then **main**, then **webapp**, and then double-click **index.jsp**.</span></span>
   
   ![打开索引页面][open-index-page]

1. <span data-ttu-id="e38b0-124">在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="e38b0-124">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="e38b0-125">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="e38b0-125">within the existing `<body>` element.</span></span> <span data-ttu-id="e38b0-126">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="e38b0-126">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ``` 

   ![编辑索引页面][edit-index-page]

1. <span data-ttu-id="e38b0-128">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="e38b0-128">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="e38b0-129">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="e38b0-129">Deploy your web app to Azure</span></span>

1. <span data-ttu-id="e38b0-130">在 IntelliJ 的项目资源管理器视图中，右键单击项目，选择“Azure”，然后选择“在 Web 应用上运行”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-130">Within IntelliJ's Project Explorer view, right-click your project, choose **Azure**, and then choose **Run on Web App**.</span></span>
   
   ![“在 Web 应用上运行”菜单][run-on-web-app-menu]

1. <span data-ttu-id="e38b0-132">在“在 Web 应用上运行”对话框中，可选择以下任一选项：</span><span class="sxs-lookup"><span data-stu-id="e38b0-132">In the Run on Web App dialog box, you can choose one of the following options:</span></span>

   * <span data-ttu-id="e38b0-133">选择现有 Web 应用（如果有），然后单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-133">Choose an existing web app (if one exists), and then click **Run**.</span></span>

      ![“在 Web 应用上运行”对话框][run-on-web-app-dialog]

   * <span data-ttu-id="e38b0-135">单击“创建新 Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-135">Click **Create New Web App**.</span></span> <span data-ttu-id="e38b0-136">如果选择创建新 Web 应用，请为 Web 应用指定必要信息，然后单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-136">If you choose to create a new web app, specify the requisite information for your web app, and then click **Run**.</span></span>

      ![创建新 Web 应用][create-new-web-app-dialog]

1. <span data-ttu-id="e38b0-138">成功部署 Web 应用后，该工具包会显示一条状态消息，其中还含有所部署 Web 应用的 URL。</span><span class="sxs-lookup"><span data-stu-id="e38b0-138">The toolkit will display a status message when it has successfully deployed your web app, which will also display the URL of your deployed web app.</span></span>

   ![成功部署][successfully-deployed]

1. <span data-ttu-id="e38b0-140">可使用状态消息中提供的链接转到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-140">You can browse to your web app using the link provided in the status message.</span></span>

   ![转到你的 Web 应用][browse-web-app]

1. <span data-ttu-id="e38b0-142">发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="e38b0-142">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="e38b0-143">可通过单击 Web 应用的下拉菜单来修改设置，然后单击“编辑配置”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-143">You can modify your settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![“编辑配置”菜单][edit-configuration-menu]

1. <span data-ttu-id="e38b0-145">出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-145">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![“编辑配置”对话框][edit-configuration-dialog]

<hr />

## <a name="create-a-hello-world-web-app-by-using-the-version-306-or-earlier-toolkit"></a><span data-ttu-id="e38b0-147">使用 3.0.6 版或更早版本的工具包创建 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-147">Create a Hello World web app by using the version 3.0.6 or earlier toolkit</span></span>

### <a name="create-a-new-web-app-project"></a><span data-ttu-id="e38b0-148">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="e38b0-148">Create a new web app project</span></span>

1. <span data-ttu-id="e38b0-149">启动 IntelliJ，并依次单击“文件”菜单、“新建”和“项目”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-149">Start IntelliJ and click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![文件新建项目][02]

2. <span data-ttu-id="e38b0-151">在“新建项目”对话框中，依次选择 Java 和“Web 应用程序”，并单击“新建”以添加项目 SDK。</span><span class="sxs-lookup"><span data-stu-id="e38b0-151">In the **New Project** dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
   ![“新建项目”对话框][03a]
   
3. <span data-ttu-id="e38b0-153">在“选择 JDK 的主目录”对话框中，选择要安装 JDK 的文件夹，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-153">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="e38b0-154">在“新建项目”对话框中单击“下一步”以继续。</span><span class="sxs-lookup"><span data-stu-id="e38b0-154">Click **Next** in the New Project dialog box to continue.</span></span>
   
   ![指定 JDK 主目录][03b]

4. <span data-ttu-id="e38b0-156">基于本教程的目的，将项目命名为 **Java-Web-App-On-Azure**，然后单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-156">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
   ![“新建项目”对话框][04]

5. <span data-ttu-id="e38b0-158">在 IntelliJ 的项目资源管理器视图中，依次展开“Java-Web-App-On-Azure”和“Web”，并双击“index.jsp”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-158">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
   ![打开索引页面][05c]

6. <span data-ttu-id="e38b0-160">在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="e38b0-160">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="e38b0-161">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="e38b0-161">within the existing `<body>` element.</span></span> <span data-ttu-id="e38b0-162">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="e38b0-162">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. <span data-ttu-id="e38b0-163">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="e38b0-163">Save index.jsp.</span></span>

### <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="e38b0-164">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="e38b0-164">Deploy your web app to Azure</span></span>
<span data-ttu-id="e38b0-165">有多种方式可将 Java Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="e38b0-165">There are several ways by which you can deploy a Java web app to Azure.</span></span> <span data-ttu-id="e38b0-166">本教程说明其中一个最简单的方式：将应用程序部署到 Azure Web 应用容器，无需特殊的项目类型或额外的工具。</span><span class="sxs-lookup"><span data-stu-id="e38b0-166">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="e38b0-167">Azure 为提供 JDK 及 Web 容器软件，因此不需要自己上传；只需要 Java Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-167">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="e38b0-168">这样，应用程序的发布过程只需数秒，连一分钟都不用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-168">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="e38b0-169">在发布应用程序之前，需要先配置模块设置。</span><span class="sxs-lookup"><span data-stu-id="e38b0-169">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="e38b0-170">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="e38b0-170">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e38b0-171">在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。</span><span class="sxs-lookup"><span data-stu-id="e38b0-171">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="e38b0-172">出现上下文菜单时，单击“打开模块设置”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-172">When the context menu appears, click **Open Module Settings**.</span></span>

   ![打开模块设置][05a]

2. <span data-ttu-id="e38b0-174">“项目结构”对话框出现后：</span><span class="sxs-lookup"><span data-stu-id="e38b0-174">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="e38b0-175">a.</span><span class="sxs-lookup"><span data-stu-id="e38b0-175">a.</span></span> <span data-ttu-id="e38b0-176">单击“项目设置”列表中的“项目”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-176">Click **Artifacts** in the list of **Project Settings**.</span></span>

   <span data-ttu-id="e38b0-177">b.</span><span class="sxs-lookup"><span data-stu-id="e38b0-177">b.</span></span> <span data-ttu-id="e38b0-178">更改“名称”框中的项目名称，使其不包含空格或特殊字符；这是必要步骤，因为该名称会在统一资源标识符 (URI) 中使用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-178">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>

   <span data-ttu-id="e38b0-179">c.</span><span class="sxs-lookup"><span data-stu-id="e38b0-179">c.</span></span> <span data-ttu-id="e38b0-180">将“类型”更改为“Web 应用程序: 存档”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-180">Change the **Type** to **Web Application: Archive**.</span></span>

   <span data-ttu-id="e38b0-181">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-181">d.</span></span> <span data-ttu-id="e38b0-182">单击“确定”关闭“项目结构”对话框。</span><span class="sxs-lookup"><span data-stu-id="e38b0-182">Click **OK** to close the Project Structure dialog box.</span></span>

   ![打开模块设置][05b]

<span data-ttu-id="e38b0-184">配置模块设置后，可以通过执行以下步骤将应用程序发布到 Azure：</span><span class="sxs-lookup"><span data-stu-id="e38b0-184">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="e38b0-185">在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。</span><span class="sxs-lookup"><span data-stu-id="e38b0-185">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="e38b0-186">出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="e38b0-186">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
   ![Azure 发布上下文菜单][06]

2. <span data-ttu-id="e38b0-188">如果尚未从 IntelliJ 登录到 Azure，系统会提示登录 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="e38b0-188">If you have not already signed in to Azure from IntelliJ, you will be prompted to sign in to your Azure account.</span></span> <span data-ttu-id="e38b0-189">（如果有多个 Azure 帐户，部分提示会在登录过程中显示多次，即使这些提示看起来是相同的。</span><span class="sxs-lookup"><span data-stu-id="e38b0-189">(If you have multiple Azure accounts, some of the prompts during the sign-in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="e38b0-190">发生此情况时，请继续遵循登录指示进行操作。）</span><span class="sxs-lookup"><span data-stu-id="e38b0-190">When this happens, continue to follow the sign-in instructions.)</span></span>
   
   ![对话框中的 Azure 日志][07]

3. <span data-ttu-id="e38b0-192">成功登录 Azure 帐户后，“管理订阅”对话框会显示与凭据关联的订阅的列表。</span><span class="sxs-lookup"><span data-stu-id="e38b0-192">After you have successfully signed in to your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="e38b0-193">（如果列出了多个订阅，而你只想使用其中几个订阅，可选择取消选中不想使用的订阅。）选择订阅后，单击“关闭”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-193">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
   ![管理订阅][08]

4. <span data-ttu-id="e38b0-195">当“部署到 Azure Web 应用容器”对话框出现时，它会显示前面创建的所有 Web 应用容器；如果尚未创建任何容器，列表将是空白的。</span><span class="sxs-lookup"><span data-stu-id="e38b0-195">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![应用容器][09]

5. <span data-ttu-id="e38b0-197">如果前面尚未创建 Azure Web 应用容器，或你想要将应用程序发布到新的容器，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="e38b0-197">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="e38b0-198">否则，请选择现有的 Web 应用容器，并跳到下面的步骤 6。</span><span class="sxs-lookup"><span data-stu-id="e38b0-198">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   <span data-ttu-id="e38b0-199">a.</span><span class="sxs-lookup"><span data-stu-id="e38b0-199">a.</span></span> <span data-ttu-id="e38b0-200">单击 + 号。</span><span class="sxs-lookup"><span data-stu-id="e38b0-200">Click the **+** sign.</span></span>
      
      ![添加应用容器][10]

   <span data-ttu-id="e38b0-202">b.</span><span class="sxs-lookup"><span data-stu-id="e38b0-202">b.</span></span> <span data-ttu-id="e38b0-203">此时会显示“新建 Web 应用容器”对话框，该对话框将用来进行接下来的几个步骤。</span><span class="sxs-lookup"><span data-stu-id="e38b0-203">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
      ![新建应用容器][11a]
   
   <span data-ttu-id="e38b0-205">c.</span><span class="sxs-lookup"><span data-stu-id="e38b0-205">c.</span></span> <span data-ttu-id="e38b0-206">为 Web 应用容器输入“DNS 标签”；这是在 Azure 中的 Web 应用程序构成主机 URL 的叶 DNS 标签。</span><span class="sxs-lookup"><span data-stu-id="e38b0-206">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="e38b0-207">请注意该名称必须是可用的，且符合 Azure Web 应用命名要求。</span><span class="sxs-lookup"><span data-stu-id="e38b0-207">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>

   <span data-ttu-id="e38b0-208">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-208">d.</span></span> <span data-ttu-id="e38b0-209">在“Web 容器”下拉菜单中，为应用程序选择适当的软件。</span><span class="sxs-lookup"><span data-stu-id="e38b0-209">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="e38b0-210">当前，可以从 Tomcat 8、Tomcat 7 或 Jetty 9 中选择。</span><span class="sxs-lookup"><span data-stu-id="e38b0-210">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="e38b0-211">Azure 将提供所选软件的最新分发版，并且该版本将基于由 JDK 8 创建并由 Azure 提供的 JDK 最新分发版运行。</span><span class="sxs-lookup"><span data-stu-id="e38b0-211">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="e38b0-212">e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-212">e.</span></span> <span data-ttu-id="e38b0-213">在“订阅”下拉菜单中，选择要用于此部署的订阅。</span><span class="sxs-lookup"><span data-stu-id="e38b0-213">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="e38b0-214">f.</span><span class="sxs-lookup"><span data-stu-id="e38b0-214">f.</span></span> <span data-ttu-id="e38b0-215">在“资源组”下拉菜单中，选择要与 Web 应用关联的资源组。</span><span class="sxs-lookup"><span data-stu-id="e38b0-215">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="e38b0-216">（使用 Azure 资源组可以将相关资源组织在一起，以便于将它们一起删除。）</span><span class="sxs-lookup"><span data-stu-id="e38b0-216">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="e38b0-217">可以选择现有资源组（如果有）并跳到下面的步骤 g，或者按照以下步骤创建新的资源组：</span><span class="sxs-lookup"><span data-stu-id="e38b0-217">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="e38b0-218">在“资源组”下拉菜单中选择“&lt;&lt;新建资源组&gt;&gt;”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-218">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="e38b0-219">此时会显示“新建资源组”对话框：</span><span class="sxs-lookup"><span data-stu-id="e38b0-219">The **New Resource Group** dialog box will be displayed:</span></span>
        
         ![新建资源组][12]

      * <span data-ttu-id="e38b0-221">在“名称”文本框中，为新的资源组指定名称。</span><span class="sxs-lookup"><span data-stu-id="e38b0-221">In the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="e38b0-222">在“区域”下拉菜单中，为资源组选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="e38b0-222">In the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="e38b0-223">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="e38b0-223">Click **OK**.</span></span>

   <span data-ttu-id="e38b0-224">g.</span><span class="sxs-lookup"><span data-stu-id="e38b0-224">g.</span></span> <span data-ttu-id="e38b0-225">“应用服务计划”下拉菜单列出了与选定资源组关联的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="e38b0-225">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="e38b0-226">（应用服务计划指定了 Web 应用的位置、定价层以及计算实例大小等信息。</span><span class="sxs-lookup"><span data-stu-id="e38b0-226">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="e38b0-227">单个应用服务计划可用于多个 Web 应用，这也是从特定 Web 应用部署中单独维护它的原因。）</span><span class="sxs-lookup"><span data-stu-id="e38b0-227">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
      <span data-ttu-id="e38b0-228">可以选择现有的应用服务计划（如果有）并跳到下面的步骤 h，或者按照以下步骤创建新的应用服务计划：</span><span class="sxs-lookup"><span data-stu-id="e38b0-228">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="e38b0-229">在“应用服务计划”下拉菜单中选择“&lt;&lt;创建新的应用服务计划&gt;&gt;”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-229">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="e38b0-230">此时会显示“新建应用服务计划”对话框：</span><span class="sxs-lookup"><span data-stu-id="e38b0-230">The **New App Service Plan** dialog box will be displayed:</span></span>
        
         ![新建应用服务计划][13]

      * <span data-ttu-id="e38b0-232">在“名称”文本框中，为新的应用服务计划指定名称。</span><span class="sxs-lookup"><span data-stu-id="e38b0-232">In the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="e38b0-233">在“位置”下拉菜单中，为计划选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="e38b0-233">In the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="e38b0-234">在“定价层”下拉菜单中，为计划选择适当的定价。</span><span class="sxs-lookup"><span data-stu-id="e38b0-234">In the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="e38b0-235">对于测试，可以选择“免费”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-235">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="e38b0-236">在“实例大小”下拉菜单中，为计划选择适当的实例大小。</span><span class="sxs-lookup"><span data-stu-id="e38b0-236">In the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="e38b0-237">对于测试，可以选择“小”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-237">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="e38b0-238">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="e38b0-238">Click **OK**.</span></span>

   <span data-ttu-id="e38b0-239">h.</span><span class="sxs-lookup"><span data-stu-id="e38b0-239">h.</span></span> <span data-ttu-id="e38b0-240">（可选）默认情况下，Azure 自动将最新的 Java 8 分发版作为 JVM 部署到 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="e38b0-240">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="e38b0-241">但是，可指定 JVM 的其他版本和分发版。</span><span class="sxs-lookup"><span data-stu-id="e38b0-241">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="e38b0-242">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="e38b0-242">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="e38b0-243">在“新建 Web 应用容器”对话框中单击“JDK”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-243">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="e38b0-244">可以选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="e38b0-244">You can choose from one of the following options:</span></span>
        
         * <span data-ttu-id="e38b0-245">部署 Azure 提供的默认 JDK</span><span class="sxs-lookup"><span data-stu-id="e38b0-245">Deploy the default JDK which is offered by Azure</span></span>
         * <span data-ttu-id="e38b0-246">可从 Azure 上提供的其他 JDK 下拉列表中部署第三方 JDK</span><span class="sxs-lookup"><span data-stu-id="e38b0-246">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
         * <span data-ttu-id="e38b0-247">部署自定义 JDK，必须将其打包为 ZIP 文件，且该 JDK 公开可用或位于 Azure 存储帐户中</span><span class="sxs-lookup"><span data-stu-id="e38b0-247">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
      ![“新建应用容器 JDK”选项卡][11b]

   <span data-ttu-id="e38b0-249">i.</span><span class="sxs-lookup"><span data-stu-id="e38b0-249">i.</span></span> <span data-ttu-id="e38b0-250">完成所有上述步骤之后，“新建 Web 应用容器”对话框看起来应如下图所示：</span><span class="sxs-lookup"><span data-stu-id="e38b0-250">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![新建应用容器][14]
   
   <span data-ttu-id="e38b0-252">j.</span><span class="sxs-lookup"><span data-stu-id="e38b0-252">j.</span></span> <span data-ttu-id="e38b0-253">单击“确定”完成创建新的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="e38b0-253">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="e38b0-254">等待 Web 应用容器列表刷新，这需要几秒，然后，新创建的 Web 应用容器应在列表中处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="e38b0-254">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

6. <span data-ttu-id="e38b0-255">现已准备好完成 Web 应用到 Azure 的初始部署；单击“确定”将 Java 应用程序部署到选定的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="e38b0-255">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="e38b0-256">默认情况下，应用程序部署为应用程序服务器的子目录。</span><span class="sxs-lookup"><span data-stu-id="e38b0-256">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="e38b0-257">如果想要将其部署为根应用程序，请选中“部署到根”复选框，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-257">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
   ![部署到 Azure][15]

7. <span data-ttu-id="e38b0-259">接下来，应会看到“Azure 活动日志”视图，其中指示了 Web 应用的部署状态。</span><span class="sxs-lookup"><span data-stu-id="e38b0-259">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![进度指示器][16]
   
   <span data-ttu-id="e38b0-261">将 Web 应用部署到 Azure 的过程只需几秒钟即可完成。</span><span class="sxs-lookup"><span data-stu-id="e38b0-261">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="e38b0-262">应用程序准备就绪后，可在“状态”列中看到名为“已发布”的链接。</span><span class="sxs-lookup"><span data-stu-id="e38b0-262">When your application is ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="e38b0-263">单击该链接时，它将转到已部署 Web 应用的主页；你也可以使用下一节中的步骤浏览到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-263">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

### <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="e38b0-264">浏览到 Azure 上的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-264">Browsing to your Web App on Azure</span></span>
<span data-ttu-id="e38b0-265">若要浏览到 Azure 上的 Web 应用，可以使用“Azure 资源管理器”视图。</span><span class="sxs-lookup"><span data-stu-id="e38b0-265">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="e38b0-266">如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。</span><span class="sxs-lookup"><span data-stu-id="e38b0-266">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="e38b0-267">如果事先未尚未登录，系统会提示登录。</span><span class="sxs-lookup"><span data-stu-id="e38b0-267">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="e38b0-268">显示“Azure 资源管理器”视图后，按以下步骤浏览到 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="e38b0-268">When the **Azure Explorer** view is displayed, follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="e38b0-269">展开“Azure”节点。</span><span class="sxs-lookup"><span data-stu-id="e38b0-269">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="e38b0-270">展开“Web 应用”节点。</span><span class="sxs-lookup"><span data-stu-id="e38b0-270">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="e38b0-271">右键单击所需的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-271">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="e38b0-272">出现上下文菜单时，单击“在浏览器中打开”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-272">When the context menu appears, click **Open in Browser**.</span></span>
   
   ![浏览 Web 应用][17]

### <a name="updating-your-web-app"></a><span data-ttu-id="e38b0-274">更新 Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-274">Updating your web app</span></span>
<span data-ttu-id="e38b0-275">更新现有运行中的 Azure Web 应用是一个快速而简单的过程，可以使用两个更新选项：</span><span class="sxs-lookup"><span data-stu-id="e38b0-275">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="e38b0-276">可以更新现有 Java Web 应用的部署。</span><span class="sxs-lookup"><span data-stu-id="e38b0-276">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="e38b0-277">可以将其他 Java 应用程序发布到同一个 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="e38b0-277">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="e38b0-278">对于任一情况，过程都是相同的，只需几秒钟即可完成：</span><span class="sxs-lookup"><span data-stu-id="e38b0-278">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="e38b0-279">在 IntelliJ 项目资源管理器中，右键单击要更新或添加到现有 Web 应用容器的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="e38b0-279">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="e38b0-280">出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="e38b0-280">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="e38b0-281">由于前面已登录，因此将看到现有 Web 应用容器的列表。</span><span class="sxs-lookup"><span data-stu-id="e38b0-281">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="e38b0-282">选择要对其发布或重新发布 Java 应用程序的 Web 应用容器，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-282">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="e38b0-283">几秒钟后，“Azure 活动日志”视图会将已更新的部署显示为“已发布”，可以在 Web 浏览器中验证已更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="e38b0-283">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

### <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="e38b0-284">启动、停止或重启现有的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="e38b0-284">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="e38b0-285">若要启动或停止现有的 Azure Web 应用容器（包括其中所有已部署的 Java 应用程序），可以使用“Azure 资源管理器”视图。</span><span class="sxs-lookup"><span data-stu-id="e38b0-285">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="e38b0-286">如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。</span><span class="sxs-lookup"><span data-stu-id="e38b0-286">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="e38b0-287">如果事先未尚未登录，系统会提示登录。</span><span class="sxs-lookup"><span data-stu-id="e38b0-287">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="e38b0-288">显示“Azure 资源管理器”视图后，按以下步骤启动或停止 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="e38b0-288">When the **Azure Explorer** view is displayed, follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="e38b0-289">展开“Azure”节点。</span><span class="sxs-lookup"><span data-stu-id="e38b0-289">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="e38b0-290">展开“Web 应用”节点。</span><span class="sxs-lookup"><span data-stu-id="e38b0-290">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="e38b0-291">右键单击所需的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-291">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="e38b0-292">出现上下文菜单时，单击“启动”、“停止”或“重启”。</span><span class="sxs-lookup"><span data-stu-id="e38b0-292">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="e38b0-293">请注意，菜单选项可识别上下文，因此，只能停止正在运行的 Web 应用，或启动当前未运行的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="e38b0-293">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![停止 Web 应用][18]

## <a name="next-steps"></a><span data-ttu-id="e38b0-295">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e38b0-295">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="e38b0-296">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="e38b0-296">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包]: azure-toolkit-for-intellij.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/azure-toolkit-for-intellij-create-hello-world-web-app/18-Stop-Web-App.png

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
