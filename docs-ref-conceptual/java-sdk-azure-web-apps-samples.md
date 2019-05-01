---
title: 用于 Java 的 Azure 管理库 Web 应用示例
description: 获取有关使用用于 Java 的 Azure 管理库创建和更新应用服务中托管的 Azure Web 应用的示例代码
keywords: Azure, Java, SDK, API, Maven, Gradle, web 应用, 应用服务
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.openlocfilehash: be4031059587ceb88f6824356f677c37a198de80
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592582"
---
# <a name="azure-management-libraries-for-java-samples-for-web-apps"></a>用于 Java 的 Azure 管理库的 Web 应用示例

| **创建应用** ||
|---|---|
| [创建 Web 应用并通过 FTP 或 GitHub 部署][1] | 从 GitHub 通过本地 Git、FTP 和持续集成部署 Web 应用。 |
| [创建 Web 应用并管理部署槽][2] | 创建 Web 应用并部署到过渡槽，然后在槽之间交换部署。 |
| **配置应用** ||
| [创建 Web 应用并配置自定义域][3] | 使用自定义域和自签名 SSL 证书创建 Web 应用。 |
| **缩放应用** ||
| [跨多个区域缩放具有高可用性的 Web 应用][4] | 在三个不同的地理区域中缩放 Web 应用，并使用 Azure 流量管理器通过单个终结点使其可用。 | 
| **将应用连接到资源** ||
| [将 Web 应用连接到存储帐户][5] | 创建 Azure 存储帐户，并将存储帐户连接字符串添加到应用设置。 |
| [将 Web 应用连接到 SQL 数据库][6] | 创建 Web 应用和 SQL 数据库，然后将 SQL 数据库连接字符串添加到应用设置。 |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/