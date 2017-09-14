---
title: "用于 IntelliJ 的 Azure 工具包中的新增功能"
description: "了解用于 IntelliJ 的 Azure 工具包中的最新功能。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 861ceb148976faff1f40055094c463171bdfb4b0
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a><span data-ttu-id="05474-103">用于 IntelliJ 的 Azure 工具包中的新增功能</span><span class="sxs-lookup"><span data-stu-id="05474-103">What's New in the Azure Toolkit for IntelliJ</span></span>

## <a name="azure-toolkit-for-intellij-releases"></a><span data-ttu-id="05474-104">用于 IntelliJ 的 Azure 工具包版本</span><span class="sxs-lookup"><span data-stu-id="05474-104">Azure Toolkit for IntelliJ Releases</span></span>
<span data-ttu-id="05474-105">本文包含有关用于 IntelliJ 的 Azure 工具包的不同版本和最新更新的信息。</span><span class="sxs-lookup"><span data-stu-id="05474-105">This article contains information on the various releases and latest updates to the Azure Toolkit for IntelliJ.</span></span>

> [!NOTE]
> <span data-ttu-id="05474-106">另外还有用于 Eclipse IDE 的 Azure 工具包。</span><span class="sxs-lookup"><span data-stu-id="05474-106">There is also an Azure Toolkit for the Eclipse IDE.</span></span> <span data-ttu-id="05474-107">有关详细信息，请参阅[用于 Eclipse 的 Azure 工具包]。</span><span class="sxs-lookup"><span data-stu-id="05474-107">For more information, see [Azure Toolkit for Eclipse].</span></span>
> 
> 

### <a name="april-14-2017"></a><span data-ttu-id="05474-108">2017 年 4 月 14 日</span><span class="sxs-lookup"><span data-stu-id="05474-108">April 14, 2017</span></span>
<span data-ttu-id="05474-109">用于 IntelliJ 的 Azure 工具包 - 2017 年 4 月版包含以下增强功能：</span><span class="sxs-lookup"><span data-stu-id="05474-109">The Azure Toolkit for IntelliJ - April 2017 release includes the following enhancements:</span></span>

* <span data-ttu-id="05474-110">**改进了 Azure 登录体验**：用于 IntelliJ 的 Azure 工具包现在支持以两种方式登录到 Azure 帐户：*交互式*和*自动*。</span><span class="sxs-lookup"><span data-stu-id="05474-110">**Improved Azure Sign In Experience**: The Azure Toolkit for IntelliJ now supports two methods of logging into your Azure account: *Interactive* and *Automated*.</span></span> <span data-ttu-id="05474-111">有关详细信息，请参阅[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明]。</span><span class="sxs-lookup"><span data-stu-id="05474-111">For more information, see [Azure Sign In Instructions for the Azure Toolkit for IntelliJ].</span></span>
* <span data-ttu-id="05474-112">**使用 Docker 容器发布**：现在可以使用用于 IntelliJ 的 Azure 工具包将 Web 应用程序发布为 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="05474-112">**Publishing using Docker Containers**: You can now publish your web applications as Docker Containers using Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="05474-113">有关详细信息，请参阅[如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器]。</span><span class="sxs-lookup"><span data-stu-id="05474-113">For more information, see [How to publish a Web App as a Docker Container using the Azure Toolkit for IntelliJ].</span></span>
* <span data-ttu-id="05474-114">**存储帐户管理**：用于 IntelliJ 的 Azure 工具包现在支持从 Azure 资源管理器视图管理存储帐户。</span><span class="sxs-lookup"><span data-stu-id="05474-114">**Storage Account Management**: The Azure Toolkit for IntelliJ now supports managing your storage accounts from the Azure Explorer View.</span></span> <span data-ttu-id="05474-115">有关详细信息，请参阅[使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户]。</span><span class="sxs-lookup"><span data-stu-id="05474-115">For more information, see [Managing Storage Accounts using the Azure Explorer for IntelliJ].</span></span>
* <span data-ttu-id="05474-116">**虚拟机管理**：用于 IntelliJ 的 Azure 工具包现在支持从“Azure 资源管理器”工具窗口管理虚拟机。</span><span class="sxs-lookup"><span data-stu-id="05474-116">**Virtual Machine Management**: The Azure Toolkit for IntelliJ now supports managing your virtual machines from the Azure Explorer Tool Window.</span></span> <span data-ttu-id="05474-117">有关详细信息，请参阅[使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机]。</span><span class="sxs-lookup"><span data-stu-id="05474-117">For more information, see [Managing Virtual Machines using the Azure Explorer for IntelliJ].</span></span>
* <span data-ttu-id="05474-118">**删除了远程调试支持**。</span><span class="sxs-lookup"><span data-stu-id="05474-118">**Removal of Remote Debugging Support**.</span></span> <span data-ttu-id="05474-119">在 Azure 应用服务中对 Java Web 应用进行远程调试的功能已从用于 IntelliJ 的 Azure 工具包中删除；这是为了解决客户在使用该工具包时遇到的问题所必需的。</span><span class="sxs-lookup"><span data-stu-id="05474-119">Remote debugging of Java web apps on Azure App Service has been removed from the Azure Toolkit for IntelliJ; this was necessary to resolve some problems which customers were experiencing when using the toolkit.</span></span>

### <a name="august-26-2016"></a><span data-ttu-id="05474-120">2016 年 8 月 26 日</span><span class="sxs-lookup"><span data-stu-id="05474-120">August 26, 2016</span></span>
<span data-ttu-id="05474-121">用于 IntelliJ 的 Azure 工具包 - 2016 年 8 月版包含以下增强功能：</span><span class="sxs-lookup"><span data-stu-id="05474-121">The Azure Toolkit for IntelliJ - August 2016 release includes the following enhancements:</span></span>

* <span data-ttu-id="05474-122">**自定义 JDK 分发版**。</span><span class="sxs-lookup"><span data-stu-id="05474-122">**Custom JDK Distributions**.</span></span> <span data-ttu-id="05474-123">用于 IntelliJ 的 Azure 工具包现在支持指定任意 JDK 版本并将其部署到 Azure WebApp 容器：</span><span class="sxs-lookup"><span data-stu-id="05474-123">The Azure Toolkit for IntelliJ now supports specifying and deploying an arbitrary JDK version to your Azure WebApp container:</span></span>
  * <span data-ttu-id="05474-124">除了 Azure 提供的 JDK 以外，还可以从 Azul Systems 在 Azure 上提供的多种 Zulu OpenJDK 版本中选择。</span><span class="sxs-lookup"><span data-stu-id="05474-124">In addition to the JDKs provided by Azure, you can also choose from a wide selection of Zulu OpenJDK versions made available on Azure by Azul Systems.</span></span>
  * <span data-ttu-id="05474-125">还可以指定自己的 JDK 分发版（如果将它作为 ZIP 文件上传存储帐户）。</span><span class="sxs-lookup"><span data-stu-id="05474-125">You can also specify your own JDK distribution if you upload it as a ZIP file to your storage account.</span></span>
* <span data-ttu-id="05474-126">**Azure 资源管理器视图增强**：</span><span class="sxs-lookup"><span data-stu-id="05474-126">**Enhancements to the Azure Explorer view**:</span></span>
  * <span data-ttu-id="05474-127">使用 Azure 的新 Resource Manager 模型为虚拟机管理提供支持：可以列出、创建和删除基于 Resource Manager 的虚拟机，而无需退出 IDE。</span><span class="sxs-lookup"><span data-stu-id="05474-127">Support for Virtual Machine management using Azure's new Resource Manager model: you can list, create and delete resource manager-based virtual machines without leaving the IDE.</span></span>
  * <span data-ttu-id="05474-128">使用 Azure 的 Resource Manager 为存储帐户 Blob 管理提供支持，对管理“经典”存储帐户的现有功能做出补充。</span><span class="sxs-lookup"><span data-stu-id="05474-128">Support for Storage Account blob management using Azure's Resource Manager, which complements the existing functionality for managing "classic" storage accounts.</span></span>
* <span data-ttu-id="05474-129">**Microsoft JDBC Driver 6.0 for SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="05474-129">**Microsoft JDBC Driver 6.0 for SQL Server**.</span></span> <span data-ttu-id="05474-130">此项更新包括适用于 Microsoft SQL Server 的最新 JDBC 驱动程序 (v6.0)，现在，该驱动程序以库的形式提供，可以轻松添加到 Java 项目，从而取代旧版本。</span><span class="sxs-lookup"><span data-stu-id="05474-130">This update includes the latest JDBC driver for Microsoft SQL Server (v6.0), which is now included as a library that you can easily add to your Java projects, thereby replacing the older version.</span></span>

### <a name="june-29-2016"></a><span data-ttu-id="05474-131">2016 年 6 月 29 日</span><span class="sxs-lookup"><span data-stu-id="05474-131">June 29, 2016</span></span>
<span data-ttu-id="05474-132">Azure Toolkit for IntelliJ - 2016 年 6 月版包含以下增强功能：</span><span class="sxs-lookup"><span data-stu-id="05474-132">The Azure Toolkit for IntelliJ - June 2016 release includes the following enhancements:</span></span>

* <span data-ttu-id="05474-133">**需要 Java 8**。</span><span class="sxs-lookup"><span data-stu-id="05474-133">**Java 8 Requirement**.</span></span> <span data-ttu-id="05474-134">Azure Toolkit for IntelliJ 现在需要 Java 8，不过此需求仅适用于此工具包 - 应用程序可以继续使用 Azure 支持的所有 Java 版本。</span><span class="sxs-lookup"><span data-stu-id="05474-134">The Azure Toolkit for IntelliJ now requires Java 8, although this requirement is only for the toolkit - your applications can continue to use all versions of Java that are supported by Azure.</span></span>
* <span data-ttu-id="05474-135">**支持最新的 Java JDK**。</span><span class="sxs-lookup"><span data-stu-id="05474-135">**Support for the latest Java JDKs**.</span></span> <span data-ttu-id="05474-136">Azure Toolkit for IntelliJ 现在支持最新版本的 Java JDK。</span><span class="sxs-lookup"><span data-stu-id="05474-136">The latest versions of the Java JDKs are now supported by the Azure Toolkit for IntelliJ.</span></span>
* <span data-ttu-id="05474-137">**支持 Azure SDK v2.9.1**。</span><span class="sxs-lookup"><span data-stu-id="05474-137">**Support for Azure SDK v2.9.1**.</span></span> <span data-ttu-id="05474-138">最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。</span><span class="sxs-lookup"><span data-stu-id="05474-138">The latest version of the Azure SDK is now the minimum pre-requisite for the Azure Toolkit for IntelliJ.</span></span>
* <span data-ttu-id="05474-139">**集成示例**。</span><span class="sxs-lookup"><span data-stu-id="05474-139">**Integrated Samples**.</span></span> <span data-ttu-id="05474-140">Azure Toolkit for IntelliJ 目前精选了数个示例应用程序，可帮助开发人员快速入门。</span><span class="sxs-lookup"><span data-stu-id="05474-140">The Azure Toolkit for IntelliJ now features several sample applications to help developers get started.</span></span>
* <span data-ttu-id="05474-141">**HDInsight 工具集成**。</span><span class="sxs-lookup"><span data-stu-id="05474-141">**HDInsight Tool Integration**.</span></span> <span data-ttu-id="05474-142">Azure 的 HDInsight 工具现在随附于 Azure Toolkit for IntelliJ。</span><span class="sxs-lookup"><span data-stu-id="05474-142">Azure's HDInsight Tools are now bundled with the Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="05474-143">有关详细信息，请参阅 [用于 IntelliJ 的 HDInsight 工具插件]。</span><span class="sxs-lookup"><span data-stu-id="05474-143">For more information, see [HDInsight Tools Plugin for IntelliJ].</span></span>
* <span data-ttu-id="05474-144">**远程调试 Java Web 应用**。</span><span class="sxs-lookup"><span data-stu-id="05474-144">**Remote Debugging of Java Web Apps**.</span></span> <span data-ttu-id="05474-145">Azure Toolkit for IntelliJ 现在支持对 Azure 应用服务上的 Java Web 应用进行远程调试。</span><span class="sxs-lookup"><span data-stu-id="05474-145">The Azure Toolkit for IntelliJ now supports remote debugging of Java web apps on Azure App Service.</span></span>

### <a name="april-12-2016"></a><span data-ttu-id="05474-146">2016 年 4 月 12 日</span><span class="sxs-lookup"><span data-stu-id="05474-146">April 12, 2016</span></span>
<span data-ttu-id="05474-147">Azure Toolkit for IntelliJ - 2016 年 4 月版包含以下增强功能：</span><span class="sxs-lookup"><span data-stu-id="05474-147">The Azure Toolkit for IntelliJ - April 2016 release includes the following enhancements:</span></span>

* <span data-ttu-id="05474-148">**支持 Azure SDK v2.9.0**。</span><span class="sxs-lookup"><span data-stu-id="05474-148">**Support for Azure SDK v2.9.0**.</span></span> <span data-ttu-id="05474-149">最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。</span><span class="sxs-lookup"><span data-stu-id="05474-149">The latest version of the Azure SDK is now the minimum pre-requisite for the Azure Toolkit for IntelliJ.</span></span>
* <span data-ttu-id="05474-150">**与 Azure Web 应用支持有关的其他可用性、响应能力和性能改进**。</span><span class="sxs-lookup"><span data-stu-id="05474-150">**Miscellaneous usability, responsiveness and performance improvements related to Azure Web App support**.</span></span> <span data-ttu-id="05474-151">工具包中多项与 Azure 通信方式的性能优化会让 UI 响应速度更快。</span><span class="sxs-lookup"><span data-stu-id="05474-151">A number of performance optimizations in how the Toolkit communicates with Azure result in a more responsive UI.</span></span>
* <span data-ttu-id="05474-152">**能够在 IntelliJ 内删除 Azure 中现有的 Web 应用程序容器**。</span><span class="sxs-lookup"><span data-stu-id="05474-152">**Ability to delete an existing Web Application container in Azure from within IntelliJ**.</span></span> <span data-ttu-id="05474-153">Azure Toolkit for IntelliJ 现在允许删除现有的 Azure Web 容器，而不需要退出 IntelliJ。</span><span class="sxs-lookup"><span data-stu-id="05474-153">The Azure Toolkit for IntelliJ now allows you to delete an existing Azure Web container without leaving IntelliJ.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05474-154">后续步骤</span><span class="sxs-lookup"><span data-stu-id="05474-154">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[用于 Eclipse 的 Azure 工具包]: ../eclipse/azure-toolkit-for-eclipse.md

[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: https://docs.microsoft.com/java/azure

[用于 IntelliJ 的 HDInsight 工具插件]: /azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin
