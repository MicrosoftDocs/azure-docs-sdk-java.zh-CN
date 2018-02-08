---
title: "安装 Azure Toolkit for IntelliJ"
description: "了解如何安装用于 IntelliJ 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 0f3df5c8cf3c83c1ce9a4ca32c753b5fc39ab1df
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="088b0-103">安装 Azure Toolkit for IntelliJ</span><span class="sxs-lookup"><span data-stu-id="088b0-103">Installing the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="088b0-104">借助 Azure Toolkit for IntelliJ 提供的模板和功能，可以轻松使用 IntelliJ IDEA 开发环境来创建、开发、测试云应用程序并将其部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="088b0-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy cloud application to Azure using the IntelliJ IDEA development environment.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="088b0-105">Azure Toolkit for IntelliJ 是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为：</span><span class="sxs-lookup"><span data-stu-id="088b0-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span> 
> 
> <span data-ttu-id="088b0-106"><https://github.com/microsoft/azure-tools-for-java></span><span class="sxs-lookup"><span data-stu-id="088b0-106"><https://github.com/microsoft/azure-tools-for-java></span></span> 
> 

<span data-ttu-id="088b0-107">可以通过两种方法来安装用于 IntelliJ 的 Azure 工具包：一种是使用“设置”对话框，另一种是使用开始屏幕上的“配置”菜单。</span><span class="sxs-lookup"><span data-stu-id="088b0-107">There are two methods of installing the Azure Toolkit for IntelliJ: by using the **Settings** dialog box, and by using the **Configure** menu on the start screen.</span></span> <span data-ttu-id="088b0-108">两种安装方法都会在以下步骤中演示。</span><span class="sxs-lookup"><span data-stu-id="088b0-108">Both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="088b0-109">从“设置”对话框安装 Azure Toolkit for IntelliJ 的步骤</span><span class="sxs-lookup"><span data-stu-id="088b0-109">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>

1. <span data-ttu-id="088b0-110">启动 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="088b0-110">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="088b0-111">IntelliJ IDEA 打开后，单击“文件”，并单击“设置”。</span><span class="sxs-lookup"><span data-stu-id="088b0-111">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
   ![打开 IntelliJ IDEA 的“设置”对话框][01a]

1. <span data-ttu-id="088b0-113">在“设置”对话框中，单击“插件”，并单击“浏览存储库”。</span><span class="sxs-lookup"><span data-stu-id="088b0-113">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
   ![IntelliJ IDEA 的“设置”对话框][02a]

1. <span data-ttu-id="088b0-115">在“浏览存储库”对话框的搜索框中键入“Azure”。</span><span class="sxs-lookup"><span data-stu-id="088b0-115">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="088b0-116">突出显示“用于 IntelliJ 的 Azure 工具包”，并单击“安装”。</span><span class="sxs-lookup"><span data-stu-id="088b0-116">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜索 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="088b0-118">IntelliJ IDEA 会在一个对话框中显示安装进度。</span><span class="sxs-lookup"><span data-stu-id="088b0-118">IntelliJ IDEA displays the installation progress in a dialog box.</span></span>
   
   ![安装进度][04]

1. <span data-ttu-id="088b0-120">安装完成后，单击“重新启动 IntelliJ IDEA”。</span><span class="sxs-lookup"><span data-stu-id="088b0-120">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新启动 IntelliJ IDEA][05]

1. <span data-ttu-id="088b0-122">单击“确定”关闭“设置”对话框。</span><span class="sxs-lookup"><span data-stu-id="088b0-122">Click **OK** to close the Settings dialog box.</span></span>
   
   ![关闭 IntelliJ IDEA 的“设置”对话框][06]

1. <span data-ttu-id="088b0-124">当系统提示是重新启动 IntelliJ IDEA 还是推迟时，请单击“重新启动”。</span><span class="sxs-lookup"><span data-stu-id="088b0-124">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
<span data-ttu-id="088b0-125">1</span><span class="sxs-lookup"><span data-stu-id="088b0-125">1</span></span>   ![重新启动 IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="088b0-127">从开始屏幕安装 Azure Toolkit for IntelliJ 的步骤</span><span class="sxs-lookup"><span data-stu-id="088b0-127">To install the Azure Toolkit for IntelliJ from the start screen</span></span>

1. <span data-ttu-id="088b0-128">启动 IntelliJ IDEA。</span><span class="sxs-lookup"><span data-stu-id="088b0-128">Start IntelliJ IDEA.</span></span>

1. <span data-ttu-id="088b0-129">IntelliJ IDEA 开始屏幕出现后，单击“配置”，并单击“插件”。</span><span class="sxs-lookup"><span data-stu-id="088b0-129">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
   ![安装 IntelliJ IDEA 插件][01b]

1. <span data-ttu-id="088b0-131">在“插件”对话框中，单击“浏览存储库”。</span><span class="sxs-lookup"><span data-stu-id="088b0-131">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
   ![浏览 IntelliJ IDEA 插件存储库][02b]

1. <span data-ttu-id="088b0-133">在“浏览存储库”对话框的搜索框中键入“Azure”。</span><span class="sxs-lookup"><span data-stu-id="088b0-133">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="088b0-134">突出显示“用于 IntelliJ 的 Azure 工具包”，并单击“安装”。</span><span class="sxs-lookup"><span data-stu-id="088b0-134">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
   ![搜索 Azure Toolkit for IntelliJ][03]
   
   <span data-ttu-id="088b0-136">IntelliJ IDEA 会在一个对话框中显示安装进度。</span><span class="sxs-lookup"><span data-stu-id="088b0-136">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
   ![安装进度][04]

1. <span data-ttu-id="088b0-138">安装完成后，单击“重新启动 IntelliJ IDEA”。</span><span class="sxs-lookup"><span data-stu-id="088b0-138">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
   ![重新启动 IntelliJ IDEA][05]

1. <span data-ttu-id="088b0-140">当系统提示是重新启动 IntelliJ IDEA 还是推迟时，请单击“重新启动”。</span><span class="sxs-lookup"><span data-stu-id="088b0-140">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
   ![重新启动 IntelliJ IDEA][07]

## <a name="next-steps"></a><span data-ttu-id="088b0-142">后续步骤</span><span class="sxs-lookup"><span data-stu-id="088b0-142">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
