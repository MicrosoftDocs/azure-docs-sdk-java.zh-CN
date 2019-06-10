---
title: 安装用于 Eclipse 的 Azure 工具包
description: 了解如何安装用于 Eclipse 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 8e6630f7e019d950249e7e84024ac800a0f2f136
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270848"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>安装用于 Eclipse 的 Azure 工具包

可以通过两种方式安装用于 Eclipse 的 Azure 工具包：

  - [Eclipse Marketplace](#eclipse-marketplace)
  - [安装新软件](#install-new-software)

> [!NOTE] 
> 
> 用于 Eclipse 的 Azure 工具包是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为： 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [azure-toolkit-for-eclipse-basic-prerequisites](../includes/azure-toolkit-for-eclipse-basic-prerequisites.md)]

## <a name="eclipse-marketplace"></a>Eclipse Marketplace

1. 将以下按钮拖到正在运行的 Eclipse 工作区。

    [![拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "拖到正在运行的 Eclipse* 工作区。*需要 Eclipse Marketplace 客户端")

2. 否则，也可在“帮助/Eclipse Marketplace”中搜索并安装**用于 Eclipse 插件的 Azure 工具包**。 

    ![市场](./media/azure-toolkit-for-eclipse-installation/marketplace.png)

## <a name="install-new-software"></a>安装新软件

1. 启动 Eclipse。

1. 单击“帮助”  菜单，然后单击“安装新软件”  ，如下图所示。

   ![安装用于 Eclipse 的 Azure 工具包][01]

1. 在“可用软件”  对话框的“使用”  文本框中，键入 `http://dl.microsoft.com/eclipse/`，然后按 Enter  键。

1. 在“名称”窗格中，选中“用于 Java 的 Azure 工具包”，并取消选中“在安装过程中访问所有更新站点以查找所需的软件”。    屏幕应与下图中所示类似：

   ![安装用于 Eclipse 的 Azure 工具包][02]

1. 如果展开“用于 Eclipse 的 Azure 工具包”，会看到一个要安装的组件的列表，例如： 

   | Feature | 说明 | 
   |---|---| 
   | **用于 Java 的 Application Insights 插件** | 可通过此组件将 Azure 的遥测日志记录和分析服务用于应用程序和服务器实例。 | 
   | **Azure 常用插件** | 提供其他工具包组件所需的常见功能。 | 
   | **用于 Eclipse 的 Azure 容器工具** | 用于生成 .WAR 并将其作为 Docker 容器部署到 Docker 计算机。 | 
   | **用于 Eclipse 的 Azure 容器** | 用于将 .WAR 或 .JAR 项目作为 Docker 容器部署到 Azure 虚拟机。 | 
   | **用于 Eclipse 的 Azure 资源管理器** | 提供一个资源管理器样式的界面，用于管理 Azure 资源。 | 
   | **Microsoft JDBC Driver 6.1 for SQL Server** | 提供适用于 SQL Server 的 JDBC API 以及适用于 Java Platform Enterprise Edition 8 的 Microsoft Azure SQL 数据库。 | 
   | **Microsoft Azure Java 库包** | 提供用于访问 Microsoft Azure 服务（例如存储、服务总线、服务运行时等）的 API。 | 

1. 单击“下一步”。  （如果在安装该工具包时遇到不正常的延迟，请确保未选中“在安装过程中访问所有更新站点以查找所需的软件”。） 

1. 在“安装详细信息”对话框中，单击“下一步”。  

   ![查看安装详细信息][03]

1. 在“查看许可证”对话框中，查看许可协议条款。  如果接受许可协议条款，请单击“我接受许可协议条款”，并单击“完成”。   （剩余步骤假定你接受许可协议条款。 如果不接受许可协议条款，请退出安装过程。）

   ![查看许可证][04]

   Eclipse 将下载并安装必要的包。

   ![安装进度][05]

1. 如果系统提示重新启动 Eclipse 以完成安装，请单击“是”。 

   ![重新启动提示][06]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[01]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png
