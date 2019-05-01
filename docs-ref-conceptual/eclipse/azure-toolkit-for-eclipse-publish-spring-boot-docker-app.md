---
title: 使用适用于 Eclipse 的 Azure 工具包将 Spring Boot 应用作为 Docker 容器发布
description: 了解如何使用用于 Eclipse 的 Azure 工具包将 Web 应用作为 Docker 容器发布到 Microsoft Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 5f70d8dd7e9b59c365d83c185f430a5a9c75e838
ms.sourcegitcommit: 4f1acf05e3bbb7eb6bca9b65300c1c5b9772185a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63456319"
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="c944b-103">使用适用于 Eclipse 的 Azure 工具包将 Spring Boot 应用作为 Docker 容器发布</span><span class="sxs-lookup"><span data-stu-id="c944b-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="c944b-104">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="c944b-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="c944b-105">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了简化的方法来创建独立的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="c944b-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="c944b-106">[Docker] 是一种开放源代码解决方案，可帮助开发人员自动部署、扩展和管理在容器中运行的应用程序。</span><span class="sxs-lookup"><span data-stu-id="c944b-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="c944b-107">本教程将指导你完成使用适用于 Eclipse 的 Azure 工具包将 Spring Boot 应用程序作为 Docker 容器部署到 Microsoft Azure 的步骤。</span><span class="sxs-lookup"><span data-stu-id="c944b-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="c944b-108">克隆默认 Spring Boot Docker 存储库</span><span class="sxs-lookup"><span data-stu-id="c944b-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="c944b-109">导入公共存储库</span><span class="sxs-lookup"><span data-stu-id="c944b-109">Import the public repository</span></span>

<span data-ttu-id="c944b-110">以下步骤将指导你完成使用 IntelliJ 将 Spring Boot Docker 存储库克隆到本地计算机的步骤。</span><span class="sxs-lookup"><span data-stu-id="c944b-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="c944b-111">如果要使用命令行，请参阅[在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上][Deploy Spring Boot on Linux in AKS]。</span><span class="sxs-lookup"><span data-stu-id="c944b-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in AKS].</span></span>

1. <span data-ttu-id="c944b-112">打开 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="c944b-112">Open Eclipse.</span></span>

1. <span data-ttu-id="c944b-113">单击“文件” > “导入”。</span><span class="sxs-lookup"><span data-stu-id="c944b-113">Click **File** > **Import**.</span></span>

   ![文件导入菜单][CL01]

1. <span data-ttu-id="c944b-115">“导入”对话框打开后：</span><span class="sxs-lookup"><span data-stu-id="c944b-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="c944b-116">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-116">a.</span></span> <span data-ttu-id="c944b-117">展开 Git。</span><span class="sxs-lookup"><span data-stu-id="c944b-117">Expand **Git**.</span></span>

   <span data-ttu-id="c944b-118">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-118">b.</span></span> <span data-ttu-id="c944b-119">选择“来自 Git 的项目”。</span><span class="sxs-lookup"><span data-stu-id="c944b-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="c944b-120">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-120">c.</span></span> <span data-ttu-id="c944b-121">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-121">Click **Next**.</span></span>

   ![“导入”对话框][CL02]

1. <span data-ttu-id="c944b-123">在“选择存储库源”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="c944b-124">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-124">a.</span></span> <span data-ttu-id="c944b-125">选择“克隆 URI”。</span><span class="sxs-lookup"><span data-stu-id="c944b-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="c944b-126">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-126">b.</span></span> <span data-ttu-id="c944b-127">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-127">Click **Next**.</span></span>

   ![选择"存储库源"页][CL03]

1. <span data-ttu-id="c944b-129">在“源 Git 存储库”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="c944b-130">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-130">a.</span></span> <span data-ttu-id="c944b-131">对于“URI”，输入 `https://github.com/spring-guides/gs-spring-boot-docker.git`。</span><span class="sxs-lookup"><span data-stu-id="c944b-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="c944b-132">此步骤应该会为“主机”和“存储库路径”字段自动填充正确的值。</span><span class="sxs-lookup"><span data-stu-id="c944b-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="c944b-133">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-133">b.</span></span> <span data-ttu-id="c944b-134">Spring Boot 存储库是公开的，因此，你不必输入 Git 用户名和密码。</span><span class="sxs-lookup"><span data-stu-id="c944b-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="c944b-135">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-135">c.</span></span> <span data-ttu-id="c944b-136">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-136">Click **Next**.</span></span>

   ![“源 Git 存储库”页][CL04]

1. <span data-ttu-id="c944b-138">在“分支选择”页上，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![“分支选择”页][CL05]

1. <span data-ttu-id="c944b-140">在“本地目标”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="c944b-141">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-141">a.</span></span> <span data-ttu-id="c944b-142">指定放置本地存储库的本地文件夹。</span><span class="sxs-lookup"><span data-stu-id="c944b-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="c944b-143">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-143">b.</span></span> <span data-ttu-id="c944b-144">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-144">Click **Next**.</span></span>

   ![“本地目标”页][CL06]

1. <span data-ttu-id="c944b-146">在“选择用于导入项目的向导”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="c944b-147">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-147">a.</span></span> <span data-ttu-id="c944b-148">选择“作为常规项目导入”。</span><span class="sxs-lookup"><span data-stu-id="c944b-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="c944b-149">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-149">b.</span></span> <span data-ttu-id="c944b-150">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-150">Click **Next**.</span></span>

   ![“选择用于导入项目的向导”页][CL07]

1. <span data-ttu-id="c944b-152">在“导入项目”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="c944b-153">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-153">a.</span></span> <span data-ttu-id="c944b-154">指定项目名称。</span><span class="sxs-lookup"><span data-stu-id="c944b-154">Specify your project name.</span></span>
   
   <span data-ttu-id="c944b-155">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-155">b.</span></span> <span data-ttu-id="c944b-156">单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="c944b-156">Click **Finish**.</span></span>

   ![“导入项目”页][CL08]

1. <span data-ttu-id="c944b-158">当存储库成功克隆时，可以看到 Eclipse 中列出的所有文件。</span><span class="sxs-lookup"><span data-stu-id="c944b-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![本地存储库][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="c944b-160">从本地存储库创建 Maven 项目</span><span class="sxs-lookup"><span data-stu-id="c944b-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="c944b-161">Spring Boot Docker 存储库包含一个已完成的 Maven 项目，可用于本教程。</span><span class="sxs-lookup"><span data-stu-id="c944b-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="c944b-162">单击“文件” > “导入”。</span><span class="sxs-lookup"><span data-stu-id="c944b-162">Click **File** > **Import**.</span></span>

   ![“文件”菜单上的“导入”命令][CL01]

1. <span data-ttu-id="c944b-164">“导入”对话框打开后：</span><span class="sxs-lookup"><span data-stu-id="c944b-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="c944b-165">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-165">a.</span></span> <span data-ttu-id="c944b-166">展开 Maven。</span><span class="sxs-lookup"><span data-stu-id="c944b-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="c944b-167">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-167">b.</span></span> <span data-ttu-id="c944b-168">选择“现有 Maven 项目”。</span><span class="sxs-lookup"><span data-stu-id="c944b-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="c944b-169">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-169">c.</span></span> <span data-ttu-id="c944b-170">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-170">Click **Next**.</span></span>

   ![“导入”对话框][MV01]

1. <span data-ttu-id="c944b-172">在“Maven 项目”页上：</span><span class="sxs-lookup"><span data-stu-id="c944b-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="c944b-173">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-173">a.</span></span> <span data-ttu-id="c944b-174">对于“根目录”，在本地存储库中指定“complete”文件夹。</span><span class="sxs-lookup"><span data-stu-id="c944b-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="c944b-175">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-175">b.</span></span> <span data-ttu-id="c944b-176">展开“高级”部分，并为“名称模板”输入自定义名称。</span><span class="sxs-lookup"><span data-stu-id="c944b-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="c944b-177">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-177">c.</span></span> <span data-ttu-id="c944b-178">选择项目中 pom.xml 文件对应的框。</span><span class="sxs-lookup"><span data-stu-id="c944b-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="c944b-179">d.</span><span class="sxs-lookup"><span data-stu-id="c944b-179">d.</span></span> <span data-ttu-id="c944b-180">单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="c944b-180">Click **Finish**.</span></span>

   ![“Maven 项目”页][MV02]

1. <span data-ttu-id="c944b-182">当 Maven 项目成功打开后，可以看到 Eclipse 中列出了第二个项目。</span><span class="sxs-lookup"><span data-stu-id="c944b-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![本地 Maven 项目][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="c944b-184">使用 Maven 生成 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="c944b-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="c944b-185">在 Eclipse 项目资源管理器中，选择“Maven 项目”。</span><span class="sxs-lookup"><span data-stu-id="c944b-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="c944b-186">单击“运行” > “运行方式” > “Maven 生成”。</span><span class="sxs-lookup"><span data-stu-id="c944b-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![以 Maven 生成方式运行的命令][BU01]

1. <span data-ttu-id="c944b-188">成功生成应用程序后，控制台窗口会显示状态。</span><span class="sxs-lookup"><span data-stu-id="c944b-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Maven 生成成功][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="c944b-190">使用 Docker 容器将 Web 应用发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="c944b-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="c944b-191">在 Eclipse 项目资源管理器中，选择“Maven 项目”。</span><span class="sxs-lookup"><span data-stu-id="c944b-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="c944b-192">单击 Azure 的“发布”菜单，然后单击“发布为 Docker 容器”。</span><span class="sxs-lookup"><span data-stu-id="c944b-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![“发布为 Docker 容器”命令][PU01]

1. <span data-ttu-id="c944b-194">出现“在 Azure 上部署 Docker 容器”对话框时：</span><span class="sxs-lookup"><span data-stu-id="c944b-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="c944b-195">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-195">a.</span></span> <span data-ttu-id="c944b-196">输入自定义的 Docker 映像名称。</span><span class="sxs-lookup"><span data-stu-id="c944b-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="c944b-197">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-197">b.</span></span> <span data-ttu-id="c944b-198">对于“要部署的项目”，指定刚生成的 gs-spring-boot-docker-0.1.0.jar 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="c944b-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![指定 Docker 选项][PU02]

   <span data-ttu-id="c944b-200">现在显示所有现有的 Docker 主机。</span><span class="sxs-lookup"><span data-stu-id="c944b-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="c944b-201">如果选择部署到现有主机，可以跳到步骤 5。</span><span class="sxs-lookup"><span data-stu-id="c944b-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="c944b-202">否则，使用以下步骤创建主机：</span><span class="sxs-lookup"><span data-stu-id="c944b-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="c944b-203">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-203">a.</span></span> <span data-ttu-id="c944b-204">单击“添加”。</span><span class="sxs-lookup"><span data-stu-id="c944b-204">Click **Add**.</span></span>

      ![添加新的 Docker 主机][PU03]

   <span data-ttu-id="c944b-206">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-206">b.</span></span> <span data-ttu-id="c944b-207">当显示“创建 Docker 主机”对话框时，可以选择接受默认设置，也可以为新的 Docker 主机指定任何自定义设置。</span><span class="sxs-lookup"><span data-stu-id="c944b-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="c944b-208">（有关各种设置的详细说明，请参阅[使用适用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器][Publish Container with Azure Toolkit]。）在指定了要使用的设置后，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![指定 Docker 主机选项][PU04]

   <span data-ttu-id="c944b-210">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-210">c.</span></span> <span data-ttu-id="c944b-211">可以选择使用 Azure Key Vault 中的现有登录凭据，也可以选择输入新的 Docker 登录凭据。</span><span class="sxs-lookup"><span data-stu-id="c944b-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="c944b-212">在指定了选项后单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="c944b-212">Click **Finish** when you have specified your options.</span></span>

      ![指定 Docker 主机凭据][PU05]

1. <span data-ttu-id="c944b-214">选择你的 Docker 主机，然后单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="c944b-214">Select your Docker host, and then click **Next**.</span></span>

   ![选择要使用的 Docker 主机][PU06]

1. <span data-ttu-id="c944b-216">在“在 Azure 上部署 Docker 容器”对话框的最后一页上，指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="c944b-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="c944b-217">a.</span><span class="sxs-lookup"><span data-stu-id="c944b-217">a.</span></span> <span data-ttu-id="c944b-218">可以选择为要托管 Docker 容器的容器指定一个自定义名称，也可以接受默认设置。</span><span class="sxs-lookup"><span data-stu-id="c944b-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="c944b-219">b.</span><span class="sxs-lookup"><span data-stu-id="c944b-219">b.</span></span> <span data-ttu-id="c944b-220">使用以下语法输入 Docker 主机的 TCP 端口：[外部端口]:[内部端口]。</span><span class="sxs-lookup"><span data-stu-id="c944b-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="c944b-221">例如，“80:8080”指定外部端口为“80”，默认的内部 Spring Boot 端口为“8080”。</span><span class="sxs-lookup"><span data-stu-id="c944b-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="c944b-222">如果已自定义内部端口（例如通过编辑 application.yml 文件自定义），则需要指定端口号以便能够在 Azure 中进行正确路由。</span><span class="sxs-lookup"><span data-stu-id="c944b-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="c944b-223">c.</span><span class="sxs-lookup"><span data-stu-id="c944b-223">c.</span></span> <span data-ttu-id="c944b-224">在配置这些选项后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="c944b-224">After you configure these options, click **Finish**.</span></span>

   ![在 Azure 上部署 Docker 容器][PU07]

1. <span data-ttu-id="c944b-226">Azure 工具包完成发布后，Azure 活动日志显示状态为“已发布”。</span><span class="sxs-lookup"><span data-stu-id="c944b-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![已成功部署 Docker 主机][PU08]

## <a name="next-steps"></a><span data-ttu-id="c944b-228">后续步骤</span><span class="sxs-lookup"><span data-stu-id="c944b-228">Next steps</span></span>

<span data-ttu-id="c944b-229">有关 Docker 的其他资源，请参阅官方 [Docker 网站](https://www.docker.com/)。</span><span class="sxs-lookup"><span data-stu-id="c944b-229">For additional resources for Docker, see the official [Docker website](https://www.docker.com/).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in AKS]: /azure/container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: azure-toolkit-for-eclipse-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
