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
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: ee87b73841013eafb47d00e0cf5028d6b3ff932c
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>用于 Eclipse 的 Azure 工具包的 Azure 登录说明

用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：

  * **自动** - 如果使用此方法，将创建一个包含服务主体数据的凭据文件，之后可以使用该凭据文件自动登录到 Azure 帐户。
  * **交互式** - 如果使用此方法，每次登录 Azure 帐户时，都需要输入 Azure 凭据。

以下部分中的步骤介绍如何使用每个方法。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>自动登录到 Azure 帐户并创建凭据文件，以便在将来使用

以下步骤将引导完成创建一个包含服务主体数据的凭据文件。 完成这些步骤后，每次打开项目时，Eclipse 都会自动使用凭据文件自动登录到 Azure。

1. 使用 Eclipse 打开项目。

1. 依次单击“工具”、“Azure”、“登录”。

   ![用于 Azure 登录的 Eclipse 菜单][A01]

1. 显示“Azure 登录”对话框时，选择“自动”，并单击“新建”。

   ![“登录”对话框][A02]

1. 显示“Azure 登录”对话框时，输入 Azure 凭据，并单击“登录”。

   ![“Azure 登录”对话框][A03]

1. 显示“创建身份验证文件”对话框时，选择要使用的订阅，选择目标目录，并单击“启动”。

   ![“Azure 登录”对话框][A04]

1. 此时会显示“服务主体创建状态”对话框，成功创建文件后，单击“确定”。

   ![“服务主体创建状态”对话框][A05]

1. 显示“Azure 登录”对话框时，单击“登录”。

   ![“Azure 登录”对话框][A06]

1. 显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。

   ![“选择订阅”对话框][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>已自动登录时，从 Azure 帐户中注销

按照上一部分中的步骤配置后，每次重新启动 Eclipse 时，Azure 工具包都会你将自动登录到 Azure 帐户。 但是，要注销 Azure 帐户并禁止 Azure 工具包你将自动登录，请使用以下步骤。

1. 在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。

   ![用于 Azure 注销的 Eclipse 菜单][L01]

1. 显示“Azure 注销”对话框时，单击“是”。

   ![“注销”对话框][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>使用已创建的凭据文件自动登录到 Azure 帐户

如果在使用 Eclipse 时从 Azure 中注销，则需要重新配置用于 Eclipse 的 Azure 工具包以使用已创建的凭据文件，才能自动登录到 Azure 帐户。 以下步骤将引导完成配置 Azure 工具包以使用现有凭据文件。

1. 使用 Eclipse 打开项目。

1. 依次单击“工具”、“Azure”、“登录”。

   ![用于 Azure 登录的 Eclipse 菜单][A01]

1. 显示“Azure 登录”对话框时，选择“自动”，并单击“浏览”。

   ![“登录”对话框][A02]

1. 显示“选择已验证的文件”对话框时，选择先前创建的凭据文件，然后单击“选择”。

   ![“登录”对话框][A08]

1. 显示“Azure 登录”对话框时，单击“登录”。

   ![“Azure 登录”对话框][A06]

1. 显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。

   ![“选择订阅”对话框][A07]

## <a name="signing-into-your-azure-account-interactively"></a>以交互方式登录到 Azure 帐户

以下步骤将说明如何通过手动输入 Azure 凭据登录到 Azure。

1. 使用 Eclipse 打开项目。

1. 依次单击“工具”、“Azure”、“登录”。

   ![用于 Azure 登录的 Eclipse 菜单][I01]

1. 显示“Azure 登录”对话框时，选择“交互式”，并单击“登录”。

   ![“登录”对话框][I02]

1. 显示“Azure 登录”对话框时，输入 Azure 凭据，并单击“登录”。

   ![“Azure 登录”对话框][I03]

1. 显示“选择订阅”对话框时，选择要使用的订阅，并单击“确定”。

   ![“选择订阅”对话框][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>已以交互方式登录时，从 Azure 帐户中注销

按照上一部分中的步骤配置后，每次重新启动 Eclipse 时，都会自动从 Azure 帐户中注销。 但是，如果要在不重新启动 Eclipse 的情况下注销 Azure 帐户，请使用以下步骤。

1. 在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。

   ![用于 Azure 注销的 Eclipse 菜单][L01]

1. 显示“Azure 注销”对话框时，单击“是”。

   ![“注销”对话框][L02]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

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
