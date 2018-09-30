---
title: 如何将 Spring Boot Starter 与 Azure 存储 API 配合使用
description: 了解如何使用 Azure 存储 API 配置 Spring Boot Initializer 应用。
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
ms.openlocfilehash: 8ee985f28b7fa80548e13681089e0a5a9226851d
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506344"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-storage-api"></a><span data-ttu-id="03558-103">如何将 Spring Boot Starter 与 Azure 存储 API 配合使用</span><span class="sxs-lookup"><span data-stu-id="03558-103">How to use the Spring Boot Starter with the Azure Storage API</span></span>

## <a name="overview"></a><span data-ttu-id="03558-104">概述</span><span class="sxs-lookup"><span data-stu-id="03558-104">Overview</span></span>

<span data-ttu-id="03558-105">本文逐步讲解如何通过 Azure 存储 API 使用 **Spring Initializr** 创建自定义应用程序，然后使用该应用程序访问 Azure 存储。</span><span class="sxs-lookup"><span data-stu-id="03558-105">This article walks you through creating a custom application using the **Spring Initializr**, and then using that application to access Azure storage by using the Azure Storage API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03558-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="03558-106">Prerequisites</span></span>

<span data-ttu-id="03558-107">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="03558-107">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="03558-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册[免费 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="03558-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="03558-109">[Azure 命令行接口 (CLI)](http://docs.microsoft.com/cli/azure/overview)。</span><span class="sxs-lookup"><span data-stu-id="03558-109">The [Azure Command-Line Interface (CLI)](http://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="03558-110">最新的 [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="03558-110">An up-to-date [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>
* <span data-ttu-id="03558-111">Apache 的 [Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="03558-111">Apache's [Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="03558-112">使用 Spring Initializr 创建自定义应用程序</span><span class="sxs-lookup"><span data-stu-id="03558-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="03558-113">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="03558-113">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="03558-114">指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。</span><span class="sxs-lookup"><span data-stu-id="03558-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Spring Initializr 的基本选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > <span data-ttu-id="03558-116">Spring Initializr 将使用“组”和“项目”名称创建包名称，例如：*com.contoso.wingtiptoysdemo*。</span><span class="sxs-lookup"><span data-stu-id="03558-116">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.contoso.wingtiptoysdemo*.</span></span>
   >

1. <span data-ttu-id="03558-117">向下滚动到“Azure”部分，并选中“Azure 存储”对应的框。</span><span class="sxs-lookup"><span data-stu-id="03558-117">Scroll down to the **Azure** section and check the box for **Azure Storage**.</span></span>

   ![Spring Initializr 的完整选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. <span data-ttu-id="03558-119">滚动到页面底部，单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="03558-119">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Spring Initializr 的完整选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. <span data-ttu-id="03558-121">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="03558-121">When prompted, download the project to a path on your local computer.</span></span>

   ![下载自定义 Spring Boot 项目](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a><span data-ttu-id="03558-123">登录到 Azure 并选择要使用的订阅</span><span class="sxs-lookup"><span data-stu-id="03558-123">Sign into Azure and select the subscription to use</span></span>

1. <span data-ttu-id="03558-124">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="03558-124">Open a command prompt.</span></span>

1. <span data-ttu-id="03558-125">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="03558-125">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="03558-126">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="03558-126">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="03558-127">列出订阅：</span><span class="sxs-lookup"><span data-stu-id="03558-127">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="03558-128">Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-128">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="03558-129">指定要用于 Azure 的帐户的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="03558-129">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="03558-130">创建 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="03558-130">Create an Azure Storage account</span></span>

1. <span data-ttu-id="03558-131">为要在本文中使用的 Azure 资源创建资源组，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-131">Create a resource group for the Azure resources you will use in this article; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="03558-132">其中：</span><span class="sxs-lookup"><span data-stu-id="03558-132">Where:</span></span>

   | <span data-ttu-id="03558-133">参数</span><span class="sxs-lookup"><span data-stu-id="03558-133">Parameter</span></span> | <span data-ttu-id="03558-134">Description</span><span class="sxs-lookup"><span data-stu-id="03558-134">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="03558-135">指定资源组的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="03558-135">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="03558-136">指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="03558-136">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="03558-137">Azure CLI 将显示资源组创建的结果；例如：</span><span class="sxs-lookup"><span data-stu-id="03558-137">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/wingtiptoysresources",
     "location": "westus",
     "managedBy": null,
     "name": "wingtiptoysresources",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. <span data-ttu-id="03558-138">在 Spring Boot 应用的资源组中创建 Azure 存储帐户，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-138">Create an Azure storage account in the in the resource group for your Spring Boot app; for example:</span></span>
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   <span data-ttu-id="03558-139">其中：</span><span class="sxs-lookup"><span data-stu-id="03558-139">Where:</span></span>

   | <span data-ttu-id="03558-140">参数</span><span class="sxs-lookup"><span data-stu-id="03558-140">Parameter</span></span> | <span data-ttu-id="03558-141">Description</span><span class="sxs-lookup"><span data-stu-id="03558-141">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="03558-142">指定存储帐户的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="03558-142">Specifies a unique name for your storage account.</span></span> |
   | `resource-group` | <span data-ttu-id="03558-143">指定在上一步骤中创建的资源组的名称。</span><span class="sxs-lookup"><span data-stu-id="03558-143">Specifies the name of the resource group group you created in the previous step.</span></span> |
   | `location` | <span data-ttu-id="03558-144">指定要在其中托管存储帐户的 [Azure 区域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="03558-144">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your storage account will be hosted.</span></span> |
   | `sku` | <span data-ttu-id="03558-145">指定以下值之一：`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS`。</span><span class="sxs-lookup"><span data-stu-id="03558-145">Specifies one of the following: `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_ZRS`.</span></span> |

   <span data-ttu-id="03558-146">Azure 将返回包含预配状态的长 JSON 字符串，例如：|</span><span class="sxs-lookup"><span data-stu-id="03558-146">Azure will return a long JSON string which contains the provisioning state; for example: |</span></span>

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "identity": null,
     "kind": "Storage"
       ...
       ... (A long list of values will be displayed here.)
       ...
     "statusOfPrimary": "available",
     "statusOfSecondary": null,
     "tags": {},
     "type": "Microsoft.Storage/storageAccounts"
   }
   ```

3. <span data-ttu-id="03558-147">检索存储帐户的连接字符串，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-147">Retrieve the connection string for your storage account; for example:</span></span>
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   <span data-ttu-id="03558-148">其中：</span><span class="sxs-lookup"><span data-stu-id="03558-148">Where:</span></span>

   | <span data-ttu-id="03558-149">参数</span><span class="sxs-lookup"><span data-stu-id="03558-149">Parameter</span></span> | <span data-ttu-id="03558-150">Description</span><span class="sxs-lookup"><span data-stu-id="03558-150">Description</span></span> |
   | ---|---|
   | `name` | <span data-ttu-id="03558-151">指定在前面步骤中创建的存储帐户的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="03558-151">Specifies a unique name of the storage account you created in previous steps.</span></span> |
   | `resource-group` | <span data-ttu-id="03558-152">指定在前面步骤中创建的资源组的名称。</span><span class="sxs-lookup"><span data-stu-id="03558-152">Specifies the name of the resource group you created in previous steps.</span></span> |

   <span data-ttu-id="03558-153">Azure 将返回包含存储帐户连接字符串的 JSON 字符串，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-153">Azure will return a JSON string which contains the connection string for your storage account; for example:</span></span>

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a><span data-ttu-id="03558-154">配置并编译 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="03558-154">Configure and compile your Spring Boot application</span></span>

1. <span data-ttu-id="03558-155">将下载的项目存档中的文件提取到某个目录。</span><span class="sxs-lookup"><span data-stu-id="03558-155">Extract the files from the downloaded project archive into a directory.</span></span>

1. <span data-ttu-id="03558-156">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="03558-156">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

1. <span data-ttu-id="03558-157">添加存储帐户的密钥，例如：</span><span class="sxs-lookup"><span data-stu-id="03558-157">Add the key for your storage account; for example:</span></span>
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. <span data-ttu-id="03558-158">导航到项目中的 */src/main/java/com/example/wingtiptoysdemo* 文件夹，并在文本编辑器中打开 *WingtiptoysdemoApplication.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="03558-158">Navigate to the */src/main/java/com/example/wingtiptoysdemo* folder in your project and open the *WingtiptoysdemoApplication.java* file in a text editor.</span></span>

1. <span data-ttu-id="03558-159">将现有 Java 代码替换为可列出容器中的 Blob 的以下示例：</span><span class="sxs-lookup"><span data-stu-id="03558-159">Replace the existing Java code with the following example that lists the blobs in a container:</span></span>

   ```java
   package com.example.wingtiptoysdemo;

   import com.microsoft.azure.storage.*;
   import com.microsoft.azure.storage.blob.*;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   import java.net.URISyntaxException;

   @SpringBootApplication
   public class WingtiptoysdemoApplication implements CommandLineRunner {

      @Autowired
      private CloudStorageAccount cloudStorageAccount;

      final String containerName = "mycontainer";

      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysdemoApplication.class, args);
      }

      public void run(String... var1)
             throws URISyntaxException, StorageException {
          // Create a container (if it does not exist).
          createContainerIfNotExists(containerName);
          // Upload a blob to the container.
          uploadTextBlob(containerName);
      }

      private void createContainerIfNotExists(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Create the container if it does not exist.
            container.createIfNotExists();
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }

      private void uploadTextBlob(String containerName)
            throws URISyntaxException, StorageException {
         try
         {
            // Create a blob client.
            final CloudBlobClient blobClient = cloudStorageAccount.createCloudBlobClient();
            // Get a reference to a container. (Name must be lower case.)
            final CloudBlobContainer container = blobClient.getContainerReference(containerName);
            // Get a blob reference for a text file.
            CloudBlockBlob blob = container.getBlockBlobReference("test.txt");
            // Upload some text into the blob.
            blob.uploadText("Hello World!");
         }
         catch (Exception e)
         {
            // Output the stack trace.
            e.printStackTrace();
         }
      }
   }
   ```
   > [!NOTE]
   >
   > <span data-ttu-id="03558-160">上述示例可自动连接 *application.properties* 文件中定义的存储帐户设置。</span><span class="sxs-lookup"><span data-stu-id="03558-160">The above example autowires the storage account settings that you defined in the *application.properties* file.</span></span>
   >

1. <span data-ttu-id="03558-161">编译并运行应用程序：</span><span class="sxs-lookup"><span data-stu-id="03558-161">Compile and run the application:</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

   <span data-ttu-id="03558-162">该应用程序将创建一个容器并将一个文本文件作为 Blob 上传到该容器，该文件将列在 [Azure 门户](https://portal.azure.com)中存储帐户的下面。</span><span class="sxs-lookup"><span data-stu-id="03558-162">The application will create a container and upload a text file as a blob to the container, which will be listed under your storage account in the [Azure portal](https://portal.azure.com).</span></span>

   ![在 Azure 门户中列出 Blob](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > <span data-ttu-id="03558-164">编译应用程序时，可能会看到以下错误消息：</span><span class="sxs-lookup"><span data-stu-id="03558-164">When you compile your application, you might see the following error message:</span></span>
   > 
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] BUILD FAILURE`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[INFO] Total time: 2.616 s`<br/>
   > `[INFO] Finished at: 2017-11-11T13:14:15Z`<br/>
   > `[INFO] Final Memory: 26M/213M`<br/>
   > `[INFO] ------------------------------------------------------------------------`<br/>
   > `[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2`<br/>
   > `.18.1:test (default-test) on project wingtiptoysdemo: Execution default-test of`<br/>
   > `goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test failed: The for`<br/>
   > `ked VM terminated without properly saying goodbye. VM crash or System.exit called?`<br/>
   > `[ERROR] Command was /bin/sh -c cd /home/robert/SpringBoot/wingtiptoysdemo && /u`<br/>
   > `sr/lib/jvm/java-8-openjdk-amd64/jre/bin/java -jar /home/robert/SpringBoot/wingt`<br/>
   > `iptoysdemo/target/surefire/surefirebooter6371623993063346766.jar /home/robert/S`<br/>
   > `pringBoot/wingtiptoysdemo/target/surefire/surefire5107893623933537917tmp /home/`<br/>
   > `robert/SpringBoot/wingtiptoysdemo/target/surefire/surefire_01414159391084128068tmp`<br/>
   > `[ERROR] -> [Help 1]`<br/>
   > 
   > <span data-ttu-id="03558-165">如果发生此情况，可能需要禁用 Maven Surefire 测试；为此，请在 *pom.xml* 文件中添加以下插件条目：</span><span class="sxs-lookup"><span data-stu-id="03558-165">If this happens, you might want to disable the Maven Surefire testing; to do so, add the following plugin entry in your *pom.xml* file:</span></span>
   > 
   > ```xml
   > <plugin>
   >   <groupId>org.apache.maven.plugins</groupId>
   >   <artifactId>maven-surefire-plugin</artifactId>
   >   <version>2.20.1</version>
   >   <configuration>
   >     <skipTests>true</skipTests>
   >   </configuration>
   > </plugin>
   > ```
   > 

## <a name="next-steps"></a><span data-ttu-id="03558-166">后续步骤</span><span class="sxs-lookup"><span data-stu-id="03558-166">Next steps</span></span>

<span data-ttu-id="03558-167">有关适用于 Microsoft Azure 的其他 Spring Boot 起动器的详细信息，请参阅[适用于 Azure 的 Spring Boot 起动器](spring-boot-starters-for-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="03558-167">For more information about the additional Spring Boot Starters that are available for Microsoft Azure, see [Spring Boot Starters for Azure](spring-boot-starters-for-azure.md).</span></span>

<span data-ttu-id="03558-168">有关将 Azure 功能集成到基于 Spring 的应用程序的其他信息，请参阅 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。</span><span class="sxs-lookup"><span data-stu-id="03558-168">For additional information about integrating Azure functionality into your Spring-based applications, see [Spring Framework on Azure](/java/azure/spring-framework/).</span></span>

<span data-ttu-id="03558-169">有关可从 Spring Boot 应用程序调用的其他 Azure 存储 API 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="03558-169">For detailed information about additional Azure storage APIs that you can call from your Spring Boot applications, see the following articles:</span></span>
* [<span data-ttu-id="03558-170">如何通过 Java 使用 Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="03558-170">How to use Azure Blob storage from Java</span></span>](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [<span data-ttu-id="03558-171">如何通过 Java 使用 Azure 队列存储</span><span class="sxs-lookup"><span data-stu-id="03558-171">How to use Azure Queue storage from Java</span></span>](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [<span data-ttu-id="03558-172">如何通过 Java 使用 Azure 表存储</span><span class="sxs-lookup"><span data-stu-id="03558-172">How to use Azure Table storage from Java</span></span>](/azure/cosmos-db/table-storage-how-to-use-java)
* [<span data-ttu-id="03558-173">如何通过 Java 使用 Azure 文件存储</span><span class="sxs-lookup"><span data-stu-id="03558-173">How to use Azure File storage from Java</span></span>](/azure/storage/files/storage-java-how-to-use-file-storage)
