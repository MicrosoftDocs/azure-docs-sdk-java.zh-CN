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
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 59a8bfb6ab4db8ea8c6c9025ca3ced8a13192628
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="424ca-103">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="424ca-103">Installing the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="424ca-104">使用 Azure Toolkit for Eclipse 提供的模板和功能，可轻松地利用 Eclipse 开发环境创建、开发、测试和部署 Azure 应用程序。</span><span class="sxs-lookup"><span data-stu-id="424ca-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span> <span data-ttu-id="424ca-105">适用于 Eclipse 的 Azure 工具包是一个开源项目。</span><span class="sxs-lookup"><span data-stu-id="424ca-105">The Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="424ca-106">源代码在 MIT 许可下可用，可从 <https://github.com/microsoft/azure-tools-for-java> 中获得。</span><span class="sxs-lookup"><span data-stu-id="424ca-106">The source code is available under the MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="424ca-107">以下步骤说明如何安装用于 Eclipse 的 Azure 工具包。</span><span class="sxs-lookup"><span data-stu-id="424ca-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="424ca-108">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="424ca-108">To install the Azure Toolkit for Eclipse</span></span>

1. <span data-ttu-id="424ca-109">启动 Eclipse。</span><span class="sxs-lookup"><span data-stu-id="424ca-109">Start Eclipse.</span></span>

1. <span data-ttu-id="424ca-110">单击“帮助”菜单，然后单击“安装新软件”，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="424ca-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][01]

1. <span data-ttu-id="424ca-112">在“可用软件”对话框的“使用”文本框中，键入 `http://dl.microsoft.com/eclipse/`，然后按 Enter 键。</span><span class="sxs-lookup"><span data-stu-id="424ca-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse/` followed by the **Enter** key.</span></span>

1. <span data-ttu-id="424ca-113">在“名称”窗格中，选中“用于 Eclipse 的 Azure 工具包”，并取消选中“在安装过程中访问所有更新站点以查找所需的软件”。</span><span class="sxs-lookup"><span data-stu-id="424ca-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="424ca-114">屏幕应与下图中所示类似：</span><span class="sxs-lookup"><span data-stu-id="424ca-114">Your screen should appear similar to the following:</span></span>
   
   ![安装用于 Eclipse 的 Azure 工具包][02]

1. <span data-ttu-id="424ca-116">如果展开“用于 Eclipse 的 Azure 工具包”，可以看到类似于以下示例的项列表：</span><span class="sxs-lookup"><span data-stu-id="424ca-116">If you expand the **Azure Toolkit for Eclipse**, you will see a list of items like following example:</span></span>
   
   * <span data-ttu-id="424ca-117">**用于 Java 的 Application Insights 插件**：使用此组件可将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。</span><span class="sxs-lookup"><span data-stu-id="424ca-117">**Application Insights Plugin for Java**: This component allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="424ca-118">**Azure 访问控制服务筛选器**：此组件对以下情况提供支持：向 Azure ACS 验证应用程序用户的身份，启用单一登录方案，以及从应用程序具体化标识逻辑。</span><span class="sxs-lookup"><span data-stu-id="424ca-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from the application.</span></span>
   * <span data-ttu-id="424ca-119">**Azure 常用插件**：此组件提供其他工具包组件所需的常见功能。</span><span class="sxs-lookup"><span data-stu-id="424ca-119">**Azure Common Plugin**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="424ca-120">**用于 Eclipse 的 Azure 资源管理器**：此组件提供其他工具包组件所需的常见功能。</span><span class="sxs-lookup"><span data-stu-id="424ca-120">**Azure Explorer for Eclipse**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="424ca-121">**用于 Eclipse 且支持 Java 的 Azure 插件**：此组件对以下情况提供支持：在 Eclipse 中，通过命令行开发可帮助构建、测试和部署适用于 Microsoft Azure 云的 Java 应用程序的项目。</span><span class="sxs-lookup"><span data-stu-id="424ca-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for the Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="424ca-122">**支持 Java 的 Azure Web 应用插件**：此组件在将 Java Web 应用程序部署到 Microsoft Azure Web 应用容器时提供支持。</span><span class="sxs-lookup"><span data-stu-id="424ca-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications to Microsoft Azure Web App containers.</span></span>
   * <span data-ttu-id="424ca-123">**Microsoft JDBC Driver 4.2 for SQL Server**：此组件提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="424ca-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="424ca-124">**Apache Qpid JMS 客户端库包**：此组件提供 Apache Qpid 项目中的 JMS 客户端组件，以允许应用程序在 Microsoft Azure 中使用 AMQP 消息传送。</span><span class="sxs-lookup"><span data-stu-id="424ca-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides the JMS client component from the Apache Qpid project to enable your application to use AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="424ca-125">**Microsoft Azure Java 库包**：此组件提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等等）的 API。</span><span class="sxs-lookup"><span data-stu-id="424ca-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>

1. <span data-ttu-id="424ca-126">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="424ca-126">Click **Next**.</span></span> <span data-ttu-id="424ca-127">（如果在安装该工具包时遇到不正常的延迟，请确保未选中“在安装过程中访问所有更新站点以查找所需的软件”。）</span><span class="sxs-lookup"><span data-stu-id="424ca-127">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>

1. <span data-ttu-id="424ca-128">在“安装详细信息”对话框中，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="424ca-128">In the **Install Details** dialog, click **Next**.</span></span>
   
   ![查看安装详细信息][03]

1. <span data-ttu-id="424ca-130">在“查看许可证”对话框中，查看许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="424ca-130">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="424ca-131">如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="424ca-131">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="424ca-132">（剩余步骤假定你接受许可协议条款。</span><span class="sxs-lookup"><span data-stu-id="424ca-132">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="424ca-133">如果不接受许可协议条款，请退出安装过程。）</span><span class="sxs-lookup"><span data-stu-id="424ca-133">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
   ![查看许可证][04]
   
   <span data-ttu-id="424ca-135">Eclipse 将下载并安装必要的包。</span><span class="sxs-lookup"><span data-stu-id="424ca-135">Eclipse will download and install the requisite packages.</span></span>
   
   ![安装进度][05]

1. <span data-ttu-id="424ca-137">如果系统提示重新启动 Eclipse 以完成安装，请单击“是”。</span><span class="sxs-lookup"><span data-stu-id="424ca-137">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
   ![重新启动提示][06]

## <a name="next-steps"></a><span data-ttu-id="424ca-139">后续步骤</span><span class="sxs-lookup"><span data-stu-id="424ca-139">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->

[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
