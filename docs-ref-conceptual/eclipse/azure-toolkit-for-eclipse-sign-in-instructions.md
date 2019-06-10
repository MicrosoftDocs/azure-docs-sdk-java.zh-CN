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
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>用于 Eclipse 的 Azure 工具包的登录说明

用于 Eclipse 的 Azure 工具包提供了两种用于登录到 Azure 帐户的方法：

  - [通过设备登录名登录到 Azure 帐户](#sign-in-to-your-azure-account-by-device-login)
  - [通过服务主体登录到 Azure 帐户](#sign-in-to-your-azure-account-by-service-principal)

还提供了[**注销**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>通过设备登录名登录到 Azure 帐户

若要通过设备登录名登录 Azure，请执行以下操作：

1. 使用 Eclipse 打开项目。

2. 依次单击“工具”、“Azure”、“登录”。   
   ![用于 Azure 登录的 Eclipse 菜单][I01]

3. 在“Azure 登录”窗口中选择“设备登录名”，然后单击“登录”。   

   ![“Azure 登录”窗口，其中已选择“设备登录”][I02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][I03]

> [!NOTE]
>
> 如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 Internet Explorer、Firefox 或 Chrome：
>
> 1. 打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”
>
> 2. 选择首选使用的浏览器
>

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][I04]

6. 最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  

   ![“选择订阅”对话框][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>通过服务主体登录到 Azure 帐户

本部分逐步引导创建一个包含服务主体数据的凭据文件。 完成此过程后，在打开项目时，Eclipse 会使用凭据文件将你自动登录到 Azure。

1. 使用 Eclipse 打开项目。

2. 依次单击“工具”、“Azure”、“登录”。   
   ![Eclipse Azure 登录命令][A01]

3. 在“Azure 登录”窗口中，选择“服务主体”。   如果还没有服务主体身份验证文件，请单击“新建”创建一个。  否则，可以单击“浏览”将其打开，然后跳到步骤 8。 

   ![已选中“服务主体”的“Azure 登录”窗口][A02]

4. 在“Azure 设备登录”对话框中单击“复制并打开”。  

   ![“Azure 登录”对话框窗口][A08]

> [!NOTE]
>
> 如果浏览器不打开，请将 Eclipse 配置为使用外部浏览器，例如 IE 或 Chrome：
>
> 1. 打开“首选项”->“常规”->“Web 浏览器”->“在 Eclipse 中使用外部 Web 浏览器”
>
> 2. 选择首选使用的浏览器
>

5. 在浏览器中粘贴设备代码（在最后一个步骤中单击“复制并打开”时已复制），然后单击“下一步”。  

   ![设备登录浏览器][A03]

6. 在“创建身份验证文件”窗口中选择要使用的订阅，选择目标目录，并单击“启动”。  

   ![“创建身份验证文件”窗口][A04]

7. 成功创建文件后，请在“服务主体创建状态”对话框中单击“确定”。  

   ![“服务主体创建状态”对话框][A05]

8. 所创建文件的地址会自动填充在“Azure 登录”窗口中，此时请单击“登录”。  

   ![“Azure 登录”对话框][A06]

9. 最后，在“选择订阅”对话框中选择要使用的订阅，然后单击“确定”。  

   ![“选择订阅”对话框][A07]

## <a name="sign-out-of-your-azure-account"></a>注销 Azure 帐户

通过上述步骤配置帐户后，每次启动 Eclipse 时都会自动登录。 但是，若要注销 Azure 帐户，请使用以下步骤。

1. 在 Eclipse 中，依次单击“工具”、“Azure”、“注销”。   

   ![用于 Azure 注销的 Eclipse 菜单][L01]

2. 显示“Azure 注销”  对话框时，单击“是”  。

   ![“注销”对话框][L02]

## <a name="next-steps"></a>后续步骤

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
