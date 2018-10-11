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
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893404"
---
# <a name="azure-application-insights-libraries-for-java"></a><span data-ttu-id="f7875-104">用于 Java 的 Azure Application Insights 库</span><span class="sxs-lookup"><span data-stu-id="f7875-104">Azure Application Insights libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="f7875-105">概述</span><span class="sxs-lookup"><span data-stu-id="f7875-105">Overview</span></span>

<span data-ttu-id="f7875-106">使用 [Application Insights](/azure/application-insights/app-insights-overview) 检测、会审和诊断 Web 应用与服务中的问题。</span><span class="sxs-lookup"><span data-stu-id="f7875-106">Detect, triage, and diagnose issues in your web apps and services with [Application Insights](/azure/application-insights/app-insights-overview).</span></span>

<span data-ttu-id="f7875-107">若要开始使用 Application Insights，请参阅 [Java Web 项目中的 Application Insights 入门](/azure/application-insights/app-insights-java-get-started)。</span><span class="sxs-lookup"><span data-stu-id="f7875-107">To get started with Application Insights, see [Get started with Application Insights in a Java web project](/azure/application-insights/app-insights-java-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="f7875-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="f7875-108">Client library</span></span>

<span data-ttu-id="f7875-109">使用 Application Insights 客户端库添加遥测，以跟踪应用中的事件、异常和用户指标。</span><span class="sxs-lookup"><span data-stu-id="f7875-109">Add telemetry to track events, exceptions, and user metrics in your apps with the Application Insights client library.</span></span>

<span data-ttu-id="f7875-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="f7875-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-web</artifactId>   
    <version>1.0.8</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="f7875-111">示例</span><span class="sxs-lookup"><span data-stu-id="f7875-111">Example</span></span>

<span data-ttu-id="f7875-112">创建新的指标条目并记录其值。</span><span class="sxs-lookup"><span data-stu-id="f7875-112">Create a new metric entry and record a value for it.</span></span>

```java
    MetricTelemetry sample = new MetricTelemetry();
    sample.setName("metric name");
    sample.setValue(42.3);
    telemetryClient.TrackMetric(sample);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="f7875-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="f7875-113">Explore the Client APIs</span></span>](/java/api/overview/azure/appinsights/client)

## <a name="samples"></a><span data-ttu-id="f7875-114">示例</span><span class="sxs-lookup"><span data-stu-id="f7875-114">Samples</span></span>

<span data-ttu-id="f7875-115">详细了解可在应用中使用的 [Application Insights 示例 Java 代码](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java)。</span><span class="sxs-lookup"><span data-stu-id="f7875-115">Explore more [sample Java code for Application Insights](https://azure.microsoft.com/en-us/resources/samples/?term=insights&platform=java) you can use in your apps.</span></span>
