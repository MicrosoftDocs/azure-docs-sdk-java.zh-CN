---
title: "使用适用于 IntelliJ 的 Azure 工具包将 Spring Boot 应用作为 Docker 容器发布"
description: "了解如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用作为 Docker 容器发布到 Microsoft Azure。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 4228352efa4354bfe4969c1a5ecd3f3b40483f85
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="76320-103">使用适用于 IntelliJ 的 Azure 工具包将 Spring Boot 应用作为 Docker 容器发布</span><span class="sxs-lookup"><span data-stu-id="76320-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="76320-104">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="76320-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="76320-105">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了简化的方法来创建独立的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="76320-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="76320-106">[Docker] 是一种开放源代码解决方案，可帮助开发人员自动部署、扩展和管理在容器中运行的应用程序。</span><span class="sxs-lookup"><span data-stu-id="76320-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="76320-107">本教程会指导你完成使用适用于 IntelliJ 的 Azure 工具包将 Spring Boot 应用程序作为 Docker 容器部署到 Microsoft Azure 的步骤。</span><span class="sxs-lookup"><span data-stu-id="76320-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="76320-108">克隆默认 Spring Boot Docker 存储库</span><span class="sxs-lookup"><span data-stu-id="76320-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="76320-109">以下步骤可指导你完成使用 IntelliJ 克隆 Spring Boot Docker 存储库的步骤。</span><span class="sxs-lookup"><span data-stu-id="76320-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="76320-110">如果要使用命令行，请参阅[在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上][Deploy Spring Boot on Linux in AKS]。</span><span class="sxs-lookup"><span data-stu-id="76320-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="76320-111">打开 IntelliJ。</span><span class="sxs-lookup"><span data-stu-id="76320-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="76320-112">在欢迎屏幕上，选择“从版本控制中签出”列表中的“GitHub”选项。</span><span class="sxs-lookup"><span data-stu-id="76320-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![用于版本控制的 GitHub 选项][CL01]

1. <span data-ttu-id="76320-114">如果系统提示登录，请输入凭据。</span><span class="sxs-lookup"><span data-stu-id="76320-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="76320-115">如果使用用户名/密码登录到 GitHub：</span><span class="sxs-lookup"><span data-stu-id="76320-115">If you are using a username/password to log in to GitHub:</span></span> 

      ![用于输入 GitHub 用户名和密码的对话框][CL02a]

   * <span data-ttu-id="76320-117">如果使用令牌登录到 GitHub：</span><span class="sxs-lookup"><span data-stu-id="76320-117">If you are using a token to log in to GitHub:</span></span> 

      ![用于输入 GitHub 令牌的对话框][CL02b]

1. <span data-ttu-id="76320-119">针对存储库 URL 输入 https://github.com/spring-guides/gs-spring-boot-docker.git，指定本地路径和文件夹信息，然后单击“克隆”。</span><span class="sxs-lookup"><span data-stu-id="76320-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![“克隆存储库”对话框][CL03]

1. <span data-ttu-id="76320-121">当系统提示创建 IntelliJ 项目时，选择“否”。</span><span class="sxs-lookup"><span data-stu-id="76320-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![拒绝创建 IntelliJ 项目][CL04]

1. <span data-ttu-id="76320-123">在欢迎屏幕上，单击“导入项目”。</span><span class="sxs-lookup"><span data-stu-id="76320-123">On the welcome page, click **Import Project**.</span></span>

   ![导入项目选择][CL05]

1. <span data-ttu-id="76320-125">找到克隆了 Spring Boot 存储库的路径，选择根路径下的“complete”文件夹，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="76320-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![选择用于导入的文件夹][CL06]

1. <span data-ttu-id="76320-127">出现提示时，选择“从现有源创建项目”。</span><span class="sxs-lookup"><span data-stu-id="76320-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![从现有源创建项目的选项][CL07]

1. <span data-ttu-id="76320-129">指定项目名称或接受默认设置，确认“complete”文件夹的路径正确无误，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="76320-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![指定项目名称][CL08]

1. <span data-ttu-id="76320-131">自定义任何用于导入的目录，然后单击“Next”（下一步）。</span><span class="sxs-lookup"><span data-stu-id="76320-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![选择目录][CL09]

1. <span data-ttu-id="76320-133">查看要导入的库，然后单击“Next”（下一步）。</span><span class="sxs-lookup"><span data-stu-id="76320-133">Review the libraries to import, and then click **Next**.</span></span>

   ![查看项目库][CL10]

1. <span data-ttu-id="76320-135">查看模块结构，然后单击“Next”（下一步）。</span><span class="sxs-lookup"><span data-stu-id="76320-135">Review the module structure, and then click **Next**.</span></span>

   ![查看模块结构][CL11]

1. <span data-ttu-id="76320-137">指定 JDK，然后单击“Next”（下一步）。</span><span class="sxs-lookup"><span data-stu-id="76320-137">Specify your JDK, and then click **Next**.</span></span>

   ![指定 JDK][CL12]

1. <span data-ttu-id="76320-139">单击“Finish”（完成）。</span><span class="sxs-lookup"><span data-stu-id="76320-139">Click **Finish**.</span></span>

   ![“完成”按钮][CL13]

<span data-ttu-id="76320-141">IntelliJ 会将 Spring Boot 应用作为项目导入，并在导入完成后显示结构。</span><span class="sxs-lookup"><span data-stu-id="76320-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![IntelliJ 中的 Spring Boot 应用][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="76320-143">生成 Spring Boot 应用</span><span class="sxs-lookup"><span data-stu-id="76320-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="76320-144">使用 Maven POM 生成应用</span><span class="sxs-lookup"><span data-stu-id="76320-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="76320-145">如果尚未打开 Maven 工具窗口，请打开它。</span><span class="sxs-lookup"><span data-stu-id="76320-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="76320-146">单击“视图” > “工具窗口” > “Maven 项目”。</span><span class="sxs-lookup"><span data-stu-id="76320-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![工具窗口和 Maven 项目命令][BU01]

1. <span data-ttu-id="76320-148">在 Maven 工具窗口中，右键单击“程序包”，然后选择“运行 Maven 生成”。</span><span class="sxs-lookup"><span data-stu-id="76320-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="76320-149">（如果 Maven 项目未自动显示，请单击 Maven 工具栏上的“重新导入”图标。）</span><span class="sxs-lookup"><span data-stu-id="76320-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![运行 Maven 生成][BU02]

1. <span data-ttu-id="76320-151">成功创建了 Spring Boot 应用后，IntelliJ 应显示“生成成功”消息。</span><span class="sxs-lookup"><span data-stu-id="76320-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![生成成功消息][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="76320-153">创建随时可用于部署的项目</span><span class="sxs-lookup"><span data-stu-id="76320-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="76320-154">若要发布 Spring Boot 应用，需要创建一个随时可用于部署的项目。</span><span class="sxs-lookup"><span data-stu-id="76320-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="76320-155">请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="76320-155">Use the following steps:</span></span>

1. <span data-ttu-id="76320-156">在 IntelliJ 中打开你的 Web 应用项目。</span><span class="sxs-lookup"><span data-stu-id="76320-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="76320-157">依次单击“File”（文件）、“Project Structure”（项目结构）。</span><span class="sxs-lookup"><span data-stu-id="76320-157">Click **File**, and then click **Project Structure**.</span></span>

   ![“项目结构”命令][ART01]

1. <span data-ttu-id="76320-159">单击绿色加号（“+”）符号添加项目，然后依次单击“JAR”、“空”。</span><span class="sxs-lookup"><span data-stu-id="76320-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![添加项目][ART02]

1. <span data-ttu-id="76320-161">在确保不添加“.jar”扩展名的同时命名你的项目，然后指定 Maven 输出的目标文件夹。</span><span class="sxs-lookup"><span data-stu-id="76320-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![指定项目属性][ART03]

1. <span data-ttu-id="76320-163">创建项目清单（可选）：</span><span class="sxs-lookup"><span data-stu-id="76320-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="76320-164">a.在“横幅徽标”下面，选择“删除上传的徽标”。</span><span class="sxs-lookup"><span data-stu-id="76320-164">a.</span></span> <span data-ttu-id="76320-165">单击“Create Manifest”（创建清单）。</span><span class="sxs-lookup"><span data-stu-id="76320-165">Click **Create Manifest**.</span></span>

      ![单击“创建清单”按钮][ART04a]

   <span data-ttu-id="76320-167">b.</span><span class="sxs-lookup"><span data-stu-id="76320-167">b.</span></span> <span data-ttu-id="76320-168">选择项目的默认路径，然后单击“OK”（确定）。</span><span class="sxs-lookup"><span data-stu-id="76320-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![指定项目路径][ART04b]

   <span data-ttu-id="76320-170">c.</span><span class="sxs-lookup"><span data-stu-id="76320-170">c.</span></span> <span data-ttu-id="76320-171">单击省略号“...”找到主类。</span><span class="sxs-lookup"><span data-stu-id="76320-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![找到主类][ART04c]

   <span data-ttu-id="76320-173">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="76320-173">d.</span></span> <span data-ttu-id="76320-174">选择主类，然后单击“OK”（确定）。</span><span class="sxs-lookup"><span data-stu-id="76320-174">Choose your main class, and then click **OK**.</span></span>

      ![指定主类][ART04d]

1. <span data-ttu-id="76320-176">单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="76320-176">Click **OK**.</span></span>

   ![关闭“项目结构”对话框][ART05]

> [!NOTE]
> <span data-ttu-id="76320-178">有关在 IntelliJ 中创建项目的详细信息，请参阅 JetBrains 网站上的 [Configuring Artifacts]（配置项目）。</span><span class="sxs-lookup"><span data-stu-id="76320-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="76320-179">生成要部署的项目</span><span class="sxs-lookup"><span data-stu-id="76320-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="76320-180">单击“Build”（生成），然后单击“Artifacts”（项目）。</span><span class="sxs-lookup"><span data-stu-id="76320-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![“生成项目”命令][BU04]

1. <span data-ttu-id="76320-182">当出现“Build Artifact”（生成项目）上下文菜单时，请单击“Build”（生成）。</span><span class="sxs-lookup"><span data-stu-id="76320-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![“生成项目”上下文菜单][BU05]

<span data-ttu-id="76320-184">IntelliJ 应在项目工具窗口中显示 Spring Boot 应用的已完成项目。</span><span class="sxs-lookup"><span data-stu-id="76320-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![创建的项目][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="76320-186">使用 Docker 容器将 Web 应用发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="76320-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="76320-187">如果尚未登录到 Azure 帐户，请执行[适用于 IntelliJ 的 Azure 工具包的登录说明][Azure Sign In for IntelliJ]中的步骤。</span><span class="sxs-lookup"><span data-stu-id="76320-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="76320-188">在“项目资源管理器”工具窗口中，右键单击该项目，然后选择“Azure” > “Publish as Docker Container”（发布为 Docker 容器）。</span><span class="sxs-lookup"><span data-stu-id="76320-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![“发布为 Docker 容器”命令][PU01]

1. <span data-ttu-id="76320-190">当显示“在 Azure 上部署 Docker 容器”对话框时，任何现有的 Docker 主机均会显示。</span><span class="sxs-lookup"><span data-stu-id="76320-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="76320-191">如果选择部署到现有主机，可以跳到步骤 4。</span><span class="sxs-lookup"><span data-stu-id="76320-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="76320-192">否则，使用以下步骤创建主机：</span><span class="sxs-lookup"><span data-stu-id="76320-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="76320-193">a.</span><span class="sxs-lookup"><span data-stu-id="76320-193">a.</span></span> <span data-ttu-id="76320-194">单击绿色加号（“+”）符号。</span><span class="sxs-lookup"><span data-stu-id="76320-194">Click the green plus (**+**) symbol.</span></span>

      ![添加新的 Docker 主机][PU02]

   <span data-ttu-id="76320-196">b.</span><span class="sxs-lookup"><span data-stu-id="76320-196">b.</span></span> <span data-ttu-id="76320-197">当显示“创建 Docker 主机”对话框时，可以选择接受默认设置，也可以为新的 Docker 主机指定任何自定义设置。</span><span class="sxs-lookup"><span data-stu-id="76320-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="76320-198">（有关各种设置的详细说明，请参阅[使用适用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器][Publish Container with Azure Toolkit]。）在指定了要使用的设置后，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="76320-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![指定 Docker 主机选项][PU03a]

   <span data-ttu-id="76320-200">c.</span><span class="sxs-lookup"><span data-stu-id="76320-200">c.</span></span> <span data-ttu-id="76320-201">可以选择使用 Azure Key Vault 中的现有登录凭据，也可以选择输入新的 Docker 登录凭据。</span><span class="sxs-lookup"><span data-stu-id="76320-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="76320-202">在指定了选项后单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="76320-202">Click **Finish** when you have specified your options.</span></span>

      ![指定 Docker 主机凭据][PU03b]

1. <span data-ttu-id="76320-204">选择你的 Docker 主机，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="76320-204">Select your Docker host, and then click **Next**.</span></span>

   ![选择要使用的 Docker 主机][PU04]

1. <span data-ttu-id="76320-206">在“在 Azure 上部署 Docker 容器”对话框的最后一页上，指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="76320-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="76320-207">a.</span><span class="sxs-lookup"><span data-stu-id="76320-207">a.</span></span> <span data-ttu-id="76320-208">可以选择为要托管 Docker 容器的容器指定一个自定义名称，也可以接受默认设置。</span><span class="sxs-lookup"><span data-stu-id="76320-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="76320-209">b.</span><span class="sxs-lookup"><span data-stu-id="76320-209">b.</span></span> <span data-ttu-id="76320-210">使用以下语法输入 Docker 主机的 TCP 端口：[外部端口]:[内部端口]。</span><span class="sxs-lookup"><span data-stu-id="76320-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="76320-211">例如，“80:8080”指定外部端口为“80”，默认的内部 Spring Boot 端口为“8080”。</span><span class="sxs-lookup"><span data-stu-id="76320-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="76320-212">如果已自定义内部端口（例如通过编辑 application.yml 文件自定义），则需要指定端口号以便能够在 Azure 中进行正确路由。</span><span class="sxs-lookup"><span data-stu-id="76320-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="76320-213">c.</span><span class="sxs-lookup"><span data-stu-id="76320-213">c.</span></span> <span data-ttu-id="76320-214">在配置了这些选项后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="76320-214">After you have configured these options, click **Finish**.</span></span>

   ![在 Azure 上部署 Docker 容器][PU05]

1. <span data-ttu-id="76320-216">Azure 工具包完成发布后，Azure 活动日志显示状态为“已发布”。</span><span class="sxs-lookup"><span data-stu-id="76320-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![已成功部署 Docker 主机][PU06]

## <a name="next-steps"></a><span data-ttu-id="76320-218">后续步骤</span><span class="sxs-lookup"><span data-stu-id="76320-218">Next steps</span></span>

<span data-ttu-id="76320-219">若要了解使用 IntelliJ 创建 Spring Boot 应用的其他方法，请参阅 JetBrains 网站上的[创建 Spring Boot 项目](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html)。</span><span class="sxs-lookup"><span data-stu-id="76320-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
