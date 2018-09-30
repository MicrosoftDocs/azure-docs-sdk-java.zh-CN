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
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506328"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a><span data-ttu-id="314b0-103">使用 Azure DevOps 对 MicroProfile 应用程序进行 CI/CD</span><span class="sxs-lookup"><span data-stu-id="314b0-103">CI/CD for MicroProfile applications using Azure DevOps</span></span>

<span data-ttu-id="314b0-104">本教程介绍 Java EE 开发人员如何使用 Azure DevOps（正式名称为 VSTS）轻松地设置 CI/CD 发布周期，以便将 [MicroProfile](http://microprofile.io) 应用程序部署到用于容器的 Azure Web 应用。</span><span class="sxs-lookup"><span data-stu-id="314b0-104">This tutorial will show how Java EE developers can easily setup a CI/CD release cycle to deploy their [MicroProfile](http://microprofile.io) applications to an Azure Web App for Containers using Azure DevOps (formally known as VSTS).</span></span>  <span data-ttu-id="314b0-105">在此示例中，我们将使用一个 MicroProfile 应用程序，该应用程序使用 [Payara Micro](https://www.payara.fish/payara_micro) 作为基础映像。</span><span class="sxs-lookup"><span data-stu-id="314b0-105">In this example, we’ll be using a MicroProfile application that uses a [Payara Micro](https://www.payara.fish/payara_micro) as a base image.</span></span>   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
<span data-ttu-id="314b0-106">我们将开始 Azure DevOps 容器化过程：先生成一个 Docker 映像，然后将该容器映像推送到 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="314b0-106">We will start the Azure DevOps containerize process by building a Docker image and pushing the container image to an Azure Container Register.</span></span>  <span data-ttu-id="314b0-107">然后，通过 Azure DevOps 发布管道将容器映像部署到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="314b0-107">Then complete with a Azure DevOps release pipeline to deploy the container image to a Web App.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="314b0-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="314b0-108">Prerequisites</span></span>
- <span data-ttu-id="314b0-109">复制并保存来自 [Github](https://github.com/Azure-Samples/microprofile-hello-azure) 的 Git URL</span><span class="sxs-lookup"><span data-stu-id="314b0-109">Copy and save the Git url from [Github](https://github.com/Azure-Samples/microprofile-hello-azure)</span></span>
- <span data-ttu-id="314b0-110">注册或登录 [Azure DevOps](https://dev.azure.com) 帐户</span><span class="sxs-lookup"><span data-stu-id="314b0-110">Register or Log into your [Azure DevOps](https://dev.azure.com) account</span></span>
- <span data-ttu-id="314b0-111">创建新的 [Azure DevOps 项目](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)，使用上述 Git URL **导入一个存储库**</span><span class="sxs-lookup"><span data-stu-id="314b0-111">Create a new [Azure DevOps project](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav) and use the above Git url to **import a repository**</span></span>
- <span data-ttu-id="314b0-112">创建 [Azure 容器注册表](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span><span class="sxs-lookup"><span data-stu-id="314b0-112">Create an [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry) (ACR)</span></span>
- <span data-ttu-id="314b0-113">创建用于容器的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="314b0-113">Create an Azure Web App for Container</span></span>
> [!NOTE]
>
> <span data-ttu-id="314b0-114">预配 Web 应用实例时，在“容器设置”中选择“快速入门”</span><span class="sxs-lookup"><span data-stu-id="314b0-114">Select "Quickstart" in the Container Settings when provisioning the Web App instance</span></span>


## <a name="create-a-build-definition"></a><span data-ttu-id="314b0-115">创建生成定义</span><span class="sxs-lookup"><span data-stu-id="314b0-115">Create a Build definition</span></span>

<span data-ttu-id="314b0-116">每次在 Java EE 应用程序源应用程序中进行提交时，Azure DevOps 中的生成定义就会自动执行生成中的所有任务。</span><span class="sxs-lookup"><span data-stu-id="314b0-116">The build definition in Azure DevOps automatically executes all the tasks in the build each time there’s a commit in Java EE application source application.</span></span>  <span data-ttu-id="314b0-117">在此示例中，Azure DevOps 将使用 Maven 来生成 Java MicroProfile 项目。</span><span class="sxs-lookup"><span data-stu-id="314b0-117">In this example, Azure DevOps will use Maven to build the Java MicroProfile project.</span></span>

1. <span data-ttu-id="314b0-118">在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。</span><span class="sxs-lookup"><span data-stu-id="314b0-118">Click on the "Build and Release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="314b0-119">然后，选择“生成”链接</span><span class="sxs-lookup"><span data-stu-id="314b0-119">Then, select the **Builds** link</span></span> 

<img src="media/VSTS/Buid-and-Release1.png">

2. <span data-ttu-id="314b0-120">单击“新建管道”按钮，然后单击“继续”，开始定义生成任务</span><span class="sxs-lookup"><span data-stu-id="314b0-120">Click on the **New Pipeline** button, and then **Continue** to start defining your build tasks</span></span>
3. <span data-ttu-id="314b0-121">从模板列表中选择“Maven”，然后单击“应用”按钮以生成 Java 项目</span><span class="sxs-lookup"><span data-stu-id="314b0-121">Select "Maven" from the list of templates, then click on the **Apply** button to build your Java project</span></span>
4. <span data-ttu-id="314b0-122">使用代理池字段对应的下拉菜单选择“托管的 Linux 预览版”选项。</span><span class="sxs-lookup"><span data-stu-id="314b0-122">Use the drop-down menu for the Agent pool field to select **Hosted Linux Preview** option.</span></span>
> [!NOTE]
>
> <span data-ttu-id="314b0-123">这会告知 Azure DevOps 要使用的生成服务器。</span><span class="sxs-lookup"><span data-stu-id="314b0-123">This informs Azure DevOps which build server to use.</span></span>  <span data-ttu-id="314b0-124">可以使用专用自定义生成服务器</span><span class="sxs-lookup"><span data-stu-id="314b0-124">You can use your private customized build server</span></span>

5. <span data-ttu-id="314b0-125">若要为持续集成配置生成，请选择“触发器”选项卡，然后勾选“启用持续集成”复选框。</span><span class="sxs-lookup"><span data-stu-id="314b0-125">To configure your build for continuous integration, select the **Triggers** tab and check the **Enable continuous integration** checkbox.</span></span>  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. <span data-ttu-id="314b0-126">选择“任务”选项卡，返回到生成管道主页</span><span class="sxs-lookup"><span data-stu-id="314b0-126">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
7. <span data-ttu-id="314b0-127">使用“保存和排队”下拉菜单选择“保存”选项</span><span class="sxs-lookup"><span data-stu-id="314b0-127">Use the **Save & queue** drop-down menu to select the **Save** option</span></span>
 

## <a name="create-a-docker-build-image"></a><span data-ttu-id="314b0-128">创建 Docker 生成映像</span><span class="sxs-lookup"><span data-stu-id="314b0-128">Create a Docker Build Image</span></span>

<span data-ttu-id="314b0-129">在此任务中，Azure DevOps 将 Dockerfile 与 Payara Micro 提供的基础映像配合使用，以便创建 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="314b0-129">In this task, Azure DevOps uses a Dockerfile with a base image from Payara Micro to create a Docker image.</span></span>  

1. <span data-ttu-id="314b0-130">选择“任务”选项卡，返回到生成管道主页</span><span class="sxs-lookup"><span data-stu-id="314b0-130">Select the **Tasks** tab to return back to the main build pipeline page</span></span>
2. <span data-ttu-id="314b0-131">单击“+”图标，向生成定义添加新任务</span><span class="sxs-lookup"><span data-stu-id="314b0-131">Click on the **+** icon to add new task to the build definition</span></span>
 
<img src="media/VSTS/Tasks-add4.png">
 
3. <span data-ttu-id="314b0-132">从模板列表中选择“Docker”，然后单击“添加”按钮</span><span class="sxs-lookup"><span data-stu-id="314b0-132">Select "Docker" from the list of templates, then click on the **Add** button</span></span>
4. <span data-ttu-id="314b0-133">为“显示名称”字段输入说明性名称</span><span class="sxs-lookup"><span data-stu-id="314b0-133">Enter a description name for the **Display name** field</span></span>
5. <span data-ttu-id="314b0-134">在“容器注册表类型”对应的下拉菜单中验证“Azure 容器注册表”是否处于选中状态</span><span class="sxs-lookup"><span data-stu-id="314b0-134">Verify that **Azure Container Registry** is selected in the drop-down menu for **Container registry type**.</span></span>
> [!NOTE]
>
>  <span data-ttu-id="314b0-135">如果使用 Docker 中心或其他注册表，请改选“容器注册表”。</span><span class="sxs-lookup"><span data-stu-id="314b0-135">If you are using Docker Hub or another registry, select "Container Registry" instead.</span></span>  <span data-ttu-id="314b0-136">然后单击“+ 新建”按钮，以便为其提供凭据和连接信息。</span><span class="sxs-lookup"><span data-stu-id="314b0-136">Then click on the "+ New" button to provide the credentials and connection information for it.</span></span> <span data-ttu-id="314b0-137">然后跳到“命令”部分，以便继续操作。</span><span class="sxs-lookup"><span data-stu-id="314b0-137">Then skip to the Commands section to continue.</span></span>

6. <span data-ttu-id="314b0-138">使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="314b0-138">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>  <span data-ttu-id="314b0-139">然后单击“授权”按钮</span><span class="sxs-lookup"><span data-stu-id="314b0-139">Then click on the **Authorize** button</span></span>
7. <span data-ttu-id="314b0-140">在“Azure 容器注册表”下拉菜单中，选择在 Azure 中创建的注册表名称。</span><span class="sxs-lookup"><span data-stu-id="314b0-140">In the **Azure container registry** drop-down menu, select registry name you created in Azure.</span></span>
8. <span data-ttu-id="314b0-141">在“命令”下拉菜单中，验证“生成”选项是否处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="314b0-141">Verify that **build** option is selected in the **Command** drop-down menu.</span></span>
9. <span data-ttu-id="314b0-142">对于 **Dockerfile**，请单击文本框旁边的路径导航图标，以便从 github 项目选择 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="314b0-142">For the **Dockerfile**, click on the path navigation icon next to the textbox to select the Dockerfile from the github project.</span></span>  <span data-ttu-id="314b0-143">然后单击“确定”按钮。</span><span class="sxs-lookup"><span data-stu-id="314b0-143">Then click the **OK** button.</span></span>

<img src="media/VSTS/Dockerfile5.png">

10. <span data-ttu-id="314b0-144">在“映像名称”下勾选“包括最新标记”复选框。</span><span class="sxs-lookup"><span data-stu-id="314b0-144">Under the **Image name**, check the **Include latest tag** checkbox.</span></span> 
11. <span data-ttu-id="314b0-145">使用“保存和排队”下拉菜单选择“保存”选项。</span><span class="sxs-lookup"><span data-stu-id="314b0-145">Use the **Save & queue** drop-down menu to select the **Save** option.</span></span>

## <a name="push-docker-image-to-acr"></a><span data-ttu-id="314b0-146">将 Docker 映像推送到 ACR</span><span class="sxs-lookup"><span data-stu-id="314b0-146">Push Docker Image to ACR</span></span>

<span data-ttu-id="314b0-147">在此任务中，Azure DevOps 会将 Docker 映像推送到 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="314b0-147">In this task, Azure DevOps will push the docker image to your Azure Container Registry.</span></span>  <span data-ttu-id="314b0-148">可以通过它将 MicroProfile API 应用程序作为容器化 Java Web 应用运行。</span><span class="sxs-lookup"><span data-stu-id="314b0-148">This will be used to run the MicroProfile API application as a containerized Java web app.</span></span>

1. <span data-ttu-id="314b0-149">由于我们在 Azure DevOps 中使用 Docker，因此请重复上面的步骤 1 - 7 以创建新的 Docker 模板，如“创建 Docker 生成映像”部分所述。</span><span class="sxs-lookup"><span data-stu-id="314b0-149">Since we are using Docker in Azure DevOps, create a new Docker template by repeating steps 1 - 7 above in the **Create a Docker Build Image** section.</span></span>
2. <span data-ttu-id="314b0-150">在“命令”下拉菜单中，选择“推送”。</span><span class="sxs-lookup"><span data-stu-id="314b0-150">Select **push** in the **Command** drop-down menu.</span></span>
3. <span data-ttu-id="314b0-151">单击“保存和排队”选项卡，然后选择“保存和排队”选项。</span><span class="sxs-lookup"><span data-stu-id="314b0-151">Click on the **Save & queue** tab, then select **Save & queue** option.</span></span>
4. <span data-ttu-id="314b0-152">对于弹出窗口中的代理池，验证“托管的 Linux 预览版”是否处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="314b0-152">Verify that the **Hosted Linux Preview** is select for the Agent pool on the pop-up window.</span></span>  <span data-ttu-id="314b0-153">然后单击“保存和排队”按钮。</span><span class="sxs-lookup"><span data-stu-id="314b0-153">Then click on the **Save & queue** button.</span></span>
5. <span data-ttu-id="314b0-154">单击生成号，验证 Java 项目的生成管道是否已成功完成。</span><span class="sxs-lookup"><span data-stu-id="314b0-154">Click on the build number to verify that the build pipeline for the Java project completed successfully.</span></span>

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a><span data-ttu-id="314b0-155">创建 Java 应用的发布定义</span><span class="sxs-lookup"><span data-stu-id="314b0-155">Create a release definition for a Java app</span></span>

<span data-ttu-id="314b0-156">一旦生成过程成功完成，Azure DevOps 中的发布管道就会自动触发部署过程，将生成项目部署到目标环境（例如 Azure）。</span><span class="sxs-lookup"><span data-stu-id="314b0-156">The release pipeline in Azure DevOps automatically triggers the deployment of build artifacts to a target environment like Azure as soon as the Build process completes successfully.</span></span>   <span data-ttu-id="314b0-157">可以针对开发、测试、过渡或生产环境创建发布管道。</span><span class="sxs-lookup"><span data-stu-id="314b0-157">The release pipeline can be created for dev, test, staging or production environments.</span></span>

1. <span data-ttu-id="314b0-158">在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。</span><span class="sxs-lookup"><span data-stu-id="314b0-158">Click on the "Build and release" tab on top your Azure DevOps project page.</span></span>  <span data-ttu-id="314b0-159">然后，选择“发布”链接。</span><span class="sxs-lookup"><span data-stu-id="314b0-159">Then, select the **Releases** link.</span></span>

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. <span data-ttu-id="314b0-160">单击“新建管道”按钮\*\*</span><span class="sxs-lookup"><span data-stu-id="314b0-160">Click on the "New pipeline\*\* button</span></span>
3. <span data-ttu-id="314b0-161">在模板列表中选择“将 Java 应用部署到 Azure 应用服务”，然后单击“应用”按钮。</span><span class="sxs-lookup"><span data-stu-id="314b0-161">Select the **Deploy a Java app to Azure App Service** in the list of templates, then click on the **Apply** button.</span></span>

<img src="media/VSTS/deploy-java-template8.png">
 
4. <span data-ttu-id="314b0-162">设置“阶段名称”（例如“开发”、“测试”、“过渡”或“生产”）。</span><span class="sxs-lookup"><span data-stu-id="314b0-162">Set a **Stage name** (e.g Dev, Test, Staging or Production).</span></span>  <span data-ttu-id="314b0-163">然后单击“X”按钮，关闭弹出窗口</span><span class="sxs-lookup"><span data-stu-id="314b0-163">Then click on the **X** button to close the pop-up window</span></span>
5. <span data-ttu-id="314b0-164">单击“项目”部分的“+ 添加”按钮。</span><span class="sxs-lookup"><span data-stu-id="314b0-164">Click on the **+ Add** button in the Artifacts section.</span></span>  <span data-ttu-id="314b0-165">这样会将项目从生成定义链接到此发布定义。</span><span class="sxs-lookup"><span data-stu-id="314b0-165">This will link artifacts from the build definition to this release definition.</span></span>  
6. <span data-ttu-id="314b0-166">使用“源(生成管道)”对应的下拉菜单选择生成定义。</span><span class="sxs-lookup"><span data-stu-id="314b0-166">Use the drop-down menu for the **Source (build pipeline)** to select your build definition.</span></span> <span data-ttu-id="314b0-167">然后单击“添加”按钮继续。</span><span class="sxs-lookup"><span data-stu-id="314b0-167">Then click the **Add** button to continue.</span></span>

<img src="media/VSTS/add-artifact9.png">
 
7. <span data-ttu-id="314b0-168">单击管道上的“任务”选项卡。</span><span class="sxs-lookup"><span data-stu-id="314b0-168">Click on the **Tasks** tab on the pipeline.</span></span>  <span data-ttu-id="314b0-169">然后，选择阶段名称。</span><span class="sxs-lookup"><span data-stu-id="314b0-169">Then, select your stage name.</span></span>
 
<img src="media/VSTS/release-stage10.png">

8. <span data-ttu-id="314b0-170">使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="314b0-170">Use the **Azure subscription** drop-down menu to select your azure subscription ID.</span></span>
9. <span data-ttu-id="314b0-171">从“应用类型”下拉菜单中选择“Linux 应用”</span><span class="sxs-lookup"><span data-stu-id="314b0-171">Select **Linux App** from the **App type** drop-down menu</span></span>
10. <span data-ttu-id="314b0-172">在“应用服务名称”下拉菜单中选择此前创建的用于容器的 Web 应用实例的名称</span><span class="sxs-lookup"><span data-stu-id="314b0-172">Select the name of the Web App for Container instance you created above in the **App service name** drop-down menu</span></span>
11. <span data-ttu-id="314b0-173">在“注册表或命名空间”字段中输入 Azure 容器注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="314b0-173">Enter the name of your azure container registry in the **Registry or Namespaces** field.</span></span>  <span data-ttu-id="314b0-174">例如 **myregistry.azure.io**</span><span class="sxs-lookup"><span data-stu-id="314b0-174">E.g **myregistry.azure.io**</span></span>
12. <span data-ttu-id="314b0-175">在“存储库”字段中输入注册表名称</span><span class="sxs-lookup"><span data-stu-id="314b0-175">Enter the registry name in the **Repository** field</span></span>
13. <span data-ttu-id="314b0-176">单击“部署 Azure 应用服务”。</span><span class="sxs-lookup"><span data-stu-id="314b0-176">Click on **Deploy Azure App Service**.</span></span>  <span data-ttu-id="314b0-177">在“标记”文本框中输入容器映像的标记</span><span class="sxs-lookup"><span data-stu-id="314b0-177">Enter the tag for the container image in the **Tag** textbox</span></span> 
14. <span data-ttu-id="314b0-178">单击“在代理上运行”。</span><span class="sxs-lookup"><span data-stu-id="314b0-178">Click on **Run on agent**.</span></span>  <span data-ttu-id="314b0-179">在代理池下拉菜单中选择“托管的 Linux 预览版”</span><span class="sxs-lookup"><span data-stu-id="314b0-179">Select **Hosted Linux Preview** in the Agent pool drop-down menu</span></span> 

## <a name="setup-environment-variables"></a><span data-ttu-id="314b0-180">设置环境变量</span><span class="sxs-lookup"><span data-stu-id="314b0-180">Setup Environment Variables</span></span>

1. <span data-ttu-id="314b0-181">单击“变量”选项卡。然后单击“+ 添加”按钮，定义环境变量</span><span class="sxs-lookup"><span data-stu-id="314b0-181">Click on the **Variables** tab.  Then click on the **+ Add** button to define your environment variables</span></span>
2. <span data-ttu-id="314b0-182">为容器注册表 URL、用户名和密码添加变量名称和值。</span><span class="sxs-lookup"><span data-stu-id="314b0-182">Add the variable name and values for your container registry url, username and password.</span></span>   <span data-ttu-id="314b0-183">为安全起见，请单击锁状图标，让密码值处于隐藏状态。</span><span class="sxs-lookup"><span data-stu-id="314b0-183">For security, click on the lock icon to keep the password value hidden.</span></span>

<span data-ttu-id="314b0-184">例如：</span><span class="sxs-lookup"><span data-stu-id="314b0-184">For example:</span></span>
- <span data-ttu-id="314b0-185">registry.password</span><span class="sxs-lookup"><span data-stu-id="314b0-185">registry.password</span></span>
- <span data-ttu-id="314b0-186">registry.url</span><span class="sxs-lookup"><span data-stu-id="314b0-186">registry.url</span></span>
- <span data-ttu-id="314b0-187">registry.username</span><span class="sxs-lookup"><span data-stu-id="314b0-187">registry.username</span></span>

<img src="media/VSTS/environment-variables12.png">

3. <span data-ttu-id="314b0-188">单击“任务”选项卡，返回到发布管道定义主页</span><span class="sxs-lookup"><span data-stu-id="314b0-188">Click on the **Tasks** tab to return to the main release pipeline definition page</span></span>
4. <span data-ttu-id="314b0-189">单击“部署 Azure 应用服务”。</span><span class="sxs-lookup"><span data-stu-id="314b0-189">Click on **Deploy Azure App Service**.</span></span> 
5. <span data-ttu-id="314b0-190">展开“应用程序和配置设置”部分，然后单击“应用设置”字段的导航路径，以便添加在部署期间连接到容器注册表所需的环境变量。</span><span class="sxs-lookup"><span data-stu-id="314b0-190">Expand the **Application and Configuration Settings** section, then click on the navigation path for the **App Settings** field to add environments variable to connect to the container registry during deployment.</span></span>
6. <span data-ttu-id="314b0-191">单击“+ 添加”按钮，定义以下应用设置并分配环境变量\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="314b0-191">Click on the \*\* + Add\*\* button to define the following app settings and assign the environment variables</span></span>
- <span data-ttu-id="314b0-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span><span class="sxs-lookup"><span data-stu-id="314b0-192">DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)</span></span>
- <span data-ttu-id="314b0-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span><span class="sxs-lookup"><span data-stu-id="314b0-193">DOCKER_REGISTRY_SERVER_URL = $(registry.url)</span></span>
- <span data-ttu-id="314b0-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span><span class="sxs-lookup"><span data-stu-id="314b0-194">DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)</span></span>

<img src="media/VSTS/environment-variables14.png">
 
7. <span data-ttu-id="314b0-195">单击“确定”按钮继续</span><span class="sxs-lookup"><span data-stu-id="314b0-195">Click on the **OK** button to continue</span></span>

## <a name="setup-continious-deployment--deploy-java-application"></a><span data-ttu-id="314b0-196">设置持续部署和部署 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="314b0-196">Setup Continious Deployment & Deploy Java Application</span></span>

1. <span data-ttu-id="314b0-197">若要启用持续部署，请单击“管道”选项卡</span><span class="sxs-lookup"><span data-stu-id="314b0-197">To enable continuous deployment, click the **Pipelines** tab</span></span>
2. <span data-ttu-id="314b0-198">在“项目”部分，单击闪电图标。</span><span class="sxs-lookup"><span data-stu-id="314b0-198">In the Artifacts section, click on the lightening icon.</span></span>  <span data-ttu-id="314b0-199">然后将“持续部署触发器”设置为“启用”。</span><span class="sxs-lookup"><span data-stu-id="314b0-199">Then set the **Continuous deployment trigger** to Enabled.</span></span>

<img src="media/VSTS/release-enable-CD.png">
 
3. <span data-ttu-id="314b0-200">单击“保存”按钮，然后单击“确定”按钮</span><span class="sxs-lookup"><span data-stu-id="314b0-200">Click on the **Save** button, then the **OK** button</span></span> 
4. <span data-ttu-id="314b0-201">单击“+ 发布”下拉菜单，然后选择“创建发布”链接</span><span class="sxs-lookup"><span data-stu-id="314b0-201">Click on the **+ Release** drop-down menu, then select **Create a release** link</span></span>
5. <span data-ttu-id="314b0-202">使用“触发器从自动改为手动的阶段”下拉菜单，选择阶段名称的复选框</span><span class="sxs-lookup"><span data-stu-id="314b0-202">Use the **Stages for a trigger change from automated to manual** drop-down menu to select the checkbox for your stage name</span></span>
6. <span data-ttu-id="314b0-203">单击“创建”按钮继续</span><span class="sxs-lookup"><span data-stu-id="314b0-203">Click the **Create** button to continue</span></span>
7. <span data-ttu-id="314b0-204">单击发行版号。</span><span class="sxs-lookup"><span data-stu-id="314b0-204">Click on the release number.</span></span>  <span data-ttu-id="314b0-205">然后将鼠标悬停在阶段名称上方，单击“部署”按钮</span><span class="sxs-lookup"><span data-stu-id="314b0-205">Then hover your mouse cursor over the stage name and click on the **Deploy** button</span></span>
8. <span data-ttu-id="314b0-206">然后在弹出窗口中单击“部署”按钮，启动部署到 Azure 的部署过程</span><span class="sxs-lookup"><span data-stu-id="314b0-206">The click on the **Deploy** button on the pop-up window to start the deployment process to Azure</span></span>


## <a name="test-the-java-web-application"></a><span data-ttu-id="314b0-207">测试 Java Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="314b0-207">Test the Java Web Application</span></span>
1. <span data-ttu-id="314b0-208">在 Web 浏览器中运行 Web 应用 URL：</span><span class="sxs-lookup"><span data-stu-id="314b0-208">Run the web app url in web browser:</span></span>  
<span data-ttu-id="314b0-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span><span class="sxs-lookup"><span data-stu-id="314b0-209">https://{your-app-service-name}.azurewebsites.net/api/hello</span></span>

 
<img src="media/VSTS/web-app16.png">

2. <span data-ttu-id="314b0-210">网页会显示 **Hello Azure!**</span><span class="sxs-lookup"><span data-stu-id="314b0-210">The web page should say **Hello Azure!**</span></span>
 
<img src="media/VSTS/web-api17.png">
