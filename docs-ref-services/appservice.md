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
# <a name="azure-app-service-libraries-for-java"></a><span data-ttu-id="1db91-104">用于 Java 的 Azure 应用服务库</span><span class="sxs-lookup"><span data-stu-id="1db91-104">Azure App Service libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="1db91-105">概述</span><span class="sxs-lookup"><span data-stu-id="1db91-105">Overview</span></span>

<span data-ttu-id="1db91-106">使用 [Azure 应用服务](/azure/app-service)部署和管理网站、Web 应用程序与 REST API。</span><span class="sxs-lookup"><span data-stu-id="1db91-106">Deploy and manage websites, web applications, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="1db91-107">若要开始使用 Azure 应用服务，请参阅[在 Azure 中创建第一个 Java Web 应用](/azure/app-service-web/app-service-web-get-started-java)。</span><span class="sxs-lookup"><span data-stu-id="1db91-107">To get started with Azure App Service, see [Create your first Java web app in Azure](/azure/app-service-web/app-service-web-get-started-java).</span></span>

## <a name="management-api"></a><span data-ttu-id="1db91-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="1db91-108">Management API</span></span>

<span data-ttu-id="1db91-109">使用管理 API 在 Azure 应用服务中部署、缩放和配置应用程序。</span><span class="sxs-lookup"><span data-stu-id="1db91-109">Deploy, scale, and configure applications in Azure App Service with the management API.</span></span>

<span data-ttu-id="1db91-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="1db91-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-appservice</artifactId>
    <version>1.3.0</version>
</dependency>
```   

> [!div class="nextstepaction"]
> [<span data-ttu-id="1db91-111">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="1db91-111">Explore the Management APIs</span></span>](/java/api/overview/azure/appservice/management)

### <a name="example"></a><span data-ttu-id="1db91-112">示例</span><span class="sxs-lookup"><span data-stu-id="1db91-112">Example</span></span>

<span data-ttu-id="1db91-113">将 Docker 映像中的 Web 应用部署到 Linux 上运行的 Azure Web 应用。</span><span class="sxs-lookup"><span data-stu-id="1db91-113">Deploy a webapp from a Docker image into an Azure Web App running on Linux.</span></span>

```java
WebApp app = azure.webApps().define("newLinuxWebApp")
    .withExistingLinuxPlan(myLinuxAppServicePlan)
    .withExistingResourceGroup("myResourceGroup")
    .withPrivateDockerHubImage("username/my-java-app")
    .withCredentials("dockerHubUser","dockerHubPassword")
    .withAppSetting("PORT","8080").
    .create();
```

## <a name="samples"></a><span data-ttu-id="1db91-114">示例</span><span class="sxs-lookup"><span data-stu-id="1db91-114">Samples</span></span>

<span data-ttu-id="1db91-115">[从 FTP 或 GitHub 部署 Web 应用][1]</span><span class="sxs-lookup"><span data-stu-id="1db91-115">[Deploy a web app from FTP or GitHub][1]</span></span>  
<span data-ttu-id="1db91-116">[使用部署槽交换应用版本][2]</span><span class="sxs-lookup"><span data-stu-id="1db91-116">[Swap between app versions with deployment slots][2]</span></span>  
<span data-ttu-id="1db91-117">[配置自定义域][3] </span><span class="sxs-lookup"><span data-stu-id="1db91-117">[Configure a custom domain][3] </span></span>  
<span data-ttu-id="1db91-118">[跨多个区域缩放 Web 应用][4]</span><span class="sxs-lookup"><span data-stu-id="1db91-118">[Scale a web app across multiple regions][4]</span></span>   

<span data-ttu-id="1db91-119">详细了解可在应用中使用的 [Azure 应用服务示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice)。</span><span class="sxs-lookup"><span data-stu-id="1db91-119">Explore more [sample Java code for Azure App Service](https://azure.microsoft.com/resources/samples/?platform=java&term=appservice) you can use in your apps.</span></span>

[1]: ../docs-ref-conceptual/java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
