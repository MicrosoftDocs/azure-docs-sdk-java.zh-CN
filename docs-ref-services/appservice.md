---
title: 用于 Java 的 Azure 应用服务库
description: 使用 Azure 管理 API 在 Azure 应用服务中自动部署 Web 应用。
keywords: Azure, Java, SDK, API, web 应用, 移动, 应用服务
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/09/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: appservice
ms.openlocfilehash: 49063e48ec739b912c418774f531f11b16a3ff1e
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799823"
---
# <a name="azure-app-service-libraries-for-java"></a>用于 Java 的 Azure 应用服务库

## <a name="overview"></a>概述

使用 [Azure 应用服务](/azure/app-service)部署和管理网站、Web 应用程序与 REST API。

若要开始使用 Azure 应用服务，请参阅[在 Azure 中创建第一个 Java Web 应用](/azure/app-service-web/app-service-web-get-started-java)。

## <a name="management-api"></a>管理 API

使用管理 API 在 Azure 应用服务中部署、缩放和配置应用程序。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/appservice/management)

### <a name="example"></a>示例

将 Docker 映像中的 Web 应用部署到 Linux 上运行的 Azure Web 应用。

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a>示例

[从 FTP 或 GitHub 部署 Web 应用][1]  
[使用部署槽交换应用版本][2]  
[配置自定义域][3]   
[跨多个区域缩放 Web 应用][4]   

详细了解可在应用中使用的 [Azure 应用服务示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)。

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
