---
title: "在 Eclipse 中的 Azure 部署启用远程访问"
description: "了解如何为使用 Azure Toolkit for Eclipse 的 Azure 部署启用远程访问。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>在 Eclipse 中的 Azure 部署启用远程访问
为了帮助排查部署问题，可启用和使用远程访问连接到托管你部署的虚拟机。 远程访问功能依赖于远程桌面协议 (RDP)。 发布到 Azure，或如果你在 Windows 操作系统使用 Eclipse，发布到 Azure 之前，你可以配置远程访问后，你可以为你的部署中配置远程访问。 请注意，你需要是与你的操作系统兼容，以便连接到 Azure 中部署的虚拟机的远程桌面客户端。

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>如何部署到 Azure 之前启用远程访问
> [!NOTE]
> 若要部署到 Azure 应用程序之前，请启用远程访问，你需要在 Windows 上运行 Eclipse。
> 
> 

下图显示**远程访问**用于启用远程访问的属性对话框。

![][ic719494]

有两种方法，以显示**远程访问**属性对话框：

* 单击**高级**中链接**远程访问**部分**发布到 Azure**对话框。

* 打开**属性**的 Azure 项目的对话框。

当你创建新的 Azure 部署项目时，项目不会按默认启用远程访问。 但是，可以轻松启用远程访问通过指定的用户名和密码在**发布到 Azure**对话框。 使用 X.509 证书进行加密的远程访问密码。 如果不使用提供您自己的证书，加密将依赖于随 Azure Plugin for Eclipse 提供的自签名证书。 此自签名的证书位于**cert**的 Azure 项目，存储作为公共证书文件 (SampleRemoteAccessPublic.cer) 和个人信息交换 (PFX) 证书文件 (SampleRemoteAccessPrivate.pfx) 的文件夹。 后者包含私钥的证书，并且具有默认密码， **Password1**。 但是，由于此密码是公开的则应仅出于学习目的，不用于生产部署中使用默认证书。 以外的其他出于学习目的，当你想要启用远程会话中的部署，则应单击**高级**中链接**发布到 Azure**对话框，可以指定您自己的证书。 请注意，你将需要证书的 PFX 版本上载到 Azure 管理门户中中, 托管的服务，以便 Azure 能够解密用户密码。

本教程的其余部分演示了如何为最初创建禁用的远程访问的 Azure 部署项目启用远程访问。 对于本教程，我们将创建新的自签名的证书，且其.pfx 文件将所选的密码。 此外可以使用的证书颁发机构颁发的证书。

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>如何部署到 Azure 之后启用远程访问
若要部署到 Azure 之后启用远程访问，请使用以下步骤：

1. 登录到 Azure 管理门户中使用你的 Azure 帐户

2. 列表中的**云服务**，选择你的部署的云服务

3. 在云服务 web 页中，单击**配置**链接

4. 在配置页的底部，单击**远程**链接

5. 当出现弹出对话框中：
   
   * 指定要为其启用远程访问角色

   * 单击以选择**启用远程桌面**复选框
   
   * 指定用户名和密码，你想要用于远程访问
   
   * 选择要使用的证书

6. 单击“确定”。 

你将看到一条消息指出更改你的配置正在进行，这可能需要几分钟才能完成。 配置更改完成后，按照中的步骤**远程登录**本文后面的部分。

## <a name="how-to-enable-remote-access-in-your-package"></a>如何在你的包中启用远程访问
1. 在 Eclipse 的项目资源管理器窗格中，右键单击你的 Azure 项目并单击**属性**。

2. 在**属性**对话框中，展开**Azure**在左侧窗格中，单击**远程访问**。

3. 在**远程访问**对话框中，确保**允许所有角色使用这些登录凭据接受远程桌面连接**已选中。

4. 指定远程桌面连接的用户名称。

5. 指定并确认用户的密码。 在远程桌面连接时，将使用此对话框中设置的用户名称和密码值。 （请注意，这是密码不同于你的 PFX 密码）。

6. 指定的用户帐户的过期日期。

7. 单击**新建**创建新的自签名的证书。 (或者，你可以选择从通过你的工作区或文件系统的证书**工作区**或**FileSystem**按钮，分别，但出于本教程中，我们将创建一个新的证书。)

   * 在**新证书**对话框中，指定并确认你将使用 PFX 文件的密码。

   * 接受提供的值**名 (CN)**，或使用自定义的名称。

   * 指定新证书，在.cer 表单中，将保存于其中的路径和文件名称。 有关此步骤和下一步，你可以使用**cert**文件夹的 Azure 项目，但你可以自由选择另一个位置。 对于此教程的目的，我们将使用**c:\mycert\mycert.cer**。 (创建**c:\mycert**然后再继续操作或使用现有文件夹如果所需的文件夹。)

   * 指定保存新的证书，其.pfx 窗体中的私钥的路径和文件名称。 对于此教程的目的，我们将使用**c:\mycert\mycert.pfx**。 你**新证书**对话框应看起来与下面类似 (如果你未使用更新的文件夹路径**c:\mycert**):
     
      ![][ic712275]

   * 单击**确定**关闭**新证书**对话框。

8. 你**远程访问**对话框应类似于以下：</p>
   
   ![][ic719495]

9. 单击**确定**关闭**远程访问**对话框。

重新应用程序时，生成的生成设置部署到云。

## <a name="to-log-in-remotely"></a>远程登录
你的角色实例准备就绪后，你可以远程登录到承载你的应用程序的虚拟机。

* 如果在 Windows 和所选上使用 Eclipse**上的启动远程桌面部署**选项期间你部署到 Azure，你将使用远程桌面连接登录屏幕时看到你的部署开始。 当系统提示输入用户名和密码的值时，输入你为远程用户指定的值，将能够登录。

* 远程登录另一种方法是通过<a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure 管理门户</a>:
  
  * 在**云服务**查看 Azure 管理门户，单击你的云服务，单击**实例**，单击某一特定实例，然后单击**连接**按钮。 **连接**按如下所示的命令栏中会显示按钮：
    
      ![][ic659273]

  * 单击后**连接**按钮，将会提示你打开 RDP 文件。 打开文件并按照提示进行操作。 （你可能还将此文件保存到本地计算机，，然后运行该文件通过双击，它就可以远程登录到你的虚拟机，而无需首先进入管理门户。）

  * 当系统提示输入用户名和密码的值时，输入你为远程用户指定的值，将能够登录。

> [!NOTE]
> 如果你是在非 Windows 操作系统上，你必须使用与你的操作系统兼容的远程桌面客户端并遵守你下载的 RDP 文件中的设置配置该客户端的步骤。
> 
> 

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
