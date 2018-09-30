---
title: 如何使用适用于 Azure 存储的 Spring Boot 起动器
description: 了解如何使用 Azure 存储起动器配置 Spring Boot Initializer 应用。
services: storage
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 1a219a066f0f89adbf3f541856b36b842520bfbb
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46505915"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a><span data-ttu-id="604d0-103">如何使用适用于 Azure 存储的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="604d0-103">How to use the Spring Boot Starter for Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="604d0-104">概述</span><span class="sxs-lookup"><span data-stu-id="604d0-104">Overview</span></span>

<span data-ttu-id="604d0-105">本文逐步讲解如何使用 **Spring Initializr** 创建自定义应用程序，然后将 Azure Storage Starter 添加到应用程序，再使用应用程序将 Blob 上传到 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="604d0-105">This article walks you through creating a custom application using the **Spring Initializr**, then adding the Azure storage starter to your application, and then using your application to upload a blob to your Azure storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="604d0-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="604d0-106">Prerequisites</span></span>

<span data-ttu-id="604d0-107">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="604d0-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="604d0-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册[免费 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="604d0-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="604d0-109">[Azure 命令行接口 (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="604d0-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="604d0-110">最新的 [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="604d0-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="604d0-111">Apache 的 [Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="604d0-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="604d0-112">完成本文中的步骤需要 Spring Boot 2.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="604d0-112">Spring Boot version 2.0 or greater is required to complete the steps in this article.</span></span>
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a><span data-ttu-id="604d0-113">为应用程序创建 Azure 存储帐户和 Blob 容器</span><span class="sxs-lookup"><span data-stu-id="604d0-113">Create an Azure Storage Account and blob container for your application</span></span>

1. <span data-ttu-id="604d0-114">浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。</span><span class="sxs-lookup"><span data-stu-id="604d0-114">Browse to the Azure portal at <https://portal.azure.com/> and sign in.</span></span>

1. <span data-ttu-id="604d0-115">依次单击“+创建资源”、“存储”、“存储帐户”。</span><span class="sxs-lookup"><span data-stu-id="604d0-115">Click **+Create a resource**, then **Storage**, and then click **Storage Account**.</span></span>

   ![创建 Azure 存储帐户][IMG01]

1. <span data-ttu-id="604d0-117">在“创建命名空间”页上，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="604d0-117">On the **Create Namespace** page, enter the following information:</span></span>

   * <span data-ttu-id="604d0-118">输入一个唯一**名称**，该名称将成为存储帐户 URI 的一部分。</span><span class="sxs-lookup"><span data-stu-id="604d0-118">Enter a unique **Name**, which will become part of the URI for your storage account.</span></span> <span data-ttu-id="604d0-119">例如，如果输入 **wingtiptoysstorage** 作为**名称**，则 URI 将为 *wingtiptoysstorage.core.windows.net*。</span><span class="sxs-lookup"><span data-stu-id="604d0-119">For example: if you entered **wingtiptoysstorage** for the **Name**, the URI would be *wingtiptoysstorage.core.windows.net*.</span></span>
   * <span data-ttu-id="604d0-120">选择“Blob 存储”作为“帐户类型”。</span><span class="sxs-lookup"><span data-stu-id="604d0-120">Choose **Blob storage** for the **Account kind**.</span></span>
   * <span data-ttu-id="604d0-121">指定存储帐户的“位置”。</span><span class="sxs-lookup"><span data-stu-id="604d0-121">Specify the **Location** for your storage account.</span></span>
   * <span data-ttu-id="604d0-122">选择需要用于存储帐户的“订阅”。</span><span class="sxs-lookup"><span data-stu-id="604d0-122">Choose the **Subscription** you want to use for your storage account.</span></span>
   * <span data-ttu-id="604d0-123">指定是为存储帐户创建新的“资源组”，还是选择现有资源组。</span><span class="sxs-lookup"><span data-stu-id="604d0-123">Specify whether to create a new **Resource group** for your storage account, or choose an existing resource group.</span></span>
   
   ![指定 Azure 存储帐户选项][IMG02]

1. <span data-ttu-id="604d0-125">指定上面列出的选项后，请单击“创建”以创建存储帐户。</span><span class="sxs-lookup"><span data-stu-id="604d0-125">When you have specified the options listed above, click **Create** to create your storage account.</span></span>

1. <span data-ttu-id="604d0-126">等到 Azure 门户创建好你的存储帐户以后，请单击“Blob”，然后单击“+容器”。</span><span class="sxs-lookup"><span data-stu-id="604d0-126">When the Azure portal has created your storage account, click **Blobs**, then click **+Container**.</span></span>

   ![创建 Blob 容器][IMG03]

1. <span data-ttu-id="604d0-128">输入 Blob 容器的“名称”，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="604d0-128">Enter a **Name** for your blob container, and then click **OK**.</span></span>

   ![指定 Blob 容器选项][IMG04]

1. <span data-ttu-id="604d0-130">Blob 容器创建好以后，Azure 门户会将其列出。</span><span class="sxs-lookup"><span data-stu-id="604d0-130">The Azure portal will list your blob container after is has been created.</span></span>

   ![查看 Blob 容器列表][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="604d0-132">使用 Spring Initializr 创建简单的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="604d0-132">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="604d0-133">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="604d0-133">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="604d0-134">指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="604d0-134">Specify the following options:</span></span>

   * <span data-ttu-id="604d0-135">使用 **Java** 生成一个 **Maven** 项目。</span><span class="sxs-lookup"><span data-stu-id="604d0-135">Generate a **Maven** project with **Java**.</span></span>
   * <span data-ttu-id="604d0-136">指定一个其值大于或等于 2.0 的 **Spring Boot** 版本。</span><span class="sxs-lookup"><span data-stu-id="604d0-136">Specify a **Spring Boot** version that is equal to or greater than 2.0.</span></span>
   * <span data-ttu-id="604d0-137">指定应用程序的“组”和“项目”名称。</span><span class="sxs-lookup"><span data-stu-id="604d0-137">Specify the **Group** and **Artifact** names for your application.</span></span>
   * <span data-ttu-id="604d0-138">添加 **Web** 依赖项。</span><span class="sxs-lookup"><span data-stu-id="604d0-138">Add the **Web** dependency.</span></span>

      ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="604d0-140">Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.wingtiptoys.storage。</span><span class="sxs-lookup"><span data-stu-id="604d0-140">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.wingtiptoys.storage*.</span></span>
   >

1. <span data-ttu-id="604d0-141">指定上面列出的选项后，请单击“生成项目”。</span><span class="sxs-lookup"><span data-stu-id="604d0-141">When you have specified the options listed above, click **Generate Project**.</span></span>

1. <span data-ttu-id="604d0-142">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="604d0-142">When prompted, download the project to a path on your local computer.</span></span>

   ![下载 Spring 项目][SI02]

1. <span data-ttu-id="604d0-144">在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。</span><span class="sxs-lookup"><span data-stu-id="604d0-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a><span data-ttu-id="604d0-145">配置 Spring Boot 应用，以使用 Azure Storage Starter</span><span class="sxs-lookup"><span data-stu-id="604d0-145">Configure your Spring Boot app to use the Azure Storage starter</span></span>

1. <span data-ttu-id="604d0-146">在应用的根目录中找到 pom.xml 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-146">Locate the *pom.xml* file in the root directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\pom.xml`

   <span data-ttu-id="604d0-147">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-147">-or-</span></span>

   `/users/example/home/storage/pom.xml`

1. <span data-ttu-id="604d0-148">在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Azure Storage Starter 添加到 `<dependencies>` 列表：</span><span class="sxs-lookup"><span data-stu-id="604d0-148">Open the *pom.xml* file in a text editor, and add the Spring Cloud Azure Storage starter to the list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][SI03]

1. <span data-ttu-id="604d0-150">保存并关闭 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="604d0-150">Save and close the *pom.xml* file.</span></span>

## <a name="create-an-azure-credential-file"></a><span data-ttu-id="604d0-151">创建 Azure 凭据文件</span><span class="sxs-lookup"><span data-stu-id="604d0-151">Create an Azure Credential File</span></span>

1. <span data-ttu-id="604d0-152">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="604d0-152">Open a command prompt.</span></span>

1. <span data-ttu-id="604d0-153">导航到 Spring Boot 应用的 *resources* 目录，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-153">Navigate to the *resources* directory of your Spring Boot app; for example:</span></span>

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   <span data-ttu-id="604d0-154">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-154">-or-</span></span>

   ```shell
   cd /users/example/home/storage/src/main/resources
   ```

1. <span data-ttu-id="604d0-155">请登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="604d0-155">Sign in to your Azure account:</span></span>

   ```azurecli
   az login
   ```

1. <span data-ttu-id="604d0-156">列出订阅：</span><span class="sxs-lookup"><span data-stu-id="604d0-156">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="604d0-157">Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-157">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="604d0-158">创建 Azure 凭据文件：</span><span class="sxs-lookup"><span data-stu-id="604d0-158">Create your Azure Credential file:</span></span>

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   <span data-ttu-id="604d0-159">此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="604d0-159">This command will create a *my.azureauth* file in your *resources* directory with contents that resemble the following example:</span></span>

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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a><span data-ttu-id="604d0-160">配置 Spring Boot 应用以使用 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="604d0-160">Configure your Spring Boot app to use your Azure Storage account</span></span>

1. <span data-ttu-id="604d0-161">在应用的 *resources* 目录中找到 *application.properties*，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-161">Locate the *application.properties* in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   <span data-ttu-id="604d0-162">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-162">-or-</span></span>

   `/users/example/home/storage/src/main/resources/application.properties`

1.  <span data-ttu-id="604d0-163">在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为存储帐户的相应属性：</span><span class="sxs-lookup"><span data-stu-id="604d0-163">Open the *application.properties* file in a text editor, add the following lines, and then replace the sample values with the appropriate properties for your storage account:</span></span>

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   <span data-ttu-id="604d0-164">其中：</span><span class="sxs-lookup"><span data-stu-id="604d0-164">Where:</span></span>
   | <span data-ttu-id="604d0-165">字段</span><span class="sxs-lookup"><span data-stu-id="604d0-165">Field</span></span> | <span data-ttu-id="604d0-166">Description</span><span class="sxs-lookup"><span data-stu-id="604d0-166">Description</span></span> |
   | ---|---|
   | `spring.cloud.azure.credential-file-path` | <span data-ttu-id="604d0-167">指定之前在本教程中创建的 Azure 凭据文件。</span><span class="sxs-lookup"><span data-stu-id="604d0-167">Specifies Azure credential file that you created earlier in this tutorial.</span></span> |
   | `spring.cloud.azure.resource-group` | <span data-ttu-id="604d0-168">指定包含 Azure 存储帐户的 Azure 资源组。</span><span class="sxs-lookup"><span data-stu-id="604d0-168">Specifies the Azure Resource Group that contains your Azure Storage account.</span></span> |
   | `spring.cloud.azure.region` | <span data-ttu-id="604d0-169">指定你在创建 Azure 存储帐户时指定的地理区域。</span><span class="sxs-lookup"><span data-stu-id="604d0-169">Specifies the geographical region that you specified when you created your Azure Storage account.</span></span> |
   | `spring.cloud.azure.storage.account` | <span data-ttu-id="604d0-170">指定之前在本教程中创建的 Azure 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="604d0-170">Specifies Azure Storage account that you created earlier in this tutorial.</span></span>

1. <span data-ttu-id="604d0-171">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="604d0-171">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a><span data-ttu-id="604d0-172">添加示例代码以实现 Azure 存储的基本功能</span><span class="sxs-lookup"><span data-stu-id="604d0-172">Add sample code to implement basic Azure storage functionality</span></span>

<span data-ttu-id="604d0-173">在本部分，请创建所需的 Java 类，用于在 Azure 存储帐户中存储 Blob。</span><span class="sxs-lookup"><span data-stu-id="604d0-173">In this section, you create the necessary Java classes for storing a blob in your Azure storage account.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="604d0-174">修改主应用程序类</span><span class="sxs-lookup"><span data-stu-id="604d0-174">Modify the main application class</span></span>

1. <span data-ttu-id="604d0-175">在应用的程序包目录中找到主应用程序 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-175">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   <span data-ttu-id="604d0-176">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-176">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. <span data-ttu-id="604d0-177">在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="604d0-177">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. <span data-ttu-id="604d0-178">保存并关闭主应用程序 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="604d0-178">Save and close the main application Java file.</span></span>

### <a name="add-a-web-controller-class"></a><span data-ttu-id="604d0-179">添加 Web 控制器类</span><span class="sxs-lookup"><span data-stu-id="604d0-179">Add a web controller class</span></span>

1. <span data-ttu-id="604d0-180">在应用的包目录中创建新的名为 *WebController.java* 的 Java 文件，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-180">Create a new Java file named *WebController.java* in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   <span data-ttu-id="604d0-181">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-181">-or-</span></span>

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. <span data-ttu-id="604d0-182">在文本编辑器中打开 Web 控制器 Java 文件，然后将以下行添加到文件中：</span><span class="sxs-lookup"><span data-stu-id="604d0-182">Open the web controller Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.storage;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;
   
   @RestController
   public class WebController {
   
      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }
   
      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```
   
   <span data-ttu-id="604d0-183">在 `@Value("blob://[container]/[blob]")` 语法分别定义容器和 Blob 的名称的位置，需存储数据。</span><span class="sxs-lookup"><span data-stu-id="604d0-183">Where the `@Value("blob://[container]/[blob]")` syntax respectively defines the names of the container and blob where you want to store the data.</span></span>

1. <span data-ttu-id="604d0-184">保存并关闭 Web 控制器 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="604d0-184">Save and close the web controller Java file.</span></span>

1. <span data-ttu-id="604d0-185">打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-185">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\storage`

   <span data-ttu-id="604d0-186">-或-</span><span class="sxs-lookup"><span data-stu-id="604d0-186">-or-</span></span>

   `cd /users/example/home/storage`

1. <span data-ttu-id="604d0-187">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-187">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="604d0-188">应用程序运行以后，即可使用 *curl* 对其进行测试，例如：</span><span class="sxs-lookup"><span data-stu-id="604d0-188">Once your application is running, you can use *curl* to test your application; for example:</span></span>

   <span data-ttu-id="604d0-189">a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。</span><span class="sxs-lookup"><span data-stu-id="604d0-189">a.</span></span> <span data-ttu-id="604d0-190">发送一个要求更新文件内容的 POST 请求：</span><span class="sxs-lookup"><span data-stu-id="604d0-190">Send a POST request to update a file's contents:</span></span>

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      <span data-ttu-id="604d0-191">此时会看到响应，指出文件已更新。</span><span class="sxs-lookup"><span data-stu-id="604d0-191">You should see a response that the file was updated.</span></span>

   <span data-ttu-id="604d0-192">b.</span><span class="sxs-lookup"><span data-stu-id="604d0-192">b.</span></span> <span data-ttu-id="604d0-193">发送一个要求验证文件内容的 GET 请求：</span><span class="sxs-lookup"><span data-stu-id="604d0-193">Send a GET request to verify the file's contents:</span></span>

      ```shell
      curl -X GET http://localhost:8080/
      ```

     <span data-ttu-id="604d0-194">此时会看到已发布的“Hello World”文本。</span><span class="sxs-lookup"><span data-stu-id="604d0-194">You should see the "Hello World" text that you posted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="604d0-195">后续步骤</span><span class="sxs-lookup"><span data-stu-id="604d0-195">Next steps</span></span>

<span data-ttu-id="604d0-196">有关适用于 Microsoft Azure 的其他 Spring Boot 起动器的详细信息，请参阅[适用于 Azure 的 Spring Boot 起动器](spring-boot-starters-for-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="604d0-196">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="604d0-197">有关将 Azure 功能集成到基于 Spring 的应用程序的其他信息，请参阅 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。</span><span class="sxs-lookup"><span data-stu-id="604d0-197">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="604d0-198">有关可从 Spring Boot 应用程序调用的其他 Azure 存储 API 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="604d0-198">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="604d0-199">如何通过 Java 使用 Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="604d0-199">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="604d0-200">如何通过 Java 使用 Azure 队列存储</span><span class="sxs-lookup"><span data-stu-id="604d0-200">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="604d0-201">如何通过 Java 使用 Azure 表存储</span><span class="sxs-lookup"><span data-stu-id="604d0-201">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="604d0-202">如何通过 Java 使用 Azure 文件存储</span><span class="sxs-lookup"><span data-stu-id="604d0-202">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
