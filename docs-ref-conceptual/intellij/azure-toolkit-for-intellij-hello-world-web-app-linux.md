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
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: d281f37b027d4011ea2e3106990c5e45b69ebc88
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48892588"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="ef6cd-103">使用用于 IntelliJ 的 Azure 工具包将 Hello World Web 应用部署到云中的 Linux 容器</span><span class="sxs-lookup"><span data-stu-id="ef6cd-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="ef6cd-104">[Docker] 容器广泛用于部署 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="ef6cd-105">开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="ef6cd-106">用于 IntelliJ 的 Azure 工具包可以添加用于将容器部署到 Microsoft Azure 的功能，为 Java 开发人员简化了部署过程。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="ef6cd-107">本文演示如何创建基本的 Hello World Web 应用和使用用于 IntelliJ 的 Azure 工具包将 Linux 容器中的 Web 应用发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* <span data-ttu-id="ef6cd-108">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ef6cd-109">若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="ef6cd-110">可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 设置菜单][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="ef6cd-112">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="ef6cd-112">Create a new web app project</span></span>

1. <span data-ttu-id="ef6cd-113">启动 IntelliJ，并按照[用于 IntelliJ 的 Azure 工具包的登录说明](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions)一文中的步骤登录 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-113">Start IntelliJ and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="ef6cd-114">依次单击“文件”菜单、“新建”、“项目”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-114">Click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
   ![创建新项目][file-new-project]

1. <span data-ttu-id="ef6cd-116">在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-116">In the **New Project** dialog box, select **Maven**, then **maven-archetype-webapp**, and then click **Next**.</span></span>
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. <span data-ttu-id="ef6cd-118">为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-118">Specify the **GroupId** and **ArtifactId** for your web app, and then click **Next**.</span></span>
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. <span data-ttu-id="ef6cd-120">自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-120">Customize any Maven settings or accept the defaults, and then click **Next**.</span></span>
   
   ![指定 Maven 设置][maven-options]

1. <span data-ttu-id="ef6cd-122">指定项目名称和位置，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-122">Specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定项目名称][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="ef6cd-124">创建 Azure 容器注册表，用作专用 Docker 注册表</span><span class="sxs-lookup"><span data-stu-id="ef6cd-124">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="ef6cd-125">以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-125">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ef6cd-126">如果想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-126">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="ef6cd-127">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-127">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="ef6cd-128">登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-128">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="ef6cd-129">单击“+ 新建”菜单图标，然后依次单击“容器”、“Azure 容器注册表”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-129">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![创建新的 Azure 容器注册表][AR01]

1. <span data-ttu-id="ef6cd-131">显示 Azure 容器注册表模板的信息页面时，请单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-131">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![创建新的 Azure 容器注册表][AR02]

1. <span data-ttu-id="ef6cd-133">当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-133">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![配置 Azure 容器注册表设置][AR03]

1. <span data-ttu-id="ef6cd-135">创建容器注册表后，在 Azure 门户中导航到你的容器注册表，然后单击“访问密钥”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-135">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="ef6cd-136">记下用户名和密码，以供后续步骤中使用。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-136">Take note of the username and password for the next steps.</span></span>

   ![Azure 容器注册表访问密钥][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="ef6cd-138">在 Docker 容器中部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="ef6cd-138">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="ef6cd-139">右键单击项目资源管理器中的项目，选择“Azure”，然后单击“添加 Docker 支持”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-139">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="ef6cd-140">将使用默认配置自动创建 Docker 文件。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-140">This will automatically create a Docker file with a default configuration.</span></span>

   ![添加 Docker 支持][add-docker-support]

1. <span data-ttu-id="ef6cd-142">添加 Docker 支持后，右键单击项目资源管理器中的项目，选择“Azure”，然后单击“在 Web 应用 (Linux) 上运行”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-142">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Run on Web App (Linux)**.</span></span>

   ![在 Web 应用 (Linux) 上运行][run-on-web-app-linux]

1. <span data-ttu-id="ef6cd-144">出现“在 Web 应用 (Linux) 上运行”对话框时，填写必要信息：</span><span class="sxs-lookup"><span data-stu-id="ef6cd-144">When the **Run on Web App (Linux)** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="ef6cd-145">名称：用于指定显示在 Azure Toolkit 中的友好名称。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-145">**Name**: This specifies the friendly name which is displayed in the Azure Toolkit.</span></span> 

   * <span data-ttu-id="ef6cd-146">服务器 URL：用于指定上文所述的容器注册表 URL，通常使用以下语法：“registry.azurecr.io”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-146">**Server URL**: This specifies the URL for your container registry from the previous section of this article; typically this will use the following syntax: "*registry*.azurecr.io".</span></span> 

   * <span data-ttu-id="ef6cd-147">用户名和密码：用于指定上文所述的容器注册表 URL 的访问密钥。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-147">**Username** and **Password**: Specifies the access keys for your container registry from the previous section of this article.</span></span> 

   * <span data-ttu-id="ef6cd-148">映像和标记：用于指定容器映像名称；通常使用以下语法：“registry.azurecr.io/appname:latest”，其中：</span><span class="sxs-lookup"><span data-stu-id="ef6cd-148">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="ef6cd-149">注册表是上文所述的容器注册表</span><span class="sxs-lookup"><span data-stu-id="ef6cd-149">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="ef6cd-150">appname 是 Web 应用的名称</span><span class="sxs-lookup"><span data-stu-id="ef6cd-150">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="ef6cd-151">“使用现有的 Web 应用”或“创建新的 Web 应用”：用于指定是将容器部署到现有 Web 应用还是创建新的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-151">**Use Existing Web App** or **Create New Web App**: Specifies whether you will deploy your container to an existing web app or create a new web app.</span></span> 

   * <span data-ttu-id="ef6cd-152">资源组：指定是要使用现有资源组还是创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-152">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="ef6cd-153">应用服务计划：指定是要使用现有应用服务计划还是创建新的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-153">**App Service Plan**: Specifies whether you willuse an existing or create a new app service plan.</span></span> 

1. <span data-ttu-id="ef6cd-154">配置完上面列出的设置后，单击“运行”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-154">When you have finished configuring the settings listed above, click **Run**.</span></span>

   ![创建 Web 应用][create-web-app]

1. <span data-ttu-id="ef6cd-156">发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-156">After you have published your web app, your settings will be saved as the default, and you can run your application on Azure by clicking the green arrow icon on the toolbar.</span></span> <span data-ttu-id="ef6cd-157">可通过单击 Web 应用的下拉菜单来修改这些设置，然后单击“编辑配置”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-157">You can modify these settings by clicking the drop-down menu for your web app and click **Edit Configurations**.</span></span>

   ![“编辑配置”菜单][edit-configuration-menu]

1. <span data-ttu-id="ef6cd-159">出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-159">When the **Run/Debug Configurations** dialog box is displayed, you can modify any of the default settings, and then click **OK**.</span></span>

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="next-steps"></a><span data-ttu-id="ef6cd-161">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ef6cd-161">Next steps</span></span>

<span data-ttu-id="ef6cd-162">有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="ef6cd-162">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

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

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
