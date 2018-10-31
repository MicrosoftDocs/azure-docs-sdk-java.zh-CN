---
title: 使用 Azure DevOps 对 MicroProfile 应用程序进行 CI/CD
description: 了解如何使用 Azure DevOps 设置一个 CI/CD 发布周期，以便将 MicroProfile 应用程序部署到用于容器的 Azure Web 应用实例
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: 818e37291fa47f99cb161c63a86062bddbf6248c
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799933"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="b144d-103">使用 Azure DevOps 对 MicroProfile 应用程序进行 CI/CD</span><span class="sxs-lookup"><span data-stu-id="b144d-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="b144d-104">本教程介绍 Java EE 开发人员如何使用 Azure DevOps（正式名称为 VSTS）轻松地设置 CI/CD 发布周期，以便将 [MicroProfile](http://microprofile.io) 应用程序部署到用于容器的 Azure Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b144d-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="b144d-105">在此示例中，我们将使用一个 MicroProfile 应用程序，该应用程序使用 [Payara Micro](https://www.payara.fish/payara_micro) 作为基础映像。</span><span class="sxs-lookup"><span data-stu-id="b144d-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="b144d-106">我们将开始 Azure DevOps 容器化过程：先生成一个 Docker 映像，然后将该容器映像推送到 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="b144d-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="b144d-107">然后，通过 Azure DevOps 发布管道将容器映像部署到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="b144d-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b144d-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="b144d-108">Prerequisites</span></span>
- <span data-ttu-id="b144d-109">复制并保存来自 [Github](https://github.com/Azure-Samples/microprofile-hello-azure) 的 Git URL</span><span class="sxs-lookup"><span data-stu-id="b144d-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="b144d-110">注册或登录 [Azure DevOps](https://dev.azure.com) 帐户</span><span class="sxs-lookup"><span data-stu-id="b144d-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="b144d-111">创建新的 [Azure DevOps 项目](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)，使用上述 Git URL **导入一个存储库**</span><span class="sxs-lookup"><span data-stu-id="b144d-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="b144d-112">创建 [Azure 容器注册表](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span><span class="sxs-lookup"><span data-stu-id="b144d-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="b144d-113">创建用于容器的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="b144d-113">Create an Azure Web App for Container</span></span>
  > [!NOTE]
  >
  > <span data-ttu-id="b144d-114">预配 Web 应用实例时，在“容器设置”中选择“快速入门”</span><span class="sxs-lookup"><span data-stu-id="b144d-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="b144d-115">创建生成定义</span><span class="sxs-lookup"><span data-stu-id="b144d-115">Create a Build definition</span></span>

<span data-ttu-id="b144d-116">每次在 Java EE 应用程序源应用程序中进行提交时，Azure DevOps 中的生成定义就会自动执行生成中的所有任务。</span><span class="sxs-lookup"><span data-stu-id="b144d-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="b144d-117">在此示例中，Azure DevOps 将使用 Maven 来生成 Java MicroProfile 项目。</span><span class="sxs-lookup"><span data-stu-id="b144d-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="b144d-118">在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。</span><span class="sxs-lookup"><span data-stu-id="b144d-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="b144d-119">然后，选择“生成”链接</span><span class="sxs-lookup"><span data-stu-id="b144d-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="b144d-120">单击“新建管道”按钮，然后单击“继续”，开始定义生成任务</span><span class="sxs-lookup"><span data-stu-id="b144d-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="b144d-121">从模板列表中选择“Maven”，然后单击“应用”按钮以生成 Java 项目</span><span class="sxs-lookup"><span data-stu-id="b144d-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="b144d-122">使用代理池字段对应的下拉菜单选择“托管的 Linux 预览版”选项。</span><span class="sxs-lookup"><span data-stu-id="b144d-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
   > [!NOTE]
   >
   > <span data-ttu-id="b144d-123">这会告知 Azure DevOps 要使用的生成服务器。</span><span class="sxs-lookup"><span data-stu-id="b144d-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="b144d-124">可以使用专用自定义生成服务器</span><span class="sxs-lookup"><span data-stu-id="b144d-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="b144d-125">若要为持续集成配置生成，请选择“触发器”选项卡，然后勾选“启用持续集成”复选框。</span><span class="sxs-lookup"><span data-stu-id="b144d-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 

6. <span data-ttu-id="b144d-126">选择“任务”选项卡，返回到生成管道主页</span><span class="sxs-lookup"><span data-stu-id="b144d-126">Select the <strong>Tasks</strong> tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="b144d-127">使用“保存和排队”下拉菜单选择“保存”选项</span><span class="sxs-lookup"><span data-stu-id="b144d-127">Use the <strong>Save &amp; queue</strong> drop-down menu to select the <strong>Save</strong> option</span></span>


## <a name="create-a-docker-build-image"></a><span data-ttu-id="b144d-128">创建 Docker 生成映像</span><span class="sxs-lookup"><span data-stu-id="b144d-128">Create a Docker Build Image</span></span>

<span data-ttu-id="b144d-129">在此任务中，Azure DevOps 将 Dockerfile 与 Payara Micro 提供的基础映像配合使用，以便创建 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="b144d-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="b144d-130">选择“任务”选项卡，返回到生成管道主页</span><span class="sxs-lookup"><span data-stu-id="b144d-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="b144d-131">单击“+”图标，向生成定义添加新任务</span><span class="sxs-lookup"><span data-stu-id="b144d-131">Click on the **+** icon to add new task to the build definition</span></span>

<img src="media/VSTS/Tasks-add4.png">

3. <span data-ttu-id="b144d-132">从模板列表中选择“Docker”，然后单击“添加”按钮&quot;&quot;</span><span class="sxs-lookup"><span data-stu-id="b144d-132">Select &quot;Docker&quot; from the list of templates, then click on the <strong>Add</strong> button</span></span>
4. <span data-ttu-id="b144d-133">为“显示名称”字段输入说明性名称</span><span class="sxs-lookup"><span data-stu-id="b144d-133">Enter a description name for the <strong>Display name</strong> field</span></span>
5. <span data-ttu-id="b144d-134">在“容器注册表类型”对应的下拉菜单中验证“Azure 容器注册表”是否处于选中状态</span><span class="sxs-lookup"><span data-stu-id="b144d-134">Verify that <strong>Azure Container Registry</strong> is selected in the drop-down menu for <strong>Container registry type</strong>.</span></span>
<span data-ttu-id="b144d-135">&gt;</span><span class="sxs-lookup"><span data-stu-id="b144d-135">&gt;</span></span> [!NOTE]
<span data-ttu-id="b144d-136">&gt; &gt;如果使用 Docker 中心或其他注册表，请改选“容器注册表”。&quot;&quot;</span><span class="sxs-lookup"><span data-stu-id="b144d-136">&gt; &gt;  If you are using Docker Hub or another registry, select &quot;Container Registry&quot; instead.</span></span>  <span data-ttu-id="b144d-137">然后单击“+ 新建”按钮，以便为其提供凭据和连接信息。&quot;&quot;</span><span class="sxs-lookup"><span data-stu-id="b144d-137">Then click on the &quot;+ New&quot; button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="b144d-138">然后跳到“命令”部分，以便继续操作。</span><span class="sxs-lookup"><span data-stu-id="b144d-138">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="b144d-139">使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="b144d-139">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="b144d-140">然后单击“授权”按钮</span><span class="sxs-lookup"><span data-stu-id="b144d-140">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="b144d-141">在“Azure 容器注册表”下拉菜单中，选择在 Azure 中创建的注册表名称。</span><span class="sxs-lookup"><span data-stu-id="b144d-141">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="b144d-142">在“命令”下拉菜单中，验证“生成”选项是否处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="b144d-142">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="b144d-143">对于 **Dockerfile**，请单击文本框旁边的路径导航图标，以便从 github 项目选择 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="b144d-143">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="b144d-144">然后单击“确定”按钮。</span><span class="sxs-lookup"><span data-stu-id="b144d-144">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="b144d-145">在“映像名称”下勾选“包括最新标记”复选框。</span><span class="sxs-lookup"><span data-stu-id="b144d-145">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="b144d-146">使用“保存和排队”下拉菜单选择“保存”选项。</span><span class="sxs-lookup"><span data-stu-id="b144d-146">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="b144d-147">将 Docker 映像推送到 ACR</span><span class="sxs-lookup"><span data-stu-id="b144d-147">Push Docker Image to ACR</span></span>

<span data-ttu-id="b144d-148">在此任务中，Azure DevOps 会将 Docker 映像推送到 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="b144d-148">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="b144d-149">可以通过它将 MicroProfile API 应用程序作为容器化 Java Web 应用运行。</span><span class="sxs-lookup"><span data-stu-id="b144d-149">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="b144d-150">由于我们在 Azure DevOps 中使用 Docker，因此请重复上面的步骤 1 - 7 以创建新的 Docker 模板，如“创建 Docker 生成映像”部分所述。</span><span class="sxs-lookup"><span data-stu-id="b144d-150">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="b144d-151">在“命令”下拉菜单中，选择“推送”。</span><span class="sxs-lookup"><span data-stu-id="b144d-151">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="b144d-152">单击“保存和排队”选项卡，然后选择“保存和排队”选项。</span><span class="sxs-lookup"><span data-stu-id="b144d-152">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="b144d-153">对于弹出窗口中的代理池，验证“托管的 Linux 预览版”是否处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="b144d-153">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="b144d-154">然后单击“保存和排队”按钮。</span><span class="sxs-lookup"><span data-stu-id="b144d-154">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="b144d-155">单击生成号，验证 Java 项目的生成管道是否已成功完成。</span><span class="sxs-lookup"><span data-stu-id="b144d-155">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">


## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="b144d-156">创建 Java 应用的发布定义</span><span class="sxs-lookup"><span data-stu-id="b144d-156">Create a release definition for a Java app</span></span>

<span data-ttu-id="b144d-157">一旦生成过程成功完成，Azure DevOps 中的发布管道就会自动触发部署过程，将生成项目部署到目标环境（例如 Azure）。</span><span class="sxs-lookup"><span data-stu-id="b144d-157">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="b144d-158">可以针对开发、测试、过渡或生产环境创建发布管道。</span><span class="sxs-lookup"><span data-stu-id="b144d-158">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="b144d-159">在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。</span><span class="sxs-lookup"><span data-stu-id="b144d-159">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="b144d-160">然后，选择“发布”链接。</span><span class="sxs-lookup"><span data-stu-id="b144d-160">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">

2. <span data-ttu-id="b144d-161">单击“新建管道”按钮&quot;\*\*</span><span class="sxs-lookup"><span data-stu-id="b144d-161">Click on the &quot;New pipeline\*\* button</span></span>
3. <span data-ttu-id="b144d-162">在模板列表中选择“将 Java 应用部署到 Azure 应用服务”，然后单击“应用”按钮。</span><span class="sxs-lookup"><span data-stu-id="b144d-162">Select the <strong>Deploy a Java app to Azure App Service</strong> in the list of templates, then click on the <strong>Apply</strong> button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">

4. <span data-ttu-id="b144d-163">设置“阶段名称”（例如“开发”、“测试”、“过渡”或“生产”）。</span><span class="sxs-lookup"><span data-stu-id="b144d-163">Set a <strong>Stage name</strong> (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="b144d-164">然后单击“X”按钮，关闭弹出窗口</span><span class="sxs-lookup"><span data-stu-id="b144d-164">Then click on the <strong>X</strong> button to close the pop-up window</span></span>
5. <span data-ttu-id="b144d-165">单击“项目”部分的“+ 添加”按钮。</span><span class="sxs-lookup"><span data-stu-id="b144d-165">Click on the <strong>+ Add</strong> button in the Artifacts section.</span></span>  <span data-ttu-id="b144d-166">这样会将项目从生成定义链接到此发布定义。</span><span class="sxs-lookup"><span data-stu-id="b144d-166">This will link artifacts from the build definition to this release definition.</span></span><br/><span data-ttu-id="b144d-167">6.使用“源(生成管道)”对应的下拉菜单选择生成定义。</span><span class="sxs-lookup"><span data-stu-id="b144d-167">6. Use the drop-down menu for the <strong>Source (build pipeline)</strong> to select your build definition.</span></span> <span data-ttu-id="b144d-168">然后单击“添加”按钮继续。</span><span class="sxs-lookup"><span data-stu-id="b144d-168">Then click the <strong>Add</strong> button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">

7. <span data-ttu-id="b144d-169">单击管道上的“任务”选项卡。</span><span class="sxs-lookup"><span data-stu-id="b144d-169">Click on the <strong>Tasks</strong> tab on the pipeline.</span></span>  <span data-ttu-id="b144d-170">然后，选择阶段名称。</span><span class="sxs-lookup"><span data-stu-id="b144d-170">Then, select your stage name.</span></span>

<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="b144d-171">使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="b144d-171">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="b144d-172">从“应用类型”下拉菜单中选择“Linux 应用”</span><span class="sxs-lookup"><span data-stu-id="b144d-172">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="b144d-173">在“应用服务名称”下拉菜单中选择此前创建的用于容器的 Web 应用实例的名称</span><span class="sxs-lookup"><span data-stu-id="b144d-173">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="b144d-174">在“注册表或命名空间”字段中输入 Azure 容器注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="b144d-174">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="b144d-175">例如 **myregistry.azure.io**</span><span class="sxs-lookup"><span data-stu-id="b144d-175">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="b144d-176">在“存储库”字段中输入注册表名称</span><span class="sxs-lookup"><span data-stu-id="b144d-176">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="b144d-177">单击“部署 Azure 应用服务”。</span><span class="sxs-lookup"><span data-stu-id="b144d-177">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="b144d-178">在“标记”文本框中输入容器映像的标记</span><span class="sxs-lookup"><span data-stu-id="b144d-178">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="b144d-179">单击“在代理上运行”。</span><span class="sxs-lookup"><span data-stu-id="b144d-179">Click on **Run on agent**.</span></span>  <span data-ttu-id="b144d-180">在代理池下拉菜单中选择“托管的 Linux 预览版”</span><span class="sxs-lookup"><span data-stu-id="b144d-180">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="b144d-181">设置环境变量</span><span class="sxs-lookup"><span data-stu-id="b144d-181">Setup Environment Variables</span></span>

1. <span data-ttu-id="b144d-182">单击“变量”选项卡。然后单击“+ 添加”按钮，定义环境变量</span><span class="sxs-lookup"><span data-stu-id="b144d-182">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="b144d-183">为容器注册表 URL、用户名和密码添加变量名称和值。</span><span class="sxs-lookup"><span data-stu-id="b144d-183">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="b144d-184">为安全起见，请单击锁状图标，让密码值处于隐藏状态。</span><span class="sxs-lookup"><span data-stu-id="b144d-184">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="b144d-185">例如：</span><span class="sxs-lookup"><span data-stu-id="b144d-185">For example:</span></span>
- <span data-ttu-id="b144d-186">registry.password</span><span class="sxs-lookup"><span data-stu-id="b144d-186">registry.password</span></span>
- <span data-ttu-id="b144d-187">registry.url</span><span class="sxs-lookup"><span data-stu-id="b144d-187">registry.url</span></span>
- <span data-ttu-id="b144d-188">registry.username</span><span class="sxs-lookup"><span data-stu-id="b144d-188">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="b144d-189">单击“任务”选项卡，返回到发布管道定义主页</span><span class="sxs-lookup"><span data-stu-id="b144d-189">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="b144d-190">单击“部署 Azure 应用服务”。</span><span class="sxs-lookup"><span data-stu-id="b144d-190">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="b144d-191">展开“应用程序和配置设置”部分，然后单击“应用设置”字段的导航路径，以便添加在部署期间连接到容器注册表所需的环境变量。</span><span class="sxs-lookup"><span data-stu-id="b144d-191">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="b144d-192">单击“+ 添加”按钮，定义以下应用设置并分配环境变量\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="b144d-192">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
7. <span data-ttu-id="b144d-193">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="b144d-193">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
8. <span data-ttu-id="b144d-194">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="b144d-194">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
9. <span data-ttu-id="b144d-195">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="b144d-195">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">

7. <span data-ttu-id="b144d-196">单击“确定”按钮继续</span><span class="sxs-lookup"><span data-stu-id="b144d-196">Click on the <strong>OK</strong> button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="b144d-197">设置持续部署和部署 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="b144d-197">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="b144d-198">若要启用持续部署，请单击“管道”选项卡</span><span class="sxs-lookup"><span data-stu-id="b144d-198">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="b144d-199">在“项目”部分，单击闪电图标。</span><span class="sxs-lookup"><span data-stu-id="b144d-199">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="b144d-200">然后将“持续部署触发器”设置为“启用”。</span><span class="sxs-lookup"><span data-stu-id="b144d-200">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">

3. <span data-ttu-id="b144d-201">单击“保存”按钮，然后单击“确定”按钮</span><span class="sxs-lookup"><span data-stu-id="b144d-201">Click on the <strong>Save</strong> button, then the <strong>OK</strong> button</span></span> 
4. <span data-ttu-id="b144d-202">单击“+ 发布”下拉菜单，然后选择“创建发布”链接</span><span class="sxs-lookup"><span data-stu-id="b144d-202">Click on the <strong>+ Release</strong> drop-down menu, then select <strong>Create a release</strong> link</span></span>
5. <span data-ttu-id="b144d-203">使用“触发器从自动改为手动的阶段”下拉菜单，选择阶段名称的复选框</span><span class="sxs-lookup"><span data-stu-id="b144d-203">Use the <strong>Stages for a trigger change from automated to manual</strong> drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="b144d-204">单击“创建”按钮继续</span><span class="sxs-lookup"><span data-stu-id="b144d-204">Click the <strong>Create</strong> button to continue</span></span>
7. <span data-ttu-id="b144d-205">单击发行版号。</span><span class="sxs-lookup"><span data-stu-id="b144d-205">Click on the release number.</span></span>  <span data-ttu-id="b144d-206">然后将鼠标悬停在阶段名称上方，单击“部署”按钮</span><span class="sxs-lookup"><span data-stu-id="b144d-206">Then hover your mouse cursor over the stage name and click on the <strong>Deploy</strong> button</span></span>
8. <span data-ttu-id="b144d-207">然后在弹出窗口中单击“部署”按钮，启动部署到 Azure 的部署过程</span><span class="sxs-lookup"><span data-stu-id="b144d-207">The click on the <strong>Deploy</strong> button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="b144d-208">测试 Java Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="b144d-208">Test the Java Web Application</span></span>
1. <span data-ttu-id="b144d-209">在 Web 浏览器中运行 Web 应用 URL：</span><span class="sxs-lookup"><span data-stu-id="b144d-209">Run the web app url in web browser:</span></span>  
   <span data-ttu-id="b144d-210">https://{your-app-service-name}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="b144d-210">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>


<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="b144d-211">网页会显示 **Hello Azure!**</span><span class="sxs-lookup"><span data-stu-id="b144d-211">The web page should say **Hello Azure!**</span></span>

<img src="media/VSTS/web-api17.png">
