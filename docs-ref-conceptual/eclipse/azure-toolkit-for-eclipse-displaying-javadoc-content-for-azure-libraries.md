---
title: "在 Eclipse 中显示 Azure Libraries Package for Java 的 Javadoc 内容"
description: "如何在 Eclipse 中显示 Azure Libraries 的 Javadoc 内容。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 6f1edcc1411e8ec716dbfe41271d21d2397515c5
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="e7fb6-103">在 Eclipse 中显示 Azure Libraries Package for Java 的 Javadoc 内容</span><span class="sxs-lookup"><span data-stu-id="e7fb6-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>

<span data-ttu-id="e7fb6-104">通过将 Javadoc 内容关联到 Azure Libraries for Java，可以在 Eclipse 环境中查看 Azure Libraries for Java 的 Javadoc 内容。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="e7fb6-105">以下步骤说明如何在 Eclipse 中使用此功能。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="e7fb6-106">此过程假设已在生成路径中添加了 Azure Libraries for Java。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="e7fb6-107">在 Eclipse 中显示 Azure Libraries for Java 的 Javadoc 内容</span><span class="sxs-lookup"><span data-stu-id="e7fb6-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>

1. <span data-ttu-id="e7fb6-108">在 Eclipse 的项目资源管理器内，在项目的“引用库”部分中，打开 Azure Library for Java JAR 的上下文菜单。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="e7fb6-109">例如 **microsoft-windowsazure-api-0.1.0.jar**（根据安装的版本，版本号可能不同）。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

1. <span data-ttu-id="e7fb6-110">单击“属性”。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-110">Click **Properties**.</span></span>

1. <span data-ttu-id="e7fb6-111">在“属性”对话框中的左窗格内，单击“Javadoc 位置”。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="e7fb6-112">此时会显示“Javadoc 位置”对话框。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-112">The **Javadoc Location** dialog is displayed.</span></span>

1. <span data-ttu-id="e7fb6-113">可以指定“Javadoc URL”或“存档中的 Javadoc”。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="e7fb6-114">如果选择指定 **Javadoc URL**，请使用类似于 **http://dl.windowsazure.com/javadoc** 或 **http://dl.windowsazure.com/storage/javadoc** 的 URL。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="e7fb6-115">如果选择使用“存档中的 Javadoc”，可以指定外部文件或工作区文件。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="e7fb6-116">做出选择并根据需要浏览/验证。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="e7fb6-117">以下示例会将 Azure Libraries for Java 关联到已本地下载到名为 **c:\MyAzureJARs** 的文件夹的相应 Javadoc JAR。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![显示 Javadoc 内容。][ic553487]

1. <span data-ttu-id="e7fb6-119">*可选步骤*：单击“验证”。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-119">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="e7fb6-120">此处可能会显示 Javadoc JAR 的潜在问题。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-120">Potential issues with the Javadoc JAR could be displayed here.</span></span>

1. <span data-ttu-id="e7fb6-121">单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-121">Click **OK**.</span></span>

<span data-ttu-id="e7fb6-122">与库关联后，Javadoc 内容应会显示在 Eclipse IDE 中。</span><span class="sxs-lookup"><span data-stu-id="e7fb6-122">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="e7fb6-123">例如，如果 `blob` 在代码中定义为 `CloudBlockBlob` 类型，则在代码中键入 `blob.acquireLease` 时，会显示以下示例 Javadoc 内容：</span><span class="sxs-lookup"><span data-stu-id="e7fb6-123">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![显示 Javadoc 内容。][ic553488]

## <a name="next-steps"></a><span data-ttu-id="e7fb6-125">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e7fb6-125">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
