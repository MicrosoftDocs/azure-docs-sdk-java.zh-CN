---
title: 使用 Maven 和 Azure 将 Spring Boot JAR 文件应用部署到云中
description: 了解如何使用适用于 Linux 的 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到云中。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: b133290d1f14429cbf36d6ed5a67d27e1a637593
ms.sourcegitcommit: 599405a9ce892d75073ef0776befa2fa22407b4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67237596"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a><span data-ttu-id="182fa-103">在 Linux 上将 Spring Boot JAR 文件 Web 应用部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="182fa-103">Deploy a Spring Boot JAR file web app to Azure App Service on Linux</span></span>

<span data-ttu-id="182fa-104">本文演示如何使用[适用于 Azure 应用服务 Web 应用的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将打包为 Java SE JAR 的 Spring Boot 应用程序部署到 [Linux 上的 Azure 应用服务](/azure/app-service/containers/)。</span><span class="sxs-lookup"><span data-stu-id="182fa-104">This article demonstrates using the [Maven Plugin for Azure App Service Web Apps](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) to deploy a Spring Boot application packaged as a Java SE JAR to [Azure App Service on Linux](/azure/app-service/containers/).</span></span> <span data-ttu-id="182fa-105">如果要将应用的依赖关系、运行时和配置合并到单个可部署项目中，请选择通过 [Tomcat 和 WAR 文件](/azure/app-service/containers/quickstart-java)进行 Java SE 部署。</span><span class="sxs-lookup"><span data-stu-id="182fa-105">Choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.</span></span>


<span data-ttu-id="182fa-106">如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="182fa-106">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="182fa-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="182fa-107">Prerequisites</span></span>

<span data-ttu-id="182fa-108">若要完成本教程中的步骤，需要安装和配置以下内容：</span><span class="sxs-lookup"><span data-stu-id="182fa-108">To complete the steps in this tutorial, you'll need to have the following installed and configured:</span></span>

* <span data-ttu-id="182fa-109">[Azure CLI](/cli/azure/)，在本地或通过 [Azure Cloud Shell](https://shell.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="182fa-109">The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).</span></span>
* <span data-ttu-id="182fa-110">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="182fa-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="182fa-111">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="182fa-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="182fa-112">Apache 的 [Maven](https://maven.apache.org/) 版本 3）。</span><span class="sxs-lookup"><span data-stu-id="182fa-112">Apache's [Maven](https://maven.apache.org/), Version 3).</span></span>
* <span data-ttu-id="182fa-113">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="182fa-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="install-and-sign-in-to-azure-cli"></a><span data-ttu-id="182fa-114">安装并登录 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="182fa-114">Install and sign in to Azure CLI</span></span>

<span data-ttu-id="182fa-115">获取用于部署 Spring Boot 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](/cli/azure/)。</span><span class="sxs-lookup"><span data-stu-id="182fa-115">The simplest and easiest way to get the Maven Plugin deploying your Spring Boot application is by using [Azure CLI](/cli/azure/).</span></span>

<span data-ttu-id="182fa-116">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="182fa-116">Sign into your Azure account by using the Azure CLI:</span></span>
   
   ```shell
   az login
   ```
   
<span data-ttu-id="182fa-117">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="182fa-117">Follow the instructions to complete the sign-in process.</span></span>

## <a name="clone-the-sample-app"></a><span data-ttu-id="182fa-118">克隆示例应用</span><span class="sxs-lookup"><span data-stu-id="182fa-118">Clone the sample app</span></span>

<span data-ttu-id="182fa-119">本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。</span><span class="sxs-lookup"><span data-stu-id="182fa-119">In this section, you will clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="182fa-120">打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="182fa-121">\- 或 -</span><span class="sxs-lookup"><span data-stu-id="182fa-121">-- or --</span></span>
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. <span data-ttu-id="182fa-122">将 [Spring Boot 入门]示例项目克隆到已创建的目录；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-122">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. <span data-ttu-id="182fa-123">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="182fa-124">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="182fa-125">创建 Web 应用后，使用 Maven 启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="182fa-126">使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="182fa-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="182fa-127">例如，如果有可用的 Curl，可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="182fa-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="182fa-128">应当会看到显示了以下消息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="182fa-128">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="configure-maven-plugin-for-azure-app-service"></a><span data-ttu-id="182fa-129">配置适用于 Azure 应用服务的 Maven 插件</span><span class="sxs-lookup"><span data-stu-id="182fa-129">Configure Maven Plugin for Azure App Service</span></span>

<span data-ttu-id="182fa-130">在本部分中，将配置 Spring Boot 项目 `pom.xml`，以便 Maven 可以将应用部署到 Linux 上的 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="182fa-130">In this section, you will configure the Spring Boot project `pom.xml` so that Maven can deploy the app to Azure App Service on Linux.</span></span>

1. <span data-ttu-id="182fa-131">在代码编辑器中打开 `pom.xml`。</span><span class="sxs-lookup"><span data-stu-id="182fa-131">Open `pom.xml` in a code editor.</span></span>

2. <span data-ttu-id="182fa-132">在 pom.xml 的 `<build>` 节的 `<plugins>` 标记内添加以下 `<plugin>` 条目。</span><span class="sxs-lookup"><span data-stu-id="182fa-132">In the `<build>` section of the pom.xml, add the following `<plugin>` entry inside the `<plugins>` tag.</span></span>

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. <span data-ttu-id="182fa-133">然后可以配置部署，在命令提示符下运行 maven 命令 `mvn azure-webapp:config`，并使用**数字**选择提示中的这些选项：</span><span class="sxs-lookup"><span data-stu-id="182fa-133">Then you can configure the deployment, run the maven command `mvn azure-webapp:config` in the Command Prompt and use the **number** to choose these options in the prompt:</span></span>
    * <span data-ttu-id="182fa-134">**OS**：linux</span><span class="sxs-lookup"><span data-stu-id="182fa-134">**OS**: linux</span></span>  
    * <span data-ttu-id="182fa-135">**javaVersion**：jre8</span><span class="sxs-lookup"><span data-stu-id="182fa-135">**javaVersion**: jre8</span></span>
    * <span data-ttu-id="182fa-136">**runtimeStack**：jre8</span><span class="sxs-lookup"><span data-stu-id="182fa-136">**runtimeStack**: jre8</span></span>

<span data-ttu-id="182fa-137">显示 **Confirm (Y/N)** 提示时，请按 **'y'** ，配置将完成。</span><span class="sxs-lookup"><span data-stu-id="182fa-137">When you get the **Confirm (Y/N)** prompt, press **'y'** and the configuration is done.</span></span>

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. jre8 [*]
2. java11
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. jre8
3. TOMCAT 8.5 [*]
4. WILDFLY 14
Enter index to use: 2
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. <span data-ttu-id="182fa-138">将 `<appSettings>` 部分添加到 `<azure-webapp-maven-plugin>` 的 `<configuration>` 部分，以便在 *80* 端口上侦听。</span><span class="sxs-lookup"><span data-stu-id="182fa-138">Add the `<appSettings>` section to the `<configuration>` section of `<azure-webapp-maven-plugin>` to listen on the *80* port.</span></span>

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.6.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>

          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          ...
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="182fa-139">将应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="182fa-139">Deploy the app to Azure</span></span>

<span data-ttu-id="182fa-140">配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="182fa-140">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="182fa-141">为此，请按照以下步骤操作：</span><span class="sxs-lookup"><span data-stu-id="182fa-141">To do so, use the following steps:</span></span>

1. <span data-ttu-id="182fa-142">在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如  ：</span><span class="sxs-lookup"><span data-stu-id="182fa-142">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="182fa-143">使用 Maven 将 Web 应用部署到 Azure；例如：</span><span class="sxs-lookup"><span data-stu-id="182fa-143">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="182fa-144">Maven 会将 Web 应用部署到 Azure；如果 Web 应用或 Web 应用计划尚不存在，则将为你创建一个。</span><span class="sxs-lookup"><span data-stu-id="182fa-144">Maven will deploy your web app to Azure; if the web app or web app plan does not already exist, it will be created for you.</span></span>

<span data-ttu-id="182fa-145">Web 部署完成后即可通过 [Azure 门户]进行管理。</span><span class="sxs-lookup"><span data-stu-id="182fa-145">When your web has been deployed, you will be able to manage it through the [Azure portal].</span></span>

* <span data-ttu-id="182fa-146">Web 应用将会在“应用服务”中列出  ：</span><span class="sxs-lookup"><span data-stu-id="182fa-146">Your web app will be listed in **App Services**:</span></span>

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* <span data-ttu-id="182fa-148">Web 应用的 URL 会在 Web 应用的“概述”中列出  ：</span><span class="sxs-lookup"><span data-stu-id="182fa-148">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![确定 Web 应用的 URL][AP02]

<span data-ttu-id="182fa-150">通过与以前相同的 cURL 命令（使用来自门户的 Web 应用 URL而不是 `localhost`）验证部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="182fa-150">Verify that the deployment was successful by using the same cURL command as before, using your web app URL from the Portal instead of `localhost`.</span></span> <span data-ttu-id="182fa-151">应当会看到显示了以下消息：**Greetings from Spring Boot!**</span><span class="sxs-lookup"><span data-stu-id="182fa-151">You should see the following message displayed: **Greetings from Spring Boot!**</span></span> 

## <a name="next-steps"></a><span data-ttu-id="182fa-152">后续步骤</span><span class="sxs-lookup"><span data-stu-id="182fa-152">Next steps</span></span>

<span data-ttu-id="182fa-153">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="182fa-153">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="182fa-154">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="182fa-154">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="182fa-155">其他资源</span><span class="sxs-lookup"><span data-stu-id="182fa-155">Additional Resources</span></span>

<span data-ttu-id="182fa-156">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="182fa-156">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="182fa-157">[适用于 Azure Web 应用的 Maven 插件]</span><span class="sxs-lookup"><span data-stu-id="182fa-157">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="182fa-158">如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="182fa-158">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [<span data-ttu-id="182fa-159">使用 Azure CLI 2.0 创建 Azure 服务主体</span><span class="sxs-lookup"><span data-stu-id="182fa-159">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="182fa-160">Maven 设置参考</span><span class="sxs-lookup"><span data-stu-id="182fa-160">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure 门户]: https://portal.azure.com/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme
[Maven Plugin for Azure Web Apps]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
