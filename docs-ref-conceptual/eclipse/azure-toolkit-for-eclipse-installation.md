---
title: "安装用于 Eclipse 的 Azure 工具包"
description: "了解如何安装用于 Eclipse 的 Azure 工具包。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 1f06b02a4c0b23d98ecd394d42f41f7148b6c8e8
ms.sourcegitcommit: 062e07cbd42cda74f02c82b933ce90da646a50a0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="deb0b-103">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="deb0b-103">Installing the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="deb0b-104">使用用于 Eclipse 的 Azure 工具包提供的模板和功能，可轻松地利用 Eclipse 开发环境创建、开发、测试和部署 Azure 应用程序。</span><span class="sxs-lookup"><span data-stu-id="deb0b-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="deb0b-105">用于 Eclipse 的 Azure 工具包是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为：</span><span class="sxs-lookup"><span data-stu-id="deb0b-105">The Azure Toolkit for Eclipse is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="deb0b-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="deb0b-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="deb0b-107">以下步骤说明如何安装用于 Eclipse 的 Azure 工具包。</span><span class="sxs-lookup"><span data-stu-id="deb0b-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="deb0b-108">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="deb0b-108">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="deb0b-109">启动 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="deb0b-109">Start Eclipse.</span></span>

1. <span data-ttu-id="deb0b-110">单击“帮助”菜单，然后单击“安装新软件”，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="deb0b-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][01]

1. <span data-ttu-id="deb0b-112">在“可用软件”对话框的“使用”文本框中，键入 `http://dl.microsoft.com/eclipse/`，然后按 Enter 键。</span><span class="sxs-lookup"><span data-stu-id="deb0b-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="deb0b-113">在“名称”窗格中，选中“用于 Eclipse 的 Azure 工具包”，并取消选中“在安装过程中访问所有更新站点以查找所需的软件”。</span><span class="sxs-lookup"><span data-stu-id="deb0b-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="deb0b-114">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="deb0b-114">Your screen should appear similar to the following:</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][02]

1. <span data-ttu-id="deb0b-116">如果展开“用于 Eclipse 的 Azure 工具包”，会看到一个要安装的组件的列表，例如：</span><span class="sxs-lookup"><span data-stu-id="deb0b-116">If you expand **Azure Toolkit for Eclipse**, you will see a list of components that will be installed; for example:</span></span>

   | <span data-ttu-id="deb0b-117">功能</span><span class="sxs-lookup"><span data-stu-id="deb0b-117">Feature</span></span> | <span data-ttu-id="deb0b-118">说明</span><span class="sxs-lookup"><span data-stu-id="deb0b-118">Description</span></span> | 
   |---|---| 
   | <span data-ttu-id="deb0b-119">**用于 Java 的 Application Insights 插件**</span><span class="sxs-lookup"><span data-stu-id="deb0b-119">**Application Insights Plugin for Java**</span></span> | <span data-ttu-id="deb0b-120">可通过此组件将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。</span><span class="sxs-lookup"><span data-stu-id="deb0b-120">Allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span> | 
   | <span data-ttu-id="deb0b-121">**Azure 常用插件**</span><span class="sxs-lookup"><span data-stu-id="deb0b-121">**Azure Common Plugin**</span></span> | <span data-ttu-id="deb0b-122">提供其他工具包组件所需的常见功能。</span><span class="sxs-lookup"><span data-stu-id="deb0b-122">provides the common functionality needed by other toolkit components.</span></span> | 
   | <span data-ttu-id="deb0b-123">**用于 Eclipse 的 Azure 容器工具**</span><span class="sxs-lookup"><span data-stu-id="deb0b-123">**Azure Container Tools for Eclipse**</span></span> | <span data-ttu-id="deb0b-124">用于生成 .WAR 并将其作为 Docker 容器部署到 Docker 计算机。</span><span class="sxs-lookup"><span data-stu-id="deb0b-124">Enables you to build and deploy a .WAR as a Docker container to a docker machine.</span></span> | 
   | <span data-ttu-id="deb0b-125">**用于 Eclipse 的 Azure 容器**</span><span class="sxs-lookup"><span data-stu-id="deb0b-125">**Azure Containers for Eclipse**</span></span> | <span data-ttu-id="deb0b-126">用于将 .WAR 或 .JAR 项目作为 Docker 容器部署到 Azure 虚拟机。</span><span class="sxs-lookup"><span data-stu-id="deb0b-126">Enables you to deploy a .WAR or .JAR artifact as a Docker container to an Azure virtual machine.</span></span> | 
   | <span data-ttu-id="deb0b-127">**用于 Eclipse 的 Azure 资源管理器**</span><span class="sxs-lookup"><span data-stu-id="deb0b-127">**Azure Explorer for Eclipse**</span></span> | <span data-ttu-id="deb0b-128">提供一个资源管理器样式的界面，用于管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="deb0b-128">Provides an explorer-style interface for managing your Azure resources.</span></span> | 
   | <span data-ttu-id="deb0b-129">**Microsoft JDBC Driver 6.1 for SQL Server**</span><span class="sxs-lookup"><span data-stu-id="deb0b-129">**Microsoft JDBC Driver 6.1 for SQL Server**</span></span> | <span data-ttu-id="deb0b-130">提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="deb0b-130">Provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span> | 
   | <span data-ttu-id="deb0b-131">**Microsoft Azure Java 库包**</span><span class="sxs-lookup"><span data-stu-id="deb0b-131">**Package for Microsoft Azure Libraries for Java**</span></span> | <span data-ttu-id="deb0b-132">提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等）的 API。</span><span class="sxs-lookup"><span data-stu-id="deb0b-132">Provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span> | 

1. <span data-ttu-id="deb0b-133">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="deb0b-133">Click **Next**.</span></span> <span data-ttu-id="deb0b-134">（如果在安装该工具包时遇到不正常的延迟，请确保未选中“在安装过程中访问所有更新站点以查找所需的软件”。）</span><span class="sxs-lookup"><span data-stu-id="deb0b-134">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="deb0b-135">在“安装详细信息”对话框中，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="deb0b-135">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![查看安装详细信息][03]

1. <span data-ttu-id="deb0b-137">在“查看许可证”对话框中，查看许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="deb0b-137">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="deb0b-138">如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="deb0b-138">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="deb0b-139">（剩余步骤假定你接受许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="deb0b-139">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="deb0b-140">如果不接受许可协议条款，请退出安装过程。）</span><span class="sxs-lookup"><span data-stu-id="deb0b-140">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![查看许可证][04]
   
   <span data-ttu-id="deb0b-142">Eclipse 将下载并安装必要的包。</span><span class="sxs-lookup"><span data-stu-id="deb0b-142">Eclipse will download and install the requisite packages.</span></span>
   
   ![安装进度][05]

1. <span data-ttu-id="deb0b-144">如果系统提示重新启动 Eclipse 以完成安装，请单击“是”。</span><span class="sxs-lookup"><span data-stu-id="deb0b-144">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![重新启动提示][06]

## <a name="next-steps"></a><span data-ttu-id="deb0b-146">后续步骤</span><span class="sxs-lookup"><span data-stu-id="deb0b-146">Next steps</span></span>

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
