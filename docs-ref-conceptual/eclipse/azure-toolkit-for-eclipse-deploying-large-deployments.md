---
title: "实施大型部署"
description: "了解如何使用 Azure Toolkit for Eclipse 实施大型部署。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 0e367e74d038043f1626dbf19aab87db6451bc02
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="3c816-103">实施大型部署</span><span class="sxs-lookup"><span data-stu-id="3c816-103">Deploying Large Deployments</span></span>

<span data-ttu-id="3c816-104">如果部署太大，从而无法包含在默认 approot 文件夹中，可以使用本地存储资源作为 JDK 和应用程序服务器的部署根文件夹。</span><span class="sxs-lookup"><span data-stu-id="3c816-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="3c816-105">使用本地存储资源作为大型部署的部署根文件夹</span><span class="sxs-lookup"><span data-stu-id="3c816-105">To use a local storage resource as the deployment root folder for large deployments</span></span>

1. <span data-ttu-id="3c816-106">创建新的本地存储资源。</span><span class="sxs-lookup"><span data-stu-id="3c816-106">Create a new local storage resource.</span></span> <span data-ttu-id="3c816-107">资源的名称并不重要。</span><span class="sxs-lookup"><span data-stu-id="3c816-107">The name of the resource does not matter.</span></span> <span data-ttu-id="3c816-108">存储资源在角色级别定义。</span><span class="sxs-lookup"><span data-stu-id="3c816-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="3c816-109">访问本地存储配置对话框（可从中创建新的本地存储资源）的最快方式是使用以下步骤：在“项目资源管理器”视图中右键单击角色（如果看不到该角色，请展开 Azure 项目节点），单击“Azure”，并单击“本地存储”。</span><span class="sxs-lookup"><span data-stu-id="3c816-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="3c816-110">在“本地存储”对话框中，单击“添加”创建新的本地存储资源。</span><span class="sxs-lookup"><span data-stu-id="3c816-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

1. <span data-ttu-id="3c816-111">将所需大小设置为至少 2048 MB（指定更小的值可能会导致你在 approot 中遇到的相同文件大小问题）。</span><span class="sxs-lookup"><span data-stu-id="3c816-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

1. <span data-ttu-id="3c816-112">确保已选中“回收角色实例时清除内容”；这有助于在回收角色实例时，防止部署启动逻辑与资源中的现有文件发生冲突。</span><span class="sxs-lookup"><span data-stu-id="3c816-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

1. <span data-ttu-id="3c816-113">确保将“部署后存储资源目录路径的环境变量”值设置为字符串 **DEPLOYROOT**。</span><span class="sxs-lookup"><span data-stu-id="3c816-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="3c816-114">本地存储资源对话框如下所示。</span><span class="sxs-lookup"><span data-stu-id="3c816-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="3c816-115">或者，如果使用 **DEPLOYROOT** 作为本地资源的*名称*，并且不更改自动生成的环境变量名称（在此情况下将设置为 **DEPLOYROOT_PATH**），则这也适用于应用程序。</span><span class="sxs-lookup"><span data-stu-id="3c816-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="3c816-116">有关创建本地存储资源的其他信息可以在[本地存储属性][Local storage properties]中找到。</span><span class="sxs-lookup"><span data-stu-id="3c816-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c816-117">后续步骤</span><span class="sxs-lookup"><span data-stu-id="3c816-117">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
