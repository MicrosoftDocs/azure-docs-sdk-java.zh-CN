---
title: 安装用于 Eclipse 的 Azure 工具包
description: 了解如何安装用于 Eclipse 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: d5f685fa62ad74c8b8cd842b3667f8161e7c5760
ms.sourcegitcommit: 798f4d4199d3be9fc5c9f8bf7a754d7393de31ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887747"
---
# <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="ada9b-103">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="ada9b-103">Install the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="ada9b-104">使用用于 Eclipse 的 Azure 工具包提供的模板和功能，可以在 Eclipse 开发环境中轻松创建、开发、测试云应用程序并将其部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="ada9b-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy cloud applications to Azure from the Eclipse development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ada9b-105">用于 Eclipse 的 Azure 工具包是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为：</span><span class="sxs-lookup"><span data-stu-id="ada9b-105">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

<span data-ttu-id="ada9b-106">以下步骤说明如何安装用于 Eclipse 的 Azure 工具包。</span><span class="sxs-lookup"><span data-stu-id="ada9b-106">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="ada9b-107">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="ada9b-107">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="ada9b-108">启动 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="ada9b-108">Start Eclipse.</span></span>

1. <span data-ttu-id="ada9b-109">单击“帮助”菜单，然后单击“安装新软件”，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="ada9b-109">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][01]

1. <span data-ttu-id="ada9b-111">在“可用软件”对话框的“使用”文本框中，键入 `http://dl.microsoft.com/eclipse/`，然后按 Enter 键。</span><span class="sxs-lookup"><span data-stu-id="ada9b-111">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="ada9b-112">在“名称”窗格中，选中“用于 Java 的 Azure 工具包”，并取消选中“在安装过程中访问所有更新站点以查找所需的软件”。</span><span class="sxs-lookup"><span data-stu-id="ada9b-112">In the **Name** pane, check **Azure Toolkit for Java**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="ada9b-113">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="ada9b-113">Your screen should appear similar to the following:</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][02]

1. <span data-ttu-id="ada9b-115">如果展开“用于 Eclipse 的 Azure 工具包”，会看到一个要安装的组件的列表，例如：</span><span class="sxs-lookup"><span data-stu-id="ada9b-115">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="ada9b-116">功能</span><span class="sxs-lookup"><span data-stu-id="ada9b-116">Feature</span></span> | <span data-ttu-id="ada9b-117">说明</span><span class="sxs-lookup"><span data-stu-id="ada9b-117">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="ada9b-118">**用于 Java 的 Application Insights 插件**</span><span class="sxs-lookup"><span data-stu-id="ada9b-118">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="ada9b-119">可通过此组件将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。</span><span class="sxs-lookup"><span data-stu-id="ada9b-119">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="ada9b-120">**Azure 常用插件**</span><span class="sxs-lookup"><span data-stu-id="ada9b-120">**Azure Common Plugin**</span></span> | <span data-ttu-id="ada9b-121">提供其他工具包组件所需的常见功能。</span><span class="sxs-lookup"><span data-stu-id="ada9b-121">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="ada9b-122">**用于 Eclipse 的 Azure 容器工具**</span><span class="sxs-lookup"><span data-stu-id="ada9b-122">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="ada9b-123">用于生成 .WAR 并将其作为 Docker 容器部署到 Docker 计算机。</span><span class="sxs-lookup"><span data-stu-id="ada9b-123">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="ada9b-124">**用于 Eclipse 的 Azure 容器**</span><span class="sxs-lookup"><span data-stu-id="ada9b-124">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="ada9b-125">用于将 .WAR 或 .JAR 项目作为 Docker 容器部署到 Azure 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="ada9b-125">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="ada9b-126">**用于 Eclipse 的 Azure 资源管理器**</span><span class="sxs-lookup"><span data-stu-id="ada9b-126">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="ada9b-127">提供一个资源管理器样式的界面，用于管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="ada9b-127">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="ada9b-128">**Microsoft JDBC Driver 6.1 for SQL Server**</span><span class="sxs-lookup"><span data-stu-id="ada9b-128">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="ada9b-129">提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="ada9b-129">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="ada9b-130">**Microsoft Azure Java 库包**</span><span class="sxs-lookup"><span data-stu-id="ada9b-130">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="ada9b-131">提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等）的 API。</span><span class="sxs-lookup"><span data-stu-id="ada9b-131">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="ada9b-132">单击“资源组名称” 的 Azure 数据工厂。</span><span class="sxs-lookup"><span data-stu-id="ada9b-132">Click **Next**.</span></span> <span data-ttu-id="ada9b-133">（如果在安装该工具包时遇到不正常的延迟，请确保未选中“在安装过程中访问所有更新站点以查找所需的软件”。）</span><span class="sxs-lookup"><span data-stu-id="ada9b-133">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="ada9b-134">在“安装详细信息”对话框中，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="ada9b-134">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![查看安装详细信息][03]

1. <span data-ttu-id="ada9b-136">在“查看许可证”对话框中，查看许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="ada9b-136">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="ada9b-137">如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="ada9b-137">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="ada9b-138">（剩余步骤假定你接受许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="ada9b-138">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="ada9b-139">如果不接受许可协议条款，请退出安装过程。）</span><span class="sxs-lookup"><span data-stu-id="ada9b-139">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![查看许可证][04]
   
   <span data-ttu-id="ada9b-141">Eclipse 将下载并安装必要的包。</span><span class="sxs-lookup"><span data-stu-id="ada9b-141">Eclipse will download and install the requisite packages.</span></span>
   
   ![安装进度][05]

1. <span data-ttu-id="ada9b-143">如果系统提示重新启动 Eclipse 以完成安装，请单击“是”。</span><span class="sxs-lookup"><span data-stu-id="ada9b-143">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![重新启动提示][06]

## <a name="next-steps"></a><span data-ttu-id="ada9b-145">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ada9b-145">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
