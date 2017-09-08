---
title: "启用会话相关性使用 Azure Toolkit for Eclipse"
description: "了解如何启用会话相关性使用 Azure Toolkit for Eclipse。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a>启用会话相关性
在 Azure Toolkit for Eclipse 中，你可以启用 HTTP 会话相关性或"粘性会话"，为你的角色。 下图显示**负载平衡**用于启用会话相关性功能的属性对话框：

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>若要启用你的角色的会话相关性
1. 右键单击 Eclipse 的项目资源管理器中的角色，单击**Azure**，然后单击**负载平衡**。

2. 在**WorkerRole1 负载平衡属性**对话框：

   a. 检查**为此角色启用 HTTP 会话相关性 （粘性会话）。**

   b。 有关**输入终结点以使用**，例如，选择一个输入终结点，若要使用， **http （public: 80，private: 8080）**。 你的应用程序必须使用此终结点作为其 HTTP 终结点。 你可以为您的角色，启用多个终结点，但你可以选择仅其中之一来支持粘性会话。

   c. 重新生成你的应用程序。

启用后，如果你有多个角色实例，来自特定客户端的 HTTP 请求将继续由同一角色实例处理。

Eclipse 工具包可以做到这一点安装到每个角色实例中调用应用程序请求路由 (ARR) 的特殊 IIS 模块。 ARR 重排 HTTP 请求路由到相应的角色实例。 该工具包会自动重新配置所选的端点，以便将传入的 HTTP 流量先路由到 ARR 软件。 该工具包还会创建新的内部终结点的 Java 服务器配置为侦听。 这是 ARR 用于将其重新路由到相应的角色实例的 HTTP 流量的终结点。 这样一来，在多实例部署中的每个角色实例用作的所有其他情况下，启用粘滞会话的反向代理。

## <a name="notes-about-session-affinity"></a>有关会话相关性的说明
* 会话相关性在计算模拟器中无效。 设置而不会干扰你的生成过程应用在计算模拟器中或计算模拟器执行，但该功能本身在计算模拟器中不起作用。

* 启用会话相关性将导致在 Azure 中，部署占用，因为将下载其他软件，并将其安装到您的角色实例，在 Azure 云中启动你的服务时的磁盘空间量增加。

* 将长时间来初始化每个角色。

* 将添加内部终结点，能够充当流量 rerouter，如上所述。


## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
