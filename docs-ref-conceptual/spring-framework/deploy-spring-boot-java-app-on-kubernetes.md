---
title: "在 Azure 容器服务中将 Spring Boot 应用部署于 Kubernetes 上"
description: "本教程将指导用户完成在 Microsoft Azure 的 Kubernetes 群集中部署 Spring Boot 应用程序的步骤。"
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Kubernetes
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f72a0eaeb932b400cd12a3ccc43706e890aebf6
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="cdb2d-104">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="cdb2d-104">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="cdb2d-105">[Spring Framework] 是一种常用的开源框架，可帮助 Java 开发人员创建 Web、移动和 API 应用程序。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-105">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="cdb2d-106">本教程使用通过 [Spring Boot] 创建的示例应用，其为使用 Spring 进行快速入门的惯例方法。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-106">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="cdb2d-107">[Kubernetes] 和 [Docker] 是开源解决方案，可帮助开发人员自动部署、扩展和管理在容器中运行的应用程序。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-107">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="cdb2d-108">本教程将指导用户将这两种常用的开源技术进行结合，从而将 Spring Boot 应用程序开发和部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-108">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="cdb2d-109">具体而言，将使用 [Spring Boot] 进行应用程序开发，使用 [Kubernetes] 进行容器部署，使用 [Azure 容器服务 (AKS)] 来托管应用程序。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-109">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (AKS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cdb2d-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="cdb2d-110">Prerequisites</span></span>

* <span data-ttu-id="cdb2d-111">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="cdb2d-112">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="cdb2d-113">最新的 [Java 开发人员工具包 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-113">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="cdb2d-114">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="cdb2d-115">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-115">A [Git] client.</span></span>
* <span data-ttu-id="cdb2d-116">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="cdb2d-117">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="cdb2d-118">创建 Docker 上的 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="cdb2d-118">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="cdb2d-119">以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-119">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="cdb2d-120">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-120">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="cdb2d-121">- 或 -</span><span class="sxs-lookup"><span data-stu-id="cdb2d-121">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="cdb2d-122">将 [Docker 上的 Spring Boot 启动入门]示例项目克隆到目录。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="cdb2d-123">将目录更改为已完成项目。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-123">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="cdb2d-124">使用 Maven 生成和运行示例应用。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-124">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="cdb2d-125">通过浏览到 http://localhost:8080 或使用以下 `curl` 命令测试 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-125">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="cdb2d-126">应会显示以下消息：“Hello Docker World”</span><span class="sxs-lookup"><span data-stu-id="cdb2d-126">You should see the following message displayed: **Hello Docker World**</span></span>

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="cdb2d-128">使用 Azure CLI 创建 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="cdb2d-128">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="cdb2d-129">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-129">Open a command prompt.</span></span>

1. <span data-ttu-id="cdb2d-130">登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-130">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="cdb2d-131">为本教程中使用的 Azure 资源创建资源组。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-131">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="cdb2d-132">在资源组中创建私有 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-132">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="cdb2d-133">本教程的后续步骤会将示例应用作为 Docker 映像推送到此注册表。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-133">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="cdb2d-134">用注册表的唯一名称替换 `wingtiptoysregistry`。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-134">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="cdb2d-135">将应用推送到容器注册表</span><span class="sxs-lookup"><span data-stu-id="cdb2d-135">Push your app to the container registry</span></span>

1. <span data-ttu-id="cdb2d-136">导航到 Maven 安装的配置目录 (default ~/.m2/ or C:\Users\username\.m2)，并使用文本编辑器打开 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-136">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="cdb2d-137">从 Azure CLI 检索容器注册表的密码。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-137">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="cdb2d-138">将你的 Azure 容器注册表 ID 和密码添加到 settings.xml 文件的 `<server>` 集合。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-138">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="cdb2d-139">`id` 和 `username` 是注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-139">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="cdb2d-140">使用上一个命令中的 `password` 值（不带引号）。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-140">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="cdb2d-141">导航到 Spring Boot 应用程序的完整项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-141">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="cdb2d-142">使用 Azure 容器注册表的登录服务器值更新 pom.xml 文件中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-142">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="cdb2d-143">更新 pom.xml 文件中的 `<plugins>` 集合，使 `<plugin>` 包含 Azure 容器注册表的登录服务器地址和注册表名称。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-143">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
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

1. <span data-ttu-id="cdb2d-144">导航到 Spring Boot 应用程序的完成项目目录，然后运行以下命令以生成 Docker 容器并将映像推送到注册表：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-144">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="cdb2d-145">Maven 将映像推送到 Azure 时，你可能会收到类似于以下内容的错误消息：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-145">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="cdb2d-146">如果收到此错误消息，请从 Docker 命令行登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-146">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="cdb2d-147">然后推送你的容器：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-147">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a><span data-ttu-id="cdb2d-148">使用 Azure CLI 在 AKS 上创建 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="cdb2d-148">Create a Kubernetes Cluster on AKS using the Azure CLI</span></span>

1. <span data-ttu-id="cdb2d-149">在 Azure 容器服务中创建 Kubernetes 群集。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-149">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="cdb2d-150">以下命令在 wingtiptoys-kubernetes 资源组中创建 kubernetes 群集，将 wingtiptoys-containerservice 作为群集名称，wingtiptoys-kubernetes 作为 DNS 前缀：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-150">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="cdb2d-151">此命令可能需要一段时间才能完成。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-151">This command may take a while to complete.</span></span>

1. <span data-ttu-id="cdb2d-152">使用 Azure CLI 安装 `kubectl`。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-152">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="cdb2d-153">Linux 用户可能必须将 `sudo` 作为此命令的前缀，因为它将 Kubernetes CLI 部署到 `/usr/local/bin`。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-153">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="cdb2d-154">下载群集配置信息，以便从 Kubernetes Web 界面和 `kubectl` 管理群集。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-154">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="cdb2d-155">将映像部署到 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="cdb2d-155">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="cdb2d-156">本教程使用 `kubectl` 部署应用，以便让你通过 Kubernetes Web 界面浏览部署。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-156">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="cdb2d-157">使用 Kubernetes Web 界面进行部署</span><span class="sxs-lookup"><span data-stu-id="cdb2d-157">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="cdb2d-158">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-158">Open a command prompt.</span></span>

1. <span data-ttu-id="cdb2d-159">在默认浏览器中打开 Kubernetes 群集的配置网站：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-159">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="cdb2d-160">在浏览器中打开 Kubernetes 配置网站后，单击“部署容器化应用”的链接：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-160">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes 配置网站][KB01]

1. <span data-ttu-id="cdb2d-162">显示“部署容器化应用”页面后，请指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-162">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="cdb2d-163">a.</span><span class="sxs-lookup"><span data-stu-id="cdb2d-163">a.</span></span> <span data-ttu-id="cdb2d-164">选择“指定以下应用详细信息”。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-164">Select **Specify app details below**.</span></span>

   <span data-ttu-id="cdb2d-165">b.</span><span class="sxs-lookup"><span data-stu-id="cdb2d-165">b.</span></span> <span data-ttu-id="cdb2d-166">在“应用名称”中输入 Spring Boot 应用程序名称；例如：gs-spring-boot-docker。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-166">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="cdb2d-167">c.</span><span class="sxs-lookup"><span data-stu-id="cdb2d-167">c.</span></span> <span data-ttu-id="cdb2d-168">在容器映像中输入之前的登录服务器和容器映像；例如：“wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest”。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-168">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="cdb2d-169">d.</span><span class="sxs-lookup"><span data-stu-id="cdb2d-169">d.</span></span> <span data-ttu-id="cdb2d-170">选择“外部”服务。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-170">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="cdb2d-171">e.</span><span class="sxs-lookup"><span data-stu-id="cdb2d-171">e.</span></span> <span data-ttu-id="cdb2d-172">在“端口”和“目标端口”文本框中指定外部和内部端口。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-172">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes 配置网站][KB02]


1. <span data-ttu-id="cdb2d-174">单击“部署”对容器进行部署。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-174">Click **Deploy** to deploy the container.</span></span>

   ![部署容器][KB05]

1. <span data-ttu-id="cdb2d-176">部署应用程序后，Spring Boot 应用程序将列在“服务”下方。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-176">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes 服务][KB06]

1. <span data-ttu-id="cdb2d-178">若单击“外部终结点”的链接，则可看见 Spring Boot 应用程序在 Azure 上运行。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-178">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes 服务][KB07]

   ![在 Azure 上浏览示例应用][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="cdb2d-181">使用 kubectl 进行部署</span><span class="sxs-lookup"><span data-stu-id="cdb2d-181">Deploy with kubectl</span></span>

1. <span data-ttu-id="cdb2d-182">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-182">Open a command prompt.</span></span>

1. <span data-ttu-id="cdb2d-183">使用 `kubectl run` 命令在 Kubernetes 群集中运行容器。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-183">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="cdb2d-184">指定 Kubernetes 中应用的服务名称和完整映像名称。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-184">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="cdb2d-185">例如：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-185">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="cdb2d-186">在此命令中：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-186">In this command:</span></span>

   * <span data-ttu-id="cdb2d-187">容器名称 `gs-spring-boot-docker` 会立即在 `run` 命令后指定</span><span class="sxs-lookup"><span data-stu-id="cdb2d-187">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="cdb2d-188">`--image` 参数将组合的登录服务器和映像名称指定为 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="cdb2d-188">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="cdb2d-189">使用 `kubectl expose` 命令外部公开你的 Kubernetes 群集。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-189">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="cdb2d-190">指定你的服务名称、用于访问应用的公开 TCP 端口以及应用侦听的内部目标端口。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-190">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="cdb2d-191">例如：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-191">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="cdb2d-192">在此命令中：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-192">In this command:</span></span>

   * <span data-ttu-id="cdb2d-193">容器名称 `gs-spring-boot-docker` 会立即在 `expose deployment` 命令后指定</span><span class="sxs-lookup"><span data-stu-id="cdb2d-193">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="cdb2d-194">`--type` 参数指定此群集使用负载均衡器</span><span class="sxs-lookup"><span data-stu-id="cdb2d-194">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="cdb2d-195">`--port` 参数指定面向公众的 TCP 端口 80。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-195">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="cdb2d-196">在此端口访问应用。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-196">You access the app on this port.</span></span>

   * <span data-ttu-id="cdb2d-197">`--target-port` 参数指定内部 TCP 端口 8080。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-197">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="cdb2d-198">负载均衡器在此端口将请求转发到你的应用。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-198">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="cdb2d-199">将应用部署到群集后，请查询外部 IP 地址并在 Web 浏览器中打开：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-199">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![在 Azure 上浏览示例应用][SB02]


## <a name="next-steps"></a><span data-ttu-id="cdb2d-201">后续步骤</span><span class="sxs-lookup"><span data-stu-id="cdb2d-201">Next steps</span></span>

<span data-ttu-id="cdb2d-202">有关使用 Azure 上的 Spring Boot 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-202">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="cdb2d-203">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="cdb2d-203">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="cdb2d-204">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上</span><span class="sxs-lookup"><span data-stu-id="cdb2d-204">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

<span data-ttu-id="cdb2d-205">有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-205">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="cdb2d-206">有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅[Docker 上的 Spring Boot 启动入门]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-206">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="cdb2d-207">以下链接提供了创建 Spring Boot 应用程序的详细信息：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-207">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="cdb2d-208">有关创建简单 Spring Boot 应用程序的详细信息，请参阅 Spring Initializr at https://start.spring.io/。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-208">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="cdb2d-209">以下链接提供了将 Kubernetes 与 Azure 配合使用的详细信息：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-209">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="cdb2d-210">容器服务中的 Kubernetes 群集入门</span><span class="sxs-lookup"><span data-stu-id="cdb2d-210">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="cdb2d-211">在 Azure 容器服务中使用 Kubernetes Web UI</span><span class="sxs-lookup"><span data-stu-id="cdb2d-211">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="cdb2d-212">有关使用 Kubernetes 命令行接口的详细信息，请在 < https://kubernetes.io/docs/user-guide/kubectl/ > 处参阅 **kubectl** 用户指南。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-212">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="cdb2d-213">Kubernetes 网站中有多篇文章讨论有关在私有注册表中使用映像的信息：</span><span class="sxs-lookup"><span data-stu-id="cdb2d-213">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="cdb2d-214">[为 Pod 配置服务帐户]</span><span class="sxs-lookup"><span data-stu-id="cdb2d-214">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="cdb2d-215">[命名空间]</span><span class="sxs-lookup"><span data-stu-id="cdb2d-215">[Namespaces]</span></span>
* <span data-ttu-id="cdb2d-216">[从私有注册表拉取映像]</span><span class="sxs-lookup"><span data-stu-id="cdb2d-216">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="cdb2d-217">有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="cdb2d-217">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure 容器服务 (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 启动入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[为 Pod 配置服务帐户]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空间]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[从私有注册表拉取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

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
