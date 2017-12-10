---
title: "将 Spring Boot 应用程序部署到 Azure 应用服务"
description: "本教程会为开发者介绍将 Spring Boot 入门 Web 应用部署到 Azure 应用服务的步骤。"
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: b520cc80360f8162c929bb2cc88c24311a7e20f8
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a><span data-ttu-id="a175b-103">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="a175b-103">Deploy a Spring Boot Application to the Azure App Service</span></span>

<span data-ttu-id="a175b-104">本教程将介绍如何创建 [Spring Boot] 入门 Web 应用示例，以及如何将其部署到 [Azure 应用服务]。</span><span class="sxs-lookup"><span data-stu-id="a175b-104">This tutorial will walk you though creating a sample [Spring Boot] Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a175b-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="a175b-105">Prerequisites</span></span>

<span data-ttu-id="a175b-106">完成本教程中的步骤需要具备以下项：</span><span class="sxs-lookup"><span data-stu-id="a175b-106">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="a175b-107">Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="a175b-107">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="a175b-108">最新的 [Java 开发人员工具包 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="a175b-108">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="a175b-109">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="a175b-109">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="a175b-110">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="a175b-110">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="a175b-111">创建 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="a175b-111">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="a175b-112">以下步骤将引导你完成创建一个简单的 Spring Boot Web 应用程序和对其进行本地测试所需的步骤。</span><span class="sxs-lookup"><span data-stu-id="a175b-112">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="a175b-113">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-113">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="a175b-114">- 或 -</span><span class="sxs-lookup"><span data-stu-id="a175b-114">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="a175b-115">将 [Spring Boot 启动入门]示例项目克隆到刚刚创建的目录；例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-115">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="a175b-116">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-116">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="a175b-117">使用 Maven 生成 JAR 文件；例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-117">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="a175b-118">创建 Web 应用后，将目录更改为 JAR 文件并启动 Web 应用；例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-118">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="a175b-119">使用 Web 浏览器浏览到 http://localhost:8080 以测试 Web 应用，如果有可用的 Curl，也可使用如以下示例所示的语法：</span><span class="sxs-lookup"><span data-stu-id="a175b-119">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="a175b-120">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="a175b-120">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![浏览示例应用][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="a175b-122">创建适用于 Java 的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="a175b-122">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="a175b-123">以下步骤将引导你完成创建 Azure Web 应用、配置在 Java 中所需的设置和配置 FTP 凭据的步骤。</span><span class="sxs-lookup"><span data-stu-id="a175b-123">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="a175b-124">浏览到 [Azure 门户]并登录。</span><span class="sxs-lookup"><span data-stu-id="a175b-124">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="a175b-125">登录 Azure 门户的帐户后，请单击“应用服务”的菜单图标：</span><span class="sxs-lookup"><span data-stu-id="a175b-125">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![Azure 门户][AZ01]

1. <span data-ttu-id="a175b-127">显示“应用服务”页面时，请单击“+ 添加”以创建新的应用服务。</span><span class="sxs-lookup"><span data-stu-id="a175b-127">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![创建应用服务][AZ02]

1. <span data-ttu-id="a175b-129">显示 Web 应用模板列表时，请单击基本 Microsoft Web 应用的链接。</span><span class="sxs-lookup"><span data-stu-id="a175b-129">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Web 应用模板][AZ03]

1. <span data-ttu-id="a175b-131">显示 Web 应用模板的信息页面时，请单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="a175b-131">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![创建 Web 应用][AZ04]

1. <span data-ttu-id="a175b-133">为 Web 应用提供唯一名称并指定任何其他设置，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="a175b-133">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![创建 Web 应用设置][AZ05]

1. <span data-ttu-id="a175b-135">创建 Web 应用后，单击“应用服务”的菜单图标，然后单击新创建的 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="a175b-135">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![列出 Web 应用][AZ06]

1. <span data-ttu-id="a175b-137">显示 Web 应用时，请执行以下步骤以指定 Java 版本：</span><span class="sxs-lookup"><span data-stu-id="a175b-137">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="a175b-138">a.</span><span class="sxs-lookup"><span data-stu-id="a175b-138">a.</span></span> <span data-ttu-id="a175b-139">单击“应用程序设置”菜单项。</span><span class="sxs-lookup"><span data-stu-id="a175b-139">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="a175b-140">b.</span><span class="sxs-lookup"><span data-stu-id="a175b-140">b.</span></span> <span data-ttu-id="a175b-141">Java 版本选择 "Java 8"。</span><span class="sxs-lookup"><span data-stu-id="a175b-141">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="a175b-142">c.</span><span class="sxs-lookup"><span data-stu-id="a175b-142">c.</span></span> <span data-ttu-id="a175b-143">Java 次要版本选择“最新版”。</span><span class="sxs-lookup"><span data-stu-id="a175b-143">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="a175b-144">d.</span><span class="sxs-lookup"><span data-stu-id="a175b-144">d.</span></span> <span data-ttu-id="a175b-145">Web 容器选择“最新的 Tomcat 8.5”。</span><span class="sxs-lookup"><span data-stu-id="a175b-145">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="a175b-146">（实际上不会使用此容器；Azure 会使用 Spring Boot 应用程序中的容器。）</span><span class="sxs-lookup"><span data-stu-id="a175b-146">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="a175b-147">e.</span><span class="sxs-lookup"><span data-stu-id="a175b-147">e.</span></span> <span data-ttu-id="a175b-148">单击“保存” 。</span><span class="sxs-lookup"><span data-stu-id="a175b-148">Click **Save**.</span></span>

   ![应用程序设置][AZ07]

1. <span data-ttu-id="a175b-150">通过执行以下步骤指定 FTP 部署凭据：</span><span class="sxs-lookup"><span data-stu-id="a175b-150">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="a175b-151">a.</span><span class="sxs-lookup"><span data-stu-id="a175b-151">a.</span></span> <span data-ttu-id="a175b-152">单击“部署凭据”菜单项。</span><span class="sxs-lookup"><span data-stu-id="a175b-152">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="a175b-153">b.</span><span class="sxs-lookup"><span data-stu-id="a175b-153">b.</span></span> <span data-ttu-id="a175b-154">指定用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="a175b-154">Specify your username and password.</span></span>

   <span data-ttu-id="a175b-155">c.</span><span class="sxs-lookup"><span data-stu-id="a175b-155">c.</span></span> <span data-ttu-id="a175b-156">单击“保存” 。</span><span class="sxs-lookup"><span data-stu-id="a175b-156">Click **Save**.</span></span>

   ![指定部署凭据][AZ08]

1. <span data-ttu-id="a175b-158">通过使用以下步骤检索 FTP 连接信息：</span><span class="sxs-lookup"><span data-stu-id="a175b-158">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="a175b-159">a.</span><span class="sxs-lookup"><span data-stu-id="a175b-159">a.</span></span> <span data-ttu-id="a175b-160">单击“部署凭据”菜单项。</span><span class="sxs-lookup"><span data-stu-id="a175b-160">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="a175b-161">b.</span><span class="sxs-lookup"><span data-stu-id="a175b-161">b.</span></span> <span data-ttu-id="a175b-162">复制完整的 FTP 用户名和 URL并保存，以供本教程下一节使用。</span><span class="sxs-lookup"><span data-stu-id="a175b-162">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![FTP URL 和凭据][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="a175b-164">将 Spring Boot Web 应用部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="a175b-164">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="a175b-165">以下步骤将引导你完成将 Spring Boot Web 应用部署到 Azure 的步骤。</span><span class="sxs-lookup"><span data-stu-id="a175b-165">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="a175b-166">打开 Windows 记事本之类的文本编辑器，将以下文本粘贴到新文档中，然后将该文件另存为 "web.config"：</span><span class="sxs-lookup"><span data-stu-id="a175b-166">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="a175b-167">将 "web.config" 文件保存到系统后，使用本教程前面部分保存的 URL、用户名和密码，通过 FTP 连接到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="a175b-167">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="a175b-168">例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-168">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="a175b-169">将远程目录更改为 Web 应用的根文件夹（在 /site/wwwroot 中），然后复制 Spring Boot 应用程序中的 JAR 文件和前面的 "web.config" 文件。</span><span class="sxs-lookup"><span data-stu-id="a175b-169">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="a175b-170">例如：</span><span class="sxs-lookup"><span data-stu-id="a175b-170">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="a175b-171">将 JAR 和 "web.config" 文件部署到 Web 应用后，需要使用 Azure 门户重新启动 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="a175b-171">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="a175b-172">使用 Web 浏览器浏览到 Web 应用的 URL 以测试 Web 应用，如果有可用的 Curl，也可使用如以下示例所示的语法：</span><span class="sxs-lookup"><span data-stu-id="a175b-172">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="a175b-173">应会显示以下消息：“来自 Spring Boot 的问候！”</span><span class="sxs-lookup"><span data-stu-id="a175b-173">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![浏览示例应用][SB02]

## <a name="next-steps"></a><span data-ttu-id="a175b-175">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a175b-175">Next steps</span></span>

<span data-ttu-id="a175b-176">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="a175b-176">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="a175b-177">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上</span><span class="sxs-lookup"><span data-stu-id="a175b-177">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)

* [<span data-ttu-id="a175b-178">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="a175b-178">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="a175b-179">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="a175b-179">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="a175b-180">有关使用 FTP 将 Web 应用部署到 Azure 的其他信息，请参阅[使用 FTP/S 将应用部署到 Azure 应用服务]。</span><span class="sxs-lookup"><span data-stu-id="a175b-180">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="a175b-181">有关 Spring Boot 示例项目的详细信息，请参阅 [Spring Boot 启动入门]。</span><span class="sxs-lookup"><span data-stu-id="a175b-181">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="a175b-182">若要获取 Spring Boot 应用程序入门的相关帮助，请参阅 https://start.spring.io/ 中的“Spring Initializr”。</span><span class="sxs-lookup"><span data-stu-id="a175b-182">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="a175b-183">有关配置 Web 应用的其他设置的详细信息，请参阅[在 Azure 应用服务中配置 Web 应用]。</span><span class="sxs-lookup"><span data-stu-id="a175b-183">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[Azure 应用服务]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure 门户]: https://portal.azure.com/
[在 Azure 应用服务中配置 Web 应用]: /azure/app-service/web-sites-configure
[使用 FTP/S 将应用部署到 Azure 应用服务]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 启动入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-web-app-on-azure/SB01.png
[SB02]: ./media/deploy-spring-boot-java-web-app-on-azure/SB02.png

[AZ01]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ01.png
[AZ02]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ02.png
[AZ03]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ03.png
[AZ04]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ04.png
[AZ05]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ05.png
[AZ06]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ06.png
[AZ07]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ07.png
[AZ08]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ08.png
[AZ09]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ09.png
[AZ10]: ./media/deploy-spring-boot-java-web-app-on-azure/AZ10.png
