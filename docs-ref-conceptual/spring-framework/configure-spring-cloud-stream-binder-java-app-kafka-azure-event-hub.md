---
title: 如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用
description: 了解如何配置使用 Spring Boot Initializer 创建的应用程序，以便将 Apache Kafka 与 Azure 事件中心配合使用。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: 00062f5442e072af30036388f2f1f066221d7316
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506327"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a><span data-ttu-id="d04e7-103">如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用</span><span class="sxs-lookup"><span data-stu-id="d04e7-103">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="d04e7-104">概述</span><span class="sxs-lookup"><span data-stu-id="d04e7-104">Overview</span></span>

<span data-ttu-id="d04e7-105">本文介绍如何配置基于 Java 的 Spring Cloud Stream Binder，它是使用 Spring Boot Initializer 创建的，目的是将 [Apache Kafka] 与 Azure 事件中心配合使用。</span><span class="sxs-lookup"><span data-stu-id="d04e7-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder created with the Spring Boot Initializer to use [Apache Kafka] with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d04e7-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="d04e7-106">Prerequisites</span></span>

<span data-ttu-id="d04e7-107">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="d04e7-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="d04e7-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="d04e7-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="d04e7-109">[Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="d04e7-109">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="d04e7-110">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="d04e7-110">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="d04e7-111">完成本文中的步骤需要 Spring Boot 2.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="d04e7-111">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="d04e7-112">使用 Azure 门户创建 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-112">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="d04e7-113">创建 Azure 事件中心命名空间</span><span class="sxs-lookup"><span data-stu-id="d04e7-113">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="d04e7-114">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="d04e7-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="d04e7-115">依次单击“+创建资源”、“物联网”、“事件中心”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-115">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![创建 Azure 事件中心命名空间][IMG01]

1. <span data-ttu-id="d04e7-117">在“创建命名空间”页上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="d04e7-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="d04e7-118">输入一个唯一**名称**，该名称将成为事件中心命名空间 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d04e7-118">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="d04e7-119">例如，如果输入 **wingtiptoys** 作为**名称**，则 URI 将为 *wingtiptoys.servicebus.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="d04e7-119">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="d04e7-120">为事件中心命名空间选择一个“定价层”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-120">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="d04e7-121">为命名空间指定“启用 Kafka”设置。</span><span class="sxs-lookup"><span data-stu-id="d04e7-121">Specify **Enable Kafka** for your namespace.</span></span>
   * <span data-ttu-id="d04e7-122">选择需要用于命名空间的“订阅”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-122">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="d04e7-123">指定是为命名空间创建新的“资源组”，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="d04e7-123">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="d04e7-124">指定事件中心命名空间的“位置”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-124">Specify the **Location** for your event hub namespace.</span></span>
   
   ![指定 Azure 事件中心命名空间选项][IMG02]

1. <span data-ttu-id="d04e7-126">指定上面列出的选项后，请单击“创建”以创建命名空间。</span><span class="sxs-lookup"><span data-stu-id="d04e7-126">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="d04e7-127">在命名空间中创建 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-127">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="d04e7-128">浏览到 <https://portal.azure.com/> 上的 Azure 门户。</span><span class="sxs-lookup"><span data-stu-id="d04e7-128">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="d04e7-129">单击“所有资源”，然后单击已创建的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="d04e7-129">Click **All resources**, and then click the namespace that you created.</span></span>

   ![选择 Azure 事件中心命名空间][IMG03]

1. <span data-ttu-id="d04e7-131">单击“事件中心”，然后单击“+事件中心”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-131">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![添加新的 Azure 事件中心][IMG04]

1. <span data-ttu-id="d04e7-133">在“创建事件中心”页上，为事件中心输入唯一的**名称**，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-133">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![创建 Azure 事件中心][IMG05]

1. <span data-ttu-id="d04e7-135">事件中心在创建后会列在“事件中心”页上。</span><span class="sxs-lookup"><span data-stu-id="d04e7-135">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![创建 Azure 事件中心][IMG06]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="d04e7-137">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="d04e7-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="d04e7-138">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="d04e7-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="d04e7-139">指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="d04e7-139">Specify the following options:</span></span>

   * <span data-ttu-id="d04e7-140">使用 **Java** 生成一个 **Maven** 项目。</span><span class="sxs-lookup"><span data-stu-id="d04e7-140">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="d04e7-141">指定一个其值大于或等于 2.0 的 **Spring Boot** 版本。</span><span class="sxs-lookup"><span data-stu-id="d04e7-141">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="d04e7-142">指定应用程序的“组”和“项目”名称。</span><span class="sxs-lookup"><span data-stu-id="d04e7-142">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="d04e7-143">添加 **Web** 依赖项。</span><span class="sxs-lookup"><span data-stu-id="d04e7-143">Add the **Web** dependency.</span></span>

      ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="d04e7-145">Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.wingtiptoys.kafka。</span><span class="sxs-lookup"><span data-stu-id="d04e7-145">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.kafka*.</span></span>
   >

1. <span data-ttu-id="d04e7-146">指定上面列出的选项后，请单击“生成项目”。</span><span class="sxs-lookup"><span data-stu-id="d04e7-146">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="d04e7-147">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="d04e7-147">When prompted, download the project to a path on your local computer.</span></span>

   ![下载 Spring 项目][SI02]

1. <span data-ttu-id="d04e7-149">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="d04e7-149">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a><span data-ttu-id="d04e7-150">配置 Spring Boot 应用，以便使用 Spring Cloud Kafka Stream和 Azure Event Hub Starter</span><span class="sxs-lookup"><span data-stu-id="d04e7-150">Configure your Spring Boot app to use the Spring Cloud Kafka Stream and Azure Event Hub starters</span></span>

1. <span data-ttu-id="d04e7-151">在应用的根目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-151">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\pom.xml`

   <span data-ttu-id="d04e7-152">-或-</span><span class="sxs-lookup"><span data-stu-id="d04e7-152">-or-</span></span>

   `/users/example/home/kafka/pom.xml`

1. <span data-ttu-id="d04e7-153">在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Kafka Stream Starter 和 Azure Event Hub Starter 添加到 `<dependencies>` 列表：</span><span class="sxs-lookup"><span data-stu-id="d04e7-153">Open the *pom.xml* file in a text editor, and add the Spring Cloud Kafka Stream and Azure Event Hub starters to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][SI03]

1. <span data-ttu-id="d04e7-155">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-155">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="d04e7-156">创建 Azure 凭据文件</span><span class="sxs-lookup"><span data-stu-id="d04e7-156">Create an Azure Credential File</span></span>

1. <span data-ttu-id="d04e7-157">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="d04e7-157">Open a command prompt.</span></span>

1. <span data-ttu-id="d04e7-158">导航到 Spring Boot 应用的 *resources* 目录，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-158">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="d04e7-159">-或-</span><span class="sxs-lookup"><span data-stu-id="d04e7-159">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="d04e7-160">请登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="d04e7-160">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="d04e7-161">列出订阅：</span><span class="sxs-lookup"><span data-stu-id="d04e7-161">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="d04e7-162">Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-162">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. <span data-ttu-id="d04e7-163">创建 Azure 凭据文件：</span><span class="sxs-lookup"><span data-stu-id="d04e7-163">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="d04e7-164">此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="d04e7-164">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="d04e7-165">配置 Spring Boot 应用以使用 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-165">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="d04e7-166">在应用的 *resources* 目录中找到 *application.properties*，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-166">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="d04e7-167">-或-</span><span class="sxs-lookup"><span data-stu-id="d04e7-167">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

1.  <span data-ttu-id="d04e7-168">在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为事件中心的相应属性：</span><span class="sxs-lookup"><span data-stu-id="d04e7-168">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   <span data-ttu-id="d04e7-169">其中：</span><span class="sxs-lookup"><span data-stu-id="d04e7-169">Where:</span></span>
   | <span data-ttu-id="d04e7-170">字段</span><span class="sxs-lookup"><span data-stu-id="d04e7-170">Field</span></span> | <span data-ttu-id="d04e7-171">Description</span><span class="sxs-lookup"><span data-stu-id="d04e7-171">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="d04e7-172">指定之前在本教程中创建的 Azure 凭据文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-172">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="d04e7-173">指定包含 Azure 事件中心的 Azure 资源组。</span><span class="sxs-lookup"><span data-stu-id="d04e7-173">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="d04e7-174">指定你在创建 Azure 事件中心时指定的地理区域。</span><span class="sxs-lookup"><span data-stu-id="d04e7-174">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.azure.eventhub.namespace` | <span data-ttu-id="d04e7-175">指定你在创建 Azure 事件中心命名空间时指定的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="d04e7-175">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span> |
   | `spring.cloud.stream.bindings.input.destination` | <span data-ttu-id="d04e7-176">指定输入目标 Azure 事件中心。在本教程中，它是你此前在本教程中创建的中心。</span><span class="sxs-lookup"><span data-stu-id="d04e7-176">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span> |
   | `spring.cloud.stream.bindings.input.group `| <span data-ttu-id="d04e7-177">在 Azure 事件中心指定一个使用者组，该组可以设置为“$Default”，以便使用你在创建 Azure 事件中心时创建的基本使用者组。</span><span class="sxs-lookup"><span data-stu-id="d04e7-177">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   | `spring.cloud.stream.bindings.output.destination` | <span data-ttu-id="d04e7-178">指定输出目标 Azure 事件中心。在本教程中，它与输入目标相同。</span><span class="sxs-lookup"><span data-stu-id="d04e7-178">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span> |

1. <span data-ttu-id="d04e7-179">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-179">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="d04e7-180">添加示例代码以实现事件中心的基本功能</span><span class="sxs-lookup"><span data-stu-id="d04e7-180">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="d04e7-181">在本部分，请创建所需的 Java 类，以便将事件发送到事件中心。</span><span class="sxs-lookup"><span data-stu-id="d04e7-181">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="d04e7-182">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="d04e7-182">Modify the main application class</span></span>

1. <span data-ttu-id="d04e7-183">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-183">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   <span data-ttu-id="d04e7-184">-或-</span><span class="sxs-lookup"><span data-stu-id="d04e7-184">-or-</span></span>

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. <span data-ttu-id="d04e7-185">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="d04e7-185">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="d04e7-186">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-186">Save and close the main application Java file.</span></span>


### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="d04e7-187">为源连接器创建新类</span><span class="sxs-lookup"><span data-stu-id="d04e7-187">Create a new class for the source connector</span></span>

1. <span data-ttu-id="d04e7-188">在应用的包目录中创建名为 *KafkaSource.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="d04e7-188">Create a new Java file named *KafkaSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   
   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. <span data-ttu-id="d04e7-189">保存并关闭 *KafkaSource.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-189">Save and close the *KafkaSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="d04e7-190">为接收器连接器创建新类</span><span class="sxs-lookup"><span data-stu-id="d04e7-190">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="d04e7-191">在应用的包目录中创建名为 *KafkaSink.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="d04e7-191">Create a new Java file named *KafkaSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.kafka;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   
   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. <span data-ttu-id="d04e7-192">保存并关闭 *KafkaSink.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="d04e7-192">Save and close the *KafkaSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="d04e7-193">生成和测试应用程序</span><span class="sxs-lookup"><span data-stu-id="d04e7-193">Build and test your application</span></span>

1. <span data-ttu-id="d04e7-194">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-194">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\kafka`

   <span data-ttu-id="d04e7-195">-或-</span><span class="sxs-lookup"><span data-stu-id="d04e7-195">-or-</span></span>

   `cd /users/example/home/kafka`

1. <span data-ttu-id="d04e7-196">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-196">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="d04e7-197">应用程序运行以后，即可使用 *curl* 对其进行测试，例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-197">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="d04e7-198">此时会看到“hello”发布到应用程序的日志中。</span><span class="sxs-lookup"><span data-stu-id="d04e7-198">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="d04e7-199">例如：</span><span class="sxs-lookup"><span data-stu-id="d04e7-199">For example:</span></span>

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> <span data-ttu-id="d04e7-200">若要进行测试，可以修改 *KafkaSource.java*，使之包含简单的 HTML 窗体，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="d04e7-200">For testing purposes, you could modify your *KafkaSource.java* so that it contains a simple HTML form like the following example:</span></span>
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> <span data-ttu-id="d04e7-201">这样就可以使用 Web 浏览器来测试应用程序：</span><span class="sxs-lookup"><span data-stu-id="d04e7-201">This will allow you to use a web browser to test your application:</span></span>
> 
> ![使用 Web 浏览器测试应用程序][TB01]
> 
> <span data-ttu-id="d04e7-203">提交窗体后，应用程序会显示结果：</span><span class="sxs-lookup"><span data-stu-id="d04e7-203">When you submit the form, your application will display the results:</span></span>
> 
> ![Web 浏览器中的应用程序响应][TB02]
> 

## <a name="next-steps"></a><span data-ttu-id="d04e7-205">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d04e7-205">Next steps</span></span>

<span data-ttu-id="d04e7-206">有关 Event Hub Stream Binder 和 Apache Kafka 的 Azure 支持的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="d04e7-206">For more information about Azure support for Event Hub Stream Binder and Apache Kafka, see the following articles:</span></span>

* [<span data-ttu-id="d04e7-207">什么是 Azure 事件中心？</span><span class="sxs-lookup"><span data-stu-id="d04e7-207">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="d04e7-208">适用于 Apache Kafka 的 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-208">Azure Event Hubs for Apache Kafka</span></span>](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [<span data-ttu-id="d04e7-209">使用 Azure 门户创建事件中心命名空间和事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-209">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="d04e7-210">创建启用了 Apache Kafka 的事件中心</span><span class="sxs-lookup"><span data-stu-id="d04e7-210">Create Apache Kafka enabled event hubs</span></span>](/azure/event-hubs/event-hubs-create-kafka-enabled)

<span data-ttu-id="d04e7-211">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="d04e7-211">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="d04e7-212">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="d04e7-212">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="d04e7-213">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="d04e7-213">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="d04e7-214">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="d04e7-214">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="d04e7-215">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="d04e7-215">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
