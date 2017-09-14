---
title: "用于 Java 的 Azure 管理库发行说明 | Microsoft Docs"
description: "了解用于 Java 的 Azure 管理库的新增功能，并留意其中的重大更改"
keywords: "Azure,  Java, API, 参考, 说明, 更新, 弃用"
author: routlaw
manager: douge
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.service: Azure
ms.devlang: java
ms.topic: reference
ms.technology: Azure
ms.date: 3/06/2016
ms.openlocfilehash: c0d5c4b3702d3bee4e93de51cec36e72aeaf598f
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="release-notes"></a>发行说明 

## <a name="june-30-2017---110"></a>2017 年 6 月 30 日 - 1.1.0 

V1.1 与 V1.0 中已达到正式版（稳定版）发行阶段的、旨在供大众使用的 API 向后兼容。

V.0 中标有 @Beta 注释的 API 引入了一些重大更改

若要将代码迁移到 1.1.0，可以参考[这些说明](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)准备将代码从 1.0.0 迁移到 1.1.0。

### <a name="generally-availabile-in-v11"></a>V1.1 中的正式版

V1.0 中仍处于 Beta 版状态的某些 API 在 V1.1 中现已推出正式版，具体而言：

- 异步方法
- CDN 中以前处于 Beta 版状态的所有方法
- 应用程序网关中以前处于 Beta 版状态的所有方法和接口

 库的某些部件仍处于预览版状态。 请参阅下表了解库的当前状态：

服务或功能 | 以正式版提供 | 以预览版提供  | 即将支持 |
---------|---------|---------|---------|
计算  | 虚拟机和 VM 扩展、虚拟机规模集、托管磁盘   | Azure 容器服务、Azure 容器注册表 |    |
存储   |  存储帐户       |         |   加密      |
SQL 数据库  | 数据库、防火墙、弹性池        |         |   其他功能      |
联网    |  虚拟网络、网络接口、IP 地址、路由表、网络安全组、DNS、流量管理器、应用程序网关  |    负载均衡器     |   VPN、网络观察程序   |
其他服务    |  资源管理器、Key Vault、Redis、CDN、Batch       |  Web 应用、函数应用、服务总线、Graph RBAC、Cosmos DB   | Monitor、计划程序、函数管理、搜索、其他 Graph RBAC 功能        |
基本     |   身份验证 - 核心身份验证、异步方法       |      |         |

### <a name="import-with-maven"></a>使用 Maven 导入

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

请查看 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) 社区文章来帮助自己在代码中使用库。 如果遇到任何 bug 或者有改进这些库的建议，请通过 [GitHub](https://github.com/Azure/azure-sdk-for-java/issues) 提出问题。

### <a name="migrate-from-previous-releases"></a>从以前的版本迁移

[从 1.0.0-beta5 迁移](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.0.0.md)[从 1.1.0 迁移](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.1.0.md)


