---
title: 如何将 Spring Data MongoDB API 用于 Azure Cosmos DB
description: 了解如何将 Spring Data MongoDB API 用于 Azure Cosmos DB。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 55fb356820e2cc5eb8d1fe4030053d9e580583af
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992038"
---
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a>如何将 Spring Data MongoDB API 用于 Azure Cosmos DB

## <a name="overview"></a>概述

本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) 存储和检索信息。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* [Git](https://git-scm.com/downloads) 客户端。

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>使用 Azure 门户创建 Cosmos DB 帐户

> [!NOTE]
> 
> 可以在 [Azure Cosmos DB 文档](/azure/cosmos-db/)中阅读有关创建 Azure Cosmos DB 帐户的更多详细信息。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次单击“+创建资源”、“数据库”和“Azure Cosmos DB”。

   ![创建 Azure Cosmos DB 帐户][COSMOSDB01]

1. 指定以下信息：

   - **订阅**：指定要使用的 Azure 订阅。
   - **资源组**：指定是要创建新资源组，还是选择现有资源组。
   - **帐户名**：为 Cosmos DB 帐户选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoysmongodb.documents.azure.com*。
   - **API**：对于本教程，请指定 `Azure Cosmos DB for MongoDB API`。
   - **位置**：指定最靠近你的数据库的地理区域。

   ![指定 Cosmos DB 帐户设置][COSMOSDB02]
   
1. 输入上述所有信息后，单击“复查 + 创建”。

1. 如果复查页面上的所有信息看起来都是正确的，则单击“创建”。

   ![复查 Cosmos DB 帐户设置][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a>检索 Azure Cosmos DB 帐户的连接字符串

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 单击“所有资源”，然后单击你刚才创建的 Azure Cosmos DB 帐户。

   ![选择 Azure Cosmos DB 帐户][COSMOSDB04]

1. 单击“连接字符串”，复制“主要连接字符串”字段的值；稍后需要使用此值来配置应用程序。

   ![检索 Cosmos DB 连接字符串][COSMOSDB06]

## <a name="configure-the-sample-application"></a>配置示例应用程序

1. 打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. 在示例项目的 *&lt;project root&gt;/complete/src/main* 目录中创建一个 *resources* 目录，然后在 *resources* 目录中创建一个 *application.properties* 文件。

1. 在文本编辑器中打开 *application.properties* 文件，在文件中添加以下行，并将示例值替换为上文中的相应值：

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `spring.data.mongodb.database` | 指定本文上文中所述的 Cosmos DB 帐户的名称。 |
   | `spring.data.mongodb.uri` | 指定本文上文中所述的**主要连接字符串**。 |

1. 保存并关闭 application.properties 文件。

## <a name="package-and-test-the-sample-application"></a>打包并测试示例应用程序 

1. 使用 Maven 生成示例应用程序，并将 Maven 配置为跳过测试；例如：

   ```shell
   mvn clean package -DskipTests
   ```

1. 启动示例应用程序；例如：

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   你的应用程序应返回如下所示的值：

   ```json
   Customers found with findAll():
   -------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   
   Customer found with findByFirstName('Alice'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customers found with findByLastName('Smith'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   ```

## <a name="summary"></a>摘要

在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 Azure Cosmos DB MongoDB API 存储和检索信息。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png
