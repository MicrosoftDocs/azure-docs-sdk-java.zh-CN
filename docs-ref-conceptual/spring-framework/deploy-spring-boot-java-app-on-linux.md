---
title: 在用于容器的 Azure 应用服务上部署 Spring Boot Web 应用
description: 本教程将指导用户完成在 Microsoft Azure 中将 Spring Boot 应用程序部署为 Linux Web 应用的步骤。
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270868"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a><span data-ttu-id="816bf-103">在用于容器的 Azure 应用服务上部署 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="816bf-103">Deploy a Spring Boot application on Azure App Service for Container</span></span>

<span data-ttu-id="816bf-104">本教程介绍如何使用 [Docker] 将 [Spring Boot] 应用程序容器化并将自己的 docker 映像部署到 [Azure 应用服务](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)中的 Linux 主机。</span><span class="sxs-lookup"><span data-stu-id="816bf-104">This tutorial walks you through using [Docker] to containerize your [Spring Boot] application and deploy your own docker image to a Linux host in the [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="816bf-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="816bf-105">Prerequisites</span></span>

<span data-ttu-id="816bf-106">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="816bf-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="816bf-107">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="816bf-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="816bf-108">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="816bf-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="816bf-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="816bf-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="816bf-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="816bf-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="816bf-111">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="816bf-111">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="816bf-112">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="816bf-112">A [Git] client.</span></span>
* <span data-ttu-id="816bf-113">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="816bf-113">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="816bf-114">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="816bf-114">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="816bf-115">创建 Docker 上的 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="816bf-115">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="816bf-116">以下步骤将引导你完成以下步骤：创建一个简单的 Spring Boot Web 应用程序并对其进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="816bf-116">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="816bf-117">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-117">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="816bf-118">- 或 -</span><span class="sxs-lookup"><span data-stu-id="816bf-118">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="816bf-119">将 [Docker 上的 Spring Boot 入门]示例项目克隆到创建的目录中；例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-119">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="816bf-120">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-120">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="816bf-121">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-121">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="816bf-122">创建 Web 应用后，将目录更改为 JAR 文件所在的 `target` 目录并启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-122">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="816bf-123">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="816bf-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="816bf-124">例如，如果你有可用的 curl，并且已将 Tomcat 服务器配置为在端口 80 上运行：</span><span class="sxs-lookup"><span data-stu-id="816bf-124">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="816bf-125">应当会看到显示了以下消息：**Hello Docker World!**</span><span class="sxs-lookup"><span data-stu-id="816bf-125">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="816bf-127">创建 Azure 容器注册表以用作专用 Docker 注册表</span><span class="sxs-lookup"><span data-stu-id="816bf-127">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="816bf-128">以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。</span><span class="sxs-lookup"><span data-stu-id="816bf-128">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="816bf-129">如果你想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表](/azure/container-registry/container-registry-get-started-azure-cli)中的步骤操作。</span><span class="sxs-lookup"><span data-stu-id="816bf-129">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="816bf-130">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="816bf-130">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="816bf-131">登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。</span><span class="sxs-lookup"><span data-stu-id="816bf-131">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="816bf-132">单击“+ 新建”  菜单图标，然后依次单击“容器”、  “Azure 容器注册表”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-132">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![创建新的 Azure 容器注册表][AR01]

1. <span data-ttu-id="816bf-134">显示 Azure 容器注册表模板的信息页面时，请单击“创建”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-134">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![创建新的 Azure 容器注册表][AR02]

1. <span data-ttu-id="816bf-136">当显示“创建容器注册表”  页时，输入你的“注册表名称”  和“资源组”  ，为“管理员用户”  选择“启用”  ，然后单击“创建”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-136">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![配置 Azure 容器注册表设置][AR03]

1. <span data-ttu-id="816bf-138">创建容器注册表后，在 Azure 门户中导航到你的容器注册表，然后单击“访问密钥”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-138">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="816bf-139">记下用户名和密码，以供后续步骤中使用。</span><span class="sxs-lookup"><span data-stu-id="816bf-139">Take note of the username and password for the next steps.</span></span>

   ![Azure 容器注册表访问密钥][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="816bf-141">配置 Maven 以使用你的 Azure 容器注册表访问密钥</span><span class="sxs-lookup"><span data-stu-id="816bf-141">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="816bf-142">导航到 Spring Boot 应用程序的已完成项目目录（例如：“C:\SpringBoot\gs-spring-boot-docker\complete”  或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”  ），并使用文本编辑器打开 pom.xml  文件。</span><span class="sxs-lookup"><span data-stu-id="816bf-142">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="816bf-143">使用本教程上一部分提供的最新版 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)、登录服务器值以及 Azure 容器注册表的访问设置更新 pom.xml  文件中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="816bf-143">Update the `<properties>` collection in the *pom.xml* file with the latest version of [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) and login server value and access settings for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="816bf-144">例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-144">For example:</span></span>

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. <span data-ttu-id="816bf-145">将 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 添加到 *pom.xml* 文件中的 `<plugins>` 集合，在 `<from>/<image>` 中指定基础映像，指定最终映像名称 `<to>/<image>`，并在 `<to>/<auth>` 中指定上一部分提供的用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="816bf-145">Add [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) to the `<plugins>` collection in the *pom.xml* file, specify the base image at `<from>/<image>` and  final image name `<to>/<image>`, specify the username and password from previous section at `<to>/<auth>`.</span></span> <span data-ttu-id="816bf-146">例如：</span><span class="sxs-lookup"><span data-stu-id="816bf-146">For example:</span></span>

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="816bf-147">导航到 Spring Boot 应用程序的已完成项目目录，然后运行以下命令以重新生成应用程序，并将容器推送到 Azure 容器注册表：</span><span class="sxs-lookup"><span data-stu-id="816bf-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> <span data-ttu-id="816bf-148">使用 Jib 将映像推送到 Azure 容器注册表时，该映像不会认可 *Dockerfile*。有关详细信息，请参阅[此](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html)文档。</span><span class="sxs-lookup"><span data-stu-id="816bf-148">When you are using Jib to push your image to the Azure Container Registry, the image will not honor *Dockerfile*, see [this](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) document for details.</span></span>
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="816bf-149">在 Azure 应用服务中使用容器映像创建 Linux 上的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="816bf-149">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="816bf-150">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="816bf-150">Browse to the [Azure portal] and sign in.</span></span>

2. <span data-ttu-id="816bf-151">依次单击“+ 创建资源”  菜单图标、“Web”、  “用于容器的 Web 应用”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-151">Click the menu icon for **+ Create a resource**, then click **Web**, and then click **Web App for Containers**.</span></span>
   
   ![在 Azure 门户中创建新的 Web 应用][LX01]

3. <span data-ttu-id="816bf-153">当显示“Linux 上的 Web 应用”  页时，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="816bf-153">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="816bf-154">a.</span><span class="sxs-lookup"><span data-stu-id="816bf-154">a.</span></span> <span data-ttu-id="816bf-155">为“应用名称”输入唯一名称；例如：“wingtiptoyslinux”   。</span><span class="sxs-lookup"><span data-stu-id="816bf-155">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*"</span></span>

   <span data-ttu-id="816bf-156">b.</span><span class="sxs-lookup"><span data-stu-id="816bf-156">b.</span></span> <span data-ttu-id="816bf-157">从下拉列表中选择一个订阅  。</span><span class="sxs-lookup"><span data-stu-id="816bf-157">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="816bf-158">c.</span><span class="sxs-lookup"><span data-stu-id="816bf-158">c.</span></span> <span data-ttu-id="816bf-159">选择现有资源组  ，或指定名称以创建新资源组。</span><span class="sxs-lookup"><span data-stu-id="816bf-159">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="816bf-160">d.</span><span class="sxs-lookup"><span data-stu-id="816bf-160">d.</span></span> <span data-ttu-id="816bf-161">选择 *Linux* 作为 **OS**。</span><span class="sxs-lookup"><span data-stu-id="816bf-161">Choose *Linux* as the **OS**.</span></span>

   <span data-ttu-id="816bf-162">e.</span><span class="sxs-lookup"><span data-stu-id="816bf-162">e.</span></span> <span data-ttu-id="816bf-163">单击“应用服务计划/位置”，选择现有的应用服务计划；或者单击“新建”，创建新的应用服务计划。  </span><span class="sxs-lookup"><span data-stu-id="816bf-163">Click **App Service plan/Location** and choose an existing app service plan, or click **Create new** to create a new app service plan.</span></span>

   <span data-ttu-id="816bf-164">f.</span><span class="sxs-lookup"><span data-stu-id="816bf-164">f.</span></span> <span data-ttu-id="816bf-165">单击“配置容器”  边栏选项卡，然后输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="816bf-165">Click **Configure container** and enter the following information:</span></span>

   * <span data-ttu-id="816bf-166">选择“单一容器”和“Azure 容器注册表”。  </span><span class="sxs-lookup"><span data-stu-id="816bf-166">Choose **Single Container** and  **Azure Container Registry**.</span></span>

   * <span data-ttu-id="816bf-167">**注册表**：选择此前创建的容器名称，例如“*wingtiptoysregistry*”</span><span class="sxs-lookup"><span data-stu-id="816bf-167">**Registry**: Choose your container name created earlier; for example: "*wingtiptoysregistry*"</span></span>

   * <span data-ttu-id="816bf-168">**映像**：选择映像名称，例如“*gs-spring-boot-docker*”</span><span class="sxs-lookup"><span data-stu-id="816bf-168">**Image**: Choose the image name; for example: "*gs-spring-boot-docker*"</span></span>
   
   * <span data-ttu-id="816bf-169">**标记**：选择映像的标记，例如“*latest*”</span><span class="sxs-lookup"><span data-stu-id="816bf-169">**Tag**: Choose the tag for the image; for example: "*latest*"</span></span>
   
   * <span data-ttu-id="816bf-170">**启动文件**：将此项留空，因为映像已经有启动命令</span><span class="sxs-lookup"><span data-stu-id="816bf-170">**Startup File**: Keep it blank since the image already has the start up command</span></span>
   
   <span data-ttu-id="816bf-171">e.</span><span class="sxs-lookup"><span data-stu-id="816bf-171">e.</span></span> <span data-ttu-id="816bf-172">输入上述所有信息后，请单击“应用”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-172">Once you have entered all of the above information, click **Apply**.</span></span>

   ![配置 Web 应用设置][LX02]

4. <span data-ttu-id="816bf-174">单击“创建”。 </span><span class="sxs-lookup"><span data-stu-id="816bf-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="816bf-175">Azure 会将 Internet 请求自动映射到在标准端口 80 或 8080 上运行的嵌入式 Tomcat 服务器。</span><span class="sxs-lookup"><span data-stu-id="816bf-175">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="816bf-176">但是，如果已将嵌入式 Tomcat 服务器配置为在自定义端口上运行，则需要将一个环境变量添加到为嵌入式 Tomcat 服务器定义端口的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="816bf-176">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="816bf-177">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="816bf-177">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="816bf-178">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="816bf-178">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="816bf-179">单击**应用服务**的图标，然后从列表中选择你的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="816bf-179">Click the icon for **App Services**, and select your web app from the list.</span></span>
>
> 4. <span data-ttu-id="816bf-180">单击“配置”  。</span><span class="sxs-lookup"><span data-stu-id="816bf-180">Click **Configuration**.</span></span> <span data-ttu-id="816bf-181">（下图中的第 1 项。）</span><span class="sxs-lookup"><span data-stu-id="816bf-181">(Item #1 in the image below.)</span></span>
>
> 5. <span data-ttu-id="816bf-182">在“应用程序设置”  部分中，添加一个名为 **PORT** 的新设置，并为该值输入自定义端口号。</span><span class="sxs-lookup"><span data-stu-id="816bf-182">In the **Application settings** section, add a new setting named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="816bf-183">（下图中的第 2、3、4 项。）</span><span class="sxs-lookup"><span data-stu-id="816bf-183">(Item #2, #3, #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="816bf-184">单击“ **保存**”。</span><span class="sxs-lookup"><span data-stu-id="816bf-184">Click **Save**.</span></span> <span data-ttu-id="816bf-185">（请参阅下图中的第 5 项。）</span><span class="sxs-lookup"><span data-stu-id="816bf-185">(Item #5 in the image below.)</span></span>
>
> ![在 Azure 门户中保存自定义端口号][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="816bf-187">后续步骤</span><span class="sxs-lookup"><span data-stu-id="816bf-187">Next steps</span></span>

<span data-ttu-id="816bf-188">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="816bf-188">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="816bf-189">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="816bf-189">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="816bf-190">其他资源</span><span class="sxs-lookup"><span data-stu-id="816bf-190">Additional Resources</span></span>

<span data-ttu-id="816bf-191">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="816bf-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="816bf-192">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="816bf-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="816bf-193">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="816bf-193">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="816bf-194">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="816bf-194">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="816bf-195">有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅 [Docker 上的 Spring Boot 入门]。</span><span class="sxs-lookup"><span data-stu-id="816bf-195">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="816bf-196">如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。</span><span class="sxs-lookup"><span data-stu-id="816bf-196">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="816bf-197">有关开始创建简单 Spring Boot 应用程序入门的详细信息，请参阅 https://start.spring.io/ 上的“Spring Initializr”。</span><span class="sxs-lookup"><span data-stu-id="816bf-197">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="816bf-198">有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="816bf-198">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Working with Azure DevOps and Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
