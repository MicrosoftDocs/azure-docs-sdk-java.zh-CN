---
title: 使用 Azure Toolkit for Eclipse 部署在云中的 Linux 容器内运行的 Hello World Web 应用
description: 在 Linux 容器中运行一个基本的 Hello World Web 应用，并使用 Azure Toolkit for Eclipse 将它部署到云中。
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
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649243"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="1af33-103">使用 Azure Toolkit for Eclipse 将 Hello World Web 应用部署到云中的 Linux 容器</span><span class="sxs-lookup"><span data-stu-id="1af33-103">Deploy a Hello World web app to a Linux container in the cloud using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="1af33-104">[Docker] 容器广泛用于部署 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="1af33-104">[Docker] containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="1af33-105">开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。</span><span class="sxs-lookup"><span data-stu-id="1af33-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="1af33-106">Azure Toolkit for Eclipse 可以添加将容器部署到 Microsoft Azure 所需的功能，为 Java 开发人员简化了该过程。</span><span class="sxs-lookup"><span data-stu-id="1af33-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding features for to deploy containers to Microsoft Azure.</span></span>

<span data-ttu-id="1af33-107">本文演示通过 Azure Toolkit for Eclipse 创建基本的 Hello World Web 应用并将 Linux 容器中的 Web 应用发布到 Azure 所需的步骤。</span><span class="sxs-lookup"><span data-stu-id="1af33-107">This article demonstrates the steps that are required to create a basic Hello World web app and publish your web app in a Linux container to Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* <span data-ttu-id="1af33-108">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="1af33-108">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="1af33-109">若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。</span><span class="sxs-lookup"><span data-stu-id="1af33-109">To complete the steps in this tutorial, you need to configure [Docker] to expose the daemon on port 2375 without TLS.</span></span> <span data-ttu-id="1af33-110">可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。</span><span class="sxs-lookup"><span data-stu-id="1af33-110">You can configure this setting when installing Docker, or through the Docker settings menu.</span></span>
>
> ![Docker 设置菜单][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a><span data-ttu-id="1af33-112">创建新 Web 应用项目</span><span class="sxs-lookup"><span data-stu-id="1af33-112">Create a new web app project</span></span>

1. <span data-ttu-id="1af33-113">按照 [Azure Toolkit for Eclipse 的登录说明](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)一文中的步骤启动 Eclipse 并登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="1af33-113">Start Eclipse and sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions) article.</span></span>

1. <span data-ttu-id="1af33-114">依次单击“文件”菜单、“新建”和“动态 Web 项目”。</span><span class="sxs-lookup"><span data-stu-id="1af33-114">Click the **File** menu, then click **New**, and then click **Dynamic Web Project**.</span></span>
   
   ![创建新项目][file-new-project]

1. <span data-ttu-id="1af33-116">在“新建动态 Web 项目”对话框中指定项目名称和位置，然后单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="1af33-116">In the **New Dynamic Web Project** dialog box, specify your project name and location, and then click **Finish**.</span></span>
   
   ![指定项目名称][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="1af33-118">创建 Azure 容器注册表，用作专用 Docker 注册表</span><span class="sxs-lookup"><span data-stu-id="1af33-118">Create an Azure Container Registry to use as a private Docker registry</span></span>

<span data-ttu-id="1af33-119">以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。</span><span class="sxs-lookup"><span data-stu-id="1af33-119">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="1af33-120">如果想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。</span><span class="sxs-lookup"><span data-stu-id="1af33-120">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0][Create Docker Registry using Azure CLI].</span></span>
>

1. <span data-ttu-id="1af33-121">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="1af33-121">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="1af33-122">登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。</span><span class="sxs-lookup"><span data-stu-id="1af33-122">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="1af33-123">依次单击“+ 创建资源”菜单图标、“容器”、“容器注册表”。</span><span class="sxs-lookup"><span data-stu-id="1af33-123">Click the menu icon for **+ Create a resource**, then click **Containers**, and then click **Container Registry**.</span></span>
   
   ![创建新的 Azure 容器注册表][create-container-registry-01]

1. <span data-ttu-id="1af33-125">当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="1af33-125">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![配置 Azure 容器注册表设置][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a><span data-ttu-id="1af33-127">在 Docker 容器中部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="1af33-127">Deploy your web app in a Docker container</span></span>

1. <span data-ttu-id="1af33-128">右键单击项目资源管理器中的项目，选择“Azure”，然后单击“添加 Docker 支持”。</span><span class="sxs-lookup"><span data-stu-id="1af33-128">Right-click your project in the project explorer, choose **Azure**, and then click **Add Docker Support**.</span></span>

   <span data-ttu-id="1af33-129">将使用默认配置自动创建 Docker 文件。</span><span class="sxs-lookup"><span data-stu-id="1af33-129">This will automatically create a Docker file with a default configuration.</span></span>

   ![添加 Docker 支持][add-docker-support]

1. <span data-ttu-id="1af33-131">添加 Docker 支持后，右键单击项目资源管理器中的项目，选择“Azure”，然后单击“发布到用于容器的 Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="1af33-131">After you have added Docker support, right-click your project in the project explorer, choose **Azure**, and then click **Publish to Web App for Containers**.</span></span>

   ![发布到用于容器的 Web 应用][run-on-web-app-for-containers]

1. <span data-ttu-id="1af33-133">显示“在用于容器的 Web 应用上运行”对话框时，填写必要信息：</span><span class="sxs-lookup"><span data-stu-id="1af33-133">When the **Run on Web App for Containers** dialog box is displayed, fill in the requisite information:</span></span>

   * <span data-ttu-id="1af33-134">**Docker 文件**：指定向项目添加 Docker 支持时创建的 Docker 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="1af33-134">**Docker File**: This specifies the path to the Docker file that was created when you added Docker support to your project.</span></span> 

   * <span data-ttu-id="1af33-135">**容器注册表**：从下拉菜单中选择在本文的上一部分创建的容器注册表。</span><span class="sxs-lookup"><span data-stu-id="1af33-135">**Container Registry**: Choose the container registry from the drop-down menu that you created in the previous section of this article.</span></span> <span data-ttu-id="1af33-136">“服务器 URL”、“用户名”和“密码”字段会自动填充。</span><span class="sxs-lookup"><span data-stu-id="1af33-136">The fields for **Server URL**, **Username**, and **Password** will be automatically populated.</span></span>

   * <span data-ttu-id="1af33-137">**映像和标记**：指定容器映像名称；通常使用以下语法：“*registry*.azurecr.io/*appname*:latest”，其中：</span><span class="sxs-lookup"><span data-stu-id="1af33-137">**Image and tag**: Specifies the container image name; typically this will use the following syntax: "*registry*.azurecr.io/*appname*:latest", where:</span></span> 
      * <span data-ttu-id="1af33-138">注册表是上文所述的容器注册表</span><span class="sxs-lookup"><span data-stu-id="1af33-138">*registry* is your container registry from the previous section of this article</span></span> 
      * <span data-ttu-id="1af33-139">appname 是 Web 应用的名称</span><span class="sxs-lookup"><span data-stu-id="1af33-139">*appname* is the name of your web app</span></span> 

   * <span data-ttu-id="1af33-140">**用于容器的 Web 应用**：选择“使用现有”或“新建”，指定是将容器部署到现有 Web 应用还是创建新的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="1af33-140">**Web App for Container**: Choose **Use Existing** or **Create New** to specify whether you will deploy your container to an existing web app or create a new web app.</span></span>  <span data-ttu-id="1af33-141">指定的“应用名称”将创建 Web 应用的 URL，例如 *wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="1af33-141">The **App name** that you specify will create the URL for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   * <span data-ttu-id="1af33-142">**资源组**：指定是要使用现有资源组还是创建新的资源组。</span><span class="sxs-lookup"><span data-stu-id="1af33-142">**Resource Group**: Specifies whether you will use an existing or create a new resource group.</span></span> 

   * <span data-ttu-id="1af33-143">**应用服务计划**：指定是要使用现有应用服务计划还是创建新的应用服务计划。</span><span class="sxs-lookup"><span data-stu-id="1af33-143">**App Service Plan**: Specifies whether you will use an existing or create a new app service plan.</span></span> 

   ![在用于容器的 Web 应用上运行][run-on-web-app-linux]

1. <span data-ttu-id="1af33-145">配置完上面列出的设置后，单击“确定”，将 Web 应用发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="1af33-145">When you have finished configuring the settings listed above, click **OK** to publish your web app to Azure.</span></span>

1. <span data-ttu-id="1af33-146">发布 Web 应用以后，即可浏览到此前为 Web 应用指定的 URL，例如 *wingtiptoys.azurewebsites.net*。</span><span class="sxs-lookup"><span data-stu-id="1af33-146">After your web app has been published, you can browse to the URL that specifed earlier for your web app; for example: *wingtiptoys.azurewebsites.net*.</span></span>

   ![浏览到 Web 应用][browsing-to-web-app]

## <a name="next-steps"></a><span data-ttu-id="1af33-148">后续步骤</span><span class="sxs-lookup"><span data-stu-id="1af33-148">Next steps</span></span>

<span data-ttu-id="1af33-149">有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。</span><span class="sxs-lookup"><span data-stu-id="1af33-149">For additional resources for Docker, see the official [Docker website][Docker].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

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

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
