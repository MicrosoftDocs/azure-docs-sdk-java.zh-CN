---
title: 如何使用 Azure 事件中心创建Spring Cloud Stream Binder 应用程序
description: 了解如何配置基于 Java 的 Spring Cloud Stream Binder 应用程序，该应用程序是在 Azure 事件中心使用 Spring Boot Initializr 创建的。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 11/21/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: 49fd85690d21fa2eb4a2830e3958ef21cbd2e8c1
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338891"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a><span data-ttu-id="49fc3-103">如何使用 Azure 事件中心创建Spring Cloud Stream Binder 应用程序</span><span class="sxs-lookup"><span data-stu-id="49fc3-103">How to create a Spring Cloud Stream Binder application with Azure Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="49fc3-104">概述</span><span class="sxs-lookup"><span data-stu-id="49fc3-104">Overview</span></span>

<span data-ttu-id="49fc3-105">本文介绍如何配置基于 Java 的 Spring Cloud Stream Binder 应用程序，该应用程序是在 Azure 事件中心使用 Spring Boot Initializer 创建的。</span><span class="sxs-lookup"><span data-stu-id="49fc3-105">This article demonstrates how to configure a Java-based Spring Cloud Stream Binder application created with the Spring Boot Initializer with Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49fc3-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="49fc3-106">Prerequisites</span></span>

<span data-ttu-id="49fc3-107">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="49fc3-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="49fc3-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="49fc3-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="49fc3-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="49fc3-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="49fc3-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="49fc3-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="49fc3-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="49fc3-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="49fc3-112">完成本文中的步骤需要 Spring Boot 2.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="49fc3-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a><span data-ttu-id="49fc3-113">使用 Azure 门户创建 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="49fc3-113">Create an Azure Event Hub using the Azure portal</span></span>

### <a name="create-an-azure-event-hub-namespace"></a><span data-ttu-id="49fc3-114">创建 Azure 事件中心命名空间</span><span class="sxs-lookup"><span data-stu-id="49fc3-114">Create an Azure Event Hub Namespace</span></span>

1. <span data-ttu-id="49fc3-115">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="49fc3-115">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="49fc3-116">依次单击“+创建资源”、“物联网”、“事件中心”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-116">Click **+Create a resource**, then **Internet of Things**, and then click **Event Hubs**.</span></span>

   ![创建 Azure 事件中心命名空间][IMG01]

1. <span data-ttu-id="49fc3-118">在“创建命名空间”页上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="49fc3-118">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="49fc3-119">输入一个唯一**名称**，该名称将成为事件中心命名空间 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="49fc3-119">Enter a unique **Name**, which will become part of the URI for your event hub namespace.</span></span> <span data-ttu-id="49fc3-120">例如，如果输入 **wingtiptoys** 作为**名称**，则 URI 将为 *wingtiptoys.servicebus.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="49fc3-120">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.servicebus.windows.net*.</span></span>
   * <span data-ttu-id="49fc3-121">为事件中心命名空间选择一个“定价层”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-121">Choose a **Pricing tier** for your event hub namespace.</span></span>
   * <span data-ttu-id="49fc3-122">选择需要用于命名空间的“订阅”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-122">Choose the **Subscription** you want to use for your namespace.</span></span>
   * <span data-ttu-id="49fc3-123">指定是为命名空间创建新的“资源组”，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="49fc3-123">Specify whether to create a new **Resource group** for your namespace, or choose an existing resource group.</span></span>
   * <span data-ttu-id="49fc3-124">指定事件中心命名空间的“位置”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-124">Specify the **Location** for your event hub namespace.</span></span>

   ![指定 Azure 事件中心命名空间选项][IMG02]

1. <span data-ttu-id="49fc3-126">指定上面列出的选项后，请单击“创建”以创建命名空间。</span><span class="sxs-lookup"><span data-stu-id="49fc3-126">When you have specified the options listed above, click **Create** to create your namespace.</span></span>

### <a name="create-an-azure-event-hub-in-your-namespace"></a><span data-ttu-id="49fc3-127">在命名空间中创建 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="49fc3-127">Create an Azure Event Hub in your namespace</span></span>

1. <span data-ttu-id="49fc3-128">浏览到 <https://portal.azure.com/> 上的 Azure 门户。</span><span class="sxs-lookup"><span data-stu-id="49fc3-128">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="49fc3-129">单击“所有资源”，然后单击已创建的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="49fc3-129">Click **All resources**, and then click the namespace that you created.</span></span>

   ![选择 Azure 事件中心命名空间][IMG03]

1. <span data-ttu-id="49fc3-131">单击“事件中心”，然后单击“+事件中心”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-131">Click **Event Hubs**, and then click **+Event Hub**.</span></span>

   ![添加新的 Azure 事件中心][IMG04]

1. <span data-ttu-id="49fc3-133">在“创建事件中心”页上，为事件中心输入唯一的**名称**，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-133">On the **Create Event Hub** page, enter a unique **Name** for your Event Hub, and then click **Create**.</span></span>

   ![创建 Azure 事件中心][IMG05]

1. <span data-ttu-id="49fc3-135">事件中心在创建后会列在“事件中心”页上。</span><span class="sxs-lookup"><span data-stu-id="49fc3-135">When your Event Hub has been created, it will be listed on the **Event Hubs** page.</span></span>

   ![创建 Azure 事件中心][IMG06]

### <a name="create-an-azure-storage-account-for-your-event-hub-checkpoints"></a><span data-ttu-id="49fc3-137">为事件中心检查点创建 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="49fc3-137">Create an Azure Storage Account for your Event Hub checkpoints</span></span>

1. <span data-ttu-id="49fc3-138">浏览到 <https://portal.azure.com/> 上的 Azure 门户。</span><span class="sxs-lookup"><span data-stu-id="49fc3-138">Browse to the Azure portal at <https://portal.azure.com/>.</span></span>

1. <span data-ttu-id="49fc3-139">依次单击“+创建资源”、“存储”、“存储帐户”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-139">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![创建 Azure 存储帐户][IMG07]

1. <span data-ttu-id="49fc3-141">在“创建命名空间”页上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="49fc3-141">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="49fc3-142">输入一个唯一**名称**，该名称将成为存储帐户 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="49fc3-142">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="49fc3-143">例如，如果输入 **wingtiptoys** 作为**名称**，则 URI 将为 *wingtiptoys.core.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="49fc3-143">For example: if you entered **wingtiptoys** for the **Name**, the URI would be *wingtiptoys.core.windows.net*.</span></span>
   * <span data-ttu-id="49fc3-144">选择“Blob 存储”作为“帐户类型”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-144">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="49fc3-145">指定存储帐户的“位置”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-145">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="49fc3-146">选择需要用于存储帐户的“订阅”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-146">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="49fc3-147">指定是为存储帐户创建新的“资源组”，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="49fc3-147">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>

   ![指定 Azure 存储帐户选项][IMG08]

1. <span data-ttu-id="49fc3-149">指定上面列出的选项后，请单击“创建”以创建存储帐户。</span><span class="sxs-lookup"><span data-stu-id="49fc3-149">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="49fc3-150">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="49fc3-150">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="49fc3-151">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="49fc3-151">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="49fc3-152">指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="49fc3-152">Specify the following options:</span></span>

   * <span data-ttu-id="49fc3-153">使用 **Java** 生成一个 **Maven** 项目。</span><span class="sxs-lookup"><span data-stu-id="49fc3-153">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="49fc3-154">指定一个其值大于或等于 2.0 的 **Spring Boot** 版本。</span><span class="sxs-lookup"><span data-stu-id="49fc3-154">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="49fc3-155">指定应用程序的“组”和“项目”名称。</span><span class="sxs-lookup"><span data-stu-id="49fc3-155">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="49fc3-156">添加 **Web** 依赖项。</span><span class="sxs-lookup"><span data-stu-id="49fc3-156">Add the **Web** dependency.</span></span>

      ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="49fc3-158">Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.wingtiptoys.eventhub。</span><span class="sxs-lookup"><span data-stu-id="49fc3-158">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.eventhub*.</span></span>
   >

1. <span data-ttu-id="49fc3-159">指定上面列出的选项后，请单击“生成项目”。</span><span class="sxs-lookup"><span data-stu-id="49fc3-159">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="49fc3-160">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="49fc3-160">When prompted, download the project to a path on your local computer.</span></span>

   ![下载 Spring 项目][SI02]

1. <span data-ttu-id="49fc3-162">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="49fc3-162">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-event-hub-starter"></a><span data-ttu-id="49fc3-163">配置 Spring Boot 应用，以使用 Azure Event Hub Starter</span><span class="sxs-lookup"><span data-stu-id="49fc3-163">Configure your Spring Boot app to use the Azure Event Hub starter</span></span>

1. <span data-ttu-id="49fc3-164">在应用的根目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-164">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\pom.xml`

   <span data-ttu-id="49fc3-165">-或-</span><span class="sxs-lookup"><span data-stu-id="49fc3-165">-or-</span></span>

   `/users/example/home/eventhub/pom.xml`

1. <span data-ttu-id="49fc3-166">在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Azure Event Hub Stream Binder Starter 添加到 `<dependencies>` 列表：</span><span class="sxs-lookup"><span data-stu-id="49fc3-166">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Event Hub Stream Binder starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-eventhub-stream-binder</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][SI03]

1. <span data-ttu-id="49fc3-168">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="49fc3-168">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="49fc3-169">创建 Azure 凭据文件</span><span class="sxs-lookup"><span data-stu-id="49fc3-169">Create an Azure Credential File</span></span>

1. <span data-ttu-id="49fc3-170">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="49fc3-170">Open a command prompt.</span></span>

1. <span data-ttu-id="49fc3-171">导航到 Spring Boot 应用的 *resources* 目录，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-171">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   <span data-ttu-id="49fc3-172">-或-</span><span class="sxs-lookup"><span data-stu-id="49fc3-172">-or-</span></span>

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. <span data-ttu-id="49fc3-173">请登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="49fc3-173">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="49fc3-174">列出订阅：</span><span class="sxs-lookup"><span data-stu-id="49fc3-174">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="49fc3-175">Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-175">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="49fc3-176">创建 Azure 凭据文件：</span><span class="sxs-lookup"><span data-stu-id="49fc3-176">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="49fc3-177">此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="49fc3-177">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a><span data-ttu-id="49fc3-178">配置 Spring Boot 应用以使用 Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="49fc3-178">Configure your Spring Boot app to use your Azure Event Hub</span></span>

1. <span data-ttu-id="49fc3-179">在应用的 *resources* 目录中找到 *application.properties*，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-179">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   <span data-ttu-id="49fc3-180">-或-</span><span class="sxs-lookup"><span data-stu-id="49fc3-180">-or-</span></span>

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. <span data-ttu-id="49fc3-181">在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为事件中心的相应属性：</span><span class="sxs-lookup"><span data-stu-id="49fc3-181">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your event hub:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace
   spring.cloud.azure.eventhub.checkpoint-storage-account=wingtiptoysstorage
   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   spring.cloud.stream.eventhub.bindings.input.consumer.checkpoint-mode=MANUAL
   ```
   <span data-ttu-id="49fc3-182">其中：</span><span class="sxs-lookup"><span data-stu-id="49fc3-182">Where:</span></span>

   |                          <span data-ttu-id="49fc3-183">字段</span><span class="sxs-lookup"><span data-stu-id="49fc3-183">Field</span></span>                           |                                                                                   <span data-ttu-id="49fc3-184">说明</span><span class="sxs-lookup"><span data-stu-id="49fc3-184">Description</span></span>                                                                                    |
   |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |        `spring.cloud.azure.credential-file-path`         |                                                    <span data-ttu-id="49fc3-185">指定之前在本教程中创建的 Azure 凭据文件。</span><span class="sxs-lookup"><span data-stu-id="49fc3-185">Specifies Azure credential file that you created earlier in this tutorial.</span></span>                                                    |
   |           `spring.cloud.azure.resource-group`            |                                                      <span data-ttu-id="49fc3-186">指定包含 Azure 事件中心的 Azure 资源组。</span><span class="sxs-lookup"><span data-stu-id="49fc3-186">Specifies the Azure Resource Group that contains your Azure Event Hub.</span></span>                                                      |
   |               `spring.cloud.azure.region`                |                                           <span data-ttu-id="49fc3-187">指定你在创建 Azure 事件中心时指定的地理区域。</span><span class="sxs-lookup"><span data-stu-id="49fc3-187">Specifies the geographical region that you specified when you created your Azure Event Hub.</span></span>                                            |
   |         `spring.cloud.azure.eventhub.namespace`          |                                          <span data-ttu-id="49fc3-188">指定你在创建 Azure 事件中心命名空间时指定的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="49fc3-188">Specifies the unique name that you specified when you created your Azure Event Hub Namespace.</span></span>                                           |
   | `spring.cloud.azure.eventhub.checkpoint-storage-account` |                                                    <span data-ttu-id="49fc3-189">指定之前在本教程中创建的 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="49fc3-189">Specifies Azure Storage Account that you created earlier in this tutorial.</span></span>                                                    |
   |     `spring.cloud.stream.bindings.input.destination`     |                            <span data-ttu-id="49fc3-190">指定输入目标 Azure 事件中心。在本教程中，它是你此前在本教程中创建的中心。</span><span class="sxs-lookup"><span data-stu-id="49fc3-190">Specifies the input destination Azure Event Hub, which for this tutorial is the  hub you created earlier in this tutorial.</span></span>                            |
   |       `spring.cloud.stream.bindings.input.group `        | <span data-ttu-id="49fc3-191">在 Azure 事件中心指定一个使用者组，该组可以设置为“$Default”，以便使用你在创建 Azure 事件中心时创建的基本使用者组。</span><span class="sxs-lookup"><span data-stu-id="49fc3-191">Specifies a Consumer Group from Azure Event Hub, which can be set to '$Default' in order to use the basic consumer group that was created when you created your Azure Event Hub.</span></span> |
   |    `spring.cloud.stream.bindings.output.destination`     |                               <span data-ttu-id="49fc3-192">指定输出目标 Azure 事件中心。在本教程中，它与输入目标相同。</span><span class="sxs-lookup"><span data-stu-id="49fc3-192">Specifies the output destination Azure Event Hub, which for this tutorial will be the same as the input destination.</span></span>                               |


3. <span data-ttu-id="49fc3-193">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="49fc3-193">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a><span data-ttu-id="49fc3-194">添加示例代码以实现事件中心的基本功能</span><span class="sxs-lookup"><span data-stu-id="49fc3-194">Add sample code to implement basic event hub functionality</span></span>

<span data-ttu-id="49fc3-195">在本部分，请创建所需的 Java 类，以便将事件发送到事件中心。</span><span class="sxs-lookup"><span data-stu-id="49fc3-195">In this section, you create the necessary Java classes for sending events to your event hub.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="49fc3-196">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="49fc3-196">Modify the main application class</span></span>

1. <span data-ttu-id="49fc3-197">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-197">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\eventhub\src\main\java\com\wingtiptoys\eventhub\EventhubApplication.java`

   <span data-ttu-id="49fc3-198">-或-</span><span class="sxs-lookup"><span data-stu-id="49fc3-198">-or-</span></span>

   `/users/example/home/eventhub/src/main/java/com/wingtiptoys/eventhub/EventhubApplication.java`

1. <span data-ttu-id="49fc3-199">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="49fc3-199">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class EventhubApplication {
      public static void main(String[] args) {
         SpringApplication.run(EventhubApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="49fc3-200">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="49fc3-200">Save and close the main application Java file.</span></span>

### <a name="create-a-new-class-for-the-source-connector"></a><span data-ttu-id="49fc3-201">为源连接器创建新类</span><span class="sxs-lookup"><span data-stu-id="49fc3-201">Create a new class for the source connector</span></span>

1. <span data-ttu-id="49fc3-202">在应用的包目录中创建名为 *EventhubSource.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="49fc3-202">Create a new Java file named *EventhubSource.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class EventhubSource {

      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String postMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```
1. <span data-ttu-id="49fc3-203">保存 *EventhubSource.java* 文件后将其关闭。</span><span class="sxs-lookup"><span data-stu-id="49fc3-203">Save and close the *EventhubSource.java* file.</span></span>

### <a name="create-a-new-class-for-the-sink-connector"></a><span data-ttu-id="49fc3-204">为接收器连接器创建新类</span><span class="sxs-lookup"><span data-stu-id="49fc3-204">Create a new class for the sink connector</span></span>

1. <span data-ttu-id="49fc3-205">在应用的包目录中创建名为 *EventhubSink.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：</span><span class="sxs-lookup"><span data-stu-id="49fc3-205">Create a new Java file named *EventhubSink.java* in the package directory of your app, then open the file in a text editor and add the following lines:</span></span>

   ```java
   package com.wingtiptoys.eventhub;

   import com.microsoft.azure.spring.integration.core.AzureHeaders;
   import com.microsoft.azure.spring.integration.core.api.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.handler.annotation.Header;

   @EnableBinding(Sink.class)
   public class EventhubSink {

      private static final Logger LOGGER = LoggerFactory.getLogger(EventhubSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
         LOGGER.info("New message received: '{}'", message);
         checkpointer.success().handle((r, ex) -> {
            if (ex == null) {
               LOGGER.info("Message '{}' successfully checkpointed", message);
            }
            return null;
         });
      }
   }
   ```

1. <span data-ttu-id="49fc3-206">保存 *EventhubSink.java* 文件后将其关闭。</span><span class="sxs-lookup"><span data-stu-id="49fc3-206">Save and close the *EventhubSink.java* file.</span></span>

## <a name="build-and-test-your-application"></a><span data-ttu-id="49fc3-207">生成和测试应用程序</span><span class="sxs-lookup"><span data-stu-id="49fc3-207">Build and test your application</span></span>

1. <span data-ttu-id="49fc3-208">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-208">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\eventhub`

   <span data-ttu-id="49fc3-209">-或-</span><span class="sxs-lookup"><span data-stu-id="49fc3-209">-or-</span></span>

   `cd /users/example/home/eventhub`

1. <span data-ttu-id="49fc3-210">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-210">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="49fc3-211">应用程序运行以后，即可使用 *curl* 对其进行测试，例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-211">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   <span data-ttu-id="49fc3-212">此时会看到“hello”发布到应用程序的日志中。</span><span class="sxs-lookup"><span data-stu-id="49fc3-212">You should see "hello" posted to your application's logs.</span></span> <span data-ttu-id="49fc3-213">例如：</span><span class="sxs-lookup"><span data-stu-id="49fc3-213">For example:</span></span>

   ```shell
   [Thread-13] INFO com.wingtiptoys.eventhub.EventhubSink - New message received: 'hello'
   [pool-10-thread-7] INFO com.wingtiptoys.eventhub.EventhubSink - Message 'hello' successfully checkpointed
   ```

## <a name="next-steps"></a><span data-ttu-id="49fc3-214">后续步骤</span><span class="sxs-lookup"><span data-stu-id="49fc3-214">Next steps</span></span>

<span data-ttu-id="49fc3-215">有关 Event Hub Stream Binder 的 Azure 支持的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="49fc3-215">For more information about Azure support for Event Hub Stream Binder, see the following articles:</span></span>

* [<span data-ttu-id="49fc3-216">什么是 Azure 事件中心？</span><span class="sxs-lookup"><span data-stu-id="49fc3-216">What is Azure Event Hubs?</span></span>](/azure/event-hubs/event-hubs-about)

* [<span data-ttu-id="49fc3-217">使用 Azure 门户创建事件中心命名空间和事件中心</span><span class="sxs-lookup"><span data-stu-id="49fc3-217">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>](/azure/event-hubs/event-hubs-create)

* [<span data-ttu-id="49fc3-218">如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用</span><span class="sxs-lookup"><span data-stu-id="49fc3-218">How to use the Spring Boot Starter for Apache Kafka with Azure Event Hubs</span></span>](configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub.md)

<span data-ttu-id="49fc3-219">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="49fc3-219">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="49fc3-220">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="49fc3-220">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="49fc3-221">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="49fc3-221">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="49fc3-222">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="49fc3-222">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="49fc3-223">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="49fc3-223">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

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

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-03.png
