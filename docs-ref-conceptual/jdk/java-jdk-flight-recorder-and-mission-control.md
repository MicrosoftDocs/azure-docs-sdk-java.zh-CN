---
title: Java Flight Recorder 和 Mission Control
description: 有关如何使用 Java Flight Recorder 和 Mission Control 收集和查看应用数据的指南。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 64f64f2e5891fccf9d62510f39bd99d73457d590
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533626"
---
# <a name="use-java-flight-recorder-and-mission-control"></a>使用 Java Flight Recorder 和 Mission Control

Zulu Mission Control 是经过充分测试的内部版 JDK Mission Control，后者由 Oracle 于 2018 年开源，现在作为一个项目在 OpenJDK 名义下托管。 Mission Control 与 Java Flight Recorder (JFR) 结合在一起，为 Java 工作负荷提供低开销交互式监视和管理功能。

Zulu Mission Control 兼容以下 Java 开发工具包 (JDK) 和 Java Runtime Environment (JRE)：

* Zulu 12.1 及更高版本
* Zulu 11.0 及更高版本
* Zulu 8u202 (8.36) 及更高版本
* Oracle OpenJDK 11 和 15 及更高版本
* Oracle Java 11.0 及更高版本
* Oracle Java 8.0 及更高版本

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a>安装 Zulu Mission Control 并连接到 JVM

若要安装 Zulu Mission Control，连接到 Java 虚拟机 (JVM) 并实时查看正在运行的应用程序的所有方面，请执行以下步骤：

1.  [安装兼容 Zulu Mission Control 的 JDK 和 JRE](java-jdk-install.md)。

1.  [下载 Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/)，选择适用于系统的版本，将其保存到本地，然后转到相应的目录。

1.  展开下载的文件。

    **Linux：**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows**：

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **macOS：**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  使用兼容的 JDK 之一启动 Java 应用程序。 例如：

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  启动 Zulu Mission Control。

    **Linux：**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows**：

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **macOS：**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  （可选）切换针对 Mission Control 的 JVM 安装。

    在 Windows 设备上，*zmc.exe* 使用在注册表中配置的默认 JVM 安装。 必须通过完整的 JDK 启动 Zulu Mission Control，否则无法自动检测本地 JVM 实例。 如果安装为 JRE，则不会检测到 JVM，你会收到以下警告：

    > [!div class="mx-imgBorder"]
    ![在 JDK 安装仅为 JRE 的情况下出现的警告](../media/jdk/azul-jfr-1.png)

    若要更改 Mission Control 使用的 JVM，请执行以下操作： 

    a. 打开 *zmc.exe* 文件所在目录中的 *zmc.ini* 配置文件。

    b. 在 `-vmargs` 行之前添加两行：  

       * 在第一行中输入 `–vm`。  
       * 在第二行中输入 JDK 安装的路径（例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`）。

1.  执行以下操作，找到正在运行应用程序的 JVM：

    a. 在 Zulu Mission Control 窗口的左窗格中，选择“JVM 浏览器”选项卡。 

    b. 在列表中，选择并展开正在运行应用程序的 JVM 实例。

    ![展开的列表中的 JVM 实例](../media/jdk/azul-jfr-2.png)


1.  必要时启动飞行记录。

    a. 在左窗格的 **Flight Recorder** 下，如果显示“没有记录”消息，请启动记录，方法是：右键单击“Flight Recorder”，然后选择“启动飞行记录”。   

    b. 选择“固定时长记录”或“持续记录”，接着选择“分析”配置（精细）或“持续”配置（降低开销），然后选择“完成”。     

    ![启动飞行记录](../media/jdk/azul-jfr-3.png)

    飞行记录应该显示在 JVM 浏览器中 **Flight Recorder** 行的下面。

1. 转储飞行记录。 为此，请右键单击表示飞行记录的行，然后选择“转储整个记录”。 

    此时会在 Zulu Mission Control 窗口右侧的大窗格中显示一个新选项卡。 此窗格表示刚从运行应用程序的 JVM 转储的飞行记录。

1. 使用 Zulu Mission Control 检查飞行记录。 为此，请在 Zulu Mission Control 窗口的左窗格中选择“大纲”选项卡。  此选项卡显示在飞行记录中收集的数据的各种视图。
 
    ![查看飞行记录](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>资源

若要了解详细信息，请访问 Azul Systems 站点并观看 [Azul Webinar:Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)（Azul 网络研讨会：开源 Flight Recorder 和 Mission Control - 管理和度量 OpenJDK 8 性能）。 此视频内容由 Azul Systems 副 CTO Simon Ritter 讲述。 Flight Recorder 讨论在 31:30 开始。

