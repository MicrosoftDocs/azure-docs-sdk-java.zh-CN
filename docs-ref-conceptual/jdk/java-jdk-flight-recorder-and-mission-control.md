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
# <a name="use-java-flight-recorder-and-mission-control"></a><span data-ttu-id="0f28a-103">使用 Java Flight Recorder 和 Mission Control</span><span class="sxs-lookup"><span data-stu-id="0f28a-103">Use Java Flight Recorder and Mission Control</span></span>

<span data-ttu-id="0f28a-104">Zulu Mission Control 是经过充分测试的内部版 JDK Mission Control，后者由 Oracle 于 2018 年开源，现在作为一个项目在 OpenJDK 名义下托管。</span><span class="sxs-lookup"><span data-stu-id="0f28a-104">Zulu Mission Control is a fully-tested build of JDK Mission Control, which was open-sourced by Oracle in 2018 and is managed as a project under the OpenJDK umbrella.</span></span> <span data-ttu-id="0f28a-105">Mission Control 与 Java Flight Recorder (JFR) 结合在一起，为 Java 工作负荷提供低开销交互式监视和管理功能。</span><span class="sxs-lookup"><span data-stu-id="0f28a-105">Coupled with Java Flight Recorder (JFR), Mission Control delivers low-overhead, interactive monitoring and management capabilities for Java workloads.</span></span>

<span data-ttu-id="0f28a-106">Zulu Mission Control 兼容以下 Java 开发工具包 (JDK) 和 Java Runtime Environment (JRE)：</span><span class="sxs-lookup"><span data-stu-id="0f28a-106">Zulu Mission Control is compatible with the following Java Development Kits (JDKs) and Java Runtime Environments (JREs):</span></span>

* <span data-ttu-id="0f28a-107">Zulu 12.1 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-107">Zulu 12.1 and later</span></span>
* <span data-ttu-id="0f28a-108">Zulu 11.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-108">Zulu 11.0 and later</span></span>
* <span data-ttu-id="0f28a-109">Zulu 8u202 (8.36) 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-109">Zulu 8u202 (8.36) and later</span></span>
* <span data-ttu-id="0f28a-110">Oracle OpenJDK 11 和 15 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-110">Oracle OpenJDK 11 and 15 and later</span></span>
* <span data-ttu-id="0f28a-111">Oracle Java 11.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-111">Oracle Java 11.0 and later</span></span>
* <span data-ttu-id="0f28a-112">Oracle Java 8.0 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0f28a-112">Oracle Java 8.0 and later</span></span>

## <a name="install-zulu-mission-control-and-connect-to-a-jvm"></a><span data-ttu-id="0f28a-113">安装 Zulu Mission Control 并连接到 JVM</span><span class="sxs-lookup"><span data-stu-id="0f28a-113">Install Zulu Mission Control and connect to a JVM</span></span>

<span data-ttu-id="0f28a-114">若要安装 Zulu Mission Control，连接到 Java 虚拟机 (JVM) 并实时查看正在运行的应用程序的所有方面，请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="0f28a-114">To install Zulu Mission Control, connect to a Java Virtual Machine (JVM), and gain real-time visibility into all aspects of a running application, do the following:</span></span>

1.  <span data-ttu-id="0f28a-115">[安装兼容 Zulu Mission Control 的 JDK 和 JRE](java-jdk-install.md)。</span><span class="sxs-lookup"><span data-stu-id="0f28a-115">[Install a Zulu Mission Control-compatible JDK and JRE](java-jdk-install.md).</span></span>

1.  <span data-ttu-id="0f28a-116">[下载 Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/)，选择适用于系统的版本，将其保存到本地，然后转到相应的目录。</span><span class="sxs-lookup"><span data-stu-id="0f28a-116">[Download Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/), choose the appropriate version for your system, save it locally, and change to that directory.</span></span>

1.  <span data-ttu-id="0f28a-117">展开下载的文件。</span><span class="sxs-lookup"><span data-stu-id="0f28a-117">Expand the downloaded file.</span></span>

    <span data-ttu-id="0f28a-118">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="0f28a-118">**Linux:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    <span data-ttu-id="0f28a-119">**Windows**：</span><span class="sxs-lookup"><span data-stu-id="0f28a-119">**Windows:**</span></span>

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    <span data-ttu-id="0f28a-120">**macOS：**</span><span class="sxs-lookup"><span data-stu-id="0f28a-120">**macOS:**</span></span>

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

1.  <span data-ttu-id="0f28a-121">使用兼容的 JDK 之一启动 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="0f28a-121">Start your Java application by using one of the compatible JDKs.</span></span> <span data-ttu-id="0f28a-122">例如：</span><span class="sxs-lookup"><span data-stu-id="0f28a-122">For example:</span></span>

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

1.  <span data-ttu-id="0f28a-123">启动 Zulu Mission Control。</span><span class="sxs-lookup"><span data-stu-id="0f28a-123">Start Zulu Mission Control.</span></span>

    <span data-ttu-id="0f28a-124">**Linux：**</span><span class="sxs-lookup"><span data-stu-id="0f28a-124">**Linux:**</span></span>

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    <span data-ttu-id="0f28a-125">**Windows**：</span><span class="sxs-lookup"><span data-stu-id="0f28a-125">**Windows:**</span></span>

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    <span data-ttu-id="0f28a-126">**macOS：**</span><span class="sxs-lookup"><span data-stu-id="0f28a-126">**macOS:**</span></span>

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

1.  <span data-ttu-id="0f28a-127">（可选）切换针对 Mission Control 的 JVM 安装。</span><span class="sxs-lookup"><span data-stu-id="0f28a-127">(Optional) Switch the JVM installation for Mission Control.</span></span>

    <span data-ttu-id="0f28a-128">在 Windows 设备上，*zmc.exe* 使用在注册表中配置的默认 JVM 安装。</span><span class="sxs-lookup"><span data-stu-id="0f28a-128">On Windows devices, *zmc.exe* uses the default JVM installation that's configured in the registry.</span></span> <span data-ttu-id="0f28a-129">必须通过完整的 JDK 启动 Zulu Mission Control，否则无法自动检测本地 JVM 实例。</span><span class="sxs-lookup"><span data-stu-id="0f28a-129">Zulu Mission Control must be launched from a full JDK to be able to detect local JVM instances automatically.</span></span> <span data-ttu-id="0f28a-130">如果安装为 JRE，则不会检测到 JVM，你会收到以下警告：</span><span class="sxs-lookup"><span data-stu-id="0f28a-130">If the installation is a JRE, no JVM will be detected, and you will receive the following warning:</span></span>

    > [!div class="mx-imgBorder"]
    <span data-ttu-id="0f28a-131">![在 JDK 安装仅为 JRE 的情况下出现的警告](../media/jdk/azul-jfr-1.png)</span><span class="sxs-lookup"><span data-stu-id="0f28a-131">![Warning if JDK installation is JRE-only](../media/jdk/azul-jfr-1.png)</span></span>

    <span data-ttu-id="0f28a-132">若要更改 Mission Control 使用的 JVM，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="0f28a-132">To change the JVM that's used by Mission Control, do the following:</span></span> 

    <span data-ttu-id="0f28a-133">a.</span><span class="sxs-lookup"><span data-stu-id="0f28a-133">a.</span></span> <span data-ttu-id="0f28a-134">打开 *zmc.exe* 文件所在目录中的 *zmc.ini* 配置文件。</span><span class="sxs-lookup"><span data-stu-id="0f28a-134">Open the *zmc.ini* configuration file, which is in the same directory as the *zmc.exe* file.</span></span>

    <span data-ttu-id="0f28a-135">b.</span><span class="sxs-lookup"><span data-stu-id="0f28a-135">b.</span></span> <span data-ttu-id="0f28a-136">在 `-vmargs` 行之前添加两行：</span><span class="sxs-lookup"><span data-stu-id="0f28a-136">Before the line `-vmargs`, add two lines:</span></span>  

       * <span data-ttu-id="0f28a-137">在第一行中输入 `–vm`。</span><span class="sxs-lookup"><span data-stu-id="0f28a-137">On the first line, enter `–vm`.</span></span>  
       * <span data-ttu-id="0f28a-138">在第二行中输入 JDK 安装的路径（例如 `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`）。</span><span class="sxs-lookup"><span data-stu-id="0f28a-138">On the second line, enter the path to your JDK installation (for example, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).</span></span>

1.  <span data-ttu-id="0f28a-139">执行以下操作，找到正在运行应用程序的 JVM：</span><span class="sxs-lookup"><span data-stu-id="0f28a-139">Locate the JVM that's running your application by doing the following:</span></span>

    <span data-ttu-id="0f28a-140">a.</span><span class="sxs-lookup"><span data-stu-id="0f28a-140">a.</span></span> <span data-ttu-id="0f28a-141">在 Zulu Mission Control 窗口的左窗格中，选择“JVM 浏览器”选项卡。 </span><span class="sxs-lookup"><span data-stu-id="0f28a-141">In the left pane of the Zulu Mission Control window, select the **JVM Browser** tab.</span></span>

    <span data-ttu-id="0f28a-142">b.</span><span class="sxs-lookup"><span data-stu-id="0f28a-142">b.</span></span> <span data-ttu-id="0f28a-143">在列表中，选择并展开正在运行应用程序的 JVM 实例。</span><span class="sxs-lookup"><span data-stu-id="0f28a-143">In the list, select and expand the JVM instance that's running your application.</span></span>

    ![展开的列表中的 JVM 实例](../media/jdk/azul-jfr-2.png)


1.  <span data-ttu-id="0f28a-145">必要时启动飞行记录。</span><span class="sxs-lookup"><span data-stu-id="0f28a-145">Start a flight recording, if necessary.</span></span>

    <span data-ttu-id="0f28a-146">a.</span><span class="sxs-lookup"><span data-stu-id="0f28a-146">a.</span></span> <span data-ttu-id="0f28a-147">在左窗格的 **Flight Recorder** 下，如果显示“没有记录”消息，请启动记录，方法是：右键单击“Flight Recorder”，然后选择“启动飞行记录”。   </span><span class="sxs-lookup"><span data-stu-id="0f28a-147">In the left pane, under **Flight Recorder**, if a *No Recordings* message is displayed, start a recording by right-clicking **Flight Recorder** and then selecting **Start Flight Recording**.</span></span>

    <span data-ttu-id="0f28a-148">b.</span><span class="sxs-lookup"><span data-stu-id="0f28a-148">b.</span></span> <span data-ttu-id="0f28a-149">选择“固定时长记录”或“持续记录”，接着选择“分析”配置（精细）或“持续”配置（降低开销），然后选择“完成”。     </span><span class="sxs-lookup"><span data-stu-id="0f28a-149">Select either **Time fixed recording** or **Continuous recording**, and either a **Profiling** configuration (fine-grained) or a **Continuous** configuration (lower overhead), and then select **Finish**.</span></span>

    ![启动飞行记录](../media/jdk/azul-jfr-3.png)

    <span data-ttu-id="0f28a-151">飞行记录应该显示在 JVM 浏览器中 **Flight Recorder** 行的下面。</span><span class="sxs-lookup"><span data-stu-id="0f28a-151">A flight recording should appear below the **Flight Recorder** line in the JVM browser.</span></span>

1. <span data-ttu-id="0f28a-152">转储飞行记录。</span><span class="sxs-lookup"><span data-stu-id="0f28a-152">Dump the flight recording.</span></span> <span data-ttu-id="0f28a-153">为此，请右键单击表示飞行记录的行，然后选择“转储整个记录”。 </span><span class="sxs-lookup"><span data-stu-id="0f28a-153">To do so, right-click the line that represents the flight recording, and then select **Dump whole recording**.</span></span>

    <span data-ttu-id="0f28a-154">此时会在 Zulu Mission Control 窗口右侧的大窗格中显示一个新选项卡。</span><span class="sxs-lookup"><span data-stu-id="0f28a-154">A new tab appears in the large pane on the right side of the Zulu Mission Control window.</span></span> <span data-ttu-id="0f28a-155">此窗格表示刚从运行应用程序的 JVM 转储的飞行记录。</span><span class="sxs-lookup"><span data-stu-id="0f28a-155">This pane represents the flight recording that was just dumped from the JVM that's running your application.</span></span>

1. <span data-ttu-id="0f28a-156">使用 Zulu Mission Control 检查飞行记录。</span><span class="sxs-lookup"><span data-stu-id="0f28a-156">Examine the flight recording by using Zulu Mission Control.</span></span> <span data-ttu-id="0f28a-157">为此，请在 Zulu Mission Control 窗口的左窗格中选择“大纲”选项卡。 </span><span class="sxs-lookup"><span data-stu-id="0f28a-157">To do so, select the **Outline** tab in the left pane of the Zulu Mission Control window.</span></span> <span data-ttu-id="0f28a-158">此选项卡显示在飞行记录中收集的数据的各种视图。</span><span class="sxs-lookup"><span data-stu-id="0f28a-158">This tab displays various views of the data that's collected in the flight recording.</span></span>
 
    ![查看飞行记录](../media/jdk/azul-jfr-4.png)

## <a name="resources"></a><span data-ttu-id="0f28a-160">资源</span><span class="sxs-lookup"><span data-stu-id="0f28a-160">Resources</span></span>

<span data-ttu-id="0f28a-161">若要了解详细信息，请访问 Azul Systems 站点并观看 [Azul Webinar:Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/)（Azul 网络研讨会：开源 Flight Recorder 和 Mission Control - 管理和度量 OpenJDK 8 性能）。</span><span class="sxs-lookup"><span data-stu-id="0f28a-161">To learn more, go to the Azul Systems site and view [Azul Webinar: Open Source Flight Recorder and Mission Control - Managing and Measuring OpenJDK 8 Performance](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/).</span></span> <span data-ttu-id="0f28a-162">此视频内容由 Azul Systems 副 CTO Simon Ritter 讲述。</span><span class="sxs-lookup"><span data-stu-id="0f28a-162">The video is narrated by Azul Systems Deputy CTO Simon Ritter.</span></span> <span data-ttu-id="0f28a-163">Flight Recorder 讨论在 31:30 开始。</span><span class="sxs-lookup"><span data-stu-id="0f28a-163">The Flight Recorder discussion starts at 31:30.</span></span>

