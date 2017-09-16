---
title: "用于 Eclipse 的 Azure 工具包的登录说明"
description: "了解如何使用用于 Eclipse 的 Azure 工具包登录到 Microsoft Azure。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/11/2017
ms.author: robmcm
ms.openlocfilehash: 6c10d3e11dd75679fca0736e02d15de1d782dcec
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="a53d0-103">用于 Eclipse 的 Azure 工具包的 Azure 登录说明</span><span class="sxs-lookup"><span data-stu-id="a53d0-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="a53d0-104">用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：</span><span class="sxs-lookup"><span data-stu-id="a53d0-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="a53d0-105">**自动** - 如果使用此方法，将创建一个包含服务主体数据的凭据文件，之后可以使用该凭据文件自动登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="a53d0-105">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>
  * <span data-ttu-id="a53d0-106">**交互式** - 如果使用此方法，每次登录 Azure 帐户时，都需要输入 Azure 凭据。</span><span class="sxs-lookup"><span data-stu-id="a53d0-106">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>

<span data-ttu-id="a53d0-107">以下部分中的步骤介绍如何使用每个方法。</span><span class="sxs-lookup"><span data-stu-id="a53d0-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="a53d0-108">自动登录到 Azure 帐户并创建凭据文件，以便在将来使用</span><span class="sxs-lookup"><span data-stu-id="a53d0-108">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="a53d0-109">以下步骤将引导完成创建一个包含服务主体数据的凭据文件。</span><span class="sxs-lookup"><span data-stu-id="a53d0-109">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="a53d0-110">完成这些步骤后，每次打开项目时，Eclipse 都会自动使用凭据文件自动登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="a53d0-110">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="a53d0-111">使用 Eclipse 打开项目。</span><span class="sxs-lookup"><span data-stu-id="a53d0-111">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a53d0-112">依次单击“工具”、“Azure”、“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-112">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![用于 Azure 登录的 Eclipse 菜单][A01]

1. <span data-ttu-id="a53d0-114">显示“Azure 登录”对话框时，选择“自动”，并单击“新建”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-114">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![“登录”对话框][A02]

1. <span data-ttu-id="a53d0-116">显示“Azure 登录”对话框时，输入 Azure 凭据，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-116">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![“Azure 登录”对话框][A03]

1. <span data-ttu-id="a53d0-118">显示“创建身份验证文件”对话框时，选择要使用的订阅，选择目标目录，并单击“启动”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-118">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![“Azure 登录”对话框][A04]

1. <span data-ttu-id="a53d0-120">此时会显示“服务主体创建状态”对话框，成功创建文件后，单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-120">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![“服务主体创建状态”对话框][A05]

1. <span data-ttu-id="a53d0-122">显示“Azure 登录”对话框时，单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-122">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![“Azure 登录”对话框][A06]

1. <span data-ttu-id="a53d0-124">显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-124">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="a53d0-126">已自动登录时，从 Azure 帐户中注销</span><span class="sxs-lookup"><span data-stu-id="a53d0-126">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="a53d0-127">按照上一部分中的步骤配置后，每次重新启动 Eclipse 时，Azure 工具包都会你将自动登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="a53d0-127">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="a53d0-128">但是，要注销 Azure 帐户并禁止 Azure 工具包你将自动登录，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="a53d0-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="a53d0-129">在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-129">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![用于 Azure 注销的 Eclipse 菜单][L01]

1. <span data-ttu-id="a53d0-131">显示“Azure 注销”对话框时，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-131">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![“注销”对话框][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="a53d0-133">使用已创建的凭据文件自动登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="a53d0-133">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="a53d0-134">如果在使用 Eclipse 时从 Azure 中注销，则需要重新配置用于 Eclipse 的 Azure 工具包以使用已创建的凭据文件，才能自动登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="a53d0-134">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="a53d0-135">以下步骤将引导完成配置 Azure 工具包以使用现有凭据文件。</span><span class="sxs-lookup"><span data-stu-id="a53d0-135">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="a53d0-136">使用 Eclipse 打开项目。</span><span class="sxs-lookup"><span data-stu-id="a53d0-136">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a53d0-137">依次单击“工具”、“Azure”、“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-137">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![用于 Azure 登录的 Eclipse 菜单][A01]

1. <span data-ttu-id="a53d0-139">显示“Azure 登录”对话框时，选择“自动”，并单击“浏览”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-139">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![“登录”对话框][A02]

1. <span data-ttu-id="a53d0-141">显示“选择已验证的文件”对话框时，选择先前创建的凭据文件，然后单击“选择”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-141">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![“登录”对话框][A08]

1. <span data-ttu-id="a53d0-143">显示“Azure 登录”对话框时，单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-143">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![“Azure 登录”对话框][A06]

1. <span data-ttu-id="a53d0-145">显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-145">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][A07]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="a53d0-147">以交互方式登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="a53d0-147">Signing into your Azure account interactively</span></span>

<span data-ttu-id="a53d0-148">以下步骤将说明如何通过手动输入 Azure 凭据登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="a53d0-148">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="a53d0-149">使用 Eclipse 打开项目。</span><span class="sxs-lookup"><span data-stu-id="a53d0-149">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a53d0-150">依次单击“工具”、“Azure”、“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-150">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![用于 Azure 登录的 Eclipse 菜单][I01]

1. <span data-ttu-id="a53d0-152">显示“Azure 登录”对话框时，选择“交互式”，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-152">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![“登录”对话框][I02]

1. <span data-ttu-id="a53d0-154">显示“Azure 登录”对话框时，输入 Azure 凭据，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-154">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![“Azure 登录”对话框][I03]

1. <span data-ttu-id="a53d0-156">显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-156">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="a53d0-158">已以交互方式登录时，从 Azure 帐户中注销</span><span class="sxs-lookup"><span data-stu-id="a53d0-158">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="a53d0-159">按照上一部分中的步骤配置后，每次重新启动 Eclipse 时，都会自动从 Azure 帐户中注销。</span><span class="sxs-lookup"><span data-stu-id="a53d0-159">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="a53d0-160">但是，如果要在不重新启动 Eclipse 的情况下注销 Azure 帐户，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="a53d0-160">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="a53d0-161">在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-161">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![用于 Azure 注销的 Eclipse 菜单][L01]

1. <span data-ttu-id="a53d0-163">显示“Azure 注销”对话框时，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="a53d0-163">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![“注销”对话框][L02]

## <a name="next-steps"></a><span data-ttu-id="a53d0-165">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a53d0-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
