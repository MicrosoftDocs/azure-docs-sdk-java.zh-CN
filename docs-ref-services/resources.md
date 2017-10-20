---
title: "用于 Java 的 Azure 资源管理器库"
description: "Java 资源管理器库的参考文档"
keywords: "Azure, Java, SDK, API, 资源组, arm, 资源管理器"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 56199b87fa64e9cbf0a14716a58c01f11f0e433b
ms.sourcegitcommit: 4b63ecd2c92a9115dfae018618e4e4046b061b3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2017
---
# <a name="azure-resource-manager-libraries-for-java"></a>用于 Java 的 Azure 资源管理器库

## <a name="overview"></a>概述

使用 [Azure 资源管理器](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)部署、监视和管理组中的资源。

## <a name="management-api"></a>管理 API

使用管理 API 通过模板创建资源组和部署资源。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-resources</artifactId>
    <version>1.3.0</version>
</dependency>
```

## <a name="example"></a>示例

在 Azure 美国东部区域中创建新的资源组。

```java
ResourceGroup resourceGroup = azure.resourceGroups().define("myResourceGroup")
            .withRegion(Region.US_EAST)
            .create();
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/resources/managementapi)

## <a name="samples"></a>示例

[使用 Java 管理 Azure 资源组][1] 
[使用 ARM 模板部署资源][2]

[1]: https://github.com/Azure-Samples/resources-java-manage-resource-group
[2]: https://github.com/Azure-Samples/resources-java-deploy-using-arm-template

查看 Azure 资源管理器示例的[完整列表](https://azure.microsoft.com/resources/samples/?platform=java&term=resource)。
