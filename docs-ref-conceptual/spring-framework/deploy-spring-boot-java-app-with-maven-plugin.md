---
title: 使用 Maven 和 Azure 将 Spring Boot 应用部署到云中
description: 了解如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到云中。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ca788354d26964bd9f1e21a0d3a8005ff65ce4bc
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324343"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a><span data-ttu-id="2f6c5-103">使用适用于 Azure 应用服务的 Maven 插件将 Spring Boot 应用部署到云中</span><span class="sxs-lookup"><span data-stu-id="2f6c5-103">Deploy a Spring Boot app to the cloud using the Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="2f6c5-104">本文演示如何使用适用于 Azure 应用服务 Web 应用的 Maven 插件来部署一个示例 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-104">This article demonstrates using the Maven Plugin for Azure App Service Web Apps to deploy a sample Spring Boot application.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="2f6c5-105">用于 [Apache Maven](http://maven.apache.org/) 的[适用于 Azure 应用服务 Web 应用的 Maven 插件](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将 Azure 应用服务无缝集成到 Maven 项目，简化了开发人员将 Web 应用部署到 Azure 应用服务的过程。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-105">The [Maven Plugin for Azure App Service Web Apps](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="2f6c5-106">在使用 Maven 插件之前，请在 Maven Central 中检查该插件的最新可用版本：[![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span><span class="sxs-lookup"><span data-stu-id="2f6c5-106">Before using the Maven plugin, check on Maven Central for the latest available version of the plugin: [![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2f6c5-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="2f6c5-107">Prerequisites</span></span>

<span data-ttu-id="2f6c5-108">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-108">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="2f6c5-109">一个 Azure 订阅；如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-109">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="2f6c5-110">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="2f6c5-111">最新 [Java 开发工具包 (JDK)] 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-111">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="2f6c5-112">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="2f6c5-113">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-113">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="2f6c5-114">克隆示例 Spring Boot Web 应用</span><span class="sxs-lookup"><span data-stu-id="2f6c5-114">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="2f6c5-115">本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-115">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="2f6c5-116">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-116">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="2f6c5-117">- 或 -</span><span class="sxs-lookup"><span data-stu-id="2f6c5-117">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="2f6c5-118">将 [Spring Boot 入门]示例项目克隆到已创建的目录；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-118">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="2f6c5-119">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-119">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="2f6c5-120">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-120">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="2f6c5-121">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-121">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="2f6c5-122">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-122">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="2f6c5-123">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-123">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="2f6c5-124">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="2f6c5-124">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a><span data-ttu-id="2f6c5-125">根据 Azure 应用服务中基于 WAR 的部署调整项目</span><span class="sxs-lookup"><span data-stu-id="2f6c5-125">Adjust project for WAR-based deployment on Azure App Service</span></span>

<span data-ttu-id="2f6c5-126">在本部分，我们将快速调整要在 Azure 应用服务中作为 WAR 文件部署的 Spring Boot 项目，该项目默认提供 Tomcat 作为运行时。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-126">In this section we will quickly adjust the Spring Boot project to be deployed as a WAR file on Azure App Service, which provides Tomcat as the runtime by default.</span></span> <span data-ttu-id="2f6c5-127">为此，需要修改两个文件：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-127">For this to work, there are two files to be modified:</span></span>

- <span data-ttu-id="2f6c5-128">Maven `pom.xml` 文件</span><span class="sxs-lookup"><span data-stu-id="2f6c5-128">The Maven `pom.xml` file</span></span>
- <span data-ttu-id="2f6c5-129">`Application` Java 类</span><span class="sxs-lookup"><span data-stu-id="2f6c5-129">The `Application` Java class</span></span>

<span data-ttu-id="2f6c5-130">让我们从 Maven 设置开始：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-130">Let's start with the Maven settings:</span></span>

1. <span data-ttu-id="2f6c5-131">打开 `pom.xml`</span><span class="sxs-lookup"><span data-stu-id="2f6c5-131">Open `pom.xml`</span></span>

1. <span data-ttu-id="2f6c5-132">紧接在顶部的 `<artifactId>` 定义后面添加 `<packaging>war</packaging>`：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-132">Add `<packaging>war</packaging>` right after the `<artifactId>` definition at the top:</span></span>
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. <span data-ttu-id="2f6c5-133">添加以下依赖项：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-133">Add the following dependency:</span></span>
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

<span data-ttu-id="2f6c5-134">现在请打开类 `Application`（但愿此时 IDE 已拾取新的依赖项），然后继续进行以下修改：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-134">Now open the class `Application`, hopefully after your IDE has already picked up the new dependencies, and proceed with the following modifications:</span></span>

1. <span data-ttu-id="2f6c5-135">将 Application 类设为 `SpringBootServletInitializer` 的子类：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-135">Make class Application a subclass of `SpringBootServletInitializer`:</span></span>
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. <span data-ttu-id="2f6c5-136">将以下方法添加到 Application 类：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-136">Add the following method to the Application class:</span></span>
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```
1. <span data-ttu-id="2f6c5-137">组织导入以确保 `SpringApplicationBuilder` 和 `SpringBootServletInitializer` 正确导入。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-137">Organize your imports to ensure `SpringApplicationBuilder` and `SpringBootServletInitializer` are properly imported.</span></span>

<span data-ttu-id="2f6c5-138">现在，可将应用程序部署到 Tomcat 和其他任何 Servlet 运行时（例如 Jetty）。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-138">Your application is now ready to be deployed to Tomcat and any other Servlet runtime (e.g. Jetty).</span></span>

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a><span data-ttu-id="2f6c5-139">添加适用于 Azure 应用服务 Web 应用的 Maven 插件</span><span class="sxs-lookup"><span data-stu-id="2f6c5-139">Add the Maven Plugin for Azure App Service Web Apps</span></span>

<span data-ttu-id="2f6c5-140">在本部分，我们将添加一个 Maven 插件，用于自动完成将此应用程序部署到 Azure 应用服务 Web 应用的整个过程。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-140">In this section, we will add a Maven plugin that will automate the entire deployment of this application into Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="2f6c5-141">再次打开 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-141">Open `pom.xml` once again.</span></span>

1. <span data-ttu-id="2f6c5-142">在 `<properties>` 中，使用属性 `maven.build.timestamp.format` 设置自定义时间戳格式。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-142">Inside `<properties>`, set a custom timestamp format with the property `maven.build.timestamp.format`.</span></span> <span data-ttu-id="2f6c5-143">由于 Azure 应用服务将为应用程序创建公共 URL，因此，此设置将用于生成部署名称，并避免与其他用户的实时部署发生冲突。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-143">Because Azure App Service creates a public URL for your application, this setting will be used to generate the name of your deployment, and avoid conflict with other users' live deployments.</span></span>
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. <span data-ttu-id="2f6c5-144">在 `<plugins>` 元素中添加以下内容：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-144">In the `<plugins>` element, add the following:</span></span>
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

<span data-ttu-id="2f6c5-145">现在，可以使用这些设置将 Maven 项目实时部署到 Azure 应用服务 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-145">With these settings, your Maven project is now ready for live deployment to Azure App Service Web App.</span></span>

## <a name="install-and-log-in-to-azure-cli"></a><span data-ttu-id="2f6c5-146">安装并登录到 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2f6c5-146">Install and log in to Azure CLI</span></span>

<span data-ttu-id="2f6c5-147">获取用于部署 Spring Boot 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-147">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span> <span data-ttu-id="2f6c5-148">确保已安装 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-148">Make sure you have it installed.</span></span>

1. <span data-ttu-id="2f6c5-149">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-149">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
   <span data-ttu-id="2f6c5-150">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-150">Follow the instructions to complete the sign-in process.</span></span>

## <a name="optionally-customize-pomxml-before-deploying"></a><span data-ttu-id="2f6c5-151">（可选）在部署之前自定义 pom.xml</span><span class="sxs-lookup"><span data-stu-id="2f6c5-151">Optionally, customize pom.xml before deploying</span></span>

<span data-ttu-id="2f6c5-152">在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件，然后找到 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="2f6c5-153">该元素应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-153">This element should resemble the following example:</span></span>

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

<span data-ttu-id="2f6c5-154">可以为 Maven 插件修改几个值，[适用于 Azure Web 应用的 Maven 插件]文档中提供了这些元素各自的详细描述。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="2f6c5-155">尽管如此，在本文中有仍几个值得注意的值：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-155">That being said, there are several values that are worth highlighting in this article:</span></span>

| <span data-ttu-id="2f6c5-156">元素</span><span class="sxs-lookup"><span data-stu-id="2f6c5-156">Element</span></span> | <span data-ttu-id="2f6c5-157">Description</span><span class="sxs-lookup"><span data-stu-id="2f6c5-157">Description</span></span> |
|---|---|
| `<version>` | <span data-ttu-id="2f6c5-158">指定[适用于 Azure Web 应用的 Maven 插件]的版本。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="2f6c5-159">验证 [Maven 中央存储库](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中列出的版本，确保使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-159">Verify the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span> |
| `<resourceGroup>` | <span data-ttu-id="2f6c5-160">指定目标资源组，在此示例中为 `maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-160">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="2f6c5-161">如果资源组不存在，则会在部署过程中进行创建。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-161">The resource group is created during deployment if it does not already exist.</span></span> |
| `<appName>` | <span data-ttu-id="2f6c5-162">指定 Web 应用的目标名称。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-162">Specifies the target name for your web app.</span></span> <span data-ttu-id="2f6c5-163">在此示例中，目标名称为 `maven-web-app-${maven.build.timestamp}`，此示例附加​​了 `${maven.build.timestamp}` 后缀以避免冲突。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-163">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="2f6c5-164">（时间戳是可选项；可为应用名称指定任何唯一的字符串。）</span><span class="sxs-lookup"><span data-stu-id="2f6c5-164">(The timestamp is optional; you can specify any unique string for the app name.)</span></span> |
| `<region>` | <span data-ttu-id="2f6c5-165">指定目标区域，在此示例中为 `westus`。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-165">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="2f6c5-166">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="2f6c5-166">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<javaVersion>` | <span data-ttu-id="2f6c5-167">为 Web 应用指定 Java 运行时版本。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-167">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="2f6c5-168">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="2f6c5-168">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span> |
| `<deploymentType>` | <span data-ttu-id="2f6c5-169">为 Web 应用指定部署类型。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="2f6c5-170">默认为 `war`。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-170">Default is `war`.</span></span> |

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="2f6c5-171">生成 Web 应用并将其部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="2f6c5-171">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="2f6c5-172">配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-172">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="2f6c5-173">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-173">To do so, use the following steps:</span></span>

1. <span data-ttu-id="2f6c5-174">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-174">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="2f6c5-175">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-175">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="2f6c5-176">Maven 会将 Web 应用部署到 Azure；如果 Web 应用不存在，则将创建一个。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-176">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="2f6c5-177">Web 部署完成后即可使用 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="2f6c5-177">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="2f6c5-178">Web 应用将会在“应用服务”中列出：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-178">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="2f6c5-180">Web 应用的 URL 会在 Web 应用的“概述”中列出：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-180">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2f6c5-182">后续步骤</span><span class="sxs-lookup"><span data-stu-id="2f6c5-182">Next steps</span></span>

<span data-ttu-id="2f6c5-183">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="2f6c5-183">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="2f6c5-184">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="2f6c5-184">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="2f6c5-185">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="2f6c5-185">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="2f6c5-186">如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="2f6c5-186">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="2f6c5-187">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="2f6c5-187">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="2f6c5-188">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="2f6c5-188">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
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
