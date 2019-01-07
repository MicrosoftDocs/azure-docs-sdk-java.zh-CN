---
title: 如何使用适用于 Azure Key Vault 的 Spring Boot 起动器
description: 了解如何使用 Azure Key Vault 起动器配置 Spring Boot Initializer 应用。
services: key-vault
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 78dadcf93bfc57ab669271495393fa9ba164c89d
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991361"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a><span data-ttu-id="d9657-103">如何使用适用于 Azure Key Vault 的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="d9657-103">How to use the Spring Boot Starter for Azure Key Vault</span></span>

## <a name="overview"></a><span data-ttu-id="d9657-104">概述</span><span class="sxs-lookup"><span data-stu-id="d9657-104">Overview</span></span>

<span data-ttu-id="d9657-105">本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Key Vault 的 Spring Boot 起动器检索密钥保管库中以机密形式存储的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d9657-105">This article demonstrates creating an app with the **[Spring Initializr]** that uses the Spring Boot Starter for Azure Key Vault to retrieve a connection string that is stored as a secret in a key vault.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9657-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="d9657-106">Prerequisites</span></span>

<span data-ttu-id="d9657-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="d9657-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="d9657-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="d9657-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="d9657-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="d9657-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="d9657-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="d9657-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="d9657-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="d9657-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-app-using-spring-initializr"></a><span data-ttu-id="d9657-112">使用 Spring Initialzr 创建应用</span><span class="sxs-lookup"><span data-stu-id="d9657-112">Create an app using Spring Initializr</span></span>

1. <span data-ttu-id="d9657-113">浏览到 <https://start.spring.io/>。</span><span class="sxs-lookup"><span data-stu-id="d9657-113">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="d9657-114">指定要使用 Java 生成的 Maven 项目，输入应用程序的“组”名称和“Aritifact”名称，然后单击链接切换到 Spring Initializr 完整版。</span><span class="sxs-lookup"><span data-stu-id="d9657-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![指定组和项目名称][secrets-01]

1. <span data-ttu-id="d9657-116">向下滚动到“Azure”部分，并选中“Azure Key Vault”对应的框。</span><span class="sxs-lookup"><span data-stu-id="d9657-116">Scroll down to the **Azure** section and check the box for **Azure Key Vault**.</span></span>

   ![选择 Azure Key Vault 起动器][secrets-02]

1. <span data-ttu-id="d9657-118">滚动到页面底部，单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="d9657-118">Scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![生成 Spring Boot 项目][secrets-03]

1. <span data-ttu-id="d9657-120">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="d9657-120">When prompted, download the project to a path on your local computer.</span></span>

## <a name="sign-into-azure"></a><span data-ttu-id="d9657-121">登录 Azure</span><span class="sxs-lookup"><span data-stu-id="d9657-121">Sign into Azure</span></span>

1. <span data-ttu-id="d9657-122">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="d9657-122">Open a command prompt.</span></span>

1. <span data-ttu-id="d9657-123">通过使用 Azure CLI 登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="d9657-123">Sign into your Azure account by using the Azure CLI:</span></span>

   ```azurecli
   az login
   ```
   <span data-ttu-id="d9657-124">按照说明完成登录过程。</span><span class="sxs-lookup"><span data-stu-id="d9657-124">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="d9657-125">列出订阅：</span><span class="sxs-lookup"><span data-stu-id="d9657-125">List your subscriptions:</span></span>

   ```azurecli
   az account list
   ```
   <span data-ttu-id="d9657-126">Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-126">Azure will return a list of your subscriptions, and you will need to copy the GUID for the subscription that you want to use; for example:</span></span>

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

1. <span data-ttu-id="d9657-127">指定要用于 Azure 的帐户的 GUID；例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-127">Specify the GUID for the account you want to use with Azure; for example:</span></span>

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a><span data-ttu-id="d9657-128">创建新的 Azure 密钥保管库</span><span class="sxs-lookup"><span data-stu-id="d9657-128">Create a new Azure Key Vault</span></span>

1. <span data-ttu-id="d9657-129">为要用于 Key Vault 的 Azure 资源创建资源组，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-129">Create a resource group for the Azure resources you will use for your key vault; for example:</span></span>
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   <span data-ttu-id="d9657-130">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-130">Where:</span></span>

   | <span data-ttu-id="d9657-131">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-131">Parameter</span></span> | <span data-ttu-id="d9657-132">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-132">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="d9657-133">指定资源组的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-133">Specifies a unique name for your resource group.</span></span> |
   | `location` | <span data-ttu-id="d9657-134">指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="d9657-134">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |

   <span data-ttu-id="d9657-135">Azure CLI 将显示资源组创建的结果；例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-135">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

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

2. <span data-ttu-id="d9657-136">从应用程序注册创建 Azure 服务主体，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-136">Create an Azure service principal from your application registration; for example:</span></span>
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   <span data-ttu-id="d9657-137">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-137">Where:</span></span>

   | <span data-ttu-id="d9657-138">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-138">Parameter</span></span> | <span data-ttu-id="d9657-139">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-139">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="d9657-140">指定 Azure 服务主体的名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-140">Specifies the name for your Azure service principal.</span></span> |

   <span data-ttu-id="d9657-141">Azure CLI 将返回包含 *appId* 和 *password* 的 JSON 状态消息，稍后要使用此信息分别作为客户端 ID 和客户端密码；例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-141">The Azure CLI will return a JSON status message that contains the *appId* and *password*, which you will use later as the client id and client password; for example:</span></span>

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. <span data-ttu-id="d9657-142">在资源组中创建新的 Key Vault，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-142">Create a new key vault in the resource group; for example:</span></span>
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   <span data-ttu-id="d9657-143">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-143">Where:</span></span>

   | <span data-ttu-id="d9657-144">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-144">Parameter</span></span> | <span data-ttu-id="d9657-145">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-145">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="d9657-146">指定 Key Vault 的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-146">Specifies a unique name for your key vault.</span></span> |
   | `location` | <span data-ttu-id="d9657-147">指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。</span><span class="sxs-lookup"><span data-stu-id="d9657-147">Specifies the [Azure region](https://azure.microsoft.com/regions/) where your resource group will be hosted.</span></span> |
   | `enabled-for-deployment` | <span data-ttu-id="d9657-148">指定 [Key Vault 部署选项](/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="d9657-148">Specifies the [key vault deployment option](/cli/azure/keyvault).</span></span> |
   | `enabled-for-disk-encryption` | <span data-ttu-id="d9657-149">指定 [Key Vault 加密选项](/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="d9657-149">Specifies the [key vault encryption option](/cli/azure/keyvault).</span></span> |
   | `enabled-for-template-deployment` | <span data-ttu-id="d9657-150">指定 [Key Vault 加密选项](/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="d9657-150">Specifies the [key vault encryption option](/cli/azure/keyvault).</span></span> |
   | `sku` | <span data-ttu-id="d9657-151">指定 [Key Vault SKU 选项](/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="d9657-151">Specifies the [key vault SKU option](/cli/azure/keyvault).</span></span> |
   | `query` | <span data-ttu-id="d9657-152">指定要从响应中检索的值，即完成本教程所需的 Key Vault URI。</span><span class="sxs-lookup"><span data-stu-id="d9657-152">Specifies a value to retrieve from the response, which is the key vault URI that you will need to complete this tutorial.</span></span> |

   <span data-ttu-id="d9657-153">Azure CLI 将显示稍后要使用的 Key Vault URI，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-153">The Azure CLI will display the URI for key vault, which you will use later; for example:</span></span>  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

4. <span data-ttu-id="d9657-154">针对前面创建的 Azure 服务主体设置访问策略，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-154">Set the access policy for the Azure service principal you created earlier; for example:</span></span>
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   <span data-ttu-id="d9657-155">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-155">Where:</span></span>

   | <span data-ttu-id="d9657-156">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-156">Parameter</span></span> | <span data-ttu-id="d9657-157">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-157">Description</span></span> |
   |---|---|
   | `name` | <span data-ttu-id="d9657-158">指定前面创建的 Key Vault 名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-158">Specifies your key vault name from earlier.</span></span> |
   | `secret-permission` | <span data-ttu-id="d9657-159">指定 Key Vault 的[安全策略](/cli/azure/keyvault)。</span><span class="sxs-lookup"><span data-stu-id="d9657-159">Specifies the [security policies](/cli/azure/keyvault) for your key vault.</span></span> |
   | `spn` | <span data-ttu-id="d9657-160">指定前面创建的应用程序注册的 GUID。</span><span class="sxs-lookup"><span data-stu-id="d9657-160">Specifies the GUID for your application registration from earlier.</span></span> |

   <span data-ttu-id="d9657-161">Azure CLI 将显示安全策略的创建结果，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-161">The Azure CLI will display the results of your security policy creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "wingtiptoyskeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "wingtiptoysresources",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. <span data-ttu-id="d9657-162">在新的 Key Vault 中存储一个机密，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-162">Store a secret in your new key vault; for example:</span></span>
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   <span data-ttu-id="d9657-163">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-163">Where:</span></span>

   | <span data-ttu-id="d9657-164">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-164">Parameter</span></span> | <span data-ttu-id="d9657-165">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-165">Description</span></span> |
   |---|---|
   | `vault-name` | <span data-ttu-id="d9657-166">指定前面创建的 Key Vault 名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-166">Specifies your key vault name from earlier.</span></span> |
   | `name` | <span data-ttu-id="d9657-167">指定机密的名称。</span><span class="sxs-lookup"><span data-stu-id="d9657-167">Specifies the name of your secret.</span></span> |
   | `value` | <span data-ttu-id="d9657-168">指定机密的值。</span><span class="sxs-lookup"><span data-stu-id="d9657-168">Specifies the value of your secret.</span></span> |

   <span data-ttu-id="d9657-169">Azure CLI 将显示机密的创建结果，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-169">The Azure CLI will display the results of your secret creation; for example:</span></span>  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://wingtiptoyskeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://wingtiptoys.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a><span data-ttu-id="d9657-170">配置并编译你的应用</span><span class="sxs-lookup"><span data-stu-id="d9657-170">Configure and compile your app</span></span>

1. <span data-ttu-id="d9657-171">将前面下载的 Spring Boot 项目存档中的文件提取到某个目录中。</span><span class="sxs-lookup"><span data-stu-id="d9657-171">Extract the files from the Spring Boot project archive files that you downloaded earlier into a directory.</span></span>

2. <span data-ttu-id="d9657-172">导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。</span><span class="sxs-lookup"><span data-stu-id="d9657-172">Navigate to the *src/main/resources* folder in your project and open the *application.properties* file in a text editor.</span></span>

3. <span data-ttu-id="d9657-173">使用前面在完成本教程的步骤时创建的值添加 Key Vault 的值，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-173">Add the values for your key vault using values from the steps that you completed earlier in this tutorial; for example:</span></span>
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   <span data-ttu-id="d9657-174">其中：</span><span class="sxs-lookup"><span data-stu-id="d9657-174">Where:</span></span>

   |          <span data-ttu-id="d9657-175">参数</span><span class="sxs-lookup"><span data-stu-id="d9657-175">Parameter</span></span>          |                                 <span data-ttu-id="d9657-176">说明</span><span class="sxs-lookup"><span data-stu-id="d9657-176">Description</span></span>                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           <span data-ttu-id="d9657-177">指定创建 Key Vault 时使用的 URI。</span><span class="sxs-lookup"><span data-stu-id="d9657-177">Specifies the URI from when you created your key vault.</span></span>           |
   | `azure.keyvault.client-id`  |  <span data-ttu-id="d9657-178">指定创建服务主体时使用的 *appId* GUID。</span><span class="sxs-lookup"><span data-stu-id="d9657-178">Specifies the *appId* GUID from when you created your service principal.</span></span>   |
   | `azure.keyvault.client-key` | <span data-ttu-id="d9657-179">指定创建服务主体时使用的 *password* GUID。</span><span class="sxs-lookup"><span data-stu-id="d9657-179">Specifies the *password* GUID from when you created your service principal.</span></span> |


4. <span data-ttu-id="d9657-180">导航到项目的主源代码文件，例如：*/src/main/java/com/wingtiptoys/secrets*。</span><span class="sxs-lookup"><span data-stu-id="d9657-180">Navigate to the main source code file of your project; for example: */src/main/java/com/wingtiptoys/secrets*.</span></span>

5. <span data-ttu-id="d9657-181">在文本编辑器中打开应用程序的主 Java 文件；例如：*SecretsApplication.java*，并向文件中添加以下行：</span><span class="sxs-lookup"><span data-stu-id="d9657-181">Open the application's main Java file in a file in a text editor; for example: *SecretsApplication.java*, and add the following lines to the file:</span></span>

   ```java
   package com.wingtiptoys.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   <span data-ttu-id="d9657-182">此代码示例从 Key Vault 中检索连接字符串，并将其显示到命令行。</span><span class="sxs-lookup"><span data-stu-id="d9657-182">This code example retrieves the connection string from the key vault and displays it to the command line.</span></span>

6. <span data-ttu-id="d9657-183">保存并关闭该 Java 文件。</span><span class="sxs-lookup"><span data-stu-id="d9657-183">Save and close the Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="d9657-184">生成并测试应用</span><span class="sxs-lookup"><span data-stu-id="d9657-184">Build and test your app</span></span>

1. <span data-ttu-id="d9657-185">导航到 Spring Boot 应用的 *pom.xml* 文件所在的目录：</span><span class="sxs-lookup"><span data-stu-id="d9657-185">Navigate to the directory where the *pom.xml* file for your Spring Boot app is located:</span></span>

1. <span data-ttu-id="d9657-186">使用 Maven 生成 Spring Boot 应用程序，例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-186">Build your Spring Boot application with Maven; for example:</span></span>

   ```bash
   mvn clean package
   ```

   <span data-ttu-id="d9657-187">Maven 将显示生成结果。</span><span class="sxs-lookup"><span data-stu-id="d9657-187">Maven will display the results of your build.</span></span>

   ![Spring Boot 应用程序生成状态][build-application-01]

1. <span data-ttu-id="d9657-189">使用 Maven 运行 Spring Boot 应用程序；该应用程序将显示 Key Vault 中的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d9657-189">Run your Spring Boot application with Maven; the application will display the connection string from your key vault.</span></span> <span data-ttu-id="d9657-190">例如：</span><span class="sxs-lookup"><span data-stu-id="d9657-190">For example:</span></span>

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 运行时消息][build-application-02]

## <a name="summary"></a><span data-ttu-id="d9657-192">摘要</span><span class="sxs-lookup"><span data-stu-id="d9657-192">Summary</span></span>

<span data-ttu-id="d9657-193">在本教程中，你使用 **[Spring Initializr]** 创建了一个新的 Java Web 应用程序，创建了一个 Azure 密钥保管库来存储敏感信息，然后将你的应用程序配置为从密钥保管库检索信息。</span><span class="sxs-lookup"><span data-stu-id="d9657-193">In this tutorial, you created a new Java web application using the **[Spring Initializr]**, created an Azure Key Vault to store sensitive information, and then configured your application to retrieve information from your key vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9657-194">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d9657-194">Next steps</span></span>

<span data-ttu-id="d9657-195">若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="d9657-195">To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d9657-196">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="d9657-196">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="d9657-197">其他资源</span><span class="sxs-lookup"><span data-stu-id="d9657-197">Additional Resources</span></span>

<span data-ttu-id="d9657-198">有关使用 Azure Key Vault 的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="d9657-198">For more information about using Azure Key Vaults, see the following articles:</span></span>

* <span data-ttu-id="d9657-199">[Key Vault 文档]。</span><span class="sxs-lookup"><span data-stu-id="d9657-199">[Key Vault Documentation].</span></span>

* <span data-ttu-id="d9657-200">[Azure 密钥保管库入门]</span><span class="sxs-lookup"><span data-stu-id="d9657-200">[Get started with Azure Key Vault]</span></span>

<span data-ttu-id="d9657-201">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="d9657-201">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="d9657-202">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="d9657-202">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="d9657-203">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="d9657-203">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="d9657-204">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="d9657-204">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<!-- URL List -->

[Key Vault 文档]: /azure/key-vault/
[Key Vault Documentation]: /azure/key-vault/
[Azure 密钥保管库入门]: /azure/key-vault/key-vault-get-started
[Get started with Azure Key Vault]: /azure/key-vault/key-vault-get-started
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
