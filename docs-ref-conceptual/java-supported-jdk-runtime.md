---
title: 针对 Azure 开发的 Java JDK 和长期支持
description: 开发和运行 Java 应用程序所需的 Azure 支持的下载内容和声明。
author: rloutlaw
manager: angerobe
ms.devlang: java
ms.topic: article
ms.date: 10/26/2017
ms.author: routlaw
ms.openlocfilehash: 7f75b26bffc02a161e8d58827970bd80a3a6c48a
ms.sourcegitcommit: 66f3dd4bdb09712b73c9194e23028567c0c4ee3f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235203"
---
# <a name="get-java-jdk-downloads-and-support-when-developing-for-azure"></a>在针对 Azure 进行开发时获取 Java JDK 下载内容和支持

Azure 和 Azure Stack 的 Java 开发人员可以使用[适用于 Azure 的 Azul Zulu Enterprise](https://www.azul.com/downloads/azure-only/zulu/) 生成并运行生产性 Java 应用程序，而不会产生额外的支持费用。 可以在 Azure 上根据需要使用任何 Java 运行时，但在使用 Zulu 时，可以获得免费的维护更新，还可以通过[符合条件的 Azure 支持计划](https://azure.microsoft.com/support/plans/)向 Microsoft 提交支持问题。

开发人员可以使用自己的 Java 运行时（包括 Oracle JDK 和 Red Hat JDK）在 Azure 上运行其应用并连接到 Azure 服务和功能。 Oracle Java SE 的生产版本继续可供在 Azure Windows 或 Linux 虚拟机中运行工作负载的 Java 开发人员使用。

## <a name="supported-java-versions-and-update-schedule"></a>支持的 Java 版本和更新计划

Azul Systems 将为 Java 的所有长期支持 (LTS) 版本（从 Java SE 7、8、11 开始）提供完全支持的[适用于 Microsoft Azure 的 Zulu Enterprise 内部版 OpenJDK](https://www.azul.com/downloads/azure-only/zulu/)。 详见 [Azul 新闻稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)。


这些 JDK 发布版本会有季度安全更新和 Bug 修复，并根据需要提供关键的带外更新和修补程序。  此支持包括后向移植在新版 Java（例如 Java 11）中报告的针对 Java 7 和 8 的安全更新和 Bug 修复，确保旧版 Java 的持续稳定性和安全性。  Azure 客户有资格使用这些安全更新和平台 Bug 修复，不需支付任何计划外 Java SE 订阅费用。 下图突出显示了每个版本的 Java SE 的支持日期。

![适用于 Azure 的 JDK 支持时间线](media/azure-jdk-support.png)

Azul Systems 为这些版本保留了一个 [Java SE 路线图](https://www.azul.com/products/azul_support_roadmap/)。

## <a name="use-for-local-development"></a>用于本地开发 

开发人员可以[下载](https://www.azul.com/downloads/azure-only/zulu/)用于 Azure 和 Azure Stack 的 Java JDK，以便在本地开发环境中使用。 下载内容适用于 Windows、Linux 和 macOS。 在 Linux 上工作的开发人员也可通过 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 包管理器获取包。

开发 Azure 或 Azure Stack 时，对 JDK 本地开发的支持随[符合条件的 Azure 支持计划](https://azure.microsoft.com/support/plans/)提供。

## <a name="use-in-docker-containers"></a>在 Docker 容器中使用

可以使用 OpenJDK 的 Zulu Enterprise 内部版本在所选的任何发行版上生成不受限制的 Docker 映像。 Zulu Docker 映像基于适用于 Azure JDK 的 Azul Zulu Enterprise，在 [Microsoft 的公共 Docker 存储库](https://hub.docker.com/r/microsoft/java-jdk/)中提供。 用于生成这些映像的 Dockerfile 在 [Microsoft 的 Java GitHub 存储库](https://github.com/Microsoft/java/tree/master/docker)中提供。

若要使用这些映像将应用容器化，需在 Dockerfile 中设置一个 `FROM` 语句，然后根据应用程序的依赖关系配置容器。 例如，若要运行一个 JAR 文件打包的绑定到端口 8080 的 Java SE 应用程序，请执行以下代码：

```Dockerfile
FROM  microsoft/java-jdk:<tag>
EXPOSE 8080
ADD target/hello.jar hello.jar
ENTRYPOINT ["java", "-jar","/hello.jar"]
```

## <a name="azure-service-runtime-support"></a>Azure 服务运行时支持

Azure 平台服务（例如[应用服务](/azure/app-service/containers/)、[Functions](/azure/azure-functions/functions-create-first-java-maven)、[Service Fabric](/azure/service-fabric/) 和 [HDInsight](/azure/hdinsight/)）可将 OpenJDK 的 Zulu Enterprise 内部版本（内置 Java 次要版本自动修补功能）与与安全修补程序和 Bug 修复配合使用。