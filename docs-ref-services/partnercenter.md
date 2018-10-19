---
title: 合作伙伴中心 Java SDK
description: 合作伙伴中心 Java SDK 的参考文档
keywords: API, CSP, 云解决方案提供商, Java, 合作伙伴中心, SDK
author: iswillia
ms.author: iswillia
manager: lesliep
ms.date: 10/09/2018
ms.topic: reference
ms.prod: partnercenter
ms.technology: partnercenter
ms.devlang: java
ms.openlocfilehash: 2adfbdbd37d6eb91e48ee37091f951b3f7d59eb9
ms.sourcegitcommit: 4da7d2f470331feb7d76a1658f5825f365cdba9a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49121638"
---
# <a name="partner-center-libraries-for-java"></a>适用于 Java 的合作伙伴中心库

## <a name="overview"></a>概述

适用于 Java 的合作伙伴中心库是一个 SDK，通过它可与 Microsoft 的合作伙伴中心服务交互。 有了它，合作伙伴便可以通过编程方式执行合作伙伴中心操作。 它是 REST API 和 .NET SDK 的现有组合的最新补充。

## <a name="client-library"></a>客户端库

使用客户端库管理合作伙伴中心中的资源。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```xml
<dependency>
    <groupId>com.microsoft.store</groupId>
    <artifactId>partnercenter</artifactId>
    <version>1.8.0</version>
</dependency>
```   

### <a name="example"></a>示例

检索与经过身份验证的经销商相关联的客户的完整列表。

```java
IPartnerCredentials appCredentials = PartnerCredentials.getInstance().generateByApplicationCredentials('YOUR_APP_ID', 'YOUR_APP_SECRET', 'YOUR_TENANT_ID');
IAggregatePartner partnerOperations = PartnerService.getInstance().createPartnerOperations(appCredentials);

// Query the customers
SeekBasedResourceCollection<Customer> customersPage = partnerOperations.getCustomers().query(QueryFactory.getInstance().buildIndexedQuery(100));
this.getContext().getConsoleHelper().stopProgress();

// Create a customer enumerator which will aid us in traversing the customer pages
IResourceCollectionEnumerator<SeekBasedResourceCollection<Customer>> customersEnumerator =
    partnerOperations.getEnumerators().getCustomers().create( customersPage );

int pageNumber = 1;

while ( customersEnumerator.hasValue() )
{
    /*
     * Perform some operation with the current page of customers using 
     * customersEnumerator.getCurrent()  
     */

    // Get the next page of customers
    customersEnumerator.next();
}
```

## <a name="samples"></a>示例

[支持的方案](https://docs.microsoft.com/partner-center/develop/scenarios)
[示例控制台应用程序](https://github.com/Microsoft/Partner-Center-Java-Samples)  