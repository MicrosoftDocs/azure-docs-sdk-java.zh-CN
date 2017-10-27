---
title: "如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到 Azure"
description: "了解如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到 Azure。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Maven
ms.assetid: 
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 10/11/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 32781b3ee923f4d64d4e1a11df73f077281eee40
ms.sourcegitcommit: 7f8538e41c833deb69c300ad3431a431136a1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a><span data-ttu-id="4c0cc-104">如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="4c0cc-104">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure</span></span>

<span data-ttu-id="4c0cc-105">用于 [Apache Maven](http://maven.apache.org/) 的[适用于 Azure Web 应用的 Maven 插件](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin)将 Azure 应用服务无缝集成到 Maven 项目，简化了开发人员将 Web 应用部署到 Azure 应用服务的过程。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-105">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="4c0cc-106">本文演示如何使用适用于 Azure Web 应用的 Maven 插件将示例 Spring Boot 应用程序部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-106">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4c0cc-107">适用于 Azure Web 应用的 Maven 插件当前提供预览版。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-107">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="4c0cc-108">目前，仅支持 FTP 发布，但计划在未来支持其他功能。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-108">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="4c0cc-109">先决条件</span><span class="sxs-lookup"><span data-stu-id="4c0cc-109">Prerequisites</span></span>

<span data-ttu-id="4c0cc-110">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-110">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="4c0cc-111">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4c0cc-112">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="4c0cc-113">最新 [Java 开发工具包 (JDK)] 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="4c0cc-114">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="4c0cc-115">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-115">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="4c0cc-116">克隆示例 Spring Boot Web 应用</span><span class="sxs-lookup"><span data-stu-id="4c0cc-116">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="4c0cc-117">本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-117">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="4c0cc-118">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-118">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="4c0cc-119">- 或 -</span><span class="sxs-lookup"><span data-stu-id="4c0cc-119">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="4c0cc-120">将 [Spring Boot 入门]示例项目克隆到已创建的目录；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-120">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="4c0cc-121">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-121">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="4c0cc-122">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-122">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4c0cc-123">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-123">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="4c0cc-124">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-124">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="4c0cc-125">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-125">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="4c0cc-126">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="4c0cc-126">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="4c0cc-127">创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="4c0cc-127">Create an Azure service principal</span></span>

<span data-ttu-id="4c0cc-128">本部分将创建 Maven 插件在将 Web 应用部署到 Azure 时使用的 Azure 服务主体。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-128">In this section, you create an Azure service principal that the Maven plugin uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="4c0cc-129">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-129">Open a command prompt.</span></span>

1. <span data-ttu-id="4c0cc-130">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-130">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="4c0cc-131">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-131">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="4c0cc-132">创建 Azure 服务主体：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-132">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="4c0cc-133">其中，`uuuuuuuu` 是服务主体的用户名，`pppppppp` 是服务主体的密码。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-133">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="4c0cc-134">Azure 使用与以下示例类似的 JSON 进行响应：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-134">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="4c0cc-135">当配置 Maven 插件以将 Web 应用部署到 Azure 时，会使用此 JSON 响应中的值。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-135">You will use the values from this JSON response when you configure the Maven plugin to deploy your web app to Azure.</span></span> <span data-ttu-id="4c0cc-136">`aaaaaaaa`、`uuuuuuuu`、`pppppppp` 和 `tttttttt` 是占位符值，在本示例中用于在下一部分中配置 Maven `settings.xml` 文件时，能够更加方便地将这些值映射到其各自的元素。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-136">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="4c0cc-137">配置 Maven 以使用 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="4c0cc-137">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="4c0cc-138">本部分将使用 Azure 服务主体中的值配置 Maven 在将 Web 应用部署到 Azure 时要使用的身份验证。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-138">In this section, you use the values from your Azure service principal to configure the authentication that Maven uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="4c0cc-139">在文本编辑器中打开 Maven `settings.xml` 文件；此文件可能位于如以下示例所示的路径中：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-139">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="4c0cc-140">将本教程上一部分中的 Azure 服务主体设置添加到 settings.xml 文件中的 `<servers>` 集合；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-140">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="4c0cc-141">其中：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-141">Where:</span></span>
   <span data-ttu-id="4c0cc-142">元素</span><span class="sxs-lookup"><span data-stu-id="4c0cc-142">Element</span></span> | <span data-ttu-id="4c0cc-143">说明</span><span class="sxs-lookup"><span data-stu-id="4c0cc-143">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="4c0cc-144">指定在将 Web 应用部署到 Azure 时，Maven 用于查找安全设置的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-144">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="4c0cc-145">包含服务主体的 `appId` 值。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-145">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="4c0cc-146">包含服务主体的 `tenant` 值。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-146">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="4c0cc-147">包含服务主体的 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-147">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="4c0cc-148">定义目标 Azure 云环境，此示例中为 `AZURE`。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-148">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="4c0cc-149">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整的环境列表）</span><span class="sxs-lookup"><span data-stu-id="4c0cc-149">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="4c0cc-150">保存并关闭 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-150">Save and close the *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a><span data-ttu-id="4c0cc-151">可选：在将 Web 应用部署到 Azure 之前自定义 pom.xml</span><span class="sxs-lookup"><span data-stu-id="4c0cc-151">OPTIONAL: Customize your pom.xml before deploying your web app to Azure</span></span>

<span data-ttu-id="4c0cc-152">在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件，然后找到 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-152">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="4c0cc-153">该元素应类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-153">This element should resemble the following example:</span></span>

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="4c0cc-154">可以为 Maven 插件修改几个值，[适用于 Azure Web 应用的 Maven 插件]文档中提供了这些元素各自的详细描述。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-154">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="4c0cc-155">尽管如此，在本文中有仍几个值得注意的值：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-155">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="4c0cc-156">元素</span><span class="sxs-lookup"><span data-stu-id="4c0cc-156">Element</span></span> | <span data-ttu-id="4c0cc-157">说明</span><span class="sxs-lookup"><span data-stu-id="4c0cc-157">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="4c0cc-158">指定[适用于 Azure Web 应用的 Maven 插件]的版本。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-158">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="4c0cc-159">应检查 [Maven 中央存储库](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中列出的版本，确保使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-159">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="4c0cc-160">指定 Azure 的身份验证信息，该信息在本示例中含有包含 `azure-auth` 的 `<serverId>` 元素；Maven 使用该值查找在本文前面部分定义的 Maven settings.xml 文件中的 Azure 服务主体值。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-160">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="4c0cc-161">指定目标资源组，在此示例中为 `maven-plugin`。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-161">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="4c0cc-162">如果资源组不存在，则会在部署过程中进行创建。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-162">The resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="4c0cc-163">指定 Web 应用的目标名称。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-163">Specifies the target name for your web app.</span></span> <span data-ttu-id="4c0cc-164">在此示例中，目标名称为 `maven-web-app-${maven.build.timestamp}`，此示例附加​​了 `${maven.build.timestamp}` 后缀以避免冲突。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-164">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="4c0cc-165">（时间戳是可选项；可为应用名称指定任何唯一的字符串。）</span><span class="sxs-lookup"><span data-stu-id="4c0cc-165">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="4c0cc-166">指定目标区域，在此示例中为 `westus`。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-166">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="4c0cc-167">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="4c0cc-167">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="4c0cc-168">为 Web 应用指定 Java 运行时版本。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-168">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="4c0cc-169">（[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。）</span><span class="sxs-lookup"><span data-stu-id="4c0cc-169">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="4c0cc-170">为 Web 应用指定部署类型。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-170">Specifies deployment type for your web app.</span></span> <span data-ttu-id="4c0cc-171">目前仅支持 `ftp`，但是正在开发其他部署类型支持。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-171">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="4c0cc-172">指定将 Web 应用部署到 Azure 时 Maven 使用的资源和目标。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-172">Specifies resources and target destinations which Maven uses when deploying your web app to Azure.</span></span> <span data-ttu-id="4c0cc-173">此示例中，两个 `<resource>` 元素指定 Maven 部署 Web 应用的 JAR 文件和 Spring Boot 项目中的 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-173">In this example, two `<resource>` elements specify that Maven will deploy the JAR file for your web app and the *web.config* file from the Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="4c0cc-174">生成 Web 应用并将其部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="4c0cc-174">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="4c0cc-175">配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-175">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="4c0cc-176">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="4c0cc-177">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-177">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4c0cc-178">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-178">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="4c0cc-179">Maven 会将 Web 应用部署到 Azure；如果 Web 应用不存在，则将创建一个。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-179">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="4c0cc-180">Web 部署完成后即可使用 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="4c0cc-180">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="4c0cc-181">Web 应用将会在“应用服务”中列出：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-181">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="4c0cc-183">Web 应用的 URL 会在 Web 应用的“概述”中列出：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-183">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4c0cc-185">后续步骤</span><span class="sxs-lookup"><span data-stu-id="4c0cc-185">Next steps</span></span>

<span data-ttu-id="4c0cc-186">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="4c0cc-186">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="4c0cc-187">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="4c0cc-187">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="4c0cc-188">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="4c0cc-188">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="4c0cc-189">如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="4c0cc-189">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="4c0cc-190">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="4c0cc-190">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="4c0cc-191">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="4c0cc-191">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure 门户]: https://portal.azure.com/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
