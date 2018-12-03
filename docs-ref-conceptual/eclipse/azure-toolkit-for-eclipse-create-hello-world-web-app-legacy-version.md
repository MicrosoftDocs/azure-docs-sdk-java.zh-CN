---
title: ''
description: 本教程说明如何使用用于 Eclipse 的 Azure 工具包 3.0.6（或更低版本）创建适用于 Azure 的 Hello World Web 应用。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/13/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: b05dcd52f36524ab17652f83c6ced4006f874365
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338711"
---
# <a name="create-a-hello-world-web-app-for-azure-using-the-legacy-toolkit-for-eclipse"></a><span data-ttu-id="bd4fc-102">使用用于 Eclipse 的旧工具包创建适用于 Azure 的 Hello World Web 应用</span><span class="sxs-lookup"><span data-stu-id="bd4fc-102">Create a Hello World web app for Azure using the legacy toolkit for Eclipse</span></span>

<span data-ttu-id="bd4fc-103">本教程说明如何使用[用于 Eclipse 的 Azure 工具包] 3.0.6（或更低版本）创建一个基本的 Hello World 应用程序，并将其部署到 Azure 作为 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-103">This tutorial shows how to create and deploy a basic Hello World application to Azure as a web app by using version 3.0.6 (or earlier) of the [Azure Toolkit for Eclipse].</span></span>

> [!NOTE]
>
> <span data-ttu-id="bd4fc-104">如需使用[用于 IntelliJ 的 Azure 工具包]的本文版本，请参阅[使用 IntelliJ 创建适用于 Azure 的 Hello World Web 应用][intellij-hello-world]。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-104">For a version of this article that uses the [Azure Toolkit for IntelliJ], see [Create a Hello World web app for Azure using IntelliJ][intellij-hello-world].</span></span>
>

> [!IMPORTANT]
> 
> <span data-ttu-id="bd4fc-105">用于 Eclipse 的 Azure 工具包于 2017 年 8 月进行了更新，采用了不同的工作流。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-105">The Azure Toolkit for Eclipse was updated in August 2017 with a different workflow.</span></span> <span data-ttu-id="bd4fc-106">本文详述如何使用用于 Eclipse 的 Azure 工具包 3.0.6（或更低版本）创建 Hello World Web 应用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-106">This article illustrates creating a Hello World web app by using version 3.0.6 (or earlier) of the Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="bd4fc-107">如果使用的是工具包 3.0.7（或更高版本），则需执行[在 Eclipse 中创建适用于 Azure 的 Hello World Web 应用][Updated Version]中的步骤。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-107">If you are using the version 3.0.7 (or later) of the toolkit, you will need to follow the steps in [Create a Hello World web app for Azure in Eclipse][Updated Version].</span></span>
>

<span data-ttu-id="bd4fc-108">完成本教程后，应用程序会在 Web 浏览器中如下图所示：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-108">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Hello World 应用预览][01]

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="bd4fc-110">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="bd4fc-110">Create a new web app project</span></span>

1. <span data-ttu-id="bd4fc-111">启动 Eclipse，然后根据[用于 Eclipse 的 Azure 工具包的 Azure 登录说明][eclipse-sign-in-instructions]一文中的说明登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-111">Start Eclipse, and sign into your Azure account by using the instructions in the [Azure Sign In Instructions for the Azure Toolkit for Eclipse][eclipse-sign-in-instructions] article.</span></span>

1. <span data-ttu-id="bd4fc-112">依次单击“文件”、“新建”和“动态 Web 项目”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-112">Click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="bd4fc-113">（如果在单击“文件”、“新建”后未看到“动态 Web 项目”作为可用项目列出，则执行以下操作：依次单击“文件”、“新建”、“项目...”，展开“Web”，单击“动态 Web 项目”，并单击“下一步”。）</span><span class="sxs-lookup"><span data-stu-id="bd4fc-113">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>

2. <span data-ttu-id="bd4fc-114">在本教程中，项目命名为 **MyWebApp**。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-114">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="bd4fc-115">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-115">Your screen will appear similar to the following:</span></span>
   
   ![创建新的动态 Web 项目][02]

3. <span data-ttu-id="bd4fc-117">单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-117">Click **Finish**.</span></span>

4. <span data-ttu-id="bd4fc-118">在 Eclipse 的“项目资源管理器”视图中，展开“MyWebApp”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-118">Within Eclipse's **Project Explorer** view, expand **MyWebApp**.</span></span> <span data-ttu-id="bd4fc-119">右键单击“WebContent”，单击“新建”，并单击“JSP 文件”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-119">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>

5. <span data-ttu-id="bd4fc-120">在“新建 JSP 文件”对话框中，将文件命名为“index.jsp”，将父文件夹保留为“MyWebApp/WebContent”，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-120">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>

6. <span data-ttu-id="bd4fc-121">基于本教程的目的，在“选择 JSP 模板”对话框中选择“新建 JSP 文件(html)”，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-121">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>

7. <span data-ttu-id="bd4fc-122">在 Eclipse 中打开 index.jsp 文件后，添加文本以将 **Hello World!** 动态显示</span><span class="sxs-lookup"><span data-stu-id="bd4fc-122">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="bd4fc-123">在现有 `<body>` 元素中。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-123">within the existing `<body>` element.</span></span> <span data-ttu-id="bd4fc-124">更新后的 `<body>` 内容应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-124">Your updated `<body>` content should resemble the following example:</span></span>
   
   ```jsp
   <body><b><% out.println("Hello World!"); %></b></body>
   ```

8. <span data-ttu-id="bd4fc-125">保存 index.jsp。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-125">Save index.jsp.</span></span>

## <a name="deploy-your-web-app-to-azure"></a><span data-ttu-id="bd4fc-126">将 Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="bd4fc-126">Deploy your web app to Azure</span></span>

<span data-ttu-id="bd4fc-127">有多种方式可以将 Java Web 应用程序部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-127">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="bd4fc-128">本教程说明其中一个最简单的方式：将应用程序部署到 Azure Web 应用容器，无需特殊的项目类型或额外的工具。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-128">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="bd4fc-129">Azure 为提供 JDK 及 Web 容器软件，因此不需要自己上传；只需要 Java Web 应用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-129">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="bd4fc-130">这样，应用程序的发布过程只需数秒，连一分钟都不用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-130">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="bd4fc-131">在 Eclipse 的项目资源管理器中，右键单击“MyWebApp”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-131">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>

2. <span data-ttu-id="bd4fc-132">在上下文菜单中选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="bd4fc-132">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
   ![发布为 Azure Web 应用][03]
   
   <span data-ttu-id="bd4fc-134">另外，如果已在项目资源管理器中选择 Web 应用程序项目，则可单击工具栏中的“发布”下拉按钮，并从该处选择“发布为 Azure Web 应用”：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-134">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
   ![发布为 Azure Web 应用][14]

3. <span data-ttu-id="bd4fc-136">如果尚未从 Eclipse 登录到 Azure 中，系统会提示登录 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-136">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
   ![Azure 登录对话框][04]
   
   <span data-ttu-id="bd4fc-138">如果有多个 Azure 帐户，部分提示会在登录过程中显示多次，即使这些提示看起来是相同的。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-138">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="bd4fc-139">发生此情况时，请遵循登录指示继续操作。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-139">When this happens, continue following the sign in instructions.</span></span>

4. <span data-ttu-id="bd4fc-140">在成功登录 Azure 帐户后，“管理订阅”对话框会显示与凭据关联的订阅列表。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-140">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="bd4fc-141">如果列出了多个订阅，而你只想使用其中几个帐户，可以选择取消选中要使用的订阅。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-141">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="bd4fc-142">选择订阅后，单击“关闭”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-142">When you have selected your subscriptions, click **Close**.</span></span>
   
   ![管理订阅对话框][05]

5. <span data-ttu-id="bd4fc-144">当“部署到 Azure Web 应用容器”对话框出现时，它会显示前面创建的所有 Web 应用容器；如果尚未创建任何容器，列表将是空白的。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-144">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
   ![部署到 Azure Web 应用容器对话框][06]

6. <span data-ttu-id="bd4fc-146">如果前面尚未创建 Azure Web 应用容器，或你想要将应用程序发布到新的容器，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-146">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="bd4fc-147">否则，请选择现有的 Web 应用容器，并跳到下面的步骤 7。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-147">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   <span data-ttu-id="bd4fc-148">a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-148">a.</span></span> <span data-ttu-id="bd4fc-149">单击“新建...”</span><span class="sxs-lookup"><span data-stu-id="bd4fc-149">Click **New...**</span></span>
      
      ![部署到 Azure Web 应用容器对话框][15]

   <span data-ttu-id="bd4fc-151">b.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-151">b.</span></span> <span data-ttu-id="bd4fc-152">此时会显示“新建 Web 应用容器”对话框：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-152">The **New Web App Container** dialog box will be displayed:</span></span>
      
      ![新建 Web 应用容器对话框][07a]

   <span data-ttu-id="bd4fc-154">c.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-154">c.</span></span> <span data-ttu-id="bd4fc-155">为 Web 应用容器输入“DNS 标签”；这是在 Azure 中的 Web 应用程序构成主机 URL 的叶 DNS 标签。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-155">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="bd4fc-156">（请注意该名称必须可用，且符合 Azure Web 应用命名要求。）</span><span class="sxs-lookup"><span data-stu-id="bd4fc-156">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>

   <span data-ttu-id="bd4fc-157">d.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-157">d.</span></span> <span data-ttu-id="bd4fc-158">在“Web 容器”下拉菜单中，为应用程序选择适当的软件。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-158">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
      <span data-ttu-id="bd4fc-159">当前，可以从 Tomcat 8、Tomcat 7 或 Jetty 9 中选择。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-159">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="bd4fc-160">Azure 将提供所选软件的最新分发版，并且该版本将基于由 Azure 提供的 JDK 最新分发版运行。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-160">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of the JDK provided by Azure.</span></span>

   <span data-ttu-id="bd4fc-161">e.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-161">e.</span></span> <span data-ttu-id="bd4fc-162">在“订阅”下拉菜单中，选择要用于此部署的订阅。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-162">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>

   <span data-ttu-id="bd4fc-163">f.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-163">f.</span></span> <span data-ttu-id="bd4fc-164">在“资源组”下拉菜单中，选择要与 Web 应用关联的资源组。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-164">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="bd4fc-165">（使用 Azure 资源组可以将相关资源组织在一起，以便于将它们一起删除。）</span><span class="sxs-lookup"><span data-stu-id="bd4fc-165">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
      <span data-ttu-id="bd4fc-166">可以选择现有资源组（如果有）并跳到下面的步骤 g，或者按照以下这些步骤创建新的资源组：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-166">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
   * <span data-ttu-id="bd4fc-167">单击“新建...”</span><span class="sxs-lookup"><span data-stu-id="bd4fc-167">Click **New...**</span></span>
   * <span data-ttu-id="bd4fc-168">此时会显示“新建资源组”对话框：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-168">The **New Resource Group** dialog box will be displayed:</span></span>
        
       ![新建资源组对话框][08]
   * <span data-ttu-id="bd4fc-170">在“名称”文本框中，为新的资源组指定名称。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-170">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
   * <span data-ttu-id="bd4fc-171">在“区域”下拉菜单中，为资源组选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-171">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
   * <span data-ttu-id="bd4fc-172">可选：默认情况下，Azure 自动将最新的 Java 8 分发版作为 JVM 部署到 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="bd4fc-173">但是，可以根据 Web 应用的需要指定 JVM 的其他版本和分发版。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-173">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="bd4fc-174">要指定 Web 应用的 JDK，请单击“JDK”选项卡，并选择下列选项之一：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-174">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
     * <span data-ttu-id="bd4fc-175">**部署由 Azure Web 应用服务提供的默认 JDK**：此选项部署将最新的 Java 分发版。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-175">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java.</span></span>
     * <span data-ttu-id="bd4fc-176">**部署 Azure 上可用的第三方 JDK**：使用此选项可从由 Microsoft Azure 提供的 JDK 列表中选择。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-176">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
     * <span data-ttu-id="bd4fc-177">**从此下载位置部署我自己的 JDK**：此选项可以指定自己的 JDK 分发版，此 JDK 必须打包为 ZIP 文件并上传到公开可用的下载位置或可以访问的 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-177">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
       ![新建 Web 应用容器对话框][07b]

   <span data-ttu-id="bd4fc-179">g.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-179">g.</span></span> <span data-ttu-id="bd4fc-180">单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-180">Click **OK**.</span></span>

   <span data-ttu-id="bd4fc-181">h.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-181">h.</span></span> <span data-ttu-id="bd4fc-182">“应用服务计划”下拉菜单列出了与选定资源组关联的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-182">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="bd4fc-183">（应用服务计划指定了 Web 应用的位置、定价层以及计算实例大小等信息。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-183">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="bd4fc-184">单个应用服务计划可用于多个 Web 应用，这也是从特定 Web 应用部署中单独维护它的原因。）</span><span class="sxs-lookup"><span data-stu-id="bd4fc-184">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:
      
      * <span data-ttu-id="bd4fc-185">单击“新建...”</span><span class="sxs-lookup"><span data-stu-id="bd4fc-185">Click **New...**</span></span>
      * <span data-ttu-id="bd4fc-186">此时会显示“新建应用服务计划”对话框：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-186">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![新建应用服务计划对话框][09]
      * <span data-ttu-id="bd4fc-188">在“名称”文本框中，为新的应用服务计划指定名称。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-188">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="bd4fc-189">在“位置”下拉菜单中，为计划选择适当的 Azure 数据中心位置。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-189">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="bd4fc-190">在“定价层”下拉菜单中，为计划选择适当的定价。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-190">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="bd4fc-191">对于测试，可以选择“免费”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-191">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="bd4fc-192">在“实例大小”下拉菜单中，为计划选择适当的实例大小。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-192">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="bd4fc-193">对于测试，可以选择“小”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-193">For testing purposes you can choose **Small**.</span></span>

   <span data-ttu-id="bd4fc-194">i.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-194">i.</span></span> <span data-ttu-id="bd4fc-195">完成所有上述步骤之后，“新建 Web 应用容器”对话框看起来应如下图所示：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-195">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
      ![新建 Web 应用容器对话框][10]

   <span data-ttu-id="bd4fc-197">j.</span><span class="sxs-lookup"><span data-stu-id="bd4fc-197">j.</span></span> <span data-ttu-id="bd4fc-198">单击“确定”完成创建新的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-198">Click **OK** to complete the creation of your new Web App container.</span></span>
       
      <span data-ttu-id="bd4fc-199">等待 Web 应用容器列表刷新，这需要几秒，然后，新创建的 Web 应用容器应在列表中处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-199">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>

7. <span data-ttu-id="bd4fc-200">现在已准备好完成将 Web 应用部署到 Azure 的初始部署：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-200">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
   ![部署到 Azure Web 应用容器对话框][11]
   
   <span data-ttu-id="bd4fc-202">单击“确定”将 Java 应用程序部署到选定的 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-202">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
   <span data-ttu-id="bd4fc-203">默认情况下，应用程序部署为应用程序服务器的子目录。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-203">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="bd4fc-204">如果想要将其部署为根应用程序，请选中“部署到根”复选框，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-204">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>

8. <span data-ttu-id="bd4fc-205">接下来，应会看到“Azure 活动日志”视图，其中指示了 Web 应用的部署状态。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-205">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
   ![Azure 活动日志][12]
   
   <span data-ttu-id="bd4fc-207">将 Web 应用部署到 Azure 的过程只需几秒钟即可完成。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-207">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="bd4fc-208">当应用程序准备就绪时，可在“状态”列中看到名为“已发布”的链接。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-208">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="bd4fc-209">单击该链接时，会转到已部署的 Web 应用的主页。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-209">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="bd4fc-210">更新 Web 应用</span><span class="sxs-lookup"><span data-stu-id="bd4fc-210">Updating your web app</span></span>

<span data-ttu-id="bd4fc-211">更新现有运行中的 Azure Web 应用是一个快速而简单的过程，可以使用两个更新选项：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-211">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="bd4fc-212">可以更新现有 Java Web 应用的部署。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-212">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="bd4fc-213">可以将其他 Java 应用程序发布到同一个 Web 应用容器。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-213">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="bd4fc-214">对于任一情况，过程都是相同的，只需几秒钟即可完成：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-214">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="bd4fc-215">在 Eclipse 项目资源管理器中，右键单击要更新或添加到现有 Web 应用容器的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-215">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>

2. <span data-ttu-id="bd4fc-216">出现上下文菜单时，请选择“Azure”，并单击“发布为 Azure Web 应用...”</span><span class="sxs-lookup"><span data-stu-id="bd4fc-216">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>

3. <span data-ttu-id="bd4fc-217">由于前面已登录，因此将看到现有 Web 应用容器的列表。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-217">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="bd4fc-218">选择要对其发布或重新发布 Java 应用程序的 Web 应用容器，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-218">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="bd4fc-219">几秒钟后，“Azure 活动日志”视图会将已更新的部署显示为“已发布”，可以在 Web 浏览器中验证已更新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-219">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="bd4fc-220">启动、停止或重启现有的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="bd4fc-220">Starting, stopping, or restarting an existing web app</span></span>

<span data-ttu-id="bd4fc-221">若要启动或停止现有的 Azure Web 应用容器（包括其中所有已部署的 Java 应用程序），可以使用“Azure 资源管理器”视图。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-221">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="bd4fc-222">如果“Azure 资源管理器”视图尚未打开，可以依次单击 Eclipse 中的“窗口”菜单、“显示视图”、“其他...”、“Azure”和“Azure 资源管理器”将它打开。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-222">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="bd4fc-223">如果事先未尚未登录，系统会提示登录。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-223">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="bd4fc-224">显示“Azure 资源管理器”视图后，使用以下步骤来启动或停止 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="bd4fc-224">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="bd4fc-225">展开“Azure”节点。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-225">Expand the **Azure** node.</span></span>

2. <span data-ttu-id="bd4fc-226">展开“Web 应用”节点。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-226">Expand the **Web Apps** node.</span></span> 

3. <span data-ttu-id="bd4fc-227">右键单击所需的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-227">Right-click the desired Web App.</span></span>

4. <span data-ttu-id="bd4fc-228">出现上下文菜单时，单击“启动”、“停止”或“重启”。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-228">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="bd4fc-229">请注意，菜单选项可识别上下文，因此，只能停止正在运行的 Web 应用，或启动当前未运行的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-229">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
   ![停止现有的 Web 应用][13]

## <a name="next-steps"></a><span data-ttu-id="bd4fc-231">后续步骤</span><span class="sxs-lookup"><span data-stu-id="bd4fc-231">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<span data-ttu-id="bd4fc-232">有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。</span><span class="sxs-lookup"><span data-stu-id="bd4fc-232">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

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
[Updated Version]: azure-toolkit-for-eclipse-create-hello-world-web-app.md
[eclipse-sign-in-instructions]: azure-toolkit-for-eclipse-sign-in-instructions.md


<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/01-Web-Page.png
[02]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/02-Dynamic-Web-Project.png
[03]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/03-Context-Menu.png
[04]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/04-Log-In-Dialog.png
[05]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/05-Manage-Subscriptions-Dialog.png
[06]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/07b-New-Web-App-Container-Dialog.png
[08]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/08-New-Resource-Group-Dialog.png
[09]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/09-New-Service-Plan-Dialog.png
[10]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/11-Completed-Deploy-Dialog.png
[12]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/12-Activity-Log-View.png
[13]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/13-Azure-Explorer-Web-App.png
[14]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/14-publishDropdownButton.png
[15]: ./media/azure-toolkit-for-eclipse-create-hello-world-web-app-legacy-version/15-New-Azure-Web-Container.png
