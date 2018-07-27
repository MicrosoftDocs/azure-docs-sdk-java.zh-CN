---
title: 用于 Java 的 Azure Application Insights 库
description: 适用于 Azure Appplication Insights 的 Java 管理 API 的参考文档
keywords: Azure, Java, SDK, API, AppInsights, 遥测, 诊断, 跟踪, 日志, 性能
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/21/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appinsights
ms.openlocfilehash: d881ff66ad806e13f7d2cbafff6ce85c23240304
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823760"
---
# <a name="azure-application-insights-libraries-for-java"></a>用于 Java 的 Azure Application Insights 库

## <a name="overview"></a>概述

使用 [Application Insights](/azure/application-insights/app-insights-overview) 检测、会审和诊断 Web 应用与服务中的问题。

若要开始使用 Application Insights，请参阅 [Java Web 项目中的 Application Insights 入门](/azure/application-insights/app-insights-java-get-started)。

## <a name="client-library"></a>客户端库

使用 Application Insights 客户端库添加遥测，以跟踪应用中的事件、异常和用户指标。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a>示例

创建新的指标条目并记录其值。

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/appinsights/client)

## <a name="samples"></a>示例

详细了解可在应用中使用的 [Application Insights 示例 Java 代码](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)。
