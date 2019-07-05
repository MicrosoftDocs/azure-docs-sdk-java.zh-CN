---
title: 针对 Azure 开发的 Java JDK 和长期支持
description: 本文提供开发和运行 Java 应用程序所需的 Azure 支持的下载链接和声明。
author: bmitchell287
manager: douge
ms.devlang: java
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: brendm
ms.openlocfilehash: 1bb93f775686b5b0f127c586dda802f4eb5f775d
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533637"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a><span data-ttu-id="42e41-103">针对 Azure 和 Azure Stack 的 Java 长期支持</span><span class="sxs-lookup"><span data-stu-id="42e41-103">Java long-term support for Azure and Azure Stack</span></span>

<span data-ttu-id="42e41-104">作为 Microsoft Azure 和 Azure Stack 的 Java 开发人员，你可以使用[适用于 Azure 的 Azul Zulu Enterprise](https://www.azul.com/downloads/azure-only/zulu/) 生成并运行生产性 Java 应用程序，而不会产生额外的支持费用。</span><span class="sxs-lookup"><span data-stu-id="42e41-104">As a Java developer on Microsoft Azure and Azure Stack, you can build and run production Java applications by using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="42e41-105">可以在 Azure 上根据需要使用任何 Java 运行时，但在使用 Zulu 时，可以获得免费的维护更新，还可以通过 Microsoft 解决支持问题。</span><span class="sxs-lookup"><span data-stu-id="42e41-105">You can use any Java runtime you want on Azure, but when you use Zulu, you get free maintenance updates and you can resolve support issues with Microsoft.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42e41-106">下载并安装 Java</span><span class="sxs-lookup"><span data-stu-id="42e41-106">Download and install Java</span></span>](java-jdk-install.md)

## <a name="long-term-support"></a><span data-ttu-id="42e41-107">长期支持</span><span class="sxs-lookup"><span data-stu-id="42e41-107">Long-term support</span></span>

* [<span data-ttu-id="42e41-108">Java 11</span><span class="sxs-lookup"><span data-stu-id="42e41-108">Java 11</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java11)
* [<span data-ttu-id="42e41-109">Java 8</span><span class="sxs-lookup"><span data-stu-id="42e41-109">Java 8</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java8)
* [<span data-ttu-id="42e41-110">Java 7</span><span class="sxs-lookup"><span data-stu-id="42e41-110">Java 7</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java7)

## <a name="technical-preview"></a><span data-ttu-id="42e41-111">Technical Preview</span><span class="sxs-lookup"><span data-stu-id="42e41-111">Technical preview</span></span>

* [<span data-ttu-id="42e41-112">Java 12</span><span class="sxs-lookup"><span data-stu-id="42e41-112">Java 12</span></span>](https://www.azul.com/downloads/azure-only/zulu/#java12)

## <a name="what-is-the-zulu-openjdk-for-azure"></a><span data-ttu-id="42e41-113">适用于 Azure 的 Zulu OpenJDK 是什么？</span><span class="sxs-lookup"><span data-stu-id="42e41-113">What is the Zulu OpenJDK for Azure?</span></span>

<span data-ttu-id="42e41-114">Azul Zulu Enterprise 内部版 OpenJDK 是适用于 Azure 和 Azure Stack 的 OpenJDK 的免费、多平台、生产就绪型发行版，由 Microsoft 及 Azul Systems 提供支持。</span><span class="sxs-lookup"><span data-stu-id="42e41-114">Azul Zulu Enterprise builds of OpenJDK are a no-cost, multi-platform, production-ready distribution of the OpenJDK for Azure and Azure Stack that's backed by Microsoft and Azul Systems.</span></span> <span data-ttu-id="42e41-115">这些发行版具有以下特点：</span><span class="sxs-lookup"><span data-stu-id="42e41-115">These distributions are:</span></span>

* <span data-ttu-id="42e41-116">是 OpenJDK 的 100% 开源内部版本，已经打包为 Java 开发工具包 (JDK)、Java Runtime Environment (JRE) 和无头 JRE。</span><span class="sxs-lookup"><span data-stu-id="42e41-116">100 percent open-source builds of OpenJDK that are packaged as Java Development Kits (JDKs), Java Runtime Environments (JREs), and Headless JREs.</span></span> <span data-ttu-id="42e41-117">这些二进制文件是 Java Standard Edition (SE) 的完全兼容且合规的商业版本，可以与 Azure 和 Azure Stack 上的 Java 应用程序或组件配合使用。</span><span class="sxs-lookup"><span data-stu-id="42e41-117">These binaries are fully compatible and compliant commercial builds of Java Standard Edition (SE) that can be used with Java applications or components on Azure and Azure Stack.</span></span>
* <span data-ttu-id="42e41-118">与长期支持（LTS，包括 Bug 修复、性能增强功能以及安全修补程序）一起提供。</span><span class="sxs-lookup"><span data-stu-id="42e41-118">Provided with long-term support (LTS), which includes bug fixes, performance enhancements, and security patches.</span></span>
* <span data-ttu-id="42e41-119">用于在 Windows、Linux 和 macOS 上开发和运行 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="42e41-119">Available for developing and running Java applications on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="42e41-120">在 Docker Hub 上以容器映像的形式提供，在 Azure 市场中以虚拟机（Windows 和 Linux）的形式提供。</span><span class="sxs-lookup"><span data-stu-id="42e41-120">Available as container images on Docker Hub and as virtual machines (Windows and Linux) in the Azure Marketplace.</span></span>
* <span data-ttu-id="42e41-121">由 Azure 用来支持许多 Azure 服务，例如：</span><span class="sxs-lookup"><span data-stu-id="42e41-121">Used by Azure to power many Azure services, such as:</span></span>
  * <span data-ttu-id="42e41-122">Azure 应用服务 (Windows)</span><span class="sxs-lookup"><span data-stu-id="42e41-122">Azure App Service (Windows)</span></span>
  * <span data-ttu-id="42e41-123">Azure 应用服务 (Linux)</span><span class="sxs-lookup"><span data-stu-id="42e41-123">Azure App Service (Linux)</span></span>
  * <span data-ttu-id="42e41-124">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="42e41-124">Azure Functions</span></span>
  * <span data-ttu-id="42e41-125">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="42e41-125">Azure Service Fabric</span></span>
  * <span data-ttu-id="42e41-126">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="42e41-126">Azure HDInsight</span></span>
  * <span data-ttu-id="42e41-127">Azure 搜索</span><span class="sxs-lookup"><span data-stu-id="42e41-127">Azure Search</span></span>
  * <span data-ttu-id="42e41-128">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="42e41-128">Azure DevOps</span></span>
  * <span data-ttu-id="42e41-129">Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="42e41-129">Azure Cloud Shell</span></span>  

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="42e41-130">支持的 Java 版本和更新计划</span><span class="sxs-lookup"><span data-stu-id="42e41-130">Supported Java versions and update schedule</span></span>

<span data-ttu-id="42e41-131">Azul Systems 为 Java 的所有 LTS 版本（从 Java SE 7、8、11 开始）提供完全支持的[适用于 Azure 的 Zulu Enterprise 内部版 OpenJDK](https://www.azul.com/downloads/azure-only/zulu/)。</span><span class="sxs-lookup"><span data-stu-id="42e41-131">Azul Systems provides fully supported [Zulu Enterprise builds of OpenJDK for Azure](https://www.azul.com/downloads/azure-only/zulu/) for all LTS versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="42e41-132">有关详细信息，请参阅 [Azul 新闻稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)。</span><span class="sxs-lookup"><span data-stu-id="42e41-132">For more information, see the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>

|<span data-ttu-id="42e41-133">Java SE LTS</span><span class="sxs-lookup"><span data-stu-id="42e41-133">Java SE LTS</span></span>  |<span data-ttu-id="42e41-134">支持截止时间</span><span class="sxs-lookup"><span data-stu-id="42e41-134">Support until</span></span>  |
|---------|----------|
|<span data-ttu-id="42e41-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span><span class="sxs-lookup"><span data-stu-id="42e41-135">[![Java 7](../media/jdk/java-7.png)](https://www.azul.com/downloads/azure-only/zulu/#java7)</span></span> |<span data-ttu-id="42e41-136">2023 年 7 月</span><span class="sxs-lookup"><span data-stu-id="42e41-136">July 2023</span></span> |
|<span data-ttu-id="42e41-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span><span class="sxs-lookup"><span data-stu-id="42e41-137">[![Java 8](../media/jdk/java-8.png)](https://www.azul.com/downloads/azure-only/zulu/#java8)</span></span> |<span data-ttu-id="42e41-138">2025 年 3 月</span><span class="sxs-lookup"><span data-stu-id="42e41-138">March 2025</span></span>|
|<span data-ttu-id="42e41-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span><span class="sxs-lookup"><span data-stu-id="42e41-139">[![Java 11](../media/jdk/java-11.png)](https://www.azul.com/downloads/azure-only/zulu/#java11)</span></span> |<span data-ttu-id="42e41-140">2026 年 9 月</span><span class="sxs-lookup"><span data-stu-id="42e41-140">September 2026</span></span>|
|<span data-ttu-id="42e41-141">[![Java 12](../media/jdk/java-12.png)]()</span><span class="sxs-lookup"><span data-stu-id="42e41-141">[![Java 12](../media/jdk/java-12.png)]()</span></span> |<span data-ttu-id="42e41-142">**预览**</span><span class="sxs-lookup"><span data-stu-id="42e41-142">**Preview**</span></span>|

<span data-ttu-id="42e41-143">这些 JDK 发布版本有季度安全更新和 Bug 修复，并根据需要提供关键的带外更新和修补程序。</span><span class="sxs-lookup"><span data-stu-id="42e41-143">These JDK releases have quarterly security updates, bug fixes, and critical out-of-band updates and patches as needed.</span></span> <span data-ttu-id="42e41-144">此支持包括后向移植在新版 Java（例如 Java 11）中报告的针对 Java 7 和 8 的安全更新和 Bug 修复。</span><span class="sxs-lookup"><span data-stu-id="42e41-144">This support includes back ports of security updates and bug fixes to Java 7 and 8 that are reported in newer versions of Java, such as Java 11.</span></span> <span data-ttu-id="42e41-145">此支持可确保旧版 Java 的持续稳定性和安全性。</span><span class="sxs-lookup"><span data-stu-id="42e41-145">The support ensures the continued stability and security of older versions of Java.</span></span> <span data-ttu-id="42e41-146">Azure 客户可以获取这些安全更新和平台 Bug 修复，不需支付任何计划外 Java SE 订阅费用。</span><span class="sxs-lookup"><span data-stu-id="42e41-146">Azure customers can get these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span>

<span data-ttu-id="42e41-147">Azul Systems 为这些版本保留了一个 [Java SE 路线图](https://www.azul.com/products/azul_support_roadmap/)。</span><span class="sxs-lookup"><span data-stu-id="42e41-147">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="benefits-for-developers"></a><span data-ttu-id="42e41-148">为开发人员带来的好处</span><span class="sxs-lookup"><span data-stu-id="42e41-148">Benefits for developers</span></span>

<span data-ttu-id="42e41-149">Azul Zulu JDK 发布版本具有以下特点：</span><span class="sxs-lookup"><span data-stu-id="42e41-149">The Azul Zulu JDK releases are:</span></span>

* <span data-ttu-id="42e41-150">由 Microsoft 和 Azul Systems 提供支持。</span><span class="sxs-lookup"><span data-stu-id="42e41-150">Backed and supported by both Microsoft and Azul Systems.</span></span>

   * <span data-ttu-id="42e41-151">Zulu 二进制文件已做好生产准备，由 Microsoft 和 Azul Systems 提供支持。</span><span class="sxs-lookup"><span data-stu-id="42e41-151">Zulu binaries are production ready and backed by Microsoft and Azul Systems.</span></span>
   * <span data-ttu-id="42e41-152">Zulu 为 Java 7、8、11 提供免费 LTS（也会为 Java 17 提供 LTS）。</span><span class="sxs-lookup"><span data-stu-id="42e41-152">Zulu comes with zero-cost LTS for Java 7, 8, and 11 (LTS will be provided for Java 17, as well).</span></span> <span data-ttu-id="42e41-153">可以只在需要的时候升级 Java 版本。</span><span class="sxs-lookup"><span data-stu-id="42e41-153">You can upgrade Java versions only when you need to.</span></span>
   * <span data-ttu-id="42e41-154">Java 7 的支持截止时间是 2023 年 7 月。</span><span class="sxs-lookup"><span data-stu-id="42e41-154">Java 7 is supported until July 2023.</span></span> <span data-ttu-id="42e41-155">在 2024 年之后仍会支持 Java 8 和 11。</span><span class="sxs-lookup"><span data-stu-id="42e41-155">Java 8 and 11 are supported beyond 2024.</span></span>
   * <span data-ttu-id="42e41-156">Microsoft 致力于在为许多 Azure 服务提供支持的计算机上以内部方式运行 Zulu。</span><span class="sxs-lookup"><span data-stu-id="42e41-156">Microsoft is committed to running Zulu internally on machines that power many Azure services.</span></span>

* <span data-ttu-id="42e41-157">生产就绪。</span><span class="sxs-lookup"><span data-stu-id="42e41-157">Production-ready.</span></span>

   * <span data-ttu-id="42e41-158">100% 开源（就其内部版 OpenJDK 来说）。</span><span class="sxs-lookup"><span data-stu-id="42e41-158">100 percent open-source for its builds of OpenJDK.</span></span>
   * <span data-ttu-id="42e41-159">可随时替换许多 Java SE 发行版。</span><span class="sxs-lookup"><span data-stu-id="42e41-159">Drop-in replacements for many Java SE distributions.</span></span>
   * <span data-ttu-id="42e41-160">JDK、JRE 和 JRE-headless。</span><span class="sxs-lookup"><span data-stu-id="42e41-160">JDK, JRE, and JRE-headless.</span></span>
   * <span data-ttu-id="42e41-161">Java 7、8 和 11。</span><span class="sxs-lookup"><span data-stu-id="42e41-161">Java 7, 8, and 11.</span></span>
   * <span data-ttu-id="42e41-162">经验证符合使用 OpenJDK 社区技术兼容性工具包 (TCK) 的 Java SE 规范。</span><span class="sxs-lookup"><span data-stu-id="42e41-162">Verified compliant with the Java SE specifications that use the OpenJDK Community Technology Compatibility Kit (TCK).</span></span>
   * <span data-ttu-id="42e41-163">作为开发人员，你会继续收到 Java SE 的生产更新，包括 Java SE 7、8 和 11 的 Bug 修复、性能增强功能以及安全修补程序。</span><span class="sxs-lookup"><span data-stu-id="42e41-163">As a developer, you continue to receive production updates for Java SE, including bug fixes, performance enhancements, and security patches for Java SE 7, 8, and 11.</span></span>

* <span data-ttu-id="42e41-164">多平台支持。</span><span class="sxs-lookup"><span data-stu-id="42e41-164">Supported for multi-platform.</span></span> <span data-ttu-id="42e41-165">Zulu 支持的二进制文件适用于多个平台和版本，包括：</span><span class="sxs-lookup"><span data-stu-id="42e41-165">Zulu supports binaries for multiple platforms and versions, including:</span></span>

   * <span data-ttu-id="42e41-166">Windows 客户端</span><span class="sxs-lookup"><span data-stu-id="42e41-166">Windows client</span></span>
     * <span data-ttu-id="42e41-167">10</span><span class="sxs-lookup"><span data-stu-id="42e41-167">10</span></span>
     * <span data-ttu-id="42e41-168">8.1</span><span class="sxs-lookup"><span data-stu-id="42e41-168">8.1</span></span>
     * <span data-ttu-id="42e41-169">8、7</span><span class="sxs-lookup"><span data-stu-id="42e41-169">8, 7</span></span>
   * <span data-ttu-id="42e41-170">Windows Server</span><span class="sxs-lookup"><span data-stu-id="42e41-170">Windows Server</span></span>
     * <span data-ttu-id="42e41-171">2016 R2</span><span class="sxs-lookup"><span data-stu-id="42e41-171">2016 R2</span></span>
     * <span data-ttu-id="42e41-172">2016</span><span class="sxs-lookup"><span data-stu-id="42e41-172">2016</span></span>
     * <span data-ttu-id="42e41-173">2012 R2</span><span class="sxs-lookup"><span data-stu-id="42e41-173">2012 R2</span></span>
     * <span data-ttu-id="42e41-174">2012</span><span class="sxs-lookup"><span data-stu-id="42e41-174">2012</span></span>
     * <span data-ttu-id="42e41-175">2008 R2</span><span class="sxs-lookup"><span data-stu-id="42e41-175">2008 R2</span></span>
   * <span data-ttu-id="42e41-176">Linux，包括</span><span class="sxs-lookup"><span data-stu-id="42e41-176">Linux, including</span></span>
     * <span data-ttu-id="42e41-177">RHEL</span><span class="sxs-lookup"><span data-stu-id="42e41-177">RHEL</span></span>
     * <span data-ttu-id="42e41-178">CentOS</span><span class="sxs-lookup"><span data-stu-id="42e41-178">CentOS</span></span>
     * <span data-ttu-id="42e41-179">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="42e41-179">Ubuntu</span></span>
     * <span data-ttu-id="42e41-180">SLES</span><span class="sxs-lookup"><span data-stu-id="42e41-180">SLES</span></span>
     * <span data-ttu-id="42e41-181">Debian</span><span class="sxs-lookup"><span data-stu-id="42e41-181">Debian</span></span>
     * <span data-ttu-id="42e41-182">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="42e41-182">Oracle Linux</span></span>
   * <span data-ttu-id="42e41-183">macOS X</span><span class="sxs-lookup"><span data-stu-id="42e41-183">macOS X</span></span>
   * <span data-ttu-id="42e41-184">提供多种包类型：</span><span class="sxs-lookup"><span data-stu-id="42e41-184">Delivery in multiple package types:</span></span>
     * <span data-ttu-id="42e41-185">MSI、ZIP、TAR、DEB、RPM 和 DMG</span><span class="sxs-lookup"><span data-stu-id="42e41-185">MSI, ZIP, TAR, DEB, RPM, and DMG</span></span>

    <span data-ttu-id="42e41-186">Docker 上提供适用于 Zulu JDK、JRE 和 JRE-headless 且基于多个基 OS 映像的认证 Docker 容器映像。</span><span class="sxs-lookup"><span data-stu-id="42e41-186">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker.</span></span>

    <span data-ttu-id="42e41-187">中心：</span><span class="sxs-lookup"><span data-stu-id="42e41-187">Hub:</span></span>

    * [<span data-ttu-id="42e41-188">JDK</span><span class="sxs-lookup"><span data-stu-id="42e41-188">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
    * [<span data-ttu-id="42e41-189">JRE</span><span class="sxs-lookup"><span data-stu-id="42e41-189">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
    * [<span data-ttu-id="42e41-190">JRE-headless</span><span class="sxs-lookup"><span data-stu-id="42e41-190">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

* <span data-ttu-id="42e41-191">无需付费。</span><span class="sxs-lookup"><span data-stu-id="42e41-191">No cost.</span></span>

   * <span data-ttu-id="42e41-192">Microsoft 免费提供在 Azure 上生成和缩放 Java 应用所需的一切。</span><span class="sxs-lookup"><span data-stu-id="42e41-192">Microsoft provides everything you need to build and scale Java apps on Azure, at no cost to you.</span></span> <span data-ttu-id="42e41-193">你可以通过 Zulu 接收针对 Java 应用的免费安全更新和平台 Bug 修复，不需任何费用。</span><span class="sxs-lookup"><span data-stu-id="42e41-193">Through Zulu you'll receive free security updates and platform bug fixes for Java apps without any fees.</span></span>
   * <span data-ttu-id="42e41-194">Zulu Java 8、11 和 12（预览版）提供 [Java Flight Recorder 和 Mission Control](java-jdk-flight-recorder-and-mission-control.md)。</span><span class="sxs-lookup"><span data-stu-id="42e41-194">[Java Flight Recorder and Mission Control](java-jdk-flight-recorder-and-mission-control.md) are available in Zulu Java 8, 11, and 12 (Preview).</span></span>

* <span data-ttu-id="42e41-195">以非 LTS 版本的技术预览形式提供。</span><span class="sxs-lookup"><span data-stu-id="42e41-195">Available as tech preview of non-LTS versions.</span></span>

   * <span data-ttu-id="42e41-196">可以通过技术预览以渐进方式对短期版本中提供的新功能进行测试，这些短期版本最终会成为 Java 17 LTS。</span><span class="sxs-lookup"><span data-stu-id="42e41-196">With tech previews, you can progressively test new features as they're delivered, in short-term versions that will eventually graduate to Java 17 LTS.</span></span>

* <span data-ttu-id="42e41-197">对 OpenJDK 的更改将向上游发送。</span><span class="sxs-lookup"><span data-stu-id="42e41-197">Changes to OpenJDK are sent upstream.</span></span>

   * <span data-ttu-id="42e41-198">Azul Systems 提交者会推送针对 OpenJDK 的 Zulu 更改，充实上游存储库。</span><span class="sxs-lookup"><span data-stu-id="42e41-198">Azul Systems committers push Zulu changes to OpenJDK, which makes the upstream repo comprehensive and inclusive.</span></span>

<span data-ttu-id="42e41-199">作为 Java 开发人员，你可以一如既往地将自己的 Java 运行时（包括 Oracle JDK 和 Red Hat JDK）引入 Azure，</span><span class="sxs-lookup"><span data-stu-id="42e41-199">As always, as a Java developer, you can bring to Azure your own Java runtimes, including the Oracle JDK and the Red Hat JDK.</span></span> <span data-ttu-id="42e41-200">也可以使用安全的基础结构和功能丰富的服务。</span><span class="sxs-lookup"><span data-stu-id="42e41-200">You can also use the secure infrastructure and feature-rich services.</span></span> <span data-ttu-id="42e41-201">Oracle Java SE 的生产版本可用于在 Azure 上的 Windows 或 Linux 虚拟机中运行 Java 工作负荷。</span><span class="sxs-lookup"><span data-stu-id="42e41-201">The production edition of Oracle Java SE is available to you for running Java workloads in Windows or Linux virtual machines on Azure.</span></span>

## <a name="use-java-jdks-for-local-development"></a><span data-ttu-id="42e41-202">使用 Java JDK 进行本地开发</span><span class="sxs-lookup"><span data-stu-id="42e41-202">Use Java JDKs for local development</span></span> 

<span data-ttu-id="42e41-203">可以[下载用于 Azure 和 Azure Stack 的 Java JDK](https://www.azul.com/downloads/azure-only/zulu/)，以便在本地开发环境中使用。</span><span class="sxs-lookup"><span data-stu-id="42e41-203">You can [download Java JDKs for Azure and Azure Stack](https://www.azul.com/downloads/azure-only/zulu/) for use in your local development environments.</span></span> <span data-ttu-id="42e41-204">下载内容适用于 Windows、Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="42e41-204">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="42e41-205">如果是在 Linux 上工作，也可通过 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 包管理器获取包。</span><span class="sxs-lookup"><span data-stu-id="42e41-205">If you're working on Linux, you can also get packages through the [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="42e41-206">如需其他指南，请参阅 [Azure 的 Docker 映像](java-jdk-docker-images.md)。</span><span class="sxs-lookup"><span data-stu-id="42e41-206">For additional guidance, see [Docker images for Azure](java-jdk-docker-images.md).</span></span>
