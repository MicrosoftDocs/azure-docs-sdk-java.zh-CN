---
title: 用于 Eclipse 的 Azure 工具包的登录说明
description: 了解如何使用用于 Eclipse 的 Azure 工具包登录到 Microsoft Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: b4b13de38913ae6e7ae2bb09210ac742efc9d0ad
ms.sourcegitcommit: d18d9dce22b7f7af178f756bd341433d24e3c3b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66575319"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="9e75f-103">用于 Eclipse 的 Azure 工具包的登录说明</span><span class="sxs-lookup"><span data-stu-id="9e75f-103">Sign-in instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="9e75f-104">用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：</span><span class="sxs-lookup"><span data-stu-id="9e75f-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  - [<span data-ttu-id="9e75f-105">通过设备登录名登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="9e75f-105">Sign in to your Azure account by Device Login</span></span>](#sign-in-to-your-azure-account-by-device-login)
  - [<span data-ttu-id="9e75f-106">通过服务主体登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="9e75f-106">Sign in to your Azure account by Service Principal</span></span>](#sign-in-to-your-azure-account-by-service-principal)

<span data-ttu-id="9e75f-107">还提供了[**注销**](#sign-out-of-your-azure-account)方法。</span><span class="sxs-lookup"><span data-stu-id="9e75f-107">[**Sign out**](#sign-out-of-your-azure-account) methods are also provided.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a><span data-ttu-id="9e75f-108">通过设备登录名登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="9e75f-108">Sign in to your Azure account by Device Login</span></span>

<span data-ttu-id="9e75f-109">若要通过设备登录名登录 Azure，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="9e75f-109">To sign in Azure by device login, do the following:</span></span>

1. <span data-ttu-id="9e75f-110">使用 Eclipse 打开项目。</span><span class="sxs-lookup"><span data-stu-id="9e75f-110">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="9e75f-111">依次单击“工具”、“Azure”、“登录”。   </span><span class="sxs-lookup"><span data-stu-id="9e75f-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="9e75f-112">![用于 Azure 登录的 Eclipse 菜单][I01]</span><span class="sxs-lookup"><span data-stu-id="9e75f-112">![Eclipse Menu for Azure Sign In][I01]</span></span>

3. <span data-ttu-id="9e75f-113">在“Azure 登录”窗口中选择“设备登录名”，然后单击“登录”。   </span><span class="sxs-lookup"><span data-stu-id="9e75f-113">In the **Azure Sign In** window, select **Device Login**, and then click **Sign in**.</span></span>

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. <span data-ttu-id="9e75f-115">在“Azure 设备登录”对话框中单击“复制并打开”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-115">Click **Copy&Open** in **Azure Device Login** dialog .</span></span>

   ![“Azure 登录”对话框窗口][I03]

> [!NOTE]
>
> <span data-ttu-id="9e75f-117">如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 Internet Explorer、Firefox 或 Chrome：</span><span class="sxs-lookup"><span data-stu-id="9e75f-117">If the browser doesn't open, configure Eclipse to use an external browser like Internet Explorer, Firefox, or Chrome:</span></span>
>
> 1. <span data-ttu-id="9e75f-118">打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”</span><span class="sxs-lookup"><span data-stu-id="9e75f-118">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="9e75f-119">选择首选使用的浏览器</span><span class="sxs-lookup"><span data-stu-id="9e75f-119">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="9e75f-120">在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-120">In the browser, paste your device code (which has been copied when you clicked **Copy&Open** in last step) and then click **Next**.</span></span>

   ![设备登录浏览器][I04]

6. <span data-ttu-id="9e75f-122">最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-122">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![“选择订阅”对话框][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a><span data-ttu-id="9e75f-124">通过服务主体登录到 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="9e75f-124">Sign in to your Azure account by Service Principal</span></span>

<span data-ttu-id="9e75f-125">本部分逐步引导创建一个包含服务主体数据的凭据文件。</span><span class="sxs-lookup"><span data-stu-id="9e75f-125">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="9e75f-126">完成此过程后，在打开项目时，Eclipse 会使用凭据文件将你自动登录到 Azure。</span><span class="sxs-lookup"><span data-stu-id="9e75f-126">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure when open your project.</span></span>

1. <span data-ttu-id="9e75f-127">使用 Eclipse 打开项目。</span><span class="sxs-lookup"><span data-stu-id="9e75f-127">Open your project with Eclipse.</span></span>

2. <span data-ttu-id="9e75f-128">依次单击“工具”、“Azure”、“登录”。   </span><span class="sxs-lookup"><span data-stu-id="9e75f-128">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>
   <span data-ttu-id="9e75f-129">![Eclipse Azure 登录命令][A01]</span><span class="sxs-lookup"><span data-stu-id="9e75f-129">![The Eclipse Azure Sign In command][A01]</span></span>

3. <span data-ttu-id="9e75f-130">在“Azure 登录”窗口中，选择“服务主体”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-130">In the **Azure Sign In** window, select **Service Principal**.</span></span> <span data-ttu-id="9e75f-131">如果还没有服务主体身份验证文件，请单击“新建”创建一个。 </span><span class="sxs-lookup"><span data-stu-id="9e75f-131">If you do not have the service principal authentication file yet, click **New** to create one.</span></span> <span data-ttu-id="9e75f-132">否则，可以单击“浏览”将其打开，然后跳到步骤 8。 </span><span class="sxs-lookup"><span data-stu-id="9e75f-132">Otherwise you can click **Browse** to open it and jump to step 8.</span></span>

   ![已选中“服务主体”的“Azure 登录”窗口][A02]

4. <span data-ttu-id="9e75f-134">在“Azure 设备登录”对话框中单击“复制并打开”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-134">Click **Copy&Open** in **Azure Device Login** dialog.</span></span>

   ![“Azure 登录”对话框窗口][A08]

> [!NOTE]
>
> <span data-ttu-id="9e75f-136">如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 IE 或 Chrome：</span><span class="sxs-lookup"><span data-stu-id="9e75f-136">If the browser doesn't open, configure eclipse to use an external browser like IE or Chrome:</span></span>
>
> 1. <span data-ttu-id="9e75f-137">打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”</span><span class="sxs-lookup"><span data-stu-id="9e75f-137">Open Preferences -> General -> Web Browser -> Use external web browser in Eclipse</span></span>
>
> 2. <span data-ttu-id="9e75f-138">选择首选使用的浏览器</span><span class="sxs-lookup"><span data-stu-id="9e75f-138">Select the browser you prefer to use</span></span>
>

5. <span data-ttu-id="9e75f-139">在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-139">In the browser, paste your device code (which has been copied when you click **Copy&Open** in last step) and then click **Next**.</span></span>

   ![设备登录浏览器][A03]

6. <span data-ttu-id="9e75f-141">在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-141">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![“创建身份验证文件”窗口][A04]

7. <span data-ttu-id="9e75f-143">成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-143">In the **Service Principal Creation Status** dialog box, click **OK** after your files have been created successfully.</span></span>

   ![“服务主体创建状态”对话框][A05]

8. <span data-ttu-id="9e75f-145">所创建文件的地址会自动填充在“Azure 登录”窗口中，此时请单击“登录”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-145">Address of the created file will be automatically filled in the **Azure Sign In** window, now click **Sign in**.</span></span>

   ![“Azure 登录”对话框][A06]

9. <span data-ttu-id="9e75f-147">最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  </span><span class="sxs-lookup"><span data-stu-id="9e75f-147">Finally, in the **Select Subscriptions** dialog box, select the subscriptions that you want to use, then click **OK**.</span></span>

   ![“选择订阅”对话框][A07]

## <a name="sign-out-of-your-azure-account"></a><span data-ttu-id="9e75f-149">注销 Azure 帐户</span><span class="sxs-lookup"><span data-stu-id="9e75f-149">Sign out of your Azure account</span></span>

<span data-ttu-id="9e75f-150">通过上述步骤配置帐户后，每次启动 Eclipse 时都会自动登录。</span><span class="sxs-lookup"><span data-stu-id="9e75f-150">After you have configured your account by preceding steps, you will be automatically signed in each time you start Eclipse.</span></span> <span data-ttu-id="9e75f-151">但是，若要注销 Azure 帐户，请使用以下步骤。</span><span class="sxs-lookup"><span data-stu-id="9e75f-151">However, if you want to sign out of your Azure account, use the following steps.</span></span>

1. <span data-ttu-id="9e75f-152">在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。   </span><span class="sxs-lookup"><span data-stu-id="9e75f-152">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![用于 Azure 注销的 Eclipse 菜单][L01]

2. <span data-ttu-id="9e75f-154">显示“Azure 注销”  对话框时，单击“是”  。</span><span class="sxs-lookup"><span data-stu-id="9e75f-154">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![“注销”对话框][L02]

## <a name="next-steps"></a><span data-ttu-id="9e75f-156">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9e75f-156">Next steps</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I01]: media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-eclipse-sign-in-instructions/I05.png

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
