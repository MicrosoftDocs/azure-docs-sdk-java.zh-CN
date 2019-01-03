---
title: 如何将 Spring Boot Starter 与 Azure 存储 API 配合使用
description: 了解如何使用 Azure 存储 API 配置 Spring Boot Initializer 应用。
services: storage
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage
ms.openlocfilehash: 984a3edb89608c806537f991b42e309f31130896
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991441"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-storage-api"></a>如何将 Spring Boot Starter 与 Azure 存储 API 配合使用

## <a name="overview"></a>概述

本文逐步讲解如何通过 Azure 存储 API 使用 **Spring Initializr** 创建自定义应用程序，然后使用该应用程序访问 Azure 存储。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册[免费的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。
* [Azure 命令行接口 (CLI)](http://docs.microsoft.com/cli/azure/overview)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 创建自定义应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后单击 Spring Initializr 的“切换到完整版”链接。

   ![Spring Initializr 的基本选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-basic.png)

   > [!NOTE]
   >
   > Spring Initializr 将使用“组”和“项目”名称创建包名称，例如：*com.contoso.wingtiptoysdemo*。
   >

1. 向下滚动到“Azure”部分，并选中“Azure 存储”对应的框。

   ![Spring Initializr 的完整选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-advanced.png)

1. 滚动到页面底部，单击“生成项目”对应的按钮。

   ![Spring Initializr 的完整选项](media/configure-spring-boot-starter-java-app-with-azure-storage/spring-initializr-generate.png)

1. 出现提示时，将项目下载到本地计算机中的路径。

   ![下载自定义 Spring Boot 项目](media/configure-spring-boot-starter-java-app-with-azure-storage/download-app.png)

## <a name="sign-into-azure-and-select-the-subscription-to-use"></a>登录到 Azure 并选择要使用的订阅

1. 打开命令提示符。

1. 通过使用 Azure CLI 登录到 Azure 帐户：

   ```azurecli
   az login
   ```
   按照说明完成登录过程。

1. 列出订阅：

   ```azurecli
   az account list
   ```
   Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：

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

1. 指定要用于 Azure 的帐户的 GUID；例如：

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

1. 为要在本文中使用的 Azure 资源创建资源组，例如：
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   其中：

   | 参数 | Description |
   |---|---|
   | `name` | 指定资源组的唯一名称。 |
   | `location` | 指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。 |

   Azure CLI 将显示资源组创建的结果；例如：  

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

2. 在 Spring Boot 应用的资源组中创建 Azure 存储帐户，例如：
   ```azurecli
   az storage account create --name wingtiptoysstorage --resource-group wingtiptoysresources --location westus --sku Standard_LRS
   ```
   其中：

   | 参数 | Description |
   |---|---|
   | `name` | 指定存储帐户的唯一名称。 |
   | `resource-group` | 指定在上一步骤中创建的资源组的名称。 |
   | `location` | 指定要在其中托管存储帐户的 [Azure 区域](https://azure.microsoft.com/regions/)。 |
   | `sku` | 指定以下值之一：`Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS`。 |

   Azure 将返回包含预配状态的长 JSON 字符串，例如：|

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

3. 检索存储帐户的连接字符串，例如：
   ```azurecli
   az storage account show-connection-string --name wingtiptoysstorage --resource-group wingtiptoysresources
   ```
   其中：

   | 参数 | Description |
   | ---|---|
   | `name` | 指定在前面步骤中创建的存储帐户的唯一名称。 |
   | `resource-group` | 指定在前面步骤中创建的资源组的名称。 |

   Azure 将返回包含存储帐户连接字符串的 JSON 字符串，例如：

   ```json
   {
     "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz=="
   }
   ```

## <a name="configure-and-compile-your-spring-boot-application"></a>配置并编译 Spring Boot 应用程序

1. 将下载的项目存档中的文件提取到某个目录。

1. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。

1. 添加存储帐户的密钥，例如：
   ```yaml
   azure.storage.connection-string=DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=wingtiptoysstorage;AccountKey=AbCdEfGhIjKlMnOpQrStUvWxYz==
   ```

1. 导航到项目中的 */src/main/java/com/example/wingtiptoysdemo* 文件夹，并在文本编辑器中打开 *WingtiptoysdemoApplication.java* 文件。

1. 将现有 Java 代码替换为可列出容器中的 Blob 的以下示例：

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
   > 上述示例可自动连接 *application.properties* 文件中定义的存储帐户设置。
   >

1. 编译并运行应用程序：
   ```shell
   mvn clean package spring-boot:run
   ```

   该应用程序将创建一个容器并将一个文本文件作为 Blob 上传到该容器，该文件将列在 [Azure 门户](https://portal.azure.com)中存储帐户的下面。

   ![在 Azure 门户中列出 Blob](media/configure-spring-boot-starter-java-app-with-azure-storage/list-blobs-in-portal.png)

   > [!NOTE]
   > 
   > 编译应用程序时，可能会看到以下错误消息：
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
   > 如果发生此情况，可能需要禁用 Maven Surefire 测试；为此，请在 *pom.xml* 文件中添加以下插件条目：
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

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关适用于 Microsoft Azure 的其他 Spring Boot 起动器的详细信息，请参阅[适用于 Azure 的 Spring Boot 起动器](spring-boot-starters-for-azure.md)。

有关将 Azure 功能集成到基于 Spring 的应用程序的其他信息，请参阅 [Azure 上的 Spring Framework](/java/azure/spring-framework/)。

有关可从 Spring Boot 应用程序调用的其他 Azure 存储 API 的详细信息，请参阅以下文章：
* [如何通过 Java 使用 Azure Blob 存储](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [如何通过 Java 使用 Azure 队列存储](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [如何通过 Java 使用 Azure 表存储](/azure/cosmos-db/table-storage-how-to-use-java)
* [如何通过 Java 使用 Azure 文件存储](/azure/storage/files/storage-java-how-to-use-file-storage)
