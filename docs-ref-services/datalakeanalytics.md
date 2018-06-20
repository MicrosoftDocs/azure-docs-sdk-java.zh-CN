---
title: 用于 Java 的 Azure Data Lake Analytics 库
description: Azure Data Lake Analytics 库的参考文档
keywords: Azure, Java, SDK, API, 大数据, data lake
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: c14c89f961951d114362adee4fec6239e78cffb3
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823770"
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a>用于 Java 的 Azure Data Lake Analytics 库

## <a name="overview"></a>概述

使用 [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) 运行可扩展为大规模数据集的大数据分析作业。

若要开始使用 Azure Data Lake Analytics，请参阅[通过 Java SDK 开始使用 Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)。

## <a name="management-api"></a>管理 API

使用管理 API 管理 Data Lake Analytics 帐户、作业、策略和目录。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a>示例

将新的 U-SQL 作业提交到 Data Lake Analytics。

```java
// authenticate with service principal credentials
ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
DataLakeAnalyticsJobManagementClient adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);

// set up job parameters
UUID jobId = java.util.UUID.randomUUID();
USqlJobProperties properties = new USqlJobProperties();
properties.setScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();");
JobInformation parameters = new JobInformation();
parameters.setName("testJob");
parameters.setJobId(jobId);
parameters.setType(JobType.USQL);
parameters.setProperties(properties);

// create the job
JobInformation jobInfo = adlaJobClient.getJobOperations().create(accountName, jobId, parameters).getBody();

```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a>示例

[使用 Java SDK 管理 Azure Data Lake Analytics][1] 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

查看 Azure Data Lake Analytics 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)。
