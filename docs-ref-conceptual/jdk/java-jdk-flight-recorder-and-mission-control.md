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
# <a name="using-java-flight-recorder-jfr-and-mission-control"></a><span data-ttu-id="9d790-103">使用 Java Flight Recorder (JFR) 和 Mission Control</span><span class="sxs-lookup"><span data-stu-id="9d790-103">Using Java Flight Recorder (JFR) and Mission Control</span></span>

<span data-ttu-id="9d790-104">Zulu Mission Control 是经过充分测试的内部版 JDK Mission Control，后者由 Oracle 于 2018 年开源，现在作为一个项目在 OpenJDK 名义下托管。</span><span class="sxs-lookup"><span data-stu-id="9d790-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="9d790-105">Mission Control 与 Flight Recorder 结合在一起，为 Java 工作负荷提供低开销交互式监视和管理功能。</span><span class="sxs-lookup"><span data-stu-id="9d790-105">Coupled with Flight Recorder, Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="9d790-106">Zulu Mission Control 兼容以下 JDK/JRE：</span><span class="sxs-lookup"><span data-stu-id="9d790-106">Zulu Mission Control is compatible with the following JDKs/JREs:</span></span>

* <span data-ttu-id="9d790-107">Zulu 12.1 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="9d790-108">Zulu 11.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="9d790-109">Zulu 8u202 (8.36) 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="9d790-110">Oracle OpenJDK 11+15 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-110">Oracle OpenJDK 11+15 and later</span></span>
* <span data-ttu-id="9d790-111">Oracle Java 11.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="9d790-112">Oracle Java 8.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="9d790-112">Oracle Java 8.0 and later</span></span>

<span data-ttu-id="9d790-113">请按以下步骤安装 Zulu Mission Control，连接到 Java 虚拟机 (JVM)，然后实时查看正在运行的应用程序的所有方面。</span><span class="sxs-lookup"><span data-stu-id="9d790-113">Follow the steps below to install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application.</span></span>

1.  <span data-ttu-id="9d790-114">[安装兼容 Zulu Mission Control 的 JDK/JRE](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="9d790-114">[Install a Zulu Mission Control compatible JDK/JRE](java-jdk-install.md).</span></span>

2.  <span data-ttu-id="9d790-115">从 [Azul 下载站点](https://www.azul.com/products/zulu-mission-control/)下载 Zulu Mission Control，选择适用于系统的版本，将其保存到本地，然后转到相应的目录。</span><span class="sxs-lookup"><span data-stu-id="9d790-115">Download Zulu Mission Control from [the Azul download site](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

3.  <span data-ttu-id="9d790-116">展开下载的文件。</span><span class="sxs-lookup"><span data-stu-id="9d790-116">Expand the downloaded file.</span></span>

    <span data-ttu-id="9d790-117">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="9d790-117">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="9d790-118">**Windows**：</span><span class="sxs-lookup"><span data-stu-id="9d790-118">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="9d790-119">**MacOS：**</span><span class="sxs-lookup"><span data-stu-id="9d790-119">**MacOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  <span data-ttu-id="9d790-120">使用兼容的 JDK 之一启动 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9d790-120">Start your Java application using one of the compatible JDKs.</span></span> <span data-ttu-id="9d790-121">例如：</span><span class="sxs-lookup"><span data-stu-id="9d790-121">E.g.:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  <span data-ttu-id="9d790-122">启动 Zulu Mission Control</span><span class="sxs-lookup"><span data-stu-id="9d790-122">Start Zulu Mission Control</span></span>

    <span data-ttu-id="9d790-123">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="9d790-123">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="9d790-124">**Windows**：</span><span class="sxs-lookup"><span data-stu-id="9d790-124">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="9d790-125">**MacOS：**</span><span class="sxs-lookup"><span data-stu-id="9d790-125">**MacOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  <span data-ttu-id="9d790-126">切换针对 Mission Control 的 JVM 安装（可选）</span><span class="sxs-lookup"><span data-stu-id="9d790-126">Switch the JVM installation for Mission Control (Optional)</span></span>

    <span data-ttu-id="9d790-127">在 Windows 中，*zmc.exe* 会使用在注册表中配置的默认 JVM 安装。</span><span class="sxs-lookup"><span data-stu-id="9d790-127">On Windows, *zmc.exe* will use the default JVM installation configured in the registry.</span></span> <span data-ttu-id="9d790-128">必须通过完整的 JDK 启动 Zulu Mission Control，否则无法自动检测本地 JVM 实例。</span><span class="sxs-lookup"><span data-stu-id="9d790-128">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="9d790-129">如果这是一个 JRE，则会出现以下警告：</span><span class="sxs-lookup"><span data-stu-id="9d790-129">If this is a JRE, you will see the warning below:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9d790-130">![在 JDK 安装仅为 JRE 的情况下出现的警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="9d790-130">![Warning if JDK install is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="9d790-131">若要更改 Mission Control 使用的 JVM，请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="9d790-131">To change the JVM used by Mission Control, follow these steps:</span></span> 
    1.  <span data-ttu-id="9d790-132">打开 *zmc.exe* 所在目录中的 *zmc.ini* 配置文件</span><span class="sxs-lookup"><span data-stu-id="9d790-132">Open *zmc.ini* configuration file, located in the same directory as the *zmc.exe*</span></span>
    2.  <span data-ttu-id="9d790-133">在 `-vmargs` 行之前添加两行：</span><span class="sxs-lookup"><span data-stu-id="9d790-133">Before the line `-vmargs`, add two lines:</span></span>
        * <span data-ttu-id="9d790-134">在第一行中，写入 `–vm`</span><span class="sxs-lookup"><span data-stu-id="9d790-134">On the first line, write `–vm`</span></span>
        * <span data-ttu-id="9d790-135">在第二行中，写入 JDK 安装的路径。</span><span class="sxs-lookup"><span data-stu-id="9d790-135">On the second line, write the path to your JDK installation.</span></span> <span data-ttu-id="9d790-136">（例如，`C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`）。</span><span class="sxs-lookup"><span data-stu-id="9d790-136">(For example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

7.  <span data-ttu-id="9d790-137">找到运行应用程序的 JVM</span><span class="sxs-lookup"><span data-stu-id="9d790-137">Locate the JVM running your application</span></span>
    1.  <span data-ttu-id="9d790-138">在 Zulu Mission Control 窗口的左上窗格中，单击标为“JVM 浏览器”的选项卡。 </span><span class="sxs-lookup"><span data-stu-id="9d790-138">In the upper left pane of the Zulu Mission Control window click on the tab labelled **JVM Browser**.</span></span>
    2.  <span data-ttu-id="9d790-139">选择并展开运行应用程序的 JVM 实例左上的列表项。</span><span class="sxs-lookup"><span data-stu-id="9d790-139">Select and expand the list item in the upper left for your the JVM instance running your application.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9d790-140">![展开 JVM 实例左上的列表项](../media/jdk/azul-jfr-2.png)</span><span class="sxs-lookup"><span data-stu-id="9d790-140">![Expand the list item in the upper-left for your JVM instance](../media/jdk/azul-jfr-2.png)</span></span>


8.  <span data-ttu-id="9d790-141">必要时启动飞行记录</span><span class="sxs-lookup"><span data-stu-id="9d790-141">Start a Flight Recording, if necessary</span></span>
    1.  <span data-ttu-id="9d790-142">如果 Flight Recorder 显示“没有记录”，请启动一个，方法是：右键单击“JVM 浏览器”选项卡中的 Flight Recorder 行，然后选择“启动飞行记录...” </span><span class="sxs-lookup"><span data-stu-id="9d790-142">If the Flight Recorder displays "No Recordings", start one by right-clicking on the Flight Recorder line in the JVM Browser tab and selecting **Start Flight Recording...**</span></span>
    2.  <span data-ttu-id="9d790-143">选择固定时长记录或持续记录，接着选择“分析”配置（精细）或“持续”配置（降低开销），然后单击“完成”。 </span><span class="sxs-lookup"><span data-stu-id="9d790-143">Select either a fixed duration recording or a continuous recording, and either a Profiling configuration (fine-grained) or a Continuous configuration (lower overhead), then click **Finish**.</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9d790-144">![启动飞行记录](../media/jdk/azul-jfr-3.png)</span><span class="sxs-lookup"><span data-stu-id="9d790-144">![Start a Flight Recording](../media/jdk/azul-jfr-3.png)</span></span>

9.  <span data-ttu-id="9d790-145">转储飞行记录</span><span class="sxs-lookup"><span data-stu-id="9d790-145">Dump the Flight Recording</span></span>
    1.  <span data-ttu-id="9d790-146">飞行记录应该显示在 JVM 浏览器中 Flight Recorder 行的下面。</span><span class="sxs-lookup"><span data-stu-id="9d790-146">A Flight Recording should appear below the Flight Recorder line in the JVM Browser.</span></span> <span data-ttu-id="9d790-147">右键单击表示飞行记录的行，然后选择“转储整个记录”。 </span><span class="sxs-lookup"><span data-stu-id="9d790-147">Right-click on the line representing the Flight Recording and select **Dump whole recording**.</span></span>
    2.  <span data-ttu-id="9d790-148">此时会在 Zulu Mission Control 窗口右侧的大窗格中显示一个新选项卡。</span><span class="sxs-lookup"><span data-stu-id="9d790-148">A new tab will appear in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="9d790-149">此窗格表示刚从运行应用程序的 JVM 转储的飞行记录。</span><span class="sxs-lookup"><span data-stu-id="9d790-149">This pane represents the Flight Recording just dumped from the JVM running your application.</span></span>

10. <span data-ttu-id="9d790-150">使用 Zulu Mission Control 检查飞行记录</span><span class="sxs-lookup"><span data-stu-id="9d790-150">Examine the Flight Recording using Zulu Mission Control</span></span>
    1.  <span data-ttu-id="9d790-151">在 Zulu Mission Control 窗口的左窗格中选择标为“大纲”的选项卡（如果尚未激活）。 </span><span class="sxs-lookup"><span data-stu-id="9d790-151">If not already activated, select the tab labelled **Outline** in the left pane of the Zulu Mission Control Window.</span></span> <span data-ttu-id="9d790-152">此选项卡包含在飞行记录中收集的数据的不同视图。</span><span class="sxs-lookup"><span data-stu-id="9d790-152">This tab contains different views of the data collected in the Flight Recording.</span></span>
 
    > [!div class="mx-imgBorder"]
    <span data-ttu-id="9d790-153">![查看飞行记录](../media/jdk/azul-jfr-4.png)</span><span class="sxs-lookup"><span data-stu-id="9d790-153">![Review the Fliight Recording](../media/jdk/azul-jfr-4.png)</span></span>

## <a name="resources"></a><span data-ttu-id="9d790-154">资源</span><span class="sxs-lookup"><span data-stu-id="9d790-154">Resources</span></span>

<span data-ttu-id="9d790-155">我们还准备了一个[演示视频](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)，由 Azul Systems 副 CTO Simon Ritter 讲述。</span><span class="sxs-lookup"><span data-stu-id="9d790-155">We have also prepared a [demonstration video](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="9d790-156">此视频详细介绍了 Flight Recorder 和 Zulu Mission Control 的配置和设置。</span><span class="sxs-lookup"><span data-stu-id="9d790-156">The video walks you through the configuration and setup of both Flight Recorder and Zulu Mission Control.</span></span> <span data-ttu-id="9d790-157">Flight Recorder 讨论在 31:30 开始。</span><span class="sxs-lookup"><span data-stu-id="9d790-157">The Flight Recorder discussion starts at 31:30.</span></span>

