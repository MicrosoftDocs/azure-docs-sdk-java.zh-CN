---
title: 在 Azure 容器服务中将 Spring Boot Web 应用部署于 Linux 上
description: 本教程将指导用户完成在 Microsoft Azure 中将 Spring Boot 应用程序部署为 Linux Web 应用的步骤。
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: asirveda;robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: container-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 49d94d11ad6a4e103ded849e477d99f01955c693
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48898721"
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a><span data-ttu-id="480f3-103">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上</span><span class="sxs-lookup"><span data-stu-id="480f3-103">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>

<span data-ttu-id="480f3-104">本教程介绍如何使用 [Docker] 在 [Azure 容器服务 (AKS)] 中开发 [Spring Boot] 应用程序并将其部署到 Linux 主机。</span><span class="sxs-lookup"><span data-stu-id="480f3-104">This tutorial walks you through using [Docker] to develop and deploy a [Spring Boot] application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="480f3-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="480f3-105">Prerequisites</span></span>

<span data-ttu-id="480f3-106">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="480f3-106">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="480f3-107">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="480f3-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="480f3-108">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="480f3-108">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="480f3-109">最新的 [Java 开发人员工具包 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="480f3-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="480f3-110">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="480f3-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="480f3-111">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="480f3-111">A [Git] client.</span></span>
* <span data-ttu-id="480f3-112">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="480f3-112">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="480f3-113">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="480f3-113">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="480f3-114">创建 Docker 上的 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="480f3-114">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="480f3-115">以下步骤将引导你完成以下步骤：创建一个简单的 Spring Boot Web 应用程序并对其进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="480f3-115">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="480f3-116">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-116">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="480f3-117">- 或 -</span><span class="sxs-lookup"><span data-stu-id="480f3-117">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="480f3-118">将 [Docker 上的 Spring Boot 入门]示例项目克隆到创建的目录中；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-118">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="480f3-119">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-119">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="480f3-120">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-120">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="480f3-121">创建 Web 应用后，将目录更改为 JAR 文件所在的 `target` 目录并启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-121">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="480f3-122">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="480f3-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="480f3-123">例如，如果你有可用的 curl，并且已将 Tomcat 服务器配置为在端口 80 上运行：</span><span class="sxs-lookup"><span data-stu-id="480f3-123">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="480f3-124">应该会显示以下消息：Hello Docker World!</span><span class="sxs-lookup"><span data-stu-id="480f3-124">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="480f3-126">创建 Azure 容器注册表以用作专用 Docker 注册表</span><span class="sxs-lookup"><span data-stu-id="480f3-126">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="480f3-127">以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。</span><span class="sxs-lookup"><span data-stu-id="480f3-127">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="480f3-128">如果你想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表](/azure/container-registry/container-registry-get-started-azure-cli)中的步骤操作。</span><span class="sxs-lookup"><span data-stu-id="480f3-128">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
>

1. <span data-ttu-id="480f3-129">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="480f3-129">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="480f3-130">登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。</span><span class="sxs-lookup"><span data-stu-id="480f3-130">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="480f3-131">单击“+ 新建”菜单图标，然后依次单击“容器”、“Azure 容器注册表”。</span><span class="sxs-lookup"><span data-stu-id="480f3-131">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![创建新的 Azure 容器注册表][AR01]

1. <span data-ttu-id="480f3-133">显示 Azure 容器注册表模板的信息页面时，请单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="480f3-133">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![创建新的 Azure 容器注册表][AR02]

1. <span data-ttu-id="480f3-135">当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="480f3-135">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![配置 Azure 容器注册表设置][AR03]

1. <span data-ttu-id="480f3-137">创建容器注册表后，在 Azure 门户中导航到你的容器注册表，然后单击“访问密钥”。</span><span class="sxs-lookup"><span data-stu-id="480f3-137">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="480f3-138">记下用户名和密码，以供后续步骤中使用。</span><span class="sxs-lookup"><span data-stu-id="480f3-138">Take note of the username and password for the next steps.</span></span>

   ![Azure 容器注册表访问密钥][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="480f3-140">配置 Maven 以使用你的 Azure 容器注册表访问密钥</span><span class="sxs-lookup"><span data-stu-id="480f3-140">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="480f3-141">导航到 Maven 安装的配置目录，并使用文本编辑器打开 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="480f3-141">Navigate to the configuration directory for your Maven installation and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="480f3-142">将本教程上一部分中的 Azure 容器注册表访问设置添加到 settings.xml 文件中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-142">Add your Azure Container Registry access settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="480f3-143">导航到 Spring Boot 应用程序的完整项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="480f3-143">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="480f3-144">使用本教程上一部分中的 Azure 容器注册表的登录服务器值更新 pom.xml 文件中的 `<properties>` 集合，例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-144">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="480f3-145">更新 pom.xml 文件中的 `<plugins>` 集合，使 `<plugin>` 包含本教程上一部分中 Azure 容器注册表的登录服务器地址和注册表名称。</span><span class="sxs-lookup"><span data-stu-id="480f3-145">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="480f3-146">例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-146">For example:</span></span>

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

1. <span data-ttu-id="480f3-147">导航到 Spring Boot 应用程序的已完成项目目录，然后运行以下命令以重新生成应用程序，并将容器推送到 Azure 容器注册表：</span><span class="sxs-lookup"><span data-stu-id="480f3-147">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="480f3-148">将 Docker 容器推送到 Azure 时，即使已成功创建 Docker 容器，也可能会收到类似于以下内容的错误消息：</span><span class="sxs-lookup"><span data-stu-id="480f3-148">When you are pushing your Docker container to Azure, you may receive an error message that is similar to one of the following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="480f3-149">如果发生这种情况，可能需要从 Docker 命令行登录到 Azure 帐户，例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-149">If this happens, you may need to sign in to your Azure account from the Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="480f3-150">然后可以从命令行推送容器；例如：</span><span class="sxs-lookup"><span data-stu-id="480f3-150">You can then push your container from the command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="480f3-151">在 Azure 应用服务中使用容器映像创建 Linux 上的 Web 应用</span><span class="sxs-lookup"><span data-stu-id="480f3-151">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="480f3-152">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="480f3-152">Browse to the [Azure portal] and sign in.</span></span>

2. <span data-ttu-id="480f3-153">单击“+ 新建”菜单图标，然后依次单击“Web + 移动”、“Linux 上的 Web 应用”。</span><span class="sxs-lookup"><span data-stu-id="480f3-153">Click the menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![在 Azure 门户中创建新的 Web 应用][LX01]

3. <span data-ttu-id="480f3-155">当显示“Linux 上的 Web 应用”页时，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="480f3-155">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="480f3-156">a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。</span><span class="sxs-lookup"><span data-stu-id="480f3-156">a.</span></span> <span data-ttu-id="480f3-157">为“应用名称”输入唯一名称；例如：“wingtiptoyslinux”。</span><span class="sxs-lookup"><span data-stu-id="480f3-157">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="480f3-158">b.</span><span class="sxs-lookup"><span data-stu-id="480f3-158">b.</span></span> <span data-ttu-id="480f3-159">从下拉列表中选择一个订阅。</span><span class="sxs-lookup"><span data-stu-id="480f3-159">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="480f3-160">c.</span><span class="sxs-lookup"><span data-stu-id="480f3-160">c.</span></span> <span data-ttu-id="480f3-161">选择现有资源组，或指定名称以创建新资源组。</span><span class="sxs-lookup"><span data-stu-id="480f3-161">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="480f3-162">d.</span><span class="sxs-lookup"><span data-stu-id="480f3-162">d.</span></span> <span data-ttu-id="480f3-163">单击“配置容器”边栏选项卡，然后输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="480f3-163">Click **Configure container** and enter the following information:</span></span>

   * <span data-ttu-id="480f3-164">选择“专用注册表”。</span><span class="sxs-lookup"><span data-stu-id="480f3-164">Choose **Private registry**.</span></span>

   * <span data-ttu-id="480f3-165">“映像和可选标记”：根据上文指定容器名称；例如：“wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest”</span><span class="sxs-lookup"><span data-stu-id="480f3-165">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

   * <span data-ttu-id="480f3-166">**服务器 URL**：指定前面记下的注册表 URL，例如“*<https://wingtiptoysregistry.azurecr.io>*”。</span><span class="sxs-lookup"><span data-stu-id="480f3-166">**Server URL**: Specify your registry URL from earlier; for example: "*<https://wingtiptoysregistry.azurecr.io>*"</span></span>

   * <span data-ttu-id="480f3-167">“登录用户名”和“密码”：根据先前步骤中使用的“访问密钥”指定登录凭据。</span><span class="sxs-lookup"><span data-stu-id="480f3-167">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="480f3-168">e.</span><span class="sxs-lookup"><span data-stu-id="480f3-168">e.</span></span> <span data-ttu-id="480f3-169">输入上述所有信息后，请单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="480f3-169">Once you have entered all of the above information, click **OK**.</span></span>

   ![配置 Web 应用设置][LX02]

4. <span data-ttu-id="480f3-171">单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="480f3-171">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="480f3-172">Azure 会将 Internet 请求自动映射到在标准端口 80 或 8080 上运行的嵌入式 Tomcat 服务器。</span><span class="sxs-lookup"><span data-stu-id="480f3-172">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="480f3-173">但是，如果已将嵌入式 Tomcat 服务器配置为在自定义端口上运行，则需要将一个环境变量添加到为嵌入式 Tomcat 服务器定义端口的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="480f3-173">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="480f3-174">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="480f3-174">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="480f3-175">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="480f3-175">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="480f3-176">单击“应用服务”图标。</span><span class="sxs-lookup"><span data-stu-id="480f3-176">Click the icon for **App Services**.</span></span> <span data-ttu-id="480f3-177">（请参阅下图中的第 1 项。）</span><span class="sxs-lookup"><span data-stu-id="480f3-177">(See item #1 in the image below.)</span></span>
>
> 3. <span data-ttu-id="480f3-178">从列表中选择你的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="480f3-178">Select your web app from the list.</span></span> <span data-ttu-id="480f3-179">（请参阅下图中的第 2 项。）</span><span class="sxs-lookup"><span data-stu-id="480f3-179">(Item #2 in the image below.)</span></span>
>
> 4. <span data-ttu-id="480f3-180">单击“应用程序设置”。</span><span class="sxs-lookup"><span data-stu-id="480f3-180">Click **Application Settings**.</span></span> <span data-ttu-id="480f3-181">（请参阅下图中的第 3 项。）</span><span class="sxs-lookup"><span data-stu-id="480f3-181">(Item #3 in the image below.)</span></span>
>
> 5. <span data-ttu-id="480f3-182">在“应用设置”部分中，添加一个名为 **PORT** 的新环境变量，并为该值输入自定义端口号。</span><span class="sxs-lookup"><span data-stu-id="480f3-182">In the **App settings** section, add a new environment variable named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="480f3-183">（请参阅下图中的第 4 项。）</span><span class="sxs-lookup"><span data-stu-id="480f3-183">(Item #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="480f3-184">单击“ **保存**”。</span><span class="sxs-lookup"><span data-stu-id="480f3-184">Click **Save**.</span></span> <span data-ttu-id="480f3-185">（请参阅下图中的第 5 项。）</span><span class="sxs-lookup"><span data-stu-id="480f3-185">(Item #5 in the image below.)</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="480f3-187">后续步骤</span><span class="sxs-lookup"><span data-stu-id="480f3-187">Next steps</span></span>

<span data-ttu-id="480f3-188">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="480f3-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="480f3-189">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="480f3-189">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="480f3-190">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="480f3-190">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="480f3-191">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="480f3-191">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="480f3-192">有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅 [Docker 上的 Spring Boot 入门]。</span><span class="sxs-lookup"><span data-stu-id="480f3-192">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="480f3-193">如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。</span><span class="sxs-lookup"><span data-stu-id="480f3-193">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="480f3-194">有关开始创建简单 Spring Boot 应用程序入门的详细信息，请参阅 https://start.spring.io/ 上的“Spring Initializr”。</span><span class="sxs-lookup"><span data-stu-id="480f3-194">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="480f3-195">有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="480f3-195">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure 容器服务 (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
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
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

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
