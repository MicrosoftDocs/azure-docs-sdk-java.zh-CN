---
title: Java Flight Recorder 和 Mission Control
description: 有关如何使用 Java Flight Recorder 和 Mission Control 收集和查看应用数据的指南。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: b27e0f741f1322b7e8e1df363dbb2f40a3d34d53
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568560"
---
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a>使用 Java Flight Recorder (JFR) 和 Mission Control

Zulu Mission Control 是经过充分测试的内部版 JDK Mission Control，后者由 Oracle 于 2018 年开源，现在作为一个项目在 OpenJDK 名义下托管。 Mission Control 与 Flight Recorder 结合在一起，为 Java 工作负荷提供低开销交互式监视和管理功能。

Zulu Mission Control 兼容以下 JDK/JRE：

* Zulu 12.1 及更高版本
* Zulu 11.0 及更高版本
* Zulu 8u202 (8.36) 及更高版本
* Oracle OpenJDK 11+15 及更高版本
* Oracle Java 11.0 及更高版本
* Oracle Java 8.0 及更高版本

请按以下步骤安装 Zulu Mission Control，连接到 Java 虚拟机 (JVM)，然后实时查看正在运行的应用程序的所有方面。

1.  [安装兼容 Zulu Mission Control 的 JDK/JRE](java-jdk-install.md)。

2.  从 [Azul 下载站点](https://www.azul.com/products/zulu-mission-control/)下载 Zulu Mission Control，选择适用于系统的版本，将其保存到本地，然后转到相应的目录。

3.  展开下载的文件。

    **Linux：**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows**：

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **MacOS：**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  使用兼容的 JDK 之一启动 Java 应用程序。 例如：

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  启动 Zulu Mission Control

    **Linux：**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows**：

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **MacOS：**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  切换针对 Mission Control 的 JVM 安装（可选）

    在 Windows 中，*zmc.exe* 会使用在注册表中配置的默认 JVM 安装。 必须通过完整的 JDK 启动 Zulu Mission Control，否则无法自动检测本地 JVM 实例。 如果这是一个 JRE，则会出现以下警告：

    > [!div class="mx-imgBorder"]
    ![在 JDK 安装仅为 JRE 的情况下出现的警告](../media/jdk/azul-jfr-1.png)

    若要更改 Mission Control 使用的 JVM，请执行以下步骤： 
    1.  打开 *zmc.exe* 所在目录中的 *zmc.ini* 配置文件
    2.  在 `-vmargs` 行之前添加两行：
        * 在第一行中，写入 `–vm`
        * 在第二行中，写入 JDK 安装的路径。 （例如，`C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`）。

7.  找到运行应用程序的 JVM
    1.  在 Zulu Mission Control 窗口的左上窗格中，单击标为“JVM 浏览器”的选项卡。 
    2.  选择并展开运行应用程序的 JVM 实例左上的列表项。

    > [!div class="mx-imgBorder"]
    ![展开 JVM 实例左上的列表项](../media/jdk/azul-jfr-2.png)


8.  必要时启动飞行记录
    1.  如果 Flight Recorder 显示“没有记录”，请启动一个，方法是：右键单击“JVM 浏览器”选项卡中的 Flight Recorder 行，然后选择“启动飞行记录...” 
    2.  选择固定时长记录或持续记录，接着选择“分析”配置（精细）或“持续”配置（降低开销），然后单击“完成”。 

    > [!div class="mx-imgBorder"]
    ![启动飞行记录](../media/jdk/azul-jfr-3.png)

9.  转储飞行记录
    1.  飞行记录应该显示在 JVM 浏览器中 Flight Recorder 行的下面。 右键单击表示飞行记录的行，然后选择“转储整个记录”。 
    2.  此时会在 Zulu Mission Control 窗口右侧的大窗格中显示一个新选项卡。 此窗格表示刚从运行应用程序的 JVM 转储的飞行记录。

10. 使用 Zulu Mission Control 检查飞行记录
    1.  在 Zulu Mission Control 窗口的左窗格中选择标为“大纲”的选项卡（如果尚未激活）。  此选项卡包含在飞行记录中收集的数据的不同视图。
 
    > [!div class="mx-imgBorder"]
    ![查看飞行记录](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a>资源

我们还准备了一个[演示视频](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)，由 Azul Systems 副 CTO Simon Ritter 讲述。 此视频详细介绍了 Flight Recorder 和 Zulu Mission Control 的配置和设置。 Flight Recorder 讨论在 31:30 开始。

