---
title: 如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure
description: 了解如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到 Azure。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: bcc56a92e2fd6891cdccb92c5541787f227d828a
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991491"
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-containerized-spring-boot-app-to-azure"></a><span data-ttu-id="136cd-103">如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="136cd-103">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>

<span data-ttu-id="136cd-104">本文演示如何使用[适用于 Azure Web 应用的 Maven 插件](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin)将 Docker 容器中的示例 Spring Boot 应用程序部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="136cd-104">This article demonstrates using the [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) to deploy a sample Spring Boot application in a Docker container to Azure App Services.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="136cd-105">用于 [Apache Maven](http://maven.apache.org/) 的适用于 Azure Web 应用的 Maven 插件提供了 Azure 应用服务到 Maven 项目的无缝集成，并简化了开发人员将 Web 应用部署到 Azure 应用服务的过程。</span><span class="sxs-lookup"><span data-stu-id="136cd-105">The Maven Plugin for Azure Web Apps for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>
> 
> <span data-ttu-id="136cd-106">适用于 Azure Web 应用的 Maven 插件当前提供预览版。</span><span class="sxs-lookup"><span data-stu-id="136cd-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="136cd-107">目前，仅支持 FTP 发布，但计划在未来支持其他功能。</span><span class="sxs-lookup"><span data-stu-id="136cd-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="136cd-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="136cd-108">Prerequisites</span></span>

<span data-ttu-id="136cd-109">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="136cd-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="136cd-110">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="136cd-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="136cd-111">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="136cd-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="136cd-112">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="136cd-112">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="136cd-113">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="136cd-113">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="136cd-114">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="136cd-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="136cd-115">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="136cd-115">A [Git] client.</span></span>
* <span data-ttu-id="136cd-116">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="136cd-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="136cd-117">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="136cd-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="136cd-118">克隆 Docker Web 应用上的示例 Spring Boot</span><span class="sxs-lookup"><span data-stu-id="136cd-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="136cd-119">本部分将克隆容器化 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="136cd-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="136cd-120">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="136cd-121">- 或 -</span><span class="sxs-lookup"><span data-stu-id="136cd-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="136cd-122">将 [Docker 上的 Spring Boot 入门]示例项目克隆到创建的目录中；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker
   ```

1. <span data-ttu-id="136cd-123">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="136cd-124">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="136cd-125">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="136cd-126">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="136cd-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="136cd-127">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="136cd-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="136cd-128">应当会看到显示了以下消息：**Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="136cd-128">You should see the following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="136cd-129">创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="136cd-129">Create an Azure service principal</span></span>

<span data-ttu-id="136cd-130">本部分将创建 Maven 插件在将容器部署到 Azure 时使用的 Azure 服务主体。</span><span class="sxs-lookup"><span data-stu-id="136cd-130">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="136cd-131">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="136cd-131">Open a command prompt.</span></span>

2. <span data-ttu-id="136cd-132">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="136cd-132">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="136cd-133">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="136cd-133">Follow the instructions to complete the sign-in process.</span></span>

3. <span data-ttu-id="136cd-134">创建 Azure 服务主体：</span><span class="sxs-lookup"><span data-stu-id="136cd-134">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="136cd-135">其中：</span><span class="sxs-lookup"><span data-stu-id="136cd-135">Where:</span></span>

   | <span data-ttu-id="136cd-136">参数</span><span class="sxs-lookup"><span data-stu-id="136cd-136">Parameter</span></span>  |                    <span data-ttu-id="136cd-137">说明</span><span class="sxs-lookup"><span data-stu-id="136cd-137">Description</span></span>                     |
   |------------|----------------------------------------------------|
   | `uuuuuuuu` | <span data-ttu-id="136cd-138">指定服务主体的用户名。</span><span class="sxs-lookup"><span data-stu-id="136cd-138">Specifies the user name for the service principal.</span></span> |
   | `pppppppp` | <span data-ttu-id="136cd-139">指定服务主体的密码。</span><span class="sxs-lookup"><span data-stu-id="136cd-139">Specifies the password for the service principal.</span></span>  |


4. <span data-ttu-id="136cd-140">Azure 使用与以下示例类似的 JSON 进行响应：</span><span class="sxs-lookup"><span data-stu-id="136cd-140">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="136cd-141">当配置 Maven 插件以将容器部署到 Azure 时，会使用此 JSON 响应中的值。</span><span class="sxs-lookup"><span data-stu-id="136cd-141">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="136cd-142">`aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是占位符值，在本示例中用于在下一部分中配置 Maven `settings.xml` 文件时，能够更加方便地将这些值映射到其各自的元素。</span><span class="sxs-lookup"><span data-stu-id="136cd-142">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="136cd-143">配置 Maven 以使用 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="136cd-143">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="136cd-144">在本部分中，将使用 Azure 服务主体中的值配置 Maven 在将容器部署到 Azure 时要使用的身份验证。</span><span class="sxs-lookup"><span data-stu-id="136cd-144">In this section, you use the values from your Azure service principal to configure the authentication that Maven will use when deploying your container to Azure.</span></span>

1. <span data-ttu-id="136cd-145">在文本编辑器中打开 Maven `settings.xml` 文件；此文件可能位于如以下示例所示的路径中：</span><span class="sxs-lookup"><span data-stu-id="136cd-145">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

2. <span data-ttu-id="136cd-146">将本教程上一部分中的 Azure 服务主体设置添加到 settings.xml 文件中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-146">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="136cd-147">其中：</span><span class="sxs-lookup"><span data-stu-id="136cd-147">Where:</span></span>

   |     <span data-ttu-id="136cd-148">元素</span><span class="sxs-lookup"><span data-stu-id="136cd-148">Element</span></span>     |                                                                                   <span data-ttu-id="136cd-149">说明</span><span class="sxs-lookup"><span data-stu-id="136cd-149">Description</span></span>                                                                                   |
   |-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `<id>`      |                                <span data-ttu-id="136cd-150">指定在将 Web 应用部署到 Azure 时，Maven 用于查找安全设置的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="136cd-150">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>                                |
   |   `<client>`    |                                                             <span data-ttu-id="136cd-151">包含服务主体的 `appId` 值。</span><span class="sxs-lookup"><span data-stu-id="136cd-151">Contains the `appId` value from your service principal.</span></span>                                                             |
   |   `<tenant>`    |                                                            <span data-ttu-id="136cd-152">包含服务主体的 `tenant` 值。</span><span class="sxs-lookup"><span data-stu-id="136cd-152">Contains the `tenant` value from your service principal.</span></span>                                                             |
   |     `<key>`     |                                                           <span data-ttu-id="136cd-153">包含服务主体的 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="136cd-153">Contains the `password` value from your service principal.</span></span>                                                            |
   | `<environment>` | <span data-ttu-id="136cd-154">定义目标 Azure 云环境，此示例中为 `AZURE`。</span><span class="sxs-lookup"><span data-stu-id="136cd-154">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="136cd-155">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整的环境列表）</span><span class="sxs-lookup"><span data-stu-id="136cd-155">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span> |


3. <span data-ttu-id="136cd-156">保存并关闭 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="136cd-156">Save and close the *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a><span data-ttu-id="136cd-157">可选：将本地 Docker 文件部署到 Docker 中心</span><span class="sxs-lookup"><span data-stu-id="136cd-157">OPTIONAL: Deploy your local Docker file to Docker Hub</span></span>

<span data-ttu-id="136cd-158">如果拥有 Docker 帐户，则可以在本地构建 Docker 容器映像，并将其推送到 Docker 中心。</span><span class="sxs-lookup"><span data-stu-id="136cd-158">If you have a Docker account, you can build your Docker container image locally and push it to Docker Hub.</span></span> <span data-ttu-id="136cd-159">为此，请按照以下步骤操作。</span><span class="sxs-lookup"><span data-stu-id="136cd-159">To do so, use the following steps.</span></span>

1. <span data-ttu-id="136cd-160">在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件。</span><span class="sxs-lookup"><span data-stu-id="136cd-160">Open the `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="136cd-161">找到 `<containerSettings>` 元素的 `<imageName>` 子元素。</span><span class="sxs-lookup"><span data-stu-id="136cd-161">Locate the `<imageName>` child element of the `<containerSettings>` element.</span></span>

1. <span data-ttu-id="136cd-162">使用你的 Docker 帐户名更新 `${docker.image.prefix}` 值：</span><span class="sxs-lookup"><span data-stu-id="136cd-162">Update the `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="136cd-163">选择以下部署方法之一：</span><span class="sxs-lookup"><span data-stu-id="136cd-163">Choose one of the following deployment methods:</span></span>

   * <span data-ttu-id="136cd-164">使用 Maven 在本地构建容器映像，然后使用 Docker 将容器推送到 Docker 中心：</span><span class="sxs-lookup"><span data-stu-id="136cd-164">Build your container image locally with Maven, and then use Docker to push your container to Docker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```

   * <span data-ttu-id="136cd-165">如果安装了[适用于 Maven 的 Docker 插件]，则可以使用 `-DpushImage` 参数自动将容器映像构建到 Docker 中心：</span><span class="sxs-lookup"><span data-stu-id="136cd-165">If you have the [Docker plugin for Maven] installed, you can automatically build and your container image to Docker Hub by using the `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a><span data-ttu-id="136cd-166">可选：在将容器部署到 Azure 之前自定义 pom.xml</span><span class="sxs-lookup"><span data-stu-id="136cd-166">OPTIONAL: Customize your pom.xml before deploying your container to Azure</span></span>

<span data-ttu-id="136cd-167">在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件，然后找到 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="136cd-167">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="136cd-168">该元素应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="136cd-168">This element should resemble the following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
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

<span data-ttu-id="136cd-169">可以为 Maven 插件修改几个值，[适用于 Azure Web 应用的 Maven 插件]文档中提供了这些元素各自的详细描述。</span><span class="sxs-lookup"><span data-stu-id="136cd-169">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="136cd-170">尽管如此，在本文中有仍几个值得注意的值：</span><span class="sxs-lookup"><span data-stu-id="136cd-170">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="136cd-171">元素</span><span class="sxs-lookup"><span data-stu-id="136cd-171">Element</span></span> | <span data-ttu-id="136cd-172">说明</span><span class="sxs-lookup"><span data-stu-id="136cd-172">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="136cd-173">指定[适用于 Azure Web 应用的 Maven 插件]的版本。</span><span class="sxs-lookup"><span data-stu-id="136cd-173">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="136cd-174">应检查 [Maven 中央存储库](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中列出的版本，确保使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="136cd-174">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<authentication>` | <span data-ttu-id="136cd-175">指定 Azure 的身份验证信息，该信息在本示例中含有包含 `azure-auth` 的 `<serverId>` 元素；Maven 使用该值查找在本文前面部分定义的 Maven settings.xml 文件中的 Azure 服务主体值。</span><span class="sxs-lookup"><span data-stu-id="136cd-175">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="136cd-176">指定目标资源组，在此示例中为 `maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="136cd-176">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="136cd-177">如果资源组不存在，则会在部署过程中进行创建。</span><span class="sxs-lookup"><span data-stu-id="136cd-177">The resource group will be created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="136cd-178">指定 Web 应用的目标名称。</span><span class="sxs-lookup"><span data-stu-id="136cd-178">Specifies the target name for your web app.</span></span> <span data-ttu-id="136cd-179">在此示例中，目标名称为 `maven-linux-app-${maven.build.timestamp}`，此示例附加​​了 `${maven.build.timestamp}` 后缀以避免冲突。</span><span class="sxs-lookup"><span data-stu-id="136cd-179">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="136cd-180">（时间戳是可选项；可为应用名称指定任何唯一的字符串。）</span><span class="sxs-lookup"><span data-stu-id="136cd-180">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="136cd-181">指定目标区域，在此示例中为 `westus`。</span><span class="sxs-lookup"><span data-stu-id="136cd-181">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="136cd-182">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="136cd-182">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<appSettings>` | <span data-ttu-id="136cd-183">指定 Maven 在将 Web 应用部署到 Azure 时使用的任何唯一设置。</span><span class="sxs-lookup"><span data-stu-id="136cd-183">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="136cd-184">在此示例中，`<property>` 元素包含指定应用端口的子元素的名称/值对。</span><span class="sxs-lookup"><span data-stu-id="136cd-184">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span> |

> [!NOTE]
>
> <span data-ttu-id="136cd-185">仅当更改默认端口时，才需要执行此示例中更改端口号的设置。</span><span class="sxs-lookup"><span data-stu-id="136cd-185">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

## <a name="build-and-deploy-your-container-to-azure"></a><span data-ttu-id="136cd-186">构建容器并将其部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="136cd-186">Build and deploy your container to Azure</span></span>

<span data-ttu-id="136cd-187">配置了本文前面部分中的所有设置后，就可以将容器部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="136cd-187">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your container to Azure.</span></span> <span data-ttu-id="136cd-188">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="136cd-188">To do so, use the following steps:</span></span>

1. <span data-ttu-id="136cd-189">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-189">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="136cd-190">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="136cd-190">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="136cd-191">Maven 会将 Web 应用部署到 Azure；如果 Web 应用不存在，则将创建一个。</span><span class="sxs-lookup"><span data-stu-id="136cd-191">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="136cd-192">如果 pom.xml 文件的 `<region>` 元素中指定的区域在启动部署时没有足够的可用服务器，则可能会看到类似于以下示例的错误：</span><span class="sxs-lookup"><span data-stu-id="136cd-192">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
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
> <span data-ttu-id="136cd-193">如果发生这种情况，可以指定另一个区域并重新运行 Maven 命令来部署应用程序。</span><span class="sxs-lookup"><span data-stu-id="136cd-193">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="136cd-194">Web 部署完成后即可使用 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="136cd-194">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="136cd-195">Web 应用将会在“应用服务”中列出：</span><span class="sxs-lookup"><span data-stu-id="136cd-195">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="136cd-197">Web 应用的 URL 会在 Web 应用的“概述”中列出：</span><span class="sxs-lookup"><span data-stu-id="136cd-197">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![确定 Web 应用的 URL][AP02]

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

## <a name="next-steps"></a><span data-ttu-id="136cd-199">后续步骤</span><span class="sxs-lookup"><span data-stu-id="136cd-199">Next steps</span></span>

<span data-ttu-id="136cd-200">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="136cd-200">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="136cd-201">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="136cd-201">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="136cd-202">其他资源</span><span class="sxs-lookup"><span data-stu-id="136cd-202">Additional Resources</span></span>

<span data-ttu-id="136cd-203">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="136cd-203">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="136cd-204">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="136cd-204">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="136cd-205">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="136cd-205">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="136cd-206">如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="136cd-206">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure App Service </span></span>](deploy-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="136cd-207">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="136cd-207">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="136cd-208">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="136cd-208">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="136cd-209">[适用于 Maven 的 Docker 插件]</span><span class="sxs-lookup"><span data-stu-id="136cd-209">[Docker plugin for Maven]</span></span>

<span data-ttu-id="136cd-210">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="136cd-210">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[适用于 Maven 的 Docker 插件]: https://github.com/spotify/docker-maven-plugin
[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-containerized-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-containerized-spring-boot-java-app-with-maven-plugin/AP02.png
