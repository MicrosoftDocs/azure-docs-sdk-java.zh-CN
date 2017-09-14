---
title: "用于 IntelliJ 的 Azure 工具包的登录说明"
description: "了解如何使用用于 IntelliJ 的 Azure 工具包登录到 Microsoft Azure。"
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
ms.openlocfilehash: 85fa5eb979e4d28d49db215ee1653b4c064a87c5
ms.sourcegitcommit: 256044d7cbce16dcb8dc4e195d0f63c10cb44d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="13ebe-103">用于 IntelliJ 的 Azure 工具包的登录说明</span><span class="sxs-lookup"><span data-stu-id="13ebe-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="13ebe-104">用于 IntelliJ 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：</span><span class="sxs-lookup"><span data-stu-id="13ebe-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="13ebe-105">**自动**：创建一个可用于自动登录到 Azure 帐户的凭据文件。</span><span class="sxs-lookup"><span data-stu-id="13ebe-105">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>
  * <span data-ttu-id="13ebe-106">**交互式**：每次登录到 Azure 帐户时都要输入 Azure 凭据。</span><span class="sxs-lookup"><span data-stu-id="13ebe-106">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>

<span data-ttu-id="13ebe-107">以下部分介绍如何使用每种方法。</span><span class="sxs-lookup"><span data-stu-id="13ebe-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="13ebe-108">自动登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="13ebe-108">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="13ebe-109">本部分逐步引导创建一个包含服务主体数据的凭据文件。</span><span class="sxs-lookup"><span data-stu-id="13ebe-109">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="13ebe-110">完成此过程后，每次打开项目时，Eclipse 都会使用凭据文件你将自动登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="13ebe-110">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="13ebe-111">使用 IntelliJ IDEA 打开项目。</span><span class="sxs-lookup"><span data-stu-id="13ebe-111">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="13ebe-112">在“工具”菜单中，指向“Azure”，并单击“Azure 登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-112">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![“IntelliJ Azure 登录”命令][A01]

1. <span data-ttu-id="13ebe-114">在“Azure 登录”窗口中选择“自动”，并单击“新建”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-114">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![已选择“自动”的“Azure 登录”窗口][A02]

1. <span data-ttu-id="13ebe-116">在“Azure 登录”对话框窗口中输入 Azure 凭据，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-116">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![“Azure 登录”对话框窗口][A03]

1. <span data-ttu-id="13ebe-118">在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-118">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![“创建身份验证文件”窗口][A04]

1. <span data-ttu-id="13ebe-120">成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-120">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![“服务主体创建状态”对话框][A05]

1. <span data-ttu-id="13ebe-122">在“Azure 登录”窗口中单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-122">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![“Azure 登录”对话框][A06]

1. <span data-ttu-id="13ebe-124">在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-124">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="13ebe-126">自动登录后从 Azure 帐户注销</span><span class="sxs-lookup"><span data-stu-id="13ebe-126">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="13ebe-127">使用上述步骤配置帐户后，每次重新启动 IntelliJ IDEA 时，Azure 工具包会你将自动登录到 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="13ebe-127">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="13ebe-128">但是，要注销 Azure 帐户并禁止 Azure 工具包你将自动登录，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="13ebe-128">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="13ebe-129">在 IntelliJ IDEA 的“工具”菜单中指向“Azure”，并单击“Azure 注销”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-129">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![“IntelliJ Azure 注销”命令][L01]

1. <span data-ttu-id="13ebe-131">在“Azure 注销”确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-131">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![“Azure 注销”确认窗口][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="13ebe-133">使用现有的凭据文件自动登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="13ebe-133">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="13ebe-134">如果使用 IntelliJ IDEA 时从 Azure 帐户注销，必须使用现有的凭据文件才能自动重新登录到该帐户。</span><span class="sxs-lookup"><span data-stu-id="13ebe-134">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="13ebe-135">要将用于 Eclipse 的 Azure 工具包配置为使用现有的凭据文件，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="13ebe-135">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="13ebe-136">使用 IntelliJ IDEA 打开项目。</span><span class="sxs-lookup"><span data-stu-id="13ebe-136">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="13ebe-137">在“工具”菜单中，指向“Azure”，并单击“Azure 登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-137">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![“IntelliJ Azure 登录”命令][A01]

1. <span data-ttu-id="13ebe-139">在“Azure 登录”窗口中选择“自动”，并单击“浏览”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-139">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![已选择“自动”的“Azure 登录”窗口][A02]

1. <span data-ttu-id="13ebe-141">在“选择身份验证文件”对话框中，选择前面创建的凭据文件，并单击“选择”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-141">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![“选择身份验证文件”对话框][A08]

1. <span data-ttu-id="13ebe-143">在“Azure 登录”窗口中单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-143">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![已选择“自动”的“Azure 登录”窗口][A06]

1. <span data-ttu-id="13ebe-145">在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-145">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][A07]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="13ebe-147">以交互方式登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="13ebe-147">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="13ebe-148">若要通过手动输入 Azure 凭据登录到 Azure，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="13ebe-148">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="13ebe-149">使用 IntelliJ IDEA 打开项目。</span><span class="sxs-lookup"><span data-stu-id="13ebe-149">Open your project with IntelliJ IDEA.</span></span>

1. <span data-ttu-id="13ebe-150">单击“工具”，指向“Azure”，并单击“Azure 登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-150">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![“IntelliJ Azure 登录”命令][I01]

1. <span data-ttu-id="13ebe-152">在“Azure 登录”窗口中选择“交互式”，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-152">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![已选择“交互式”的“Azure 登录”窗口][I02]

1. <span data-ttu-id="13ebe-154">在显示的“Azure 登录”对话框中输入 Azure 凭据，并单击“登录”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-154">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![“Azure 登录”对话框窗口][I03]

1. <span data-ttu-id="13ebe-156">在“选择订阅”对话框中选择要使用的订阅，并单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-156">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![“选择订阅”对话框][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="13ebe-158">以交互方式登录后从 Azure 帐户注销</span><span class="sxs-lookup"><span data-stu-id="13ebe-158">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="13ebe-159">使用上述步骤配置帐户后，每次重新启动 IntelliJ IDEA 时，都会自动从 Azure 帐户中注销。</span><span class="sxs-lookup"><span data-stu-id="13ebe-159">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="13ebe-160">但是，如果要在*不*重新启动 IntelliJ IDEA 的情况下注销 Azure 帐户，请执行以下操作。</span><span class="sxs-lookup"><span data-stu-id="13ebe-160">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="13ebe-161">在 IntelliJ IDEA 的“工具”菜单中指向“Azure”，并单击“Azure 注销”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-161">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![“IntelliJ Azure 注销”命令][L01]

1. <span data-ttu-id="13ebe-163">在“Azure 注销”确认窗口中，单击“是”。</span><span class="sxs-lookup"><span data-stu-id="13ebe-163">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![“Azure 注销”确认窗口][L02]

## <a name="next-steps"></a><span data-ttu-id="13ebe-165">后续步骤</span><span class="sxs-lookup"><span data-stu-id="13ebe-165">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
