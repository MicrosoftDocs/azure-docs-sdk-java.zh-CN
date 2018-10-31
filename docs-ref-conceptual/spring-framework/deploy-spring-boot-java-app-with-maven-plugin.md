---
title: 使用 Maven 和 Azure 将 Spring Boot JAR 文件应用部署到云中
description: 了解如何使用适用于 Linux 的 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到云中。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.author: robmcm
ms.date: 10/18/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: dc3038fed6859203f36e0c4dc9a9b01e81a7c4c5
ms.sourcegitcommit: dae7511a9d93ca7f388d5b0e05dc098e22c2f2f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "49962491"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="13985-103">在 Linux 上将 Spring Boot JAR 文件 Web 应用部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="13985-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="13985-104">本文演示如何使用[适用于 Azure 应用服务 Web 应用的 Maven 插件](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将打包为 Java SE JAR 的 Spring Boot 应用程序部署到 [Linux 上的 Azure 应用服务](https://docs.microsoft.com/en-us/azure/app-service/containers/)。</span><span class="sxs-lookup"><span data-stu-id="13985-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](https://docs.microsoft.com/en-us/azure/app-service/containers/).</span></span> <span data-ttu-id="13985-105">如果要将应用的依赖关系、运行时和配置合并到单个可部署项目中，请选择通过 [Tomcat 和 WAR 文件](/azure/app-service/containers/quickstart-java)进行 Java SE 部署。</span><span class="sxs-lookup"><span data-stu-id="13985-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="13985-106">如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="13985-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13985-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="13985-107">Prerequisites</span></span>

<span data-ttu-id="13985-108">若要完成本教程中的步骤，需要安装和配置以下内容：</span><span class="sxs-lookup"><span data-stu-id="13985-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="13985-109">[Azure CLI](/cli/azure/)，在本地或通过 [Azure Cloud Shell](https://shell.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="13985-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="13985-110">[Java 开发工具包 (JDK)](https://www.azul.com/downloads/azure-only/zulu/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="13985-110">[Java Development Kit (JDK)](https://www.azul.com/downloads/azure-only/zulu/), version 1.7 or later.</span></span>
* <span data-ttu-id="13985-111">Apache 的 [Maven](https://maven.apache.org/) 版本 3）。</span><span class="sxs-lookup"><span data-stu-id="13985-111">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="13985-112">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="13985-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="13985-113">安装并登录 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="13985-113">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="13985-114">获取用于部署 Spring Boot 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="13985-114">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span>

<span data-ttu-id="13985-115">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="13985-115">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="13985-116">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="13985-116">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="13985-117">克隆示例应用</span><span class="sxs-lookup"><span data-stu-id="13985-117">Clone the sample app</span></span>

<span data-ttu-id="13985-118">本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="13985-118">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="13985-119">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-119">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="13985-120">- 或 -</span><span class="sxs-lookup"><span data-stu-id="13985-120">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="13985-121">将 [Spring Boot 入门]示例项目克隆到已创建的目录；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-121">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="13985-122">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-122">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="13985-123">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-123">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="13985-124">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-124">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="13985-125">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="13985-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="13985-126">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="13985-126">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="13985-127">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="13985-127">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="13985-128">配置适用于 Azure 应用服务的 Maven 插件</span><span class="sxs-lookup"><span data-stu-id="13985-128">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="13985-129">在本部分中，将配置 Spring Boot 项目 `pom.xml`，以便 Maven 可以将应用部署到 Linux 上的 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="13985-129">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="13985-130">在代码编辑器中打开 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="13985-130">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="13985-131">在 pom.xml 的 `<build>` 节的 `<plugins>` 标记内添加以下 `<plugin>` 条目。</span><span class="sxs-lookup"><span data-stu-id="13985-131">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. <span data-ttu-id="13985-132">更新插件配置中的以下占位符：</span><span class="sxs-lookup"><span data-stu-id="13985-132">Update the following placeholders in the plugin configuration:</span></span>

| <span data-ttu-id="13985-133">占位符</span><span class="sxs-lookup"><span data-stu-id="13985-133">Placeholder</span></span> | <span data-ttu-id="13985-134">Description</span><span class="sxs-lookup"><span data-stu-id="13985-134">Description</span></span> |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | <span data-ttu-id="13985-135">要在其中创建 Web 应用的新资源组的名称。</span><span class="sxs-lookup"><span data-stu-id="13985-135">Name for the new resource group in which to create your web app.</span></span> <span data-ttu-id="13985-136">通过将应用的所有资源都放在一个组中，可以一起管理它们。</span><span class="sxs-lookup"><span data-stu-id="13985-136">By putting all the resources for an app in a group, you can manage them together.</span></span> <span data-ttu-id="13985-137">例如，删除资源组会删除与该应用关联的所有资源。</span><span class="sxs-lookup"><span data-stu-id="13985-137">For example, deleting the resource group would delete all resources associated with the app.</span></span> <span data-ttu-id="13985-138">使用唯一的新资源组名称（例如 *TestResources*）更新此值。</span><span class="sxs-lookup"><span data-stu-id="13985-138">Update this value with a unique new resource group name, for example, *TestResources*.</span></span> <span data-ttu-id="13985-139">将在后面的部分使用此资源组名称来清除所有 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="13985-139">You will use this resource group name to clean up all Azure resources in a later section.</span></span> |
| `WEBAPP_NAME` | <span data-ttu-id="13985-140">应用名称将成为部署到 Azure 时的 Web 应用 (WEBAPP_NAME.azurewebsites.net) 的主机名的一部分。</span><span class="sxs-lookup"><span data-stu-id="13985-140">The app name will be part the host name for the web app when deployed to Azure (WEBAPP_NAME.azurewebsites.net).</span></span> <span data-ttu-id="13985-141">使用将用于托管 Java 应用的新 Azure Web 应用的唯一名称（例如 *contoso*）更新此值。</span><span class="sxs-lookup"><span data-stu-id="13985-141">Update this value with a unique name for the new Azure web app, which will host your Java app, for example *contoso*.</span></span> |
| `REGION` | <span data-ttu-id="13985-142">托管着 Web 应用的 Azure 区域，例如 `westus2`。</span><span class="sxs-lookup"><span data-stu-id="13985-142">An Azure region where the web app is hosted, for example `westus2`.</span></span> <span data-ttu-id="13985-143">可以从 Cloud Shell 或 CLI 使用 `az account list-locations` 命令获取区域列表。</span><span class="sxs-lookup"><span data-stu-id="13985-143">You can get a list of regions from the Cloud Shell or CLI using the `az account list-locations` command.</span></span> |

<span data-ttu-id="13985-144">可以在 [GitHub 上的 Maven 插件参考](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)中找到完整的配置选项列表。</span><span class="sxs-lookup"><span data-stu-id="13985-144">A full list of configuration options can be found in the [Maven plugin reference on GitHub](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin).</span></span>

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="13985-145">将应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="13985-145">Deploy the app to Azure</span></span>

<span data-ttu-id="13985-146">配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="13985-146">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="13985-147">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="13985-147">To do so, use the following steps:</span></span>

1. <span data-ttu-id="13985-148">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-148">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="13985-149">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="13985-149">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="13985-150">Maven 会将 Web 应用部署到 Azure；如果 Web 应用或 Web 应用计划尚不存在，则将为你创建一个。</span><span class="sxs-lookup"><span data-stu-id="13985-150">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="13985-151">Web 部署完成后即可通过 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="13985-151">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="13985-152">Web 应用将会在“应用服务”中列出：</span><span class="sxs-lookup"><span data-stu-id="13985-152">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="13985-154">Web 应用的 URL 会在 Web 应用的“概述”中列出：</span><span class="sxs-lookup"><span data-stu-id="13985-154">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![确定 Web 应用的 URL][AP02]

<span data-ttu-id="13985-156">通过与以前相同的 cURL 命令（使用来自门户的 Web 应用 URL而不是 `localhost`）验证部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="13985-156">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="13985-157">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="13985-157">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="13985-158">后续步骤</span><span class="sxs-lookup"><span data-stu-id="13985-158">Next steps</span></span>

<span data-ttu-id="13985-159">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="13985-159">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="13985-160">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="13985-160">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="13985-161">如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="13985-161">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="13985-162">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="13985-162">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="13985-163">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="13985-163">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
