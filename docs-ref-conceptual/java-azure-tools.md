---
title: "面向 Azure Java 开发人员的工具 | Microsoft Docs"
description: "面向使用 Azure 的 Java 开发人员的 IDE 集成、仿真器、资源浏览器和命令行接口。"
author: rloutlaw
manager: douge
ms.assetid: b55923b7-d60a-460d-b77c-af5fac67f1cc
ms.devlang: java
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 4/10/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: ce0b003cc7c48c8690f4236547ddec36e6ea9d53
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="azure-tools-for-java-developers"></a>面向 Java 开发人员的 Azure 工具

## <a name="client-and-management-libraries"></a>客户端和管理库

使用用于 Java 的 Azure 库，通过应用程序连接到服务和管理 Azure 资源。 通过将此依赖项添加到项目 *pom.xml*，将管理库导入 Maven 项目。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

查看[库的完整列表](java-sdk-azure-install.md)和用于 Java 的 Azure 库的[入门](java-sdk-azure-get-started.md)文档。

## <a name="eclipse-and-intellij-plugins"></a>Eclipse 和 IntelliJ 插件

使用用于 [Eclipse](eclipse/azure-toolkit-for-eclipse.md) 和 [IntelliJ](intellij/azure-toolkit-for-intellij.md) 的 Azure 工具包通过 IDE 管理 Azure 资源及部署应用。   

![显示 Azure 资源管理器的 IntelliJ 工具包](media/intelliJ-azure-explorer.png)

[用于 Eclipse 的 Azure 工具包入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-eclipse-create-hello-world-web-app) | [用于 IntelliJ 的 Azure 工具包入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-intellij-create-hello-world-web-app) 

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure 2.0 CLI 提供用于管理 Azure 资源的命令行体验。 可以通过 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) 在浏览器中使用它，也可以将其[安装](https://docs.microsoft.com/cli/azure/install-azure-cli)在 macOS、Linux 和 Windows 上，然后从命令行运行它。

[Azure CLI 2.0 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。

## <a name="azure-storage-explorer"></a>Azure 存储资源管理器 

从桌面管理 Azure 存储帐户、容器和 Blob/文件。 Azure 存储资源管理器目前以预览版提供，适用于 Windows、macOS 和 Linux。

[Azure 存储资源管理器入门](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)