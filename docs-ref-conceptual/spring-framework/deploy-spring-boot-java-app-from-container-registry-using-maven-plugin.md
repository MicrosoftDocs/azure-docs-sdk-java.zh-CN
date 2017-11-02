---
title: "如何使用适用于 Azure Web 应用的 Maven 插件将 Azure 容器注册表中的 Spring Boot 应用部署到 Azure App Service"
description: "本教程介绍使用 Maven 插件将 Azure 容器注册表中的 Spring Boot 应用程序部署到 Azure App Service 的步骤。"
services: container-registry
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Maven
ms.assetid: 
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/11/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: b7c2735cdcdeb3dcbfe91fad6c57616e4e5c620c
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="ebd97-104">如何使用适用于 Azure Web 应用的 Maven 插件将 Azure 容器注册表中的 Spring Boot 应用部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="ebd97-104">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="ebd97-105">[Spring Framework] 是一种常用的开源框架，可帮助 Java 开发人员创建 Web、移动和 API 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ebd97-105">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="ebd97-106">本教程使用通过 [Spring Boot] 创建的示例应用，其为使用 Spring 进行快速入门的惯例方法。</span><span class="sxs-lookup"><span data-stu-id="ebd97-106">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="ebd97-107">本文演示如何将示例 Spring Boot 应用程序部署到 Azure 容器注册表以及在此之后如何使用适用于 Azure Web 应用的 Maven 插件将应用程序部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="ebd97-107">This article demonstrates how to deploy a sample Spring Boot application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebd97-108">适用于 Azure Web 应用的 Maven 插件当前提供预览版。</span><span class="sxs-lookup"><span data-stu-id="ebd97-108">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="ebd97-109">目前，仅支持 FTP 发布，但计划在未来支持其他功能。</span><span class="sxs-lookup"><span data-stu-id="ebd97-109">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="ebd97-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="ebd97-110">Prerequisites</span></span>

<span data-ttu-id="ebd97-111">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="ebd97-111">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="ebd97-112">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="ebd97-112">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="ebd97-113">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="ebd97-113">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="ebd97-114">最新 [Java 开发工具包 (JDK)] 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ebd97-114">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="ebd97-115">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="ebd97-115">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="ebd97-116">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="ebd97-116">A [Git] client.</span></span>
* <span data-ttu-id="ebd97-117">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="ebd97-117">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebd97-118">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="ebd97-118">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="ebd97-119">克隆 Docker Web 应用上的示例 Spring Boot</span><span class="sxs-lookup"><span data-stu-id="ebd97-119">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="ebd97-120">本部分将克隆容器化 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="ebd97-120">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="ebd97-121">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-121">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="ebd97-122">- 或 -</span><span class="sxs-lookup"><span data-stu-id="ebd97-122">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="ebd97-123">将 [Docker 上的 Spring Boot 入门]示例项目克隆到创建的目录中；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-123">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="ebd97-124">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-124">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="ebd97-125">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-125">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="ebd97-126">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-126">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="ebd97-127">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="ebd97-127">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="ebd97-128">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="ebd97-128">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="ebd97-129">应会显示以下消息：“Hello Docker World”</span><span class="sxs-lookup"><span data-stu-id="ebd97-129">You should see the following message displayed: **Hello Docker World**</span></span>

   ![本地浏览示例应用][SB01]

> [!NOTE]
>
> <span data-ttu-id="ebd97-131">在本地使用 Docker 时，可能会看到一条错误，其中说明无法在端口 2375 上连接到本地主机。</span><span class="sxs-lookup"><span data-stu-id="ebd97-131">When you are using Docker locally, you may see an error which states that you cannot connect to localhost on port 2375.</span></span> <span data-ttu-id="ebd97-132">如果发生这种情况，可能需要允许在不使用 TLS 的情况下在本地使用 Docker。</span><span class="sxs-lookup"><span data-stu-id="ebd97-132">If this happens, you may need to enable using Docker locally without TLS.</span></span> <span data-ttu-id="ebd97-133">为此，请打开 Docker 设置，并选中选项“在不使用 TLS 情况下在 TCP://localhost:2375 上公开 Docker 守护程序”。</span><span class="sxs-lookup"><span data-stu-id="ebd97-133">To do so, open your Docker settings and check the option to **Expose Docker Daemon on TCP://localhost:2375 without TLS**.</span></span>
>
> ![在本地 TCP 端口 2375 上公开 Docker 守护程序][TL01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="ebd97-135">创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="ebd97-135">Create an Azure service principal</span></span>

<span data-ttu-id="ebd97-136">本部分将创建 Maven 插件在将容器部署到 Azure 时使用的 Azure 服务主体。</span><span class="sxs-lookup"><span data-stu-id="ebd97-136">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="ebd97-137">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="ebd97-137">Open a command prompt.</span></span>

1. <span data-ttu-id="ebd97-138">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="ebd97-138">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="ebd97-139">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="ebd97-139">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="ebd97-140">创建 Azure 服务主体：</span><span class="sxs-lookup"><span data-stu-id="ebd97-140">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="ebd97-141">其中，`uuuuuuuu` 是服务主体的用户名，`pppppppp` 是服务主体的密码。</span><span class="sxs-lookup"><span data-stu-id="ebd97-141">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="ebd97-142">Azure 使用与以下示例类似的 JSON 进行响应：</span><span class="sxs-lookup"><span data-stu-id="ebd97-142">Azure responds with JSON that resembles the following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="ebd97-143">当配置 Maven 插件以将容器部署到 Azure 时，会使用此 JSON 响应中的值。</span><span class="sxs-lookup"><span data-stu-id="ebd97-143">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="ebd97-144">`aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是占位符值，在本示例中用于在下一部分中配置 Maven `settings.xml` 文件时，能够更加方便地将这些值映射到其各自的元素。</span><span class="sxs-lookup"><span data-stu-id="ebd97-144">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="ebd97-145">使用 Azure CLI 创建 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="ebd97-145">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="ebd97-146">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="ebd97-146">Open a command prompt.</span></span>

1. <span data-ttu-id="ebd97-147">登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="ebd97-147">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="ebd97-148">为本文中要使用的 Azure 资源创建资源组：</span><span class="sxs-lookup"><span data-stu-id="ebd97-148">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="ebd97-149">将此示例中的 `wingtiptoysresources` 替换为资源组的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-149">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="ebd97-150">为 Spring Boot 应用在资源组中创建私有 Azure 容器注册表：</span><span class="sxs-lookup"><span data-stu-id="ebd97-150">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="ebd97-151">将此示例中的 `wingtiptoysregistry` 替换为容器注册表的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-151">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="ebd97-152">检索容器注册表的密码：</span><span class="sxs-lookup"><span data-stu-id="ebd97-152">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="ebd97-153">Azure 会响应密码；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-153">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="ebd97-154">将 Azure 容器注册表和 Azure 服务主体添加到 Maven 设置</span><span class="sxs-lookup"><span data-stu-id="ebd97-154">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="ebd97-155">在文本编辑器中打开 Maven `settings.xml` 文件；此文件可能位于如以下示例所示的路径中：</span><span class="sxs-lookup"><span data-stu-id="ebd97-155">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="ebd97-156">将本文上一部分中的 Azure 容器注册表访问设置添加到 settings.xml 文件中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-156">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="ebd97-157">其中：</span><span class="sxs-lookup"><span data-stu-id="ebd97-157">Where:</span></span>
   <span data-ttu-id="ebd97-158">元素</span><span class="sxs-lookup"><span data-stu-id="ebd97-158">Element</span></span> | <span data-ttu-id="ebd97-159">说明</span><span class="sxs-lookup"><span data-stu-id="ebd97-159">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="ebd97-160">包含私有 Azure 容器注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-160">Contains the name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="ebd97-161">包含私有 Azure 容器注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-161">Contains the name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="ebd97-162">包含在本文上一部分中检索的密码。</span><span class="sxs-lookup"><span data-stu-id="ebd97-162">Contains the password you retrieved in the previous section of this article.</span></span>

1. <span data-ttu-id="ebd97-163">将本文先前部分中的 Azure 服务主体设置添加到 settings.xml 文件中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-163">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="ebd97-164">其中：</span><span class="sxs-lookup"><span data-stu-id="ebd97-164">Where:</span></span>
   <span data-ttu-id="ebd97-165">元素</span><span class="sxs-lookup"><span data-stu-id="ebd97-165">Element</span></span> | <span data-ttu-id="ebd97-166">说明</span><span class="sxs-lookup"><span data-stu-id="ebd97-166">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="ebd97-167">指定在将 Web 应用部署到 Azure 时，Maven 用于查找安全设置的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-167">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="ebd97-168">包含服务主体的 `appId` 值。</span><span class="sxs-lookup"><span data-stu-id="ebd97-168">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="ebd97-169">包含服务主体的 `tenant` 值。</span><span class="sxs-lookup"><span data-stu-id="ebd97-169">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="ebd97-170">包含服务主体的 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="ebd97-170">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="ebd97-171">定义目标 Azure 云环境，此示例中为 `AZURE`。</span><span class="sxs-lookup"><span data-stu-id="ebd97-171">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="ebd97-172">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整的环境列表）</span><span class="sxs-lookup"><span data-stu-id="ebd97-172">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="ebd97-173">保存并关闭 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="ebd97-173">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="ebd97-174">生成 Docker 容器映像并将其推送到 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="ebd97-174">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="ebd97-175">导航到 Spring Boot 应用程序的已完成项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="ebd97-175">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="ebd97-176">使用本教程上一部分中的 Azure 容器注册表的登录服务器值更新 pom.xml 文件中的 `<properties>` 集合，例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-176">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="ebd97-177">其中：</span><span class="sxs-lookup"><span data-stu-id="ebd97-177">Where:</span></span>
   <span data-ttu-id="ebd97-178">元素</span><span class="sxs-lookup"><span data-stu-id="ebd97-178">Element</span></span> | <span data-ttu-id="ebd97-179">说明</span><span class="sxs-lookup"><span data-stu-id="ebd97-179">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="ebd97-180">指定私有 Azure 容器注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-180">Specifies the name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="ebd97-181">指定私有 Azure 容器注册表的 URL，将“.azurecr.io”附加到私有容器注册表名称后即可派生为此 URL。</span><span class="sxs-lookup"><span data-stu-id="ebd97-181">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span>

1. <span data-ttu-id="ebd97-182">确保 pom.xml 文件中 Docker 插件的 `<plugin>` 包含正确的登录服务器地址属性和本教程上一步骤中的注册表名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-182">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="ebd97-183">例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-183">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="ebd97-184">其中：</span><span class="sxs-lookup"><span data-stu-id="ebd97-184">Where:</span></span>
   <span data-ttu-id="ebd97-185">元素</span><span class="sxs-lookup"><span data-stu-id="ebd97-185">Element</span></span> | <span data-ttu-id="ebd97-186">说明</span><span class="sxs-lookup"><span data-stu-id="ebd97-186">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="ebd97-187">指定包含私有 Azure 容器注册表名称的属性。</span><span class="sxs-lookup"><span data-stu-id="ebd97-187">Specifies the property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="ebd97-188">指定包含私有 Azure 容器注册表 URL 的属性。</span><span class="sxs-lookup"><span data-stu-id="ebd97-188">Specifies the property which contains the URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="ebd97-189">导航到 Spring Boot 应用程序的已完成项目目录，然后运行以下命令以重新生成应用程序，并将容器推送到 Azure 容器注册表：</span><span class="sxs-lookup"><span data-stu-id="ebd97-189">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="ebd97-190">可选：浏览至 [Azure 门户]，验证容器注册表中是否具有名为 gs-spring-boot-docker 的 Docker 容器映像。</span><span class="sxs-lookup"><span data-stu-id="ebd97-190">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![验证 Azure 门户中的容器][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="ebd97-192">自定义 pom.xml，然后生成容器并将容器部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="ebd97-192">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="ebd97-193">在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件，然后找到 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="ebd97-193">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="ebd97-194">该元素应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="ebd97-194">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="ebd97-195">可以为 Maven 插件修改几个值，[适用于 Azure Web 应用的 Maven 插件]文档中提供了这些元素各自的详细描述。</span><span class="sxs-lookup"><span data-stu-id="ebd97-195">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="ebd97-196">尽管如此，在本文中有仍几个值得注意的值：</span><span class="sxs-lookup"><span data-stu-id="ebd97-196">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="ebd97-197">元素</span><span class="sxs-lookup"><span data-stu-id="ebd97-197">Element</span></span> | <span data-ttu-id="ebd97-198">说明</span><span class="sxs-lookup"><span data-stu-id="ebd97-198">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="ebd97-199">指定[适用于 Azure Web 应用的 Maven 插件]的版本。</span><span class="sxs-lookup"><span data-stu-id="ebd97-199">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="ebd97-200">应检查 [Maven 中央存储库](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中列出的版本，确保使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="ebd97-200">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="ebd97-201">指定 Azure 的身份验证信息，该信息在本示例中含有包含 `azure-auth` 的 `<serverId>` 元素；Maven 使用该值查找在本文前面部分定义的 Maven settings.xml 文件中的 Azure 服务主体值。</span><span class="sxs-lookup"><span data-stu-id="ebd97-201">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="ebd97-202">指定目标资源组，在此示例中为 `wingtiptoysresources`。</span><span class="sxs-lookup"><span data-stu-id="ebd97-202">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="ebd97-203">如果资源组不存在，则会在部署过程中进行创建。</span><span class="sxs-lookup"><span data-stu-id="ebd97-203">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="ebd97-204">指定 Web 应用的目标名称。</span><span class="sxs-lookup"><span data-stu-id="ebd97-204">Specifies the target name for your web app.</span></span> <span data-ttu-id="ebd97-205">在此示例中，目标名称为 `maven-linux-app-${maven.build.timestamp}`，此示例附加​​了 `${maven.build.timestamp}` 后缀以避免冲突。</span><span class="sxs-lookup"><span data-stu-id="ebd97-205">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="ebd97-206">（时间戳是可选项；可为应用名称指定任何唯一的字符串。）</span><span class="sxs-lookup"><span data-stu-id="ebd97-206">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="ebd97-207">指定目标区域，在此示例中为 `westus`。</span><span class="sxs-lookup"><span data-stu-id="ebd97-207">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="ebd97-208">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="ebd97-208">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="ebd97-209">指定包含容器名称和 URL 的属性。</span><span class="sxs-lookup"><span data-stu-id="ebd97-209">Specifies the properties which contain the name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="ebd97-210">指定 Maven 在将 Web 应用部署到 Azure 时使用的任何唯一设置。</span><span class="sxs-lookup"><span data-stu-id="ebd97-210">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="ebd97-211">在此示例中，`<property>` 元素包含指定应用端口的子元素的名称/值对。</span><span class="sxs-lookup"><span data-stu-id="ebd97-211">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebd97-212">仅当更改默认端口时，才需要执行此示例中更改端口号的设置。</span><span class="sxs-lookup"><span data-stu-id="ebd97-212">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="ebd97-213">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-213">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="ebd97-214">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="ebd97-214">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="ebd97-215">Maven 会将 Web 应用部署到 Azure；如果 Web 应用不存在，则将创建一个。</span><span class="sxs-lookup"><span data-stu-id="ebd97-215">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ebd97-216">如果 pom.xml 文件的 `<region>` 元素中指定的区域在启动部署时没有足够的可用服务器，则可能会看到类似于以下示例的错误：</span><span class="sxs-lookup"><span data-stu-id="ebd97-216">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="ebd97-217">如果发生这种情况，可以指定另一个区域并重新运行 Maven 命令来部署应用程序。</span><span class="sxs-lookup"><span data-stu-id="ebd97-217">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="ebd97-218">Web 部署完成后即可使用 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="ebd97-218">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="ebd97-219">Web 应用将会在“应用服务”中列出：</span><span class="sxs-lookup"><span data-stu-id="ebd97-219">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="ebd97-221">Web 应用的 URL 会在 Web 应用的“概述”中列出：</span><span class="sxs-lookup"><span data-stu-id="ebd97-221">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![确定 Web 应用的 URL][AP02]

## <a name="next-steps"></a><span data-ttu-id="ebd97-223">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ebd97-223">Next steps</span></span>

<span data-ttu-id="ebd97-224">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="ebd97-224">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="ebd97-225">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="ebd97-225">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="ebd97-226">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="ebd97-226">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="ebd97-227">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="ebd97-227">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="ebd97-228">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="ebd97-228">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="ebd97-229">[适用于 Maven 的 Docker 插件]</span><span class="sxs-lookup"><span data-stu-id="ebd97-229">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure 门户]: https://portal.azure.com/
[适用于 Azure Web 应用的 Maven 插件]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service/containers/tutorial-custom-docker-image
[Docker]: https://www.docker.com/
[适用于 Maven 的 Docker 插件]: https://github.com/spotify/docker-maven-plugin
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/AP02.png
[TL01]: ./media/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin/TL01.png