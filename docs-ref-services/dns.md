---
title: 用于 Java 的 Azure DNS 库
description: Azure DNS Java 管理库的参考文档
keywords: Azure, Java, SDK, API, 域, DNS, 名称, 服务, 域名服务
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: dns
ms.openlocfilehash: 2cd8fe7ee4d6a87da32a349fe8f1d2815d3fd36d
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823630"
---
# <a name="azure-traffic-manager-libraries-for-java"></a><span data-ttu-id="2b76c-104">用于 Java 的 Azure 流量管理器库</span><span class="sxs-lookup"><span data-stu-id="2b76c-104">Azure Traffic Manager libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="2b76c-105">概述</span><span class="sxs-lookup"><span data-stu-id="2b76c-105">Overview</span></span>

<span data-ttu-id="2b76c-106">通过 [Azure DNS](/azure/dns/dns-overview)，使用与其他 Azure 服务相同的凭据、API、工具和计费来提供域名解析和管理 DNS 记录。</span><span class="sxs-lookup"><span data-stu-id="2b76c-106">Provide domain name resolution and manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services with [Azure DNS](/azure/dns/dns-overview).</span></span>

<span data-ttu-id="2b76c-107">若要开始使用 Azure DNS，请参阅[通过 Azure CLI 2.0 开始使用 Azure DNS](/azure/dns/dns-getstarted-cli)。</span><span class="sxs-lookup"><span data-stu-id="2b76c-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure CLI 2.0](/azure/dns/dns-getstarted-cli).</span></span>

## <a name="management-api"></a><span data-ttu-id="2b76c-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="2b76c-108">Management API</span></span>

<span data-ttu-id="2b76c-109">使用管理 API 创建 DNS 区域并将记录添加到区域。</span><span class="sxs-lookup"><span data-stu-id="2b76c-109">Create DNS zones and add records to zones with the management API.</span></span>

<span data-ttu-id="2b76c-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="2b76c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.3.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="2b76c-111">示例</span><span class="sxs-lookup"><span data-stu-id="2b76c-111">Example</span></span>

<span data-ttu-id="2b76c-112">在现有资源组中创建根 DNS 区域并添加 `www` CNAME 记录。</span><span class="sxs-lookup"><span data-stu-id="2b76c-112">Create a root DNS zone and add a `www` CNAME record in an existing resource group.</span></span>

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2b76c-113">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="2b76c-113">Explore the Management APIs</span></span>](/java/api/overview/azure/dns/management)

## <a name="samples"></a><span data-ttu-id="2b76c-114">示例</span><span class="sxs-lookup"><span data-stu-id="2b76c-114">Samples</span></span>

[<span data-ttu-id="2b76c-115">使用 Azure DNS 来托管和管理域</span><span class="sxs-lookup"><span data-stu-id="2b76c-115">Host and manage your domains with Azure DNS</span></span>](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

<span data-ttu-id="2b76c-116">详细了解可在应用中使用的 [Azure DNS 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)。</span><span class="sxs-lookup"><span data-stu-id="2b76c-116">Explore more [sample Java code for Azure DNS](https://azure.microsoft.com/resources/samples/?platform=java&term=dns) you can use in your apps.</span></span>

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
