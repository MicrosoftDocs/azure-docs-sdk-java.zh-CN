---
title: 安装 Azure Toolkit for IntelliJ
description: 了解如何安装用于 IntelliJ 的 Azure 工具包插件，以创建云应用程序并将其部署到 Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 86cef07873ae7a2ba75aab1044fe4d241cd5b13e
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270874"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>安装 Azure Toolkit for IntelliJ

借助 Azure Toolkit for IntelliJ 提供的模板和功能，可以轻松使用 IntelliJ IDEA 开发环境来创建、开发、测试云应用程序并将其部署到 Azure。

> [!NOTE] 
> 
> Azure Toolkit for IntelliJ 是一个开放源代码项目，其源代码可从 GitHub 上项目站点的 MIT 许可证下获取，URL 为： 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

可以通过两种方法来安装用于 IntelliJ 的 Azure 工具包：一种是使用“设置”对话框，另一种是使用开始屏幕上的“配置”菜单。   两种安装方法都会在以下步骤中演示。

## <a name="prerequisites"></a>先决条件

使用用于 IntelliJ 的 Azure 工具包需要以下软件组件：

* 已安装的 Java 开发工具包 (JDK) 8+，例如：[OpenJDK](https://openjdk.java.net/) 或 [Oracle JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* 已安装的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) 旗舰版或社区版

> [!NOTE]
> 
> JetBrains 插件存储库中的 [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 页列出了与该工具包兼容的内部版本。
> 

<!--
> [!IMPORTANT]
> 
> If you are using the Azure Toolkit for IntelliJ on Windows, the toolkit requires installing the Azure SDK 2.9.6 or later in order to use the Azure emulator. You have two options for installing the Azure SDK:
> 
> * You can download and install the Azure SDK by using the [Web Platform Installer (WebPI)](http://go.microsoft.com/fwlink/?LinkID=252838).
> * If you do not have the Azure SDK installed when you create your first Azure deployment project, you will be prompted to automatically download install the requisite version of the Azure SDK.
> 
> Note that the Azure SDK is only required on Windows.
> 
-->


## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>从“设置”对话框安装 Azure Toolkit for IntelliJ 的步骤

1. 启动 IntelliJ IDEA。

1. IntelliJ IDEA 打开后，单击“文件”，并单击“设置”。  
   
   ![打开 IntelliJ IDEA 的“设置”对话框][01a]

1. 在“设置”对话框中，单击“插件”，并单击“浏览存储库”。  
   
   ![IntelliJ IDEA 的“设置”对话框][02a]

1. 在“浏览存储库”对话框的搜索框中键入“Azure”。  突出显示“用于 IntelliJ 的 Azure 工具包”，并单击“安装”。  
   
   ![搜索 Azure Toolkit for IntelliJ][03]
   
   IntelliJ IDEA 会在一个对话框中显示安装进度。
   
   ![安装进度][04]

1. 安装完成后，单击“重新启动 IntelliJ IDEA”。 
   
   ![重新启动 IntelliJ IDEA][05]

1. 单击“确定”关闭“设置”对话框。 
   
   ![关闭 IntelliJ IDEA 的“设置”对话框][06]

1. 当系统提示是重新启动 IntelliJ IDEA 还是推迟时，请单击“重新启动”。 
   
1   ![重新启动 IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>从开始屏幕安装 Azure Toolkit for IntelliJ 的步骤

1. 启动 IntelliJ IDEA。

1. IntelliJ IDEA 开始屏幕出现后，单击“配置”，并单击“插件”。  
   
   ![安装 IntelliJ IDEA 插件][01b]

1. 在“插件”对话框中，单击“浏览存储库”。  
   
   ![浏览 IntelliJ IDEA 插件存储库][02b]

1. 在“浏览存储库”对话框的搜索框中键入“Azure”。  突出显示“用于 IntelliJ 的 Azure 工具包”，并单击“安装”。  
   
   ![搜索 Azure Toolkit for IntelliJ][03]
   
   IntelliJ IDEA 会在一个对话框中显示安装进度。
   
   ![安装进度][04]

1. 安装完成后，单击“重新启动 IntelliJ IDEA”。 
   
   ![重新启动 IntelliJ IDEA][05]

1. 当系统提示是重新启动 IntelliJ IDEA 还是推迟时，请单击“重新启动”。 
   
   ![重新启动 IntelliJ IDEA][07]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
