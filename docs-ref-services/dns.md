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
ms.openlocfilehash: 90751d2134b218e16415effeb336a62c6c737cb3
ms.sourcegitcommit: 733115fe0a7b5109b511b4a32490f8264cf91217
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626070"
---
# <a name="azure-dns-libraries-for-java"></a>用于 Java 的 Azure DNS 库

## <a name="overview"></a>概述

通过 [Azure DNS](/azure/dns/dns-overview)，使用与其他 Azure 服务相同的凭据、API、工具和计费来提供域名解析和管理 DNS 记录。

若要开始使用 Azure DNS，请参阅[通过 Azure CLI 2.0 开始使用 Azure DNS](/azure/dns/dns-getstarted-cli)。

## <a name="management-api"></a>管理 API

使用管理 API 创建 DNS 区域并将记录添加到区域。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-dns</artifactId>
    <version>1.22.0</version>
</dependency>
```   

### <a name="example"></a>示例

在现有资源组中创建根 DNS 区域并添加 `www` CNAME 记录。

```java
DnsZone rootDnsZone = azure.dnsZones().define("contoso.com")
        .withExistingResourceGroup("myResourceGroup")
        .create();
rootDnsZone = rootDnsZone.update()
        .withCNameRecordSet("www", "172.30.241.20")
        .apply();
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/dns/management)

## <a name="samples"></a>示例

[使用 Azure DNS 来托管和管理域](https://github.com/Azure-Samples/dns-java-host-and-manage-your-domains)

详细了解可在应用中使用的 [Azure DNS 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=dns)。

<!---Loc Comment: Please, refer to conversation section to check the issue. Thanks.--->
