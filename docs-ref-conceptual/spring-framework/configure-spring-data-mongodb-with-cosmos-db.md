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
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a><span data-ttu-id="66fea-103">如何将 Spring Data MongoDB API 用于 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="66fea-103">How to use Spring Data MongoDB API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="66fea-104">概述</span><span class="sxs-lookup"><span data-stu-id="66fea-104">Overview</span></span>

<span data-ttu-id="66fea-105">本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) 存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="66fea-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66fea-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="66fea-106">Prerequisites</span></span>

<span data-ttu-id="66fea-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="66fea-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="66fea-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="66fea-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="66fea-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="66fea-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="66fea-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="66fea-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="66fea-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="66fea-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="66fea-112">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="66fea-112">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="66fea-113">创建 Azure Cosmos DB 帐户</span><span class="sxs-lookup"><span data-stu-id="66fea-113">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="66fea-114">使用 Azure 门户创建 Cosmos DB 帐户</span><span class="sxs-lookup"><span data-stu-id="66fea-114">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="66fea-115">可以在 [Azure Cosmos DB 文档](/azure/cosmos-db/)中阅读有关创建 Azure Cosmos DB 帐户的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="66fea-115">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="66fea-116">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="66fea-116">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="66fea-117">依次单击“+创建资源”、“数据库”和“Azure Cosmos DB”。</span><span class="sxs-lookup"><span data-stu-id="66fea-117">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![创建 Azure Cosmos DB 帐户][COSMOSDB01]

1. <span data-ttu-id="66fea-119">指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="66fea-119">Specify the following information:</span></span>

   - <span data-ttu-id="66fea-120">**订阅**：指定要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="66fea-120">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="66fea-121">**资源组**：指定是要创建新资源组，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="66fea-121">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="66fea-122">**帐户名**：为 Cosmos DB 帐户选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoysmongodb.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="66fea-122">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoysmongodb.documents.azure.com*.</span></span>
   - <span data-ttu-id="66fea-123">**API**：对于本教程，请指定 `Azure Cosmos DB for MongoDB API`。</span><span class="sxs-lookup"><span data-stu-id="66fea-123">**API**: Specify `Azure Cosmos DB for MongoDB API` for this tutorial.</span></span>
   - <span data-ttu-id="66fea-124">**位置**：指定最靠近你的数据库的地理区域。</span><span class="sxs-lookup"><span data-stu-id="66fea-124">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 Cosmos DB 帐户设置][COSMOSDB02]
   
1. <span data-ttu-id="66fea-126">输入上述所有信息后，单击“复查 + 创建”。</span><span class="sxs-lookup"><span data-stu-id="66fea-126">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="66fea-127">如果复查页面上的所有信息看起来都是正确的，则单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="66fea-127">If everything looks correct on the review page, click **Create**.</span></span>

   ![复查 Cosmos DB 帐户设置][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a><span data-ttu-id="66fea-129">检索 Azure Cosmos DB 帐户的连接字符串</span><span class="sxs-lookup"><span data-stu-id="66fea-129">Retrieve the connection string for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="66fea-130">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="66fea-130">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="66fea-131">单击“所有资源”，然后单击你刚才创建的 Azure Cosmos DB 帐户。</span><span class="sxs-lookup"><span data-stu-id="66fea-131">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![选择 Azure Cosmos DB 帐户][COSMOSDB04]

1. <span data-ttu-id="66fea-133">单击“连接字符串”，复制“主要连接字符串”字段的值；稍后需要使用此值来配置应用程序。</span><span class="sxs-lookup"><span data-stu-id="66fea-133">Click **Connection strings**, and copy the value for the **Primary Connection String** field; you will use that value to configure your application later.</span></span>

   ![检索 Cosmos DB 连接字符串][COSMOSDB06]

## <a name="configure-the-sample-application"></a><span data-ttu-id="66fea-135">配置示例应用程序</span><span class="sxs-lookup"><span data-stu-id="66fea-135">Configure the sample application</span></span>

1. <span data-ttu-id="66fea-136">打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="66fea-136">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. <span data-ttu-id="66fea-137">在示例项目的 *&lt;project root&gt;/complete/src/main* 目录中创建一个 *resources* 目录，然后在 *resources* 目录中创建一个 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="66fea-137">Create a *resources* directory in the *&lt;project root&gt;/complete/src/main* directory of the sample project, and create an *application.properties* file in the *resources* directory.</span></span>

1. <span data-ttu-id="66fea-138">在文本编辑器中打开 *application.properties* 文件，在文件中添加以下行，并将示例值替换为上文中的相应值：</span><span class="sxs-lookup"><span data-stu-id="66fea-138">Open the *application.properties* file in a text editor, and add the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   <span data-ttu-id="66fea-139">其中：</span><span class="sxs-lookup"><span data-stu-id="66fea-139">Where:</span></span>

   | <span data-ttu-id="66fea-140">参数</span><span class="sxs-lookup"><span data-stu-id="66fea-140">Parameter</span></span> | <span data-ttu-id="66fea-141">说明</span><span class="sxs-lookup"><span data-stu-id="66fea-141">Description</span></span> |
   |---|---|
   | `spring.data.mongodb.database` | <span data-ttu-id="66fea-142">指定本文上文中所述的 Cosmos DB 帐户的名称。</span><span class="sxs-lookup"><span data-stu-id="66fea-142">Specifies the name of your Cosmos DB account from earlier in this article.</span></span> |
   | `spring.data.mongodb.uri` | <span data-ttu-id="66fea-143">指定本文上文中所述的**主要连接字符串**。</span><span class="sxs-lookup"><span data-stu-id="66fea-143">Specifies the **Primary Connection String** from earlier in this article.</span></span> |

1. <span data-ttu-id="66fea-144">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="66fea-144">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="66fea-145">打包并测试示例应用程序</span><span class="sxs-lookup"><span data-stu-id="66fea-145">Package and test the sample application</span></span> 

1. <span data-ttu-id="66fea-146">使用 Maven 生成示例应用程序，并将 Maven 配置为跳过测试；例如：</span><span class="sxs-lookup"><span data-stu-id="66fea-146">Build the sample application with Maven, and configure Maven to skip tests; for example:</span></span>

   ```shell
   mvn clean package -DskipTests
   ```

1. <span data-ttu-id="66fea-147">启动示例应用程序；例如：</span><span class="sxs-lookup"><span data-stu-id="66fea-147">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/gs-accessing-data-mongodb-0.1.0.jar
   ```
    
   <span data-ttu-id="66fea-148">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="66fea-148">Your application should return values like the following:</span></span>

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

## <a name="summary"></a><span data-ttu-id="66fea-149">摘要</span><span class="sxs-lookup"><span data-stu-id="66fea-149">Summary</span></span>

<span data-ttu-id="66fea-150">在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 Azure Cosmos DB MongoDB API 存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="66fea-150">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB MongoDB API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66fea-151">后续步骤</span><span class="sxs-lookup"><span data-stu-id="66fea-151">Next steps</span></span>

<span data-ttu-id="66fea-152">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="66fea-152">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="66fea-153">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="66fea-153">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="66fea-154">其他资源</span><span class="sxs-lookup"><span data-stu-id="66fea-154">Additional Resources</span></span>

<span data-ttu-id="66fea-155">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="66fea-155">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
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
