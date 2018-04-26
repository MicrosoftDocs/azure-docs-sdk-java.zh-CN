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
---
# <a name="azure-data-lake-analytics-libraries-for-java"></a><span data-ttu-id="6efec-104">用于 Java 的 Azure Data Lake Analytics 库</span><span class="sxs-lookup"><span data-stu-id="6efec-104">Azure Data Lake Analytics libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="6efec-105">概述</span><span class="sxs-lookup"><span data-stu-id="6efec-105">Overview</span></span>

<span data-ttu-id="6efec-106">使用 [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) 运行可扩展为大规模数据集的大数据分析作业。</span><span class="sxs-lookup"><span data-stu-id="6efec-106">Run big data analysis jobs that scale to massive data sets with [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview).</span></span>

<span data-ttu-id="6efec-107">若要开始使用 Azure Data Lake Analytics，请参阅[通过 Java SDK 开始使用 Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk)。</span><span class="sxs-lookup"><span data-stu-id="6efec-107">To get started with Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics using Java SDK](/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk).</span></span>

## <a name="management-api"></a><span data-ttu-id="6efec-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="6efec-108">Management API</span></span>

<span data-ttu-id="6efec-109">使用管理 API 管理 Data Lake Analytics 帐户、作业、策略和目录。</span><span class="sxs-lookup"><span data-stu-id="6efec-109">Use the management API to manage Data Lake Analytics accounts, jobs, policies, and catalogs.</span></span>

<span data-ttu-id="6efec-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="6efec-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-analytics</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="6efec-111">示例</span><span class="sxs-lookup"><span data-stu-id="6efec-111">Example</span></span>

<span data-ttu-id="6efec-112">将新的 U-SQL 作业提交到 Data Lake Analytics。</span><span class="sxs-lookup"><span data-stu-id="6efec-112">Submit a new U-SQL job to Data Lake Analytics.</span></span>

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
> [<span data-ttu-id="6efec-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="6efec-113">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a><span data-ttu-id="6efec-114">示例</span><span class="sxs-lookup"><span data-stu-id="6efec-114">Samples</span></span>

<span data-ttu-id="6efec-115">[使用 Java SDK 管理 Azure Data Lake Analytics][1]</span><span class="sxs-lookup"><span data-stu-id="6efec-115">[Azure Data Lake Analytics using Java SDK][1]</span></span> 

[1]: https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-java-sdk

<span data-ttu-id="6efec-116">查看 Azure Data Lake Analytics 示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics)。</span><span class="sxs-lookup"><span data-stu-id="6efec-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=java&term=analytics) of Azure Data Lake Analytics samples.</span></span>
