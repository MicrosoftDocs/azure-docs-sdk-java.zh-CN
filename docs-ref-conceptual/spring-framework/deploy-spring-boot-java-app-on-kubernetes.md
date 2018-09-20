---
title: 在 Azure Kubernetes 服务中将 Spring Boot 应用部署于 Kubernetes
description: 本教程将指导用户完成在 Microsoft Azure 的 Kubernetes 群集中部署 Spring Boot 应用程序的步骤。
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 07/05/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 546aa2dc18143ca173d72198ea8e6c30bda3c97f
ms.sourcegitcommit: e017de4677c5bedd6ef88c8c1b6da279dc973efe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2018
ms.locfileid: "45639720"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a><span data-ttu-id="43a2b-103">在 Azure Kubernetes 服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="43a2b-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Kubernetes Service</span></span>

<span data-ttu-id="43a2b-104">[Kubernetes] 和 [Docker] 是开源解决方案，可帮助开发人员自动部署、缩放和管理在容器中运行的应用程序。</span><span class="sxs-lookup"><span data-stu-id="43a2b-104">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications running in containers.</span></span>

<span data-ttu-id="43a2b-105">本教程将指导用户将这两种常用的开源技术进行结合，从而将 Spring Boot 应用程序开发和部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="43a2b-105">This tutorial walks you through combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="43a2b-106">具体而言，将使用 [Spring Boot] 进行应用程序开发，使用 [Kubernetes] 进行容器部署，使用 [Azure Kubernetes 服务 (AKS)] 来托管应用程序。</span><span class="sxs-lookup"><span data-stu-id="43a2b-106">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Kubernetes Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43a2b-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="43a2b-107">Prerequisites</span></span>

* <span data-ttu-id="43a2b-108">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="43a2b-109">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="43a2b-110">最新的 [Java 开发人员工具包 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-110">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="43a2b-111">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="43a2b-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="43a2b-112">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="43a2b-112">A [Git] client.</span></span>
* <span data-ttu-id="43a2b-113">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="43a2b-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="43a2b-114">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="43a2b-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="43a2b-115">创建 Docker 上的 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="43a2b-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="43a2b-116">以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。</span><span class="sxs-lookup"><span data-stu-id="43a2b-116">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="43a2b-117">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="43a2b-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="43a2b-118">- 或 -</span><span class="sxs-lookup"><span data-stu-id="43a2b-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="43a2b-119">将 [Docker 上的 Spring Boot 入门]示例项目克隆到目录。</span><span class="sxs-lookup"><span data-stu-id="43a2b-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="43a2b-120">将目录更改为已完成项目。</span><span class="sxs-lookup"><span data-stu-id="43a2b-120">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="43a2b-121">使用 Maven 生成和运行示例应用。</span><span class="sxs-lookup"><span data-stu-id="43a2b-121">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="43a2b-122">通过浏览到 http://localhost:8080 或使用以下 `curl` 命令测试 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="43a2b-122">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="43a2b-123">应会显示以下消息：“Hello Docker World”</span><span class="sxs-lookup"><span data-stu-id="43a2b-123">You should see the following message displayed: **Hello Docker World**</span></span>

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="43a2b-125">使用 Azure CLI 创建 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="43a2b-125">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="43a2b-126">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="43a2b-126">Open a command prompt.</span></span>

1. <span data-ttu-id="43a2b-127">登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="43a2b-127">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="43a2b-128">选择自己的 Azure 订阅：</span><span class="sxs-lookup"><span data-stu-id="43a2b-128">Choose your Azure Subscription:</span></span>
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. <span data-ttu-id="43a2b-129">为本教程中使用的 Azure 资源创建资源组。</span><span class="sxs-lookup"><span data-stu-id="43a2b-129">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="43a2b-130">在资源组中创建私有 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="43a2b-130">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="43a2b-131">本教程的后续步骤会将示例应用作为 Docker 映像推送到此注册表。</span><span class="sxs-lookup"><span data-stu-id="43a2b-131">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="43a2b-132">用注册表的唯一名称替换 `wingtiptoysregistry`。</span><span class="sxs-lookup"><span data-stu-id="43a2b-132">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="43a2b-133">将应用推送到容器注册表</span><span class="sxs-lookup"><span data-stu-id="43a2b-133">Push your app to the container registry</span></span>

1. <span data-ttu-id="43a2b-134">导航到 Maven 安装的配置目录 (default ~/.m2/ or C:\Users\username\.m2)，并使用文本编辑器打开 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="43a2b-134">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="43a2b-135">从 Azure CLI 检索容器注册表的密码。</span><span class="sxs-lookup"><span data-stu-id="43a2b-135">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="43a2b-136">将你的 Azure 容器注册表 ID 和密码添加到 settings.xml 文件的 `<server>` 集合。</span><span class="sxs-lookup"><span data-stu-id="43a2b-136">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="43a2b-137">`id` 和 `username` 是注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="43a2b-137">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="43a2b-138">使用上一个命令中的 `password` 值（不带引号）。</span><span class="sxs-lookup"><span data-stu-id="43a2b-138">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="43a2b-139">导航到 Spring Boot 应用程序的完整项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="43a2b-139">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="43a2b-140">使用 Azure 容器注册表的登录服务器值更新 pom.xml 文件中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="43a2b-140">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="43a2b-141">更新 pom.xml 文件中的 `<plugins>` 集合，使 `<plugin>` 包含 Azure 容器注册表的登录服务器地址和注册表名称。</span><span class="sxs-lookup"><span data-stu-id="43a2b-141">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <buildArgs>
            <JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
         </buildArgs>
         <baseImage>java</baseImage>
         <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="43a2b-142">导航到 Spring Boot 应用程序的完成项目目录，然后运行以下命令以生成 Docker 容器并将映像推送到注册表：</span><span class="sxs-lookup"><span data-stu-id="43a2b-142">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="43a2b-143">Maven 将映像推送到 Azure 时，你可能会收到类似于以下内容的错误消息：</span><span class="sxs-lookup"><span data-stu-id="43a2b-143">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="43a2b-144">如果收到此错误消息，请从 Docker 命令行登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="43a2b-144">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="43a2b-145">然后推送你的容器：</span><span class="sxs-lookup"><span data-stu-id="43a2b-145">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="43a2b-146">使用 Azure CLI 在 AKS 上创建 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="43a2b-146">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="43a2b-147">在 Azure Kubernetes 服务中创建 Kubernetes 群集。</span><span class="sxs-lookup"><span data-stu-id="43a2b-147">Create a Kubernetes cluster in Azure Kubernetes Service.</span></span> <span data-ttu-id="43a2b-148">以下命令在 wingtiptoys-kubernetes 资源组中创建 kubernetes 群集，将 wingtiptoys-akscluster 作为群集名称，wingtiptoys-kubernetes 作为 DNS 前缀：</span><span class="sxs-lookup"><span data-stu-id="43a2b-148">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-akscluster* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   <span data-ttu-id="43a2b-149">此命令可能需要一段时间才能完成。</span><span class="sxs-lookup"><span data-stu-id="43a2b-149">This command may take a while to complete.</span></span>

1. <span data-ttu-id="43a2b-150">结合使用 Azure 容器注册表 (ACR) 和 Azure Kubernetes 服务 (AKS) 时，需要建立身份验证机制。</span><span class="sxs-lookup"><span data-stu-id="43a2b-150">When you're using Azure Container Registry (ACR) with Azure Kubernetes Service (AKS), an authentication mechanism needs to be established.</span></span> <span data-ttu-id="43a2b-151">按照[使用 Azure 容器注册表从 Azure Kubernetes 服务进行身份验证]中的步骤授予 AKS 对 ACR 的访问权限。</span><span class="sxs-lookup"><span data-stu-id="43a2b-151">Follow the steps in [Authenticate with Azure Container Registry from Azure Kubernetes Service] to grant AKS access to ACR.</span></span>


1. <span data-ttu-id="43a2b-152">使用 Azure CLI 安装 `kubectl`。</span><span class="sxs-lookup"><span data-stu-id="43a2b-152">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="43a2b-153">Linux 用户可能必须将 `sudo` 作为此命令的前缀，因为它将 Kubernetes CLI 部署到 `/usr/local/bin`。</span><span class="sxs-lookup"><span data-stu-id="43a2b-153">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az aks install-cli
   ```

1. <span data-ttu-id="43a2b-154">下载群集配置信息，以便从 Kubernetes Web 界面和 `kubectl` 管理群集。</span><span class="sxs-lookup"><span data-stu-id="43a2b-154">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="43a2b-155">将映像部署到 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="43a2b-155">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="43a2b-156">本教程使用 `kubectl` 部署应用，以便让你通过 Kubernetes Web 界面浏览部署。</span><span class="sxs-lookup"><span data-stu-id="43a2b-156">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="43a2b-157">使用 Kubernetes Web 界面进行部署</span><span class="sxs-lookup"><span data-stu-id="43a2b-157">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="43a2b-158">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="43a2b-158">Open a command prompt.</span></span>

1. <span data-ttu-id="43a2b-159">在默认浏览器中打开 Kubernetes 群集的配置网站：</span><span class="sxs-lookup"><span data-stu-id="43a2b-159">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. <span data-ttu-id="43a2b-160">在浏览器中打开 Kubernetes 配置网站后，单击“部署容器化应用”的链接：</span><span class="sxs-lookup"><span data-stu-id="43a2b-160">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 配置网站][KB01]

1. <span data-ttu-id="43a2b-162">显示“资源创建”页时，请指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="43a2b-162">When the **Resource Creation** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="43a2b-163">a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。</span><span class="sxs-lookup"><span data-stu-id="43a2b-163">a.</span></span> <span data-ttu-id="43a2b-164">选择“创建应用”。</span><span class="sxs-lookup"><span data-stu-id="43a2b-164">Select **CREATE AN APP**.</span></span>

   <span data-ttu-id="43a2b-165">b.</span><span class="sxs-lookup"><span data-stu-id="43a2b-165">b.</span></span> <span data-ttu-id="43a2b-166">在“应用名称”中输入 Spring Boot 应用程序名称；例如：gs-spring-boot-docker。</span><span class="sxs-lookup"><span data-stu-id="43a2b-166">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="43a2b-167">c.</span><span class="sxs-lookup"><span data-stu-id="43a2b-167">c.</span></span> <span data-ttu-id="43a2b-168">在容器映像中输入之前的登录服务器和容器映像；例如：“wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest”。</span><span class="sxs-lookup"><span data-stu-id="43a2b-168">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="43a2b-169">d.</span><span class="sxs-lookup"><span data-stu-id="43a2b-169">d.</span></span> <span data-ttu-id="43a2b-170">选择“外部”服务。</span><span class="sxs-lookup"><span data-stu-id="43a2b-170">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="43a2b-171">e.</span><span class="sxs-lookup"><span data-stu-id="43a2b-171">e.</span></span> <span data-ttu-id="43a2b-172">在“端口”和“目标端口”文本框中指定外部和内部端口。</span><span class="sxs-lookup"><span data-stu-id="43a2b-172">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 配置网站][KB02]


1. <span data-ttu-id="43a2b-174">单击“部署”对容器进行部署。</span><span class="sxs-lookup"><span data-stu-id="43a2b-174">Click **Deploy** to deploy the container.</span></span>

   ![Kubernetes 部署][KB05]

1. <span data-ttu-id="43a2b-176">部署应用程序后，Spring Boot 应用程序将列在“服务”下方。</span><span class="sxs-lookup"><span data-stu-id="43a2b-176">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes 服务][KB06]

1. <span data-ttu-id="43a2b-178">若单击“外部终结点”的链接，则可看见 Spring Boot 应用程序在 Azure 上运行。</span><span class="sxs-lookup"><span data-stu-id="43a2b-178">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes 服务][KB07]

   ![在 Azure 上浏览示例应用][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="43a2b-181">使用 kubectl 进行部署</span><span class="sxs-lookup"><span data-stu-id="43a2b-181">Deploy with kubectl</span></span>

1. <span data-ttu-id="43a2b-182">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="43a2b-182">Open a command prompt.</span></span>

1. <span data-ttu-id="43a2b-183">使用 `kubectl run` 命令在 Kubernetes 群集中运行容器。</span><span class="sxs-lookup"><span data-stu-id="43a2b-183">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="43a2b-184">指定 Kubernetes 中应用的服务名称和完整映像名称。</span><span class="sxs-lookup"><span data-stu-id="43a2b-184">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="43a2b-185">例如：</span><span class="sxs-lookup"><span data-stu-id="43a2b-185">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="43a2b-186">在此命令中：</span><span class="sxs-lookup"><span data-stu-id="43a2b-186">In this command:</span></span>

   * <span data-ttu-id="43a2b-187">容器名称 `gs-spring-boot-docker` 会立即在 `run` 命令后指定</span><span class="sxs-lookup"><span data-stu-id="43a2b-187">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="43a2b-188">`--image` 参数将组合的登录服务器和映像名称指定为 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="43a2b-188">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="43a2b-189">使用 `kubectl expose` 命令外部公开你的 Kubernetes 群集。</span><span class="sxs-lookup"><span data-stu-id="43a2b-189">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="43a2b-190">指定你的服务名称、用于访问应用的公开 TCP 端口以及应用侦听的内部目标端口。</span><span class="sxs-lookup"><span data-stu-id="43a2b-190">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="43a2b-191">例如：</span><span class="sxs-lookup"><span data-stu-id="43a2b-191">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="43a2b-192">在此命令中：</span><span class="sxs-lookup"><span data-stu-id="43a2b-192">In this command:</span></span>

   * <span data-ttu-id="43a2b-193">容器名称 `gs-spring-boot-docker` 会立即在 `expose deployment` 命令后指定</span><span class="sxs-lookup"><span data-stu-id="43a2b-193">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="43a2b-194">`--type` 参数指定此群集使用负载均衡器</span><span class="sxs-lookup"><span data-stu-id="43a2b-194">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="43a2b-195">`--port` 参数指定面向公众的 TCP 端口 80。</span><span class="sxs-lookup"><span data-stu-id="43a2b-195">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="43a2b-196">在此端口访问应用。</span><span class="sxs-lookup"><span data-stu-id="43a2b-196">You access the app on this port.</span></span>

   * <span data-ttu-id="43a2b-197">`--target-port` 参数指定内部 TCP 端口 8080。</span><span class="sxs-lookup"><span data-stu-id="43a2b-197">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="43a2b-198">负载均衡器在此端口将请求转发到你的应用。</span><span class="sxs-lookup"><span data-stu-id="43a2b-198">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="43a2b-199">将应用部署到群集后，请查询外部 IP 地址并在 Web 浏览器中打开：</span><span class="sxs-lookup"><span data-stu-id="43a2b-199">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![在 Azure 上浏览示例应用][SB02]


## <a name="next-steps"></a><span data-ttu-id="43a2b-201">后续步骤</span><span class="sxs-lookup"><span data-stu-id="43a2b-201">Next steps</span></span>

<span data-ttu-id="43a2b-202">有关使用 Azure 上的 Spring Boot 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="43a2b-202">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="43a2b-203">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="43a2b-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="43a2b-204">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上</span><span class="sxs-lookup"><span data-stu-id="43a2b-204">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="43a2b-205">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-205">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="43a2b-206"><!-- Newly added --> 有关使用 Visual Studio Code 将 Java 应用程序部署到 Kubernetes 的详细信息，请参阅 [Visual Studio Code Java 教程]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-206"><!-- Newly added --> For more information about deploying a Java application to Kubernetes with Visual Studio Code, see [Visual Studio Code Java Tutorials].</span></span>

<span data-ttu-id="43a2b-207">有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅[Docker 上的 Spring Boot 入门]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-207">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="43a2b-208">以下链接提供了创建 Spring Boot 应用程序的详细信息：</span><span class="sxs-lookup"><span data-stu-id="43a2b-208">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="43a2b-209">有关创建简单 Spring Boot 应用程序的详细信息，请参阅 https://start.spring.io/ 中的 Spring Initializr。</span><span class="sxs-lookup"><span data-stu-id="43a2b-209">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="43a2b-210">以下链接提供了将 Kubernetes 与 Azure 配合使用的详细信息：</span><span class="sxs-lookup"><span data-stu-id="43a2b-210">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="43a2b-211">Azure Kubernetes 服务中的 Kubernetes 群集入门</span><span class="sxs-lookup"><span data-stu-id="43a2b-211">Get started with a Kubernetes cluster in Azure Kubernetes Service</span></span>](https://docs.microsoft.com/azure/aks/intro-kubernetes)

<span data-ttu-id="43a2b-212">有关使用 Kubernetes 命令行接口的详细信息，请在 <https://kubernetes.io/docs/user-guide/kubectl/> 处参阅 **kubectl** 用户指南。</span><span class="sxs-lookup"><span data-stu-id="43a2b-212">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="43a2b-213">Kubernetes 网站中有多篇文章讨论有关在私有注册表中使用映像的信息：</span><span class="sxs-lookup"><span data-stu-id="43a2b-213">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="43a2b-214">[为 Pod 配置服务帐户]</span><span class="sxs-lookup"><span data-stu-id="43a2b-214">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="43a2b-215">[命名空间]</span><span class="sxs-lookup"><span data-stu-id="43a2b-215">[Namespaces]</span></span>
* <span data-ttu-id="43a2b-216">[从私有注册表拉取映像]</span><span class="sxs-lookup"><span data-stu-id="43a2b-216">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="43a2b-217">有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="43a2b-217">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Kubernetes 服务 (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[Azure Kubernetes Service (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[为 Pod 配置服务帐户]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空间]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[从私有注册表拉取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- Newly added -->
[使用 Azure 容器注册表从 Azure Kubernetes 服务进行身份验证]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks/
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java 教程]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Visual Studio Code Java Tutorials]: https://code.visualstudio.com/docs/java/java-kubernetes/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
