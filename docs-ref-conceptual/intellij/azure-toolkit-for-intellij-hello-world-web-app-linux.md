---
title: 使用用于 IntelliJ 的 Azure 工具包部署在云中的 Linux 容器内运行的 Hello World Web 应用
description: 在 Linux 容器中运行一个基本的 Hello World Web 应用，并使用用于 IntelliJ 的 Azure 工具包将它部署到云中。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: fdff8dc2bd7a29473314d5c0bc99b7bcda369156
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2019
ms.locfileid: "55648721"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="ebdbf-103">使用用于 IntelliJ 的 Azure 工具包将 Hello World Web 应用部署到云中的 Linux 容器</span><span class="sxs-lookup"><span data-stu-id="ebdbf-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="ebdbf-104">[Docker] 容器广泛用于部署 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="ebdbf-105">开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="ebdbf-106">用于 IntelliJ 的 Azure 工具包可以添加用于将容器部署到 Microsoft Azure 的功能，为 Java 开发人员简化了部署过程。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="ebdbf-107">本文演示如何创建基本的 Hello World Web 应用和使用用于 IntelliJ 的 Azure 工具包将 Linux 容器中的 Web 应用发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="ebdbf-108">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebdbf-109">若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="ebdbf-110">可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 设置菜单][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="ebdbf-112">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="ebdbf-112">Create a new web app project</span></span>

1. <span data-ttu-id="ebdbf-113">启动 IntelliJ，并按照[用于 IntelliJ 的 Azure 工具包的登录说明](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions)一文中的步骤登录 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="ebdbf-114">依次单击“文件”菜单、“新建”、“项目”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![创建新项目][file-new-project]

1. <span data-ttu-id="ebdbf-116">在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="ebdbf-118">为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="ebdbf-120">自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 设置][maven-options]

1. <span data-ttu-id="ebdbf-122">指定项目名称和位置，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定项目名称][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="ebdbf-124">创建 Azure 容器注册表，用作专用 Docker 注册表</span><span class="sxs-lookup"><span data-stu-id="ebdbf-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="ebdbf-125">以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebdbf-126">如果想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="ebdbf-127">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="ebdbf-128">登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="ebdbf-129">依次单击“+ 创建资源”菜单图标、“容器”、“容器注册表”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-129">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![创建新的 Azure 容器注册表][create-container-registry-01]

1. <span data-ttu-id="ebdbf-131">当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-131">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![配置 Azure 容器注册表设置][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="ebdbf-133">在 Docker 容器中部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="ebdbf-133">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="ebdbf-134">右键单击项目资源管理器中的项目，选择“Azure”，然后单击“添加 Docker 支持”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-134">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="ebdbf-135">将使用默认配置自动创建 Docker 文件。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-135">This will automatically create a Docker file with a default configuration.</span></span>

   ![添加 Docker 支持][add-docker-support]

1. <span data-ttu-id="ebdbf-137">添加 Docker 支持后，右键单击项目资源管理器中的项目，选择“Azure”，然后单击“在用于容器的 Web 应用上运行”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-137">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App for Containers**.</span></span>

   ![在用于容器的 Web 应用上运行][run-on-web-app-for-containers]

1. <span data-ttu-id="ebdbf-139">显示“在用于容器的 Web 应用上运行”对话框时，填写必要信息：</span><span class="sxs-lookup"><span data-stu-id="ebdbf-139">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="ebdbf-140">**名称**：指定在 Azure Toolkit 中显示的易记名称。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-140">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="ebdbf-141">**容器注册表**：从下拉菜单中选择在本文的上一部分创建的容器注册表。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-141">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="ebdbf-142">“服务器 URL”、“用户名”和“密码”字段会自动填充。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-142">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="ebdbf-143">**映像和标记**：指定容器映像名称；通常使用以下语法：“*registry*.azurecr.io/*appname*:latest”，其中：</span><span class="sxs-lookup"><span data-stu-id="ebdbf-143">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="ebdbf-144">注册表是上文所述的容器注册表</span><span class="sxs-lookup"><span data-stu-id="ebdbf-144">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="ebdbf-145">appname 是 Web 应用的名称</span><span class="sxs-lookup"><span data-stu-id="ebdbf-145">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="ebdbf-146">**使用现有的 Web 应用**或**创建新的 Web 应用**：指定是将容器部署到现有 Web 应用还是创建新的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-146">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> <span data-ttu-id="ebdbf-147">指定的“应用名称”将创建 Web 应用的 URL，例如 *wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-147">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="ebdbf-148">**资源组**：指定是要使用现有资源组还是创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-148">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="ebdbf-149">**应用服务计划**：指定是要使用现有应用服务计划还是创建新的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-149">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![在用于容器的 Web 应用上运行][run-on-web-app-linux]

1. <span data-ttu-id="ebdbf-151">配置完上面列出的设置后，单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-151">When you have finished configuring the settings listed above, click **Run**.</span></span> <span data-ttu-id="ebdbf-152">成功部署 Web 应用以后，状态会显示在“运行”窗口中。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-152">When your web app has been successfully deployed, the status will be displayed in the **Run** window.</span></span>

   ![成功部署的 Web 应用][successfully-deployed]

1. <span data-ttu-id="ebdbf-154">发布 Web 应用以后，即可浏览到此前为 Web 应用指定的 URL，例如 *wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-154">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![浏览到 Web 应用][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a><span data-ttu-id="ebdbf-156">可选：修改 Web 应用发布设置</span><span class="sxs-lookup"><span data-stu-id="ebdbf-156">Optional: Modify your web app publish settings</span></span>

1. <span data-ttu-id="ebdbf-157">发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-157">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="ebdbf-158">可通过单击 Web 应用的下拉菜单来修改这些设置，然后单击“编辑配置”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-158">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![“编辑配置”菜单][edit-configuration-menu]

1. <span data-ttu-id="ebdbf-160">出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-160">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="ebdbf-162">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ebdbf-162">Next steps</span></span>

<span data-ttu-id="ebdbf-163">有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="ebdbf-163">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-intellij-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
