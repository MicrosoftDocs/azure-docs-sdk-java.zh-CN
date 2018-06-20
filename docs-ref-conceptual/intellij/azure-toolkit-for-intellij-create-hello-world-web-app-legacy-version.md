---
title: 使用用于 IntelliJ 的旧工具包创建适用于 Azure 的 Hello World Web 应用
description: 本教程说明如何使用用于 IntelliJ 的 Azure 工具包 3.0.6（或更低版本）创建适用于 Azure 的 Hello World Web 应用。
services: app-service
documentationcenter: java
author: selvasingh
manager: routlaw
editor: ''
ms.assetid: ''
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
ms.locfileid: "28954778"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-intellij"></a><span data-ttu-id="147bc-103">使用用于 IntelliJ 的旧工具包创建适用于 Azure 的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="147bc-103">Create a Hello World web app for Azure using the legacy toolkit for IntelliJ</span></span>

<span data-ttu-id="147bc-104">本教程说明如何使用[用于 IntelliJ 的 Azure 工具包] 3.0.6（或更低版本）创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using version 3.0.6 (or earlier) of the [Azure Toolkit for IntelliJ].</span></span>

> [!NOTE]
>
> <span data-ttu-id="147bc-105">如需使用[用于 Eclipse 的 Azure 工具包]的本文版本，请参阅[使用 Eclipse 创建适用于 Azure 的 Hello World Web 应用][eclipse-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="147bc-105">For a version of this article that uses the [Azure Toolkit for Eclipse], see [Create a Hello World web app for Azure using Eclipse][eclipse-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="147bc-106">用于 IntelliJ 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。</span><span class="sxs-lookup"><span data-stu-id="147bc-106">The Azure Toolkit for IntelliJ was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="147bc-107">本文详述如何使用用于 IntelliJ 的 Azure 工具包 3.0.6（或更低版本）创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-107">This article illustrates creating a Hello World web app by using version 3.0.6 (or earlier) of the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="147bc-108">如果使用的是工具包 3.0.7（或更高版本），则需执行[在 IntelliJ 中创建适用于 Azure 的 Hello World Web 应用][Updated Version]中的步骤。</span><span class="sxs-lookup"><span data-stu-id="147bc-108">If you are using the version 3.0.7 (or later) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in IntelliJ][Updated Version].</span></span>
>

<span data-ttu-id="147bc-109">完成本教程后，应用程序会在 Web 浏览器中如下图所示：</span><span class="sxs-lookup"><span data-stu-id="147bc-109">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 应用预览][01]

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="147bc-111">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="147bc-111">Create a new web app project</span></span>

1. <span data-ttu-id="147bc-112">启动 IntelliJ，然后根据[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明][intelliJ-sign-in-instructions]一文中的说明登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="147bc-112">Start IntelliJ, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for IntelliJ][intelliJ-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="147bc-113">依次单击“文件”菜单、“新建”、“项目”。</span><span class="sxs-lookup"><span data-stu-id="147bc-113">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![文件新建项目][02]

2. <span data-ttu-id="147bc-115">在“新建项目”对话框中，依次选择 Java 和“Web 应用程序”，并单击“新建”以添加项目 SDK。</span><span class="sxs-lookup"><span data-stu-id="147bc-115">In the **New Project** dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
   ![“新建项目”对话框][03a]
   
3. <span data-ttu-id="147bc-117">在“选择 JDK 的主目录”对话框中，选择要安装 JDK 的文件夹，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="147bc-117">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="147bc-118">在“新建项目”对话框中单击“下一步”以继续。</span><span class="sxs-lookup"><span data-stu-id="147bc-118">Click **Next** in the New Project dialog box to continue.</span></span>
   
   ![指定 JDK 主目录][03b]

4. <span data-ttu-id="147bc-120">基于本教程的目的，将项目命名为 **Java-Web-App-On-Azure**，然后单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="147bc-120">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
   ![“新建项目”对话框][04]

5. <span data-ttu-id="147bc-122">在 IntelliJ 的项目资源管理器视图中，依次展开“Java-Web-App-On-Azure”和“Web”，并双击“index.jsp”。</span><span class="sxs-lookup"><span data-stu-id="147bc-122">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
   ![打开索引页面][05c]

6. <span data-ttu-id="147bc-124">在 IntelliJ 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="147bc-124">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="147bc-125">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="147bc-125">within the existing `<body>` element.</span></span> <span data-ttu-id="147bc-126">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="147bc-126">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```java
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

7. <span data-ttu-id="147bc-127">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="147bc-127">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="147bc-128">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="147bc-128">Deploy your web app to Azure</span></span>

<span data-ttu-id="147bc-129">有多种方式可将 Java Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="147bc-129">There are several ways by which you can deploy a Java web app to Azure.</span></span> <span data-ttu-id="147bc-130">本教程说明其中一个最简单的方式：将应用程序部署到 Azure Web 应用容器，无需特殊的项目类型或额外的工具。</span><span class="sxs-lookup"><span data-stu-id="147bc-130">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="147bc-131">Azure 为提供 JDK 及 Web 容器软件，因此不需要自己上传；只需要 Java Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-131">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="147bc-132">这样，应用程序的发布过程只需数秒，连一分钟都不用。</span><span class="sxs-lookup"><span data-stu-id="147bc-132">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="147bc-133">在发布应用程序之前，需要先配置模块设置。</span><span class="sxs-lookup"><span data-stu-id="147bc-133">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="147bc-134">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="147bc-134">To do so, use the following steps:</span></span>

1. <span data-ttu-id="147bc-135">在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。</span><span class="sxs-lookup"><span data-stu-id="147bc-135">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="147bc-136">出现上下文菜单时，单击“打开模块设置”。</span><span class="sxs-lookup"><span data-stu-id="147bc-136">When the context menu appears, click **Open Module Settings**.</span></span>

   ![打开模块设置][05a]

2. <span data-ttu-id="147bc-138">“项目结构”对话框出现后：</span><span class="sxs-lookup"><span data-stu-id="147bc-138">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="147bc-139">a.</span><span class="sxs-lookup"><span data-stu-id="147bc-139">a.</span></span> <span data-ttu-id="147bc-140">单击“项目设置”列表中的“项目”。</span><span class="sxs-lookup"><span data-stu-id="147bc-140">Click **Artifacts** in the list of **Project Settings**.</span></span>

   <span data-ttu-id="147bc-141">b.</span><span class="sxs-lookup"><span data-stu-id="147bc-141">b.</span></span> <span data-ttu-id="147bc-142">更改“名称”框中的项目名称，使其不包含空格或特殊字符；这是必要步骤，因为该名称会在统一资源标识符 (URI) 中使用。</span><span class="sxs-lookup"><span data-stu-id="147bc-142">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>

   <span data-ttu-id="147bc-143">c.</span><span class="sxs-lookup"><span data-stu-id="147bc-143">c.</span></span> <span data-ttu-id="147bc-144">将“类型”更改为“Web 应用程序: 存档”。</span><span class="sxs-lookup"><span data-stu-id="147bc-144">Change the **Type** to **Web Application: Archive**.</span></span>

   <span data-ttu-id="147bc-145">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="147bc-145">d.</span></span> <span data-ttu-id="147bc-146">单击“确定”关闭“项目结构”对话框。</span><span class="sxs-lookup"><span data-stu-id="147bc-146">Click **OK** to close the Project Structure dialog box.</span></span>

   ![打开模块设置][05b]

<span data-ttu-id="147bc-148">配置模块设置后，可以通过执行以下步骤将应用程序发布到 Azure：</span><span class="sxs-lookup"><span data-stu-id="147bc-148">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="147bc-149">在 IntelliJ 的项目资源管理器中，右键单击 **Java-Web-App-On-Azure** 项目。</span><span class="sxs-lookup"><span data-stu-id="147bc-149">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="147bc-150">出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="147bc-150">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
   ![Azure 发布上下文菜单][06]

2. <span data-ttu-id="147bc-152">如果尚未从 IntelliJ 登录到 Azure，系统会提示登录 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="147bc-152">If you have not already signed in to Azure from IntelliJ, you will be prompted to sign in to your Azure account.</span></span> <span data-ttu-id="147bc-153">（如果有多个 Azure 帐户，部分提示会在登录过程中显示多次，即使这些提示看起来是相同的。</span><span class="sxs-lookup"><span data-stu-id="147bc-153">(If you have multiple Azure accounts, some of the prompts during the sign-in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="147bc-154">发生此情况时，请继续遵循登录指示进行操作。）</span><span class="sxs-lookup"><span data-stu-id="147bc-154">When this happens, continue to follow the sign-in instructions.)</span></span>
   
   ![对话框中的 Azure 日志][07]

3. <span data-ttu-id="147bc-156">成功登录 Azure 帐户后，“管理订阅”对话框会显示与凭据关联的订阅的列表。</span><span class="sxs-lookup"><span data-stu-id="147bc-156">After you have successfully signed in to your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="147bc-157">（如果列出了多个订阅，而你只想使用其中几个订阅，可选择取消选中不想使用的订阅。）选择订阅后，单击“关闭”。</span><span class="sxs-lookup"><span data-stu-id="147bc-157">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
   ![管理订阅][08]

4. <span data-ttu-id="147bc-159">当“部署到 Azure Web 应用容器”对话框出现时，它会显示前面创建的所有 Web 应用容器；如果尚未创建任何容器，列表将是空白的。</span><span class="sxs-lookup"><span data-stu-id="147bc-159">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![应用容器][09]

5. <span data-ttu-id="147bc-161">如果前面尚未创建 Azure Web 应用容器，或你想要将应用程序发布到新的容器，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="147bc-161">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="147bc-162">否则，请选择现有的 Web 应用容器，并跳到下面的步骤 6。</span><span class="sxs-lookup"><span data-stu-id="147bc-162">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   <span data-ttu-id="147bc-163">a.</span><span class="sxs-lookup"><span data-stu-id="147bc-163">a.</span></span> <span data-ttu-id="147bc-164">单击 + 号。</span><span class="sxs-lookup"><span data-stu-id="147bc-164">Click the **+** sign.</span></span>
      
      ![添加应用容器][10]

   <span data-ttu-id="147bc-166">b.</span><span class="sxs-lookup"><span data-stu-id="147bc-166">b.</span></span> <span data-ttu-id="147bc-167">此时会显示“新建 Web 应用容器”对话框，该对话框将用来进行接下来的几个步骤。</span><span class="sxs-lookup"><span data-stu-id="147bc-167">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
      ![新建应用容器][11a]
   
   <span data-ttu-id="147bc-169">c.</span><span class="sxs-lookup"><span data-stu-id="147bc-169">c.</span></span> <span data-ttu-id="147bc-170">为 Web 应用容器输入“DNS 标签”；这是在 Azure 中的 Web 应用程序构成主机 URL 的叶 DNS 标签。</span><span class="sxs-lookup"><span data-stu-id="147bc-170">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="147bc-171">请注意该名称必须是可用的，且符合 Azure Web 应用命名要求。</span><span class="sxs-lookup"><span data-stu-id="147bc-171">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>

   <span data-ttu-id="147bc-172">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="147bc-172">d.</span></span> <span data-ttu-id="147bc-173">在“Web 容器”下拉菜单中，为应用程序选择适当的软件。</span><span class="sxs-lookup"><span data-stu-id="147bc-173">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="147bc-174">当前，可以从 Tomcat 8、Tomcat 7 或 Jetty 9 中选择。</span><span class="sxs-lookup"><span data-stu-id="147bc-174">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="147bc-175">Azure 将提供所选软件的最新分发版，并且该版本将基于由 JDK 8 创建并由 Azure 提供的 JDK 最新分发版运行。</span><span class="sxs-lookup"><span data-stu-id="147bc-175">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>

   <span data-ttu-id="147bc-176">e.</span><span class="sxs-lookup"><span data-stu-id="147bc-176">e.</span></span> <span data-ttu-id="147bc-177">在“订阅”下拉菜单中，选择要用于此部署的订阅。</span><span class="sxs-lookup"><span data-stu-id="147bc-177">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="147bc-178">f.</span><span class="sxs-lookup"><span data-stu-id="147bc-178">f.</span></span> <span data-ttu-id="147bc-179">在“资源组”下拉菜单中，选择要与 Web 应用关联的资源组。</span><span class="sxs-lookup"><span data-stu-id="147bc-179">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="147bc-180">（使用 Azure 资源组可以将相关资源组织在一起，以便于将它们一起删除。）</span><span class="sxs-lookup"><span data-stu-id="147bc-180">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="147bc-181">可以选择现有资源组（如果有）并跳到下面的步骤 g，或者按照以下步骤创建新的资源组：</span><span class="sxs-lookup"><span data-stu-id="147bc-181">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="147bc-182">在“资源组”下拉菜单中选择“&lt;&lt;新建资源组&gt;&gt;”。</span><span class="sxs-lookup"><span data-stu-id="147bc-182">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="147bc-183">此时会显示“新建资源组”对话框：</span><span class="sxs-lookup"><span data-stu-id="147bc-183">The **New Resource Group** dialog box will be displayed:</span></span>
        
         ![新建资源组][12]

      * <span data-ttu-id="147bc-185">在“名称”文本框中，为新的资源组指定名称。</span><span class="sxs-lookup"><span data-stu-id="147bc-185">In the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="147bc-186">在“区域”下拉菜单中，为资源组选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="147bc-186">In the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="147bc-187">单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="147bc-187">Click **OK**.</span></span>

   <span data-ttu-id="147bc-188">g.</span><span class="sxs-lookup"><span data-stu-id="147bc-188">g.</span></span> <span data-ttu-id="147bc-189">“应用服务计划”下拉菜单列出了与选定资源组关联的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="147bc-189">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="147bc-190">（应用服务计划指定了 Web 应用的位置、定价层以及计算实例大小等信息。</span><span class="sxs-lookup"><span data-stu-id="147bc-190">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="147bc-191">单个应用服务计划可用于多个 Web 应用，这也是从特定 Web 应用部署中单独维护它的原因。）</span><span class="sxs-lookup"><span data-stu-id="147bc-191">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
      <span data-ttu-id="147bc-192">可以选择现有的应用服务计划（如果有）并跳到下面的步骤 h，或者按照以下步骤创建新的应用服务计划：</span><span class="sxs-lookup"><span data-stu-id="147bc-192">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="147bc-193">在“应用服务计划”下拉菜单中选择“&lt;&lt;创建新的应用服务计划&gt;&gt;”。</span><span class="sxs-lookup"><span data-stu-id="147bc-193">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="147bc-194">此时会显示“新建应用服务计划”对话框：</span><span class="sxs-lookup"><span data-stu-id="147bc-194">The **New App Service Plan** dialog box will be displayed:</span></span>
        
         ![新建应用服务计划][13]

      * <span data-ttu-id="147bc-196">在“名称”文本框中，为新的应用服务计划指定名称。</span><span class="sxs-lookup"><span data-stu-id="147bc-196">In the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="147bc-197">在“位置”下拉菜单中，为计划选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="147bc-197">In the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="147bc-198">在“定价层”下拉菜单中，为计划选择适当的定价。</span><span class="sxs-lookup"><span data-stu-id="147bc-198">In the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="147bc-199">对于测试，可以选择“免费”。</span><span class="sxs-lookup"><span data-stu-id="147bc-199">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="147bc-200">在“实例大小”下拉菜单中，为计划选择适当的实例大小。</span><span class="sxs-lookup"><span data-stu-id="147bc-200">In the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="147bc-201">对于测试，可以选择“小”。</span><span class="sxs-lookup"><span data-stu-id="147bc-201">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="147bc-202">单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="147bc-202">Click **OK**.</span></span>

   <span data-ttu-id="147bc-203">h.</span><span class="sxs-lookup"><span data-stu-id="147bc-203">h.</span></span> <span data-ttu-id="147bc-204">（可选）默认情况下，Azure 自动将最新的 Java 8 分发版作为 JVM 部署到 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="147bc-204">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="147bc-205">但是，可指定 JVM 的其他版本和分发版。</span><span class="sxs-lookup"><span data-stu-id="147bc-205">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="147bc-206">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="147bc-206">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="147bc-207">在“新建 Web 应用容器”对话框中单击“JDK”。</span><span class="sxs-lookup"><span data-stu-id="147bc-207">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="147bc-208">可以选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="147bc-208">You can choose from one of the following options:</span></span>
        
         * <span data-ttu-id="147bc-209">部署 Azure 提供的默认 JDK</span><span class="sxs-lookup"><span data-stu-id="147bc-209">Deploy the default JDK which is offered by Azure</span></span>
         * <span data-ttu-id="147bc-210">可从 Azure 上提供的其他 JDK 下拉列表中部署第三方 JDK</span><span class="sxs-lookup"><span data-stu-id="147bc-210">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
         * <span data-ttu-id="147bc-211">部署自定义 JDK，必须将其打包为 ZIP 文件，且该 JDK 公开可用或位于 Azure 存储帐户中</span><span class="sxs-lookup"><span data-stu-id="147bc-211">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
      ![“新建应用容器 JDK”选项卡][11b]

   <span data-ttu-id="147bc-213">i.</span><span class="sxs-lookup"><span data-stu-id="147bc-213">i.</span></span> <span data-ttu-id="147bc-214">完成所有上述步骤之后，“新建 Web 应用容器”对话框看起来应如下图所示：</span><span class="sxs-lookup"><span data-stu-id="147bc-214">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![新建应用容器][14]
   
   <span data-ttu-id="147bc-216">j.</span><span class="sxs-lookup"><span data-stu-id="147bc-216">j.</span></span> <span data-ttu-id="147bc-217">单击“确定”完成创建新的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="147bc-217">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="147bc-218">等待 Web 应用容器列表刷新，这需要几秒，然后，新创建的 Web 应用容器应在列表中处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="147bc-218">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

6. <span data-ttu-id="147bc-219">现已准备好完成 Web 应用到 Azure 的初始部署；单击“确定”将 Java 应用程序部署到选定的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="147bc-219">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="147bc-220">默认情况下，应用程序部署为应用程序服务器的子目录。</span><span class="sxs-lookup"><span data-stu-id="147bc-220">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="147bc-221">如果想要将其部署为根应用程序，请选中“部署到根”复选框，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="147bc-221">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
   ![部署到 Azure][15]

7. <span data-ttu-id="147bc-223">接下来，应会看到“Azure 活动日志”视图，其中指示了 Web 应用的部署状态。</span><span class="sxs-lookup"><span data-stu-id="147bc-223">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![进度指示器][16]
   
   <span data-ttu-id="147bc-225">将 Web 应用部署到 Azure 的过程只需几秒钟即可完成。</span><span class="sxs-lookup"><span data-stu-id="147bc-225">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="147bc-226">应用程序准备就绪后，可在“状态”列中看到名为“已发布”的链接。</span><span class="sxs-lookup"><span data-stu-id="147bc-226">When your application is ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="147bc-227">单击该链接时，它将转到已部署 Web 应用的主页；你也可以使用下一节中的步骤浏览到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-227">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

## <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="147bc-228">浏览到 Azure 上的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="147bc-228">Browsing to your Web App on Azure</span></span>

<span data-ttu-id="147bc-229">若要浏览到 Azure 上的 Web 应用，可以使用“Azure 资源管理器”视图。</span><span class="sxs-lookup"><span data-stu-id="147bc-229">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="147bc-230">如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。</span><span class="sxs-lookup"><span data-stu-id="147bc-230">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="147bc-231">如果事先未尚未登录，系统会提示登录。</span><span class="sxs-lookup"><span data-stu-id="147bc-231">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="147bc-232">显示“Azure 资源管理器”视图后，按以下步骤浏览到 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="147bc-232">When the **Azure Explorer** view is displayed, follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="147bc-233">展开“Azure”节点。</span><span class="sxs-lookup"><span data-stu-id="147bc-233">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="147bc-234">展开“Web 应用”节点。</span><span class="sxs-lookup"><span data-stu-id="147bc-234">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="147bc-235">右键单击所需的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-235">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="147bc-236">出现上下文菜单时，单击“在浏览器中打开”。</span><span class="sxs-lookup"><span data-stu-id="147bc-236">When the context menu appears, click **Open in Browser**.</span></span>
   
   ![浏览 Web 应用][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="147bc-238">更新 Web 应用</span><span class="sxs-lookup"><span data-stu-id="147bc-238">Updating your web app</span></span>

<span data-ttu-id="147bc-239">更新现有运行中的 Azure Web 应用是一个快速而简单的过程，可以使用两个更新选项：</span><span class="sxs-lookup"><span data-stu-id="147bc-239">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="147bc-240">可以更新现有 Java Web 应用的部署。</span><span class="sxs-lookup"><span data-stu-id="147bc-240">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="147bc-241">可以将其他 Java 应用程序发布到同一个 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="147bc-241">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="147bc-242">对于任一情况，过程都是相同的，只需几秒钟即可完成：</span><span class="sxs-lookup"><span data-stu-id="147bc-242">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="147bc-243">在 IntelliJ 项目资源管理器中，右键单击要更新或添加到现有 Web 应用容器的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="147bc-243">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="147bc-244">出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="147bc-244">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="147bc-245">由于前面已登录，因此将看到现有 Web 应用容器的列表。</span><span class="sxs-lookup"><span data-stu-id="147bc-245">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="147bc-246">选择要对其发布或重新发布 Java 应用程序的 Web 应用容器，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="147bc-246">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="147bc-247">几秒钟后，“Azure 活动日志”视图会将已更新的部署显示为“已发布”，可以在 Web 浏览器中验证已更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="147bc-247">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="147bc-248">启动、停止或重启现有的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="147bc-248">Starting, stopping, or restarting an existing web app</span></span>

<span data-ttu-id="147bc-249">若要启动或停止现有的 Azure Web 应用容器（包括其中所有已部署的 Java 应用程序），可以使用“Azure 资源管理器”视图。</span><span class="sxs-lookup"><span data-stu-id="147bc-249">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="147bc-250">如果“Azure 资源管理器”视图尚未打开，可以依次单击 IntelliJ 中的“视图”菜单、“工具窗口”和“服务资源管理器”将它打开。</span><span class="sxs-lookup"><span data-stu-id="147bc-250">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="147bc-251">如果事先未尚未登录，系统会提示登录。</span><span class="sxs-lookup"><span data-stu-id="147bc-251">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="147bc-252">显示“Azure 资源管理器”视图后，按以下步骤启动或停止 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="147bc-252">When the **Azure Explorer** view is displayed, follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="147bc-253">展开“Azure”节点。</span><span class="sxs-lookup"><span data-stu-id="147bc-253">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="147bc-254">展开“Web 应用”节点。</span><span class="sxs-lookup"><span data-stu-id="147bc-254">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="147bc-255">右键单击所需的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-255">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="147bc-256">出现上下文菜单时，单击“启动”、“停止”或“重启”。</span><span class="sxs-lookup"><span data-stu-id="147bc-256">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="147bc-257">请注意，菜单选项可识别上下文，因此，只能停止正在运行的 Web 应用，或启动当前未运行的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="147bc-257">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![停止 Web 应用][18]

## <a name="next-steps"></a><span data-ttu-id="147bc-259">后续步骤</span><span class="sxs-lookup"><span data-stu-id="147bc-259">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<span data-ttu-id="147bc-260">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="147bc-260">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

<!-- URL List -->

[用于 IntelliJ 的 Azure 工具包]: azure-toolkit-for-intellij.md
[Azure Toolkit for IntelliJ]: azure-toolkit-for-intellij.md
[用于 Eclipse 的 Azure 工具包]: ../eclipse/azure-toolkit-for-eclipse.md
[Azure Toolkit for Eclipse]: ../eclipse/azure-toolkit-for-eclipse.md
[eclipse-hello-world]: ../eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app.md
[Web 应用概述]: /azure/app-service/app-service-web-overview
[Web Apps Overview]: /azure/app-service/app-service-web-overview
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
