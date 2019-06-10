---
title: 安装适用于 Azure 和 Azure Stack 的 Azul Zulu JDK
description: 如何安装 Azul Zulu Java 开发工具包 (JDK)，以便在 Windows、Linux 和 Mac 中进行 Azure 开发
author: erickson-doug
manager: douge
ms.author: douge
ms.date: 4/19/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: 33d2206f9a0a1cc9fadc60b17c1a93f66c592fe3
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568590"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a><span data-ttu-id="87d37-103">安装适用于 Azure 和 Azure Stack 的 JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-103">Install the JDK for Azure and Azure Stack</span></span>

<span data-ttu-id="87d37-104">Azul Zulu Enterprise 内部版 OpenJDK 是适用于 Azure 和 Azure Stack 的 OpenJDK 的免费、多平台、生产就绪型发行版，由 Microsoft 及 Azul Systems 提供支持。</span><span class="sxs-lookup"><span data-stu-id="87d37-104">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="87d37-105">这些版本包含构建和运行 Java SE 应用程序所需的所有组件。</span><span class="sxs-lookup"><span data-stu-id="87d37-105">They contain all the components for building and running Java SE applications.</span></span>

<span data-ttu-id="87d37-106">有[多个支持用于每个客户端 OS 的下载包类型](https://www.azul.com/downloads/azure-only/zulu/)。</span><span class="sxs-lookup"><span data-stu-id="87d37-106">There are [multiple download package types supported for each client OS](https://www.azul.com/downloads/azure-only/zulu/).</span></span> <span data-ttu-id="87d37-107">也可从以下平台的 Azure 市场库获取虚拟机映像：</span><span class="sxs-lookup"><span data-stu-id="87d37-107">You can also get a virtual machine image from the Azure Marketplace Gallery for the following platforms:</span></span>

  * [<span data-ttu-id="87d37-108">Azul Zulu：基于 Ubuntu 18.04 的 Java 8</span><span class="sxs-lookup"><span data-stu-id="87d37-108">Azul Zulu: Java 8 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-ubuntu-1804)
  * [<span data-ttu-id="87d37-109">Azul Zulu：基于 Windows Server 2019 的 Java 8</span><span class="sxs-lookup"><span data-stu-id="87d37-109">Azul Zulu: Java 8 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu8-windows-2019)
  
  * [<span data-ttu-id="87d37-110">Azul Zulu：基于 Ubuntu 18.04 的 Java 11</span><span class="sxs-lookup"><span data-stu-id="87d37-110">Azul Zulu: Java 11 on Ubuntu 18.04</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-ubuntu-1804)
  * [<span data-ttu-id="87d37-111">Azul Zulu：基于 Windows Server 2019 的 Java 11</span><span class="sxs-lookup"><span data-stu-id="87d37-111">Azul Zulu: Java 11 on Windows Server 2019</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azul.azul-zulu11-windows-2019)


> [!NOTE]
> <span data-ttu-id="87d37-112">这些说明针对 64 位 Java 8 版 JDK。</span><span class="sxs-lookup"><span data-stu-id="87d37-112">These instructions target the 64-bit Java 8 version of the JDK.</span></span> <span data-ttu-id="87d37-113">Azul 还提供 Java Run-time Environment (JRE) 作为独立安装。</span><span class="sxs-lookup"><span data-stu-id="87d37-113">Azul also provides the Java Run-time Environment (JRE) as a stand-alone installation.</span></span> <span data-ttu-id="87d37-114">此 JRE 随附在 JDK 安装中。</span><span class="sxs-lookup"><span data-stu-id="87d37-114">The JRE is included with the JDK install.</span></span>
>
>  <span data-ttu-id="87d37-115">Java 11 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。</span><span class="sxs-lookup"><span data-stu-id="87d37-115">Java 11 packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-windows"></a><span data-ttu-id="87d37-116">下载并安装 Windows 版 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-116">Download and install the Azul Zulu JDKs for Windows</span></span> 

1. <span data-ttu-id="87d37-117">[将 64 位 Azul Zulu JDK 8 作为 MSI 下载](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi)到客户端的某个位置，例如 `C:\Users\<your_login>\Downloads`。</span><span class="sxs-lookup"><span data-stu-id="87d37-117">[Download the 64-bit Azul Zulu JDK 8 as an MSI](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-win_x64.msi) to a location on your client, such as `C:\Users\<your_login>\Downloads`.</span></span> <span data-ttu-id="87d37-118">（.ZIP 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）</span><span class="sxs-lookup"><span data-stu-id="87d37-118">(.ZIP packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="87d37-119">导航到该目录，双击下载的 MSI 文件即可开始安装。</span><span class="sxs-lookup"><span data-stu-id="87d37-119">Navigate to the directory and double-click the downloaded MSI file to begin installation.</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-for-mac"></a><span data-ttu-id="87d37-120">下载并安装 Mac 版 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-120">Download and install the Azul Zulu JDKs for Mac</span></span> 

<span data-ttu-id="87d37-121">这些步骤将 ZIP 文件下载到 Mac。</span><span class="sxs-lookup"><span data-stu-id="87d37-121">These steps download a ZIP file to your Mac.</span></span> <span data-ttu-id="87d37-122">此外还提供 DMG 版本。</span><span class="sxs-lookup"><span data-stu-id="87d37-122">There is also a DMG version available.</span></span>

1. <span data-ttu-id="87d37-123">[将 64 位 Azul Zulu JDK 8 作为 ZIP 文件下载](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip)到客户端的某个位置，例如 `/Library/Java/JavaVirtualMachines/`。</span><span class="sxs-lookup"><span data-stu-id="87d37-123">[Download the 64-bit Azul Zulu JDK 8 as a ZIP file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-macosx_x64.zip) to a location on your client, such as `/Library/Java/JavaVirtualMachines/`.</span></span> <span data-ttu-id="87d37-124">（.DMG 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）</span><span class="sxs-lookup"><span data-stu-id="87d37-124">(.DMG packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="87d37-125">启动 Finder，导航到下载目录，然后双击 ZIP 文件。</span><span class="sxs-lookup"><span data-stu-id="87d37-125">Launch Finder, navigate to the download directory, and double-click the ZIP file.</span></span> <span data-ttu-id="87d37-126">也可启动终端命令窗口，然后导航到目录并运行：</span><span class="sxs-lookup"><span data-stu-id="87d37-126">Alternatively, you can launch a terminal command window, navigate to the directory, and run:</span></span>

```cli
unzip <name_of_zulu_package>.zip
```

## <a name="download-and-install-the-azul-zulu-jdks-for-alpine-linux"></a><span data-ttu-id="87d37-127">下载并安装 Alpine Linux 版 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-127">Download and install the Azul Zulu JDKs for Alpine Linux</span></span>

1. <span data-ttu-id="87d37-128">[将 64 位 Azul Zulu JDK 8 作为 TAR 文件下载](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz)到客户端的某个位置，例如 `/usr/lib/jvm`。</span><span class="sxs-lookup"><span data-stu-id="87d37-128">[Download the 64-bit Azul Zulu JDK 8 as a TAR file](https://repos.azul.com/azure-only/zulu/packages/zulu-11/11.0.3/zulu-11-azure-jdk_11.31.11-11.0.3-linux_x64.tar.gz) to a location on your client, such as `/usr/lib/jvm`.</span></span> <span data-ttu-id="87d37-129">（.RPM 和 .DEB 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）</span><span class="sxs-lookup"><span data-stu-id="87d37-129">(.RPM and .DEB packages are also provided on [Azul's Azure downloads page](https://www.azul.com/downloads/azure-only/zulu/).)</span></span>

2. <span data-ttu-id="87d37-130">转到你的目录并运行以下命令，将文件解压缩并展开：</span><span class="sxs-lookup"><span data-stu-id="87d37-130">Go to your directory and run the following command to unzip and expand the file:</span></span>

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a><span data-ttu-id="87d37-131">确认安装</span><span class="sxs-lookup"><span data-stu-id="87d37-131">Confirm your installation</span></span>

<span data-ttu-id="87d37-132">若要确认安装，请转到命令行并运行 `java -version`。</span><span class="sxs-lookup"><span data-stu-id="87d37-132">To confirm your installation, go to the command-line and run `java -version`.</span></span>

<span data-ttu-id="87d37-133">此命令的输出应该是：</span><span class="sxs-lookup"><span data-stu-id="87d37-133">The output of the command should be:</span></span>

```cli
$ java -version

openjdk version "1.8.0_212"
OpenJDK Runtime Environment (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 1.8.0_212-b04)
OpenJDK 64-Bit Server VM (Zulu 8.38.0.13-macosx)-Microsoft-Azure-restricted (build 25.212-b04, mixed mode)

```

## <a name="download-and-install-the-azul-zulu-jdks-from-a-yum-repository"></a><span data-ttu-id="87d37-134">下载并安装 Yum 存储库中的 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-134">Download and install the Azul Zulu JDKs from a Yum repository</span></span>

<span data-ttu-id="87d37-135">Azul Zulu JDK 在 [Yum 存储库](http://repos.azul.com/azure-only/zulu-azure.repo)中由 Azul 提供。</span><span class="sxs-lookup"><span data-stu-id="87d37-135">The Azul Zulu JDKs are provided in a [Yum repository](http://repos.azul.com/azure-only/zulu-azure.repo) by Azul.</span></span>

<span data-ttu-id="87d37-136">**若要安装适用于 Java 8 的 Azul Zulu JDK，请在 CLI 中运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-136">**To install the Azul Zulu JDK for Java 8, run the following commands from your CLI:**</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="87d37-137">对于 Java 11，请运行：</span><span class="sxs-lookup"><span data-stu-id="87d37-137">For Java 11, run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

<span data-ttu-id="87d37-138">对于 Java 12（预览版），请运行：</span><span class="sxs-lookup"><span data-stu-id="87d37-138">For Java 12 (Preview), run:</span></span>

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-12-azure-jdk
```

<span data-ttu-id="87d37-139">**若要更新 Yum 存储库中的 Zulu JDK 8 包，请运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-139">**To update a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

<span data-ttu-id="87d37-140">（如果使用版本 11 或 12，请更改以上命令中的版本号。）</span><span class="sxs-lookup"><span data-stu-id="87d37-140">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="87d37-141">**若要删除 Yum 存储库中的 Zulu JDK 8 包，请运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-141">**To remove a Zulu JDK 8 package from a Yum repository:**</span></span>

```cli
sudo yum -y erase zulu-8-azure-jdk
```
<span data-ttu-id="87d37-142">（如果使用版本 11 或 12，请更改以上命令中的版本号。）</span><span class="sxs-lookup"><span data-stu-id="87d37-142">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a><span data-ttu-id="87d37-143">下载并安装 apt-get 存储库中的 Azul Zulu JDK</span><span class="sxs-lookup"><span data-stu-id="87d37-143">Download and install the Azul Zulu JDKs from an apt-get repository</span></span>

<span data-ttu-id="87d37-144">Azul Zulu JDK 也在 [apt-get 存储库](http://repos.azul.com/azure-only/zulu/apt)中由 Azul 提供。</span><span class="sxs-lookup"><span data-stu-id="87d37-144">The Azul Zulu JDKs are also provided in an [apt-get repository](http://repos.azul.com/azure-only/zulu/apt) by Azul.</span></span>

<span data-ttu-id="87d37-145">**若要通过 apt-get 安装适用于 Java 8 的 Azul Zulu JDK，请在 CLI 中运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-145">**To install the Azul Zulu JDK for Java 8 with apt-get, run the following commands from your CLI:**</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="87d37-146">对于 Java 11，请运行：</span><span class="sxs-lookup"><span data-stu-id="87d37-146">For Java 11, run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

<span data-ttu-id="87d37-147">对于 Java 12（预览版），请运行：</span><span class="sxs-lookup"><span data-stu-id="87d37-147">For Java 12 (Preview), run:</span></span>

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-12-azure-jdk
```

<span data-ttu-id="87d37-148">**若要更新 apt-get 存储库中的 Zulu JDK 8 包，请运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-148">**To update a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

<span data-ttu-id="87d37-149">以前的版本会被自动删除。</span><span class="sxs-lookup"><span data-stu-id="87d37-149">The previous release will be automatically removed.</span></span>
<span data-ttu-id="87d37-150">（如果使用版本 11 或 12，请更改以上命令中的版本号。）</span><span class="sxs-lookup"><span data-stu-id="87d37-150">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="87d37-151">**若要删除 apt-get 存储库中的 Zulu JDK 8 包，请运行以下命令：**</span><span class="sxs-lookup"><span data-stu-id="87d37-151">**To remove a Zulu JDK 8 package from an apt-get repository:**</span></span>

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

<span data-ttu-id="87d37-152">（如果使用版本 11 或 12，请更改以上命令中的版本号。）</span><span class="sxs-lookup"><span data-stu-id="87d37-152">(Change the version number in the command above if you are using versions 11 or 12.)</span></span>

<span data-ttu-id="87d37-153">若要更详细地了解如何准备、安装和管理用于 Azure 开发的 Azul Zulu JDK，请阅读[官方 Zulu 文档](https://docs.azul.com/zulu/zuludocs/index.htm)。</span><span class="sxs-lookup"><span data-stu-id="87d37-153">For more detailed guidance on preparing, installing, and managing your Azul Zulu JDKs for Azure development, read [the official Zulu docs](https://docs.azul.com/zulu/zuludocs/index.htm).</span></span>

