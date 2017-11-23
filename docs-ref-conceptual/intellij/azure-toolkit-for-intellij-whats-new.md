---
title: "用于 IntelliJ 的 Azure 工具包中的新增功能"
description: "了解用于 IntelliJ 的 Azure 工具包中的最新功能。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 40d93306b840736bd7eb860933cdbfe03c82691c
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>用于 IntelliJ 的 Azure 工具包中的新增功能

## <a name="azure-toolkit-for-intellij-releases"></a>用于 IntelliJ 的 Azure 工具包版本
本文包含有关用于 IntelliJ 的 Azure 工具包的不同版本和最新更新的信息。

> [!NOTE]
> 请参阅以下网页以了解最新信息：
> 
> <https://github.com/Microsoft/azure-tools-for-java/releases>

> [!NOTE]
> 另外还有 Azure Toolkit for Eclipse IDE。 有关详细信息，请参阅[用于 Eclipse 的 Azure 工具包]。
> 
> 

### <a name="april-14-2017"></a>2017 年 4 月 14 日
用于 IntelliJ 的 Azure 工具包 - 2017 年 4 月版包含以下增强功能：

* **改进了 Azure 登录体验**：用于 IntelliJ 的 Azure 工具包现在支持以两种方式登录到 Azure 帐户：*交互式*和*自动*。 有关详细信息，请参阅[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明]。
* **使用 Docker 容器发布**：现在可以使用用于 IntelliJ 的 Azure 工具包将 Web 应用程序发布为 Docker 容器。 有关详细信息，请参阅[如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器]。
* **存储帐户管理**：用于 IntelliJ 的 Azure 工具包现在支持从 Azure 资源管理器视图管理存储帐户。 有关详细信息，请参阅[使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户]。
* **虚拟机管理**：用于 IntelliJ 的 Azure 工具包现在支持从“Azure 资源管理器”工具窗口管理虚拟机。 有关详细信息，请参阅[使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机]。
* **删除了远程调试支持**。 在 Azure 应用服务中对 Java Web 应用进行远程调试的功能已从用于 IntelliJ 的 Azure 工具包中删除；这是为了解决客户在使用该工具包时遇到的问题所必需的。

### <a name="august-26-2016"></a>2016 年 8 月 26 日
用于 IntelliJ 的 Azure 工具包 - 2016 年 8 月版包含以下增强功能：

* **自定义 JDK 分发版**。 用于 IntelliJ 的 Azure 工具包现在支持指定任意 JDK 版本并将其部署到 Azure WebApp 容器：
  * 除了 Azure 提供的 JDK 以外，还可以从 Azul Systems 在 Azure 上提供的多种 Zulu OpenJDK 版本中选择。
  * 还可以指定自己的 JDK 分发版（如果将它作为 ZIP 文件上传存储帐户）。
* **Azure 资源管理器视图增强**：
  * 使用 Azure 的新 Resource Manager 模型为虚拟机管理提供支持：可以列出、创建和删除基于 Resource Manager 的虚拟机，而无需退出 IDE。
  * 使用 Azure 的 Resource Manager 为存储帐户 Blob 管理提供支持，对管理“经典”存储帐户的现有功能做出补充。
* **Microsoft JDBC Driver 6.0 for SQL Server**。 此项更新包括适用于 Microsoft SQL Server 的最新 JDBC 驱动程序 (v6.0)，现在，该驱动程序以库的形式提供，可以轻松添加到 Java 项目，从而取代旧版本。

### <a name="june-29-2016"></a>2016 年 6 月 29 日
Azure Toolkit for IntelliJ - 2016 年 6 月版包含以下增强功能：

* **需要 Java 8**。 Azure Toolkit for IntelliJ 现在需要 Java 8，不过此需求仅适用于此工具包 - 应用程序可以继续使用 Azure 支持的所有 Java 版本。
* **支持最新的 Java JDK**。 Azure Toolkit for IntelliJ 现在支持最新版本的 Java JDK。
* **支持 Azure SDK v2.9.1**。 最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。
* **集成示例**。 Azure Toolkit for IntelliJ 目前精选了数个示例应用程序，可帮助开发人员快速入门。
* **HDInsight 工具集成**。 Azure 的 HDInsight 工具现在随附于 Azure Toolkit for IntelliJ。 有关详细信息，请参阅 [用于 IntelliJ 的 HDInsight 工具插件]。
* **远程调试 Java Web 应用**。 Azure Toolkit for IntelliJ 现在支持对 Azure 应用服务上的 Java Web 应用进行远程调试。

### <a name="april-12-2016"></a>2016 年 4 月 12 日
Azure Toolkit for IntelliJ - 2016 年 4 月版包含以下增强功能：

* **支持 Azure SDK v2.9.0**。 最新版 Azure SDK 现在是 Azure Toolkit for IntelliJ 的最低必备组件。
* **与 Azure Web 应用支持有关的其他可用性、响应能力和性能改进**。 工具包中多项与 Azure 通信方式的性能优化会让 UI 响应速度更快。
* **能够在 IntelliJ 内删除 Azure 中现有的 Web 应用程序容器**。 Azure Toolkit for IntelliJ 现在允许删除现有的 Azure Web 容器，而不需要退出 IntelliJ。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[用于 Eclipse 的 Azure 工具包]: ../eclipse/azure-toolkit-for-eclipse.md

[用于 IntelliJ 的 Azure 工具包的 Azure 登录说明]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[使用用于 IntelliJ 的 Azure 资源管理器管理存储帐户]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[使用用于 IntelliJ 的 Azure 资源管理器管理虚拟机]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: https://docs.microsoft.com/java/azure

[用于 IntelliJ 的 HDInsight 工具插件]: /azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin
