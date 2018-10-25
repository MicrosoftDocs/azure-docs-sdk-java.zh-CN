---
title: 用于 Java 的 Azure Data Lake Store 库
description: Azure Data Lake Store 库的参考文档
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
ms.openlocfilehash: ebbadff83e1e081153f0ae376fb71d6607e63f15
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799793"
---
# <a name="azure-data-lake-store-libraries-for-java"></a><span data-ttu-id="730e1-104">用于 Java 的 Azure Data Lake Store 库</span><span class="sxs-lookup"><span data-stu-id="730e1-104">Azure Data Lake Store libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="730e1-105">概述</span><span class="sxs-lookup"><span data-stu-id="730e1-105">Overview</span></span>

<span data-ttu-id="730e1-106">使用 [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) 在单个位置捕获任意大小、类型和集成速度的数据以进行分析。</span><span class="sxs-lookup"><span data-stu-id="730e1-106">Capture data of any size, type, and ingestion speed in a single place for analytics with [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview).</span></span>

<span data-ttu-id="730e1-107">若要开始使用 Data Lake Store，请参阅[通过 Java 开始使用 Azure Data Lake Store](/azure/data-lake-store/data-lake-store-get-started-java-sdk)。</span><span class="sxs-lookup"><span data-stu-id="730e1-107">To get started with Data Lake Store, see [Get started with Azure Data Lake Store using Java](/azure/data-lake-store/data-lake-store-get-started-java-sdk).</span></span>


## <a name="client-library"></a><span data-ttu-id="730e1-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="730e1-108">Client library</span></span>

<span data-ttu-id="730e1-109">使用客户端库在 Data Lake Store 中读取和写入文件、设置权限和元数据，以及管理文件和目录。</span><span class="sxs-lookup"><span data-stu-id="730e1-109">Read and write files, set permissions and metadata, and manage files and directories in Data Lake Store with the client library.</span></span>

<span data-ttu-id="730e1-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="730e1-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="730e1-111">示例</span><span class="sxs-lookup"><span data-stu-id="730e1-111">Example</span></span>

<span data-ttu-id="730e1-112">通过完全限定的域名和 OAuth2 访问令牌创建 Data Lake 客户端，然后在 Data Lake 中创建文件和写入数据。</span><span class="sxs-lookup"><span data-stu-id="730e1-112">Create a Data Lake client from a fully qualified domain name and OAuth2 access token, then create a file in Data Lake and write to it.</span></span>

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="730e1-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="730e1-113">Explore the Client APIs</span></span>](/java/api/overview/azure/datalakestore/client)


## <a name="management-api"></a><span data-ttu-id="730e1-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="730e1-114">Management API</span></span>

<span data-ttu-id="730e1-115">使用管理 API 来管理 Data Lake Store 帐户、防火墙规则和受信任的标识提供者。</span><span class="sxs-lookup"><span data-stu-id="730e1-115">Use the management API to manage Data Lake Store accounts, firewall rules, and trusted identity providers.</span></span>

<span data-ttu-id="730e1-116">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="730e1-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="730e1-117">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="730e1-117">Explore the Management APIs</span></span>](/java/api/overview/azure/datalakestore/management)

## <a name="samples"></a><span data-ttu-id="730e1-118">示例</span><span class="sxs-lookup"><span data-stu-id="730e1-118">Samples</span></span>

<span data-ttu-id="730e1-119">[Azure Data Lake 入门][1]</span><span class="sxs-lookup"><span data-stu-id="730e1-119">[Azure Data Lake Get Started][1]</span></span> 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

<span data-ttu-id="730e1-120">详细了解可在应用中使用的 [Azure Data Lake Store 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)。</span><span class="sxs-lookup"><span data-stu-id="730e1-120">Explore more [sample Java code for Azure Data Lake Store](https://azure.microsoft.com/resources/samples/?platform=java&term=lake) you can use in your apps.</span></span>
