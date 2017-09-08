---
title: "面向 Azure Java 开发人员的工具 | Microsoft Docs"
description: "面向使用 Azure 的 Java 开发人员的 IDE 集成、仿真器、资源浏览器和命令行接口。"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 01fe31f2c59810f972875331d49ce5130755c8f2
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-tools-for-java-developers"></a><span data-ttu-id="7c39f-103">面向 Java 开发人员的 Azure 工具</span><span class="sxs-lookup"><span data-stu-id="7c39f-103">Azure tools for Java developers</span></span>

## <a name="client-and-management-libraries"></a><span data-ttu-id="7c39f-104">客户端和管理库</span><span class="sxs-lookup"><span data-stu-id="7c39f-104">Client and management libraries</span></span>

<span data-ttu-id="7c39f-105">使用用于 Java 的 Azure 库，通过应用程序连接到服务和管理 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="7c39f-105">Connect to services and manage Azure resources from your applications with the Azure libraries for Java.</span></span> <span data-ttu-id="7c39f-106">通过将此依赖项添加到项目 *pom.xml*，将管理库导入 Maven 项目。</span><span class="sxs-lookup"><span data-stu-id="7c39f-106">Import the management libraries into your Maven projects by adding this dependency to your project *pom.xml*.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.1.2</version>
</dependency>
```

<span data-ttu-id="7c39f-107">查看[库的完整列表](java-sdk-azure-install.md)和用于 Java 的 Azure 库的[入门](java-sdk-azure-get-started.md)文档。</span><span class="sxs-lookup"><span data-stu-id="7c39f-107">View the [complete list of libraries](java-sdk-azure-install.md) and [get started](java-sdk-azure-get-started.md) with the Azure libraries for Java.</span></span>

## <a name="eclipse-and-intellij-plugins"></a><span data-ttu-id="7c39f-108">Eclipse 和 IntelliJ 插件</span><span class="sxs-lookup"><span data-stu-id="7c39f-108">Eclipse and IntelliJ plugins</span></span>

<span data-ttu-id="7c39f-109">使用用于 [Eclipse](https://docs.microsoft.com/azure/azure-toolkit-for-eclipse) 和 [IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij) 的 Azure 工具包通过 IDE 管理 Azure 资源及部署应用。</span><span class="sxs-lookup"><span data-stu-id="7c39f-109">Manage Azure resources and deploy apps from your IDE with The Azure toolkits for [Eclipse](https://docs.microsoft.com/azure/azure-toolkit-for-eclipse) and [IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij).</span></span>   

![显示 Azure 资源管理器的 IntelliJ 工具包](media/intelliJ-azure-explorer.png)

[<span data-ttu-id="7c39f-111">用于 Eclipse 的 Azure 工具包入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [用于 IntelliJ 的 Azure 工具包入门</span><span class="sxs-lookup"><span data-stu-id="7c39f-111">Get started with Azure Toolkit for Eclipse](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [Get started with Azure Toolkit for IntelliJ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="azure-cli-20"></a><span data-ttu-id="7c39f-112">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7c39f-112">Azure CLI 2.0</span></span>

<span data-ttu-id="7c39f-113">Azure 2.0 CLI 提供用于管理 Azure 资源的命令行体验。</span><span class="sxs-lookup"><span data-stu-id="7c39f-113">The Azure 2.0 CLI provides a command-line experience to manage Azure resources.</span></span> <span data-ttu-id="7c39f-114">可以通过 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在浏览器中使用它，也可以将其[安装](https://docs.microsoft.com/cli/azure/install-azure-cli)在 macOS、Linux 和 Windows 上，然后从命令行运行它。</span><span class="sxs-lookup"><span data-stu-id="7c39f-114">You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), or you can [install](https://docs.microsoft.com/cli/azure/install-azure-cli) it on macOS, Linux, and Windows and run it from the command line.</span></span>

<span data-ttu-id="7c39f-115">[Azure CLI 2.0 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="7c39f-115">[Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>

## <a name="azure-storage-explorer"></a><span data-ttu-id="7c39f-116">Azure 存储资源管理器</span><span class="sxs-lookup"><span data-stu-id="7c39f-116">Azure Storage Explorer</span></span> 

<span data-ttu-id="7c39f-117">从桌面管理 Azure 存储帐户、容器和 Blob/文件。</span><span class="sxs-lookup"><span data-stu-id="7c39f-117">Manage Azure storage accounts, containers, and blobs/files from your desktop.</span></span> <span data-ttu-id="7c39f-118">Azure 存储资源管理器目前以预览版提供，适用于 Windows、macOS 和 Linux。</span><span class="sxs-lookup"><span data-stu-id="7c39f-118">Azure Storage Explorer is currently in Preview and works on Windows, macOS, and Linux.</span></span>

[<span data-ttu-id="7c39f-119">Azure 存储资源管理器入门</span><span class="sxs-lookup"><span data-stu-id="7c39f-119">Get started with Azure Storage Explorer</span></span>](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)