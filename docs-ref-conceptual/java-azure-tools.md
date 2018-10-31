---
title: 面向 Azure Java 开发人员的工具 | Microsoft Docs
description: 面向使用 Azure 的 Java 开发人员的 IDE 集成、仿真器、资源浏览器和命令行接口。
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: dac0a1c576974a141950919292129890f4e15be4
ms.sourcegitcommit: 19876d17fed0afd9af0cb8e161f5a463696e74cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "49634450"
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="02cb9-103">面向 Java 开发人员的 Azure 工具</span><span class="sxs-lookup"><span data-stu-id="02cb9-103">Azure tools for Java developers</span></span>

## <a name="supported-jdk-runtimes"></a><span data-ttu-id="02cb9-104">支持的 JDK 运行时</span><span class="sxs-lookup"><span data-stu-id="02cb9-104">Supported JDK runtimes</span></span>

<span data-ttu-id="02cb9-105">Azure 和 Azure Stack 的 Java 开发人员可以使用 [OpenJDK 的 Azul Systems Zulu Enterprise 内部版本](https://www.azul.com/downloads/azure-only/zulu/)生成并运行生产性 Java 7、8、11 应用程序，不需付出更多的支持成本。</span><span class="sxs-lookup"><span data-stu-id="02cb9-105">Java developers on Azure and Azure Stack can build and run production Java 7,8, and 11 applications using [Azul Systems Zulu Enterprise builds of OpenJDK](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="02cb9-106">如果目前正通过其他 JDK 运行 Java 应用，请考虑迁移到 Azure 上的 Zulu，以便获取免费支持和维护。</span><span class="sxs-lookup"><span data-stu-id="02cb9-106">If you’re currently running Java apps with other JDKs, consider migrating to Zulu on Azure for free support and maintenance.</span></span> 

<span data-ttu-id="02cb9-107">[详细了解](java-supported-jdk-runtime.md) Azure 支持的 Java 运行时。</span><span class="sxs-lookup"><span data-stu-id="02cb9-107">[Learn more](java-supported-jdk-runtime.md) about Azure supported Java runtimes.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="02cb9-108">Eclipse 和 IntelliJ 插件</span><span class="sxs-lookup"><span data-stu-id="02cb9-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="02cb9-109">使用用于 [Eclipse](eclipse/azure-toolkit-for-eclipse.md) 和 [IntelliJ](intellij/azure-toolkit-for-intellij.md) 的 Azure 工具包通过 IDE 管理 Azure 资源及部署应用。</span><span class="sxs-lookup"><span data-stu-id="02cb9-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](eclipse/azure-toolkit-for-eclipse.md) and [IntelliJ](intellij/azure-toolkit-for-intellij.md).</span></span>   

![显示 Azure 资源管理器的 IntelliJ 工具包](media/intelliJ-azure-explorer.png)

<span data-ttu-id="02cb9-111">[用于 Eclipse 的 Azure 工具包入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [用于 IntelliJ 的 Azure 工具包入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span><span class="sxs-lookup"><span data-stu-id="02cb9-111">[Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app)</span></span> 

## <a name="visual-studio-code"></a><span data-ttu-id="02cb9-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="02cb9-112">Visual Studio Code</span></span>

<span data-ttu-id="02cb9-113">[VS Code](https://code.visualstudio.com/) 是适用于 MacOS、Windows 和 Linux 的精简而强大的代码编辑器。</span><span class="sxs-lookup"><span data-stu-id="02cb9-113">[VS Code](https://code.visualstudio.com/) is a lightweight but powerful code editor available for MacOS, Windows, and Linux.</span></span> <span data-ttu-id="02cb9-114">VS Code 通过一系列可提供项目支持、代码完成、调试、检查和导航的扩展，支持简单新式的 Java 开发工作流。</span><span class="sxs-lookup"><span data-stu-id="02cb9-114">VS Code supports a simple, modern Java development workflow through a set of extensions that provide project support, code completion, debugging, linting, and navigation.</span></span>

<span data-ttu-id="02cb9-115">[VS Code 和 Java 入门](https://code.visualstudio.com/docs/java)
[VS Code 的 Java 扩展包](https://code.visualstudio.com/docs/java/extensions)</span><span class="sxs-lookup"><span data-stu-id="02cb9-115">[Get Started with VS Code and Java](https://code.visualstudio.com/docs/java)
[Java extension pack for VS Code](https://code.visualstudio.com/docs/java/extensions)</span></span>  

## <a name="azure-cli-20"></a><span data-ttu-id="02cb9-116">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="02cb9-116">Azure CLI 2.0</span></span>

<span data-ttu-id="02cb9-117">Azure 2.0 CLI 提供用于管理 Azure 资源的命令行体验。</span><span class="sxs-lookup"><span data-stu-id="02cb9-117">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="02cb9-118">可以通过 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在浏览器中使用它，也可以将其[安装](https://docs.microsoft.com/cli/azure/install-azure-cli)在 macOS、Linux 和 Windows 上，然后从命令行运行它。</span><span class="sxs-lookup"><span data-stu-id="02cb9-118">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="02cb9-119">[Azure CLI 2.0 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="02cb9-119">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="02cb9-120">Azure 存储资源管理器</span><span class="sxs-lookup"><span data-stu-id="02cb9-120">Azure Storage Explorer</span></span> 

<span data-ttu-id="02cb9-121">从桌面管理 Azure 存储帐户、容器和 Blob/文件。</span><span class="sxs-lookup"><span data-stu-id="02cb9-121">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="02cb9-122">Azure 存储资源管理器目前以预览版提供，适用于 Windows、macOS 和 Linux。</span><span class="sxs-lookup"><span data-stu-id="02cb9-122">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="02cb9-123">Azure 存储资源管理器入门</span><span class="sxs-lookup"><span data-stu-id="02cb9-123">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
