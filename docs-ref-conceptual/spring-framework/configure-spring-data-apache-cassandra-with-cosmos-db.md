---
title: 如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB
description: 了解如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB。
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
ms.openlocfilehash: 1f3f7a55b49d64077df9e7d4d6acc3af4495b424
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992019"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a><span data-ttu-id="8d759-103">如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8d759-103">How to use Spring Data Apache Cassandra API with Azure Cosmos DB</span></span>

## <a name="overview"></a><span data-ttu-id="8d759-104">概述</span><span class="sxs-lookup"><span data-stu-id="8d759-104">Overview</span></span>

<span data-ttu-id="8d759-105">本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction) 存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="8d759-105">This article demonstrates creating a sample application that uses [Spring Data] to store and retrieve information using the [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d759-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="8d759-106">Prerequisites</span></span>

<span data-ttu-id="8d759-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="8d759-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="8d759-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="8d759-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="8d759-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="8d759-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="8d759-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="8d759-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="8d759-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="8d759-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>
* <span data-ttu-id="8d759-112">用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。</span><span class="sxs-lookup"><span data-stu-id="8d759-112">[Curl](https://curl.haxx.se/) or similar HTTP utility to test functionality.</span></span>
* <span data-ttu-id="8d759-113">[Git](https://git-scm.com/downloads) 客户端。</span><span class="sxs-lookup"><span data-stu-id="8d759-113">A [Git](https://git-scm.com/downloads) client.</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="8d759-114">创建 Azure Cosmos DB 帐户</span><span class="sxs-lookup"><span data-stu-id="8d759-114">Create an Azure Cosmos DB account</span></span>

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a><span data-ttu-id="8d759-115">使用 Azure 门户创建 Cosmos DB 帐户</span><span class="sxs-lookup"><span data-stu-id="8d759-115">Create a Cosmos DB account using the Azure Portal</span></span>

> [!NOTE]
> 
> <span data-ttu-id="8d759-116">可以在 [Azure Cosmos DB 文档](/azure/cosmos-db/)中阅读有关创建 Azure Cosmos DB 帐户的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="8d759-116">You can read more detailed information about creating Azure Cosmos DB accounts in [Azure Cosmos DB Documentation](/azure/cosmos-db/).</span></span>

1. <span data-ttu-id="8d759-117">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="8d759-117">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="8d759-118">依次单击“+创建资源”、“数据库”和“Azure Cosmos DB”。</span><span class="sxs-lookup"><span data-stu-id="8d759-118">Click **+Create a resource**, then **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![创建 Azure Cosmos DB 帐户][COSMOSDB01]

1. <span data-ttu-id="8d759-120">指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="8d759-120">Specify the following information:</span></span>

   - <span data-ttu-id="8d759-121">**订阅**：指定要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="8d759-121">**Subscription**: Specify your Azure subscription to use.</span></span>
   - <span data-ttu-id="8d759-122">**资源组**：指定是要创建新资源组，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="8d759-122">**Resource group**: Specify whether to create a new resource group, or choose an existing resource group.</span></span>
   - <span data-ttu-id="8d759-123">**帐户名**：为 Cosmos DB 帐户选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoyscassandra.documents.azure.com*。</span><span class="sxs-lookup"><span data-stu-id="8d759-123">**Account name**: Choose a unique name for your Cosmos DB account; this will be used to create a fully-qualified domain name like *wingtiptoyscassandra.documents.azure.com*.</span></span>
   - <span data-ttu-id="8d759-124">**API**：对于本教程，请指定 `Cassandra`。</span><span class="sxs-lookup"><span data-stu-id="8d759-124">**API**: Specify `Cassandra` for this tutorial.</span></span>
   - <span data-ttu-id="8d759-125">**位置**：指定最靠近你的数据库的地理区域。</span><span class="sxs-lookup"><span data-stu-id="8d759-125">**Location**: Specify the closest geographic region for your database.</span></span>

   ![指定 Cosmos DB 帐户设置][COSMOSDB02]
   
1. <span data-ttu-id="8d759-127">输入上述所有信息后，单击“复查 + 创建”。</span><span class="sxs-lookup"><span data-stu-id="8d759-127">When you have entered all of the above information, click **Review + create**.</span></span>

1. <span data-ttu-id="8d759-128">如果复查页面上的所有信息看起来都是正确的，则单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="8d759-128">If everything looks correct on the review page, click **Create**.</span></span>

   ![复查 Cosmos DB 帐户设置][COSMOSDB03]

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a><span data-ttu-id="8d759-130">向 Azure Cosmos DB 帐户添加密钥空间</span><span class="sxs-lookup"><span data-stu-id="8d759-130">Add a keyspace to your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="8d759-131">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="8d759-131">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="8d759-132">单击“所有资源”，然后单击你刚才创建的 Azure Cosmos DB 帐户。</span><span class="sxs-lookup"><span data-stu-id="8d759-132">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![选择 Azure Cosmos DB 帐户][COSMOSDB04]

1. <span data-ttu-id="8d759-134">依次单击“数据资源管理器”、“新建密钥空间”。</span><span class="sxs-lookup"><span data-stu-id="8d759-134">Click **Data Explorer**, then click **New Keyspace**.</span></span> <span data-ttu-id="8d759-135">为“密钥空间 ID”输入一个唯一标识符，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="8d759-135">Enter a unique identifier for your **Keyspace id**, then click **OK**.</span></span>

   ![创建 Cosmos DB 密钥空间][COSMOSDB05]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a><span data-ttu-id="8d759-137">检索 Azure Cosmos DB 帐户的连接设置</span><span class="sxs-lookup"><span data-stu-id="8d759-137">Retrieve the connection settings for your Azure Cosmos DB account</span></span>

1. <span data-ttu-id="8d759-138">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="8d759-138">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="8d759-139">单击“所有资源”，然后单击你刚才创建的 Azure Cosmos DB 帐户。</span><span class="sxs-lookup"><span data-stu-id="8d759-139">Click **All Resources**, then click the Azure Cosmos DB account you just created.</span></span>

   ![选择 Azure Cosmos DB 帐户][COSMOSDB04]

1. <span data-ttu-id="8d759-141">单击“连接字符串”，复制“接触点”、“端口”、“用户名”和“主要密码”字段的值；稍后需要使用这些值来配置应用程序。</span><span class="sxs-lookup"><span data-stu-id="8d759-141">Click **Connection strings**, and copy the values for the **Contact Point**, **Port**, **Username**, and **Primary Password** fields; you will use those values to configure your application later.</span></span>

   ![检索 Cosmos DB 连接设置][COSMOSDB05]

## <a name="configure-the-sample-application"></a><span data-ttu-id="8d759-143">配置示例应用程序</span><span class="sxs-lookup"><span data-stu-id="8d759-143">Configure the sample application</span></span>

1. <span data-ttu-id="8d759-144">打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="8d759-144">Open a command shell and clone the sample project using a git command like the following example:</span></span>

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. <span data-ttu-id="8d759-145">在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。</span><span class="sxs-lookup"><span data-stu-id="8d759-145">Locate the *application.properties* file in the *resources* directory of the sample project, or create the file if it does not already exist.</span></span>

1. <span data-ttu-id="8d759-146">在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：</span><span class="sxs-lookup"><span data-stu-id="8d759-146">Open the *application.properties* file in a text editor, and add or configure the following lines in the file, and replace the sample values with the appropriate values from earlier:</span></span>

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmosdb.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   <span data-ttu-id="8d759-147">其中：</span><span class="sxs-lookup"><span data-stu-id="8d759-147">Where:</span></span>

   | <span data-ttu-id="8d759-148">参数</span><span class="sxs-lookup"><span data-stu-id="8d759-148">Parameter</span></span> | <span data-ttu-id="8d759-149">说明</span><span class="sxs-lookup"><span data-stu-id="8d759-149">Description</span></span> |
   |---|---|
   | `spring.data.cassandra.contact-points` | <span data-ttu-id="8d759-150">指定本文上文中所述的**接触点**。</span><span class="sxs-lookup"><span data-stu-id="8d759-150">Specifies the **Contact Point** from earlier in this article.</span></span> |
   | `spring.data.cassandra.port` | <span data-ttu-id="8d759-151">指定本文上文中所述的**端口**。</span><span class="sxs-lookup"><span data-stu-id="8d759-151">Specifies the **Port** from earlier in this article.</span></span> |
   | `spring.data.cassandra.username` | <span data-ttu-id="8d759-152">指定本文上文中所述的**用户名**。</span><span class="sxs-lookup"><span data-stu-id="8d759-152">Specifies your **Username** from earlier in this article.</span></span> |
   | `spring.data.cassandra.password` | <span data-ttu-id="8d759-153">指定本文上文中所述的**主要密码**。</span><span class="sxs-lookup"><span data-stu-id="8d759-153">Specifies your **Primary Password** from earlier in this article.</span></span> |

1. <span data-ttu-id="8d759-154">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="8d759-154">Save and close the *application.properties* file.</span></span>

## <a name="package-and-test-the-sample-application"></a><span data-ttu-id="8d759-155">打包并测试示例应用程序</span><span class="sxs-lookup"><span data-stu-id="8d759-155">Package and test the sample application</span></span> 

1. <span data-ttu-id="8d759-156">使用 Maven 生成示例应用程序，例如：</span><span class="sxs-lookup"><span data-stu-id="8d759-156">Build the sample application with Maven; for example:</span></span>

   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="8d759-157">启动示例应用程序；例如：</span><span class="sxs-lookup"><span data-stu-id="8d759-157">Start the sample application; for example:</span></span>

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. <span data-ttu-id="8d759-158">在命令提示符下使用 `curl` 创建新记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="8d759-158">Create new records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   <span data-ttu-id="8d759-159">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="8d759-159">Your application should return values like the following:</span></span>

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. <span data-ttu-id="8d759-160">在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="8d759-160">Retrieve all of the existing records using `curl` from a command prompt like the following examples:</span></span>

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   <span data-ttu-id="8d759-161">你的应用程序应返回如下所示的值：</span><span class="sxs-lookup"><span data-stu-id="8d759-161">Your application should return values like the following:</span></span>

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a><span data-ttu-id="8d759-162">摘要</span><span class="sxs-lookup"><span data-stu-id="8d759-162">Summary</span></span>

<span data-ttu-id="8d759-163">在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 Azure Cosmos DB Cassandra API 存储和检索信息。</span><span class="sxs-lookup"><span data-stu-id="8d759-163">In this tutorial, you created a sample Java application that uses Spring Data to store and retrieve information using the Azure Cosmos DB Cassandra API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d759-164">后续步骤</span><span class="sxs-lookup"><span data-stu-id="8d759-164">Next steps</span></span>

<span data-ttu-id="8d759-165">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="8d759-165">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8d759-166">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="8d759-166">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="8d759-167">其他资源</span><span class="sxs-lookup"><span data-stu-id="8d759-167">Additional Resources</span></span>

<span data-ttu-id="8d759-168">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="8d759-168">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

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

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
