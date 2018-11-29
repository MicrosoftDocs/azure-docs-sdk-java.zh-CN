---
title: 针对 Azure 开发的 Java JDK 和长期支持
description: 开发和运行 Java 应用程序所需的 Azure 支持的下载内容和声明。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 11/13/2018
ms.author: routlaw
ms.openlocfilehash: 0ced17eb17c9d2eda7b1fcc12ffff27b303e8d7e
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339031"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a><span data-ttu-id="3924b-103">在针对 Azure 进行开发时获取 Java JDK 下载内容和支持</span><span class="sxs-lookup"><span data-stu-id="3924b-103">Get Java JDK downloads and support when developing for Azure</span></span>

<span data-ttu-id="3924b-104">Azure 和 Azure Stack 的 Java 开发人员可以使用[适用于 Azure 的 Azul Zulu Enterprise](https://www.azul.com/downloads/azure-only/zulu/) 生成并运行生产性 Java 应用程序，而不会产生额外的支持费用。</span><span class="sxs-lookup"><span data-stu-id="3924b-104">Java developers on Azure and Azure Stack can build and run production Java applications using [Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) without incurring additional support costs.</span></span> <span data-ttu-id="3924b-105">可以在 Azure 上根据需要使用任何 Java 运行时，但在使用 Zulu 时，可以获得免费的维护更新，还可以通过[符合条件的 Azure 支持计划](https://azure.microsoft.com/support/plans/)向 Microsoft 提交支持问题。</span><span class="sxs-lookup"><span data-stu-id="3924b-105">You can use any Java runtime you want on Azure, but when you use Zulu you get free maintenance updates and can create support issues with Microsoft with a  [qualified Azure support plan](https://azure.microsoft.com/support/plans/).</span></span>

<span data-ttu-id="3924b-106">开发人员可以使用自己的 Java 运行时（包括 Oracle JDK 和 Red Hat JDK）在 Azure 上运行其应用并连接到 Azure 服务和功能。</span><span class="sxs-lookup"><span data-stu-id="3924b-106">Developers can use their own Java runtimes, including Oracle JDK and Red Hat JDK, to run their apps on Azure and connect to Azure services and features.</span></span> <span data-ttu-id="3924b-107">Oracle Java SE 的生产版本继续可供在 Azure Windows 或 Linux 虚拟机中运行工作负载的 Java 开发人员使用。</span><span class="sxs-lookup"><span data-stu-id="3924b-107">The production edition of Oracle Java SE continues to be available to Java developers running  workloads in Azure Windows or Linux virtual machines.</span></span>

## <a name="supported-java-versions-and-update-schedule"></a><span data-ttu-id="3924b-108">支持的 Java 版本和更新计划</span><span class="sxs-lookup"><span data-stu-id="3924b-108">Supported Java versions and update schedule</span></span>

<span data-ttu-id="3924b-109">Azul Systems 将为 Java 的所有长期支持 (LTS) 版本（从 Java SE 7、8、11 开始）提供完全支持的[适用于 Microsoft Azure 的 Zulu Enterprise 内部版 OpenJDK](https://www.azul.com/downloads/azure-only/zulu/)。</span><span class="sxs-lookup"><span data-stu-id="3924b-109">Azul Systems will provide fully-supported [Zulu Enterprise builds of OpenJDK for Microsoft Azure](https://www.azul.com/downloads/azure-only/zulu/) for all long-term support (LTS) versions of Java, starting with Java SE 7, 8, and 11.</span></span> <span data-ttu-id="3924b-110">详见 [Azul 新闻稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)。</span><span class="sxs-lookup"><span data-stu-id="3924b-110">More information can be found in the [Azul press release](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack).</span></span>


<span data-ttu-id="3924b-111">这些 JDK 发布版本会有季度安全更新和 Bug 修复，并根据需要提供关键的带外更新和修补程序。</span><span class="sxs-lookup"><span data-stu-id="3924b-111">These JDK releases will have quarterly security updates and bug fixes as well as critical out-of-band updates and patches as needed.</span></span>  <span data-ttu-id="3924b-112">此支持包括后向移植在新版 Java（例如 Java 11）中报告的针对 Java 7 和 8 的安全更新和 Bug 修复，确保旧版 Java 的持续稳定性和安全性。</span><span class="sxs-lookup"><span data-stu-id="3924b-112">This support includes back porting of security updates and bug fixes to Java 7 and 8 reported in newer versions of Java such as Java 11, and ensures the continued stability and security of older versions of Java.</span></span>  <span data-ttu-id="3924b-113">Azure 客户有资格使用这些安全更新和平台 Bug 修复，不需支付任何计划外 Java SE 订阅费用。</span><span class="sxs-lookup"><span data-stu-id="3924b-113">Azure customers are entitled to these security updates and platform bug fixes without incurring any unplanned Java SE subscription fees.</span></span> <span data-ttu-id="3924b-114">下图突出显示了每个版本的 Java SE 的支持日期。</span><span class="sxs-lookup"><span data-stu-id="3924b-114">The dates of support for each version of Java SE are highlighted in the image below.</span></span>

![适用于 Azure 的 JDK 支持时间线](media/azure-jdk-support.png)

<span data-ttu-id="3924b-116">Azul Systems 为这些版本保留了一个 [Java SE 路线图](https://www.azul.com/products/azul_support_roadmap/)。</span><span class="sxs-lookup"><span data-stu-id="3924b-116">Azul Systems maintains a [Java SE roadmap](https://www.azul.com/products/azul_support_roadmap/) for these releases.</span></span>

## <a name="use-for-local-development"></a><span data-ttu-id="3924b-117">用于本地开发</span><span class="sxs-lookup"><span data-stu-id="3924b-117">Use for local development</span></span> 

<span data-ttu-id="3924b-118">开发人员可以[下载](https://www.azul.com/downloads/azure-only/zulu/)用于 Azure 和 Azure Stack 的 Java JDK，以便在本地开发环境中使用。</span><span class="sxs-lookup"><span data-stu-id="3924b-118">Developers can [download](https://www.azul.com/downloads/azure-only/zulu/) Java JDKs for Azure and Azure Stack for use in local devlopment environments.</span></span> <span data-ttu-id="3924b-119">下载内容适用于 Windows、Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="3924b-119">Downloads are available for Windows, Linux, and macOS.</span></span> <span data-ttu-id="3924b-120">在 Linux 上工作的开发人员也可通过 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 包管理器获取包。</span><span class="sxs-lookup"><span data-stu-id="3924b-120">Developers working on Linux can also get packages through the  [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) and [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) package managers.</span></span>

<span data-ttu-id="3924b-121">开发 Azure 或 Azure Stack 时，对 JDK 本地开发的支持随[符合条件的 Azure 支持计划](https://azure.microsoft.com/support/plans/)提供。</span><span class="sxs-lookup"><span data-stu-id="3924b-121">Support for local development for the JDK is available with a [qualified Azure support plan](https://azure.microsoft.com/support/plans/) when developing for Azure or Azure Stack.</span></span>

## <a name="use-in-docker-containers"></a><span data-ttu-id="3924b-122">在 Docker 容器中使用</span><span class="sxs-lookup"><span data-stu-id="3924b-122">Use in Docker containers</span></span>

<span data-ttu-id="3924b-123">可以使用 OpenJDK 的 Zulu Enterprise 内部版本在所选的任何发行版上生成不受限制的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="3924b-123">You can build unlimited Docker images using Zulu Enterprise builds of OpenJDK on any distros of your choice.</span></span> <span data-ttu-id="3924b-124">Zulu Docker 映像基于适用于 Azure JDK 的 Azul Zulu Enterprise，在 [Microsoft 的公共 Docker 存储库](https://hub.docker.com/r/microsoft/java-jdk/)中提供。</span><span class="sxs-lookup"><span data-stu-id="3924b-124">Zulu Docker images based off the Azul Zulu Enterprise for Azure JDKs are available on [Microsoft's public Docker repository](https://hub.docker.com/r/microsoft/java-jdk/).</span></span> <span data-ttu-id="3924b-125">用于生成这些映像的 Dockerfile 在 [Microsoft 的 Java GitHub 存储库](https://github.com/Microsoft/java/tree/master/docker)中提供。</span><span class="sxs-lookup"><span data-stu-id="3924b-125">The  Dockerfiles that used to build these images are available on [Microsoft's Java GitHub repo](https://github.com/Microsoft/java/tree/master/docker).</span></span>

<span data-ttu-id="3924b-126">若要使用这些映像将应用容器化，需在 Dockerfile 中设置一个 `FROM` 语句，然后根据应用程序的依赖关系配置容器。</span><span class="sxs-lookup"><span data-stu-id="3924b-126">To containerize your apps using these images, you will need to set a `FROM` statement in your Dockerfile and then configure the container with your application's dependencies.</span></span> <span data-ttu-id="3924b-127">例如，若要运行一个 JAR 文件打包的绑定到端口 8080 的 Java SE 应用程序，请执行以下代码：</span><span class="sxs-lookup"><span data-stu-id="3924b-127">For example, to run a JAR file packaged Java SE application that binds to port 8080:</span></span>

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a><span data-ttu-id="3924b-128">Azure 服务运行时支持</span><span class="sxs-lookup"><span data-stu-id="3924b-128">Azure service runtime support</span></span>

<span data-ttu-id="3924b-129">Azure 平台服务（例如[应用服务](/azure/app-service/containers/)、[Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/) 和 [HDInsight](/azure/hdinsight/)）可将 OpenJDK 的 Zulu Enterprise 内部版本（内置 Java 次要版本自动修补功能）与与安全修补程序和 Bug 修复配合使用。</span><span class="sxs-lookup"><span data-stu-id="3924b-129">Azure platform services such as [App Service](/azure/app-service/containers/), [Functions](/azure/azure-functions/functions-create-first-java-maven), [Service Fabric](/azure/service-fabric/) and [HDInsight](/azure/hdinsight/)  use Zulu Enterprise builds of OpenJDK with built-in auto-patching of minor releases of Java with security patches and bug fixes.</span></span>