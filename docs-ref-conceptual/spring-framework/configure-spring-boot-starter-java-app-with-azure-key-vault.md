---
title: "如何使用适用于 Azure Key Vault 的 Spring Boot 起动器"
description: "了解如何使用 Azure Key Vault 起动器配置 Spring Boot Initializer 应用。"
services: key-vault
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 52e7dc3f84ea96f22d8e478a597452c76ed8bf22
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>如何使用适用于 Azure Key Vault 的 Spring Boot 起动器

## <a name="overview"></a>概述

本文演示如何使用 **[Spring Initializr]** 创建一个应用，该应用使用适用于 Azure Key Vault 的 Spring Boot 起动器检索密钥保管库中以机密形式存储的连接字符串。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。
* [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-app-using-the-spring-initialzr"></a>使用 Spring Initialzr 创建应用

1. 浏览到 <https://start.spring.io/>。

1. 指定要使用 Java 生成的 Maven 项目，输入应用程序的“组”名称和“Aritifact”名称，然后单击链接切换到 Spring Initializr 完整版。

   ![指定组和项目名称][secrets-01]

1. 向下滚动到“Azure”部分，并选中“Azure Key Vault”对应的框。

   ![选择 Azure Key Vault 起动器][secrets-02]

1. 滚动到页面底部，单击“生成项目”对应的按钮。

   ![生成 Spring Boot 项目][secrets-03]

1. 出现提示时，将项目下载到本地计算机中的路径。

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

1. Specify the GUID for the account you want to use with Azure; for example:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-and-configure-a-new-azure-key-vault-using-the-azure-cli"></a>使用 Azure CLI 创建并配置新的 Azure Key Vault

1. 为要用于 Key Vault 的 Azure 资源创建资源组，例如：
   ```azurecli
   az group create --name wingtiptoysresources --location westus
   ```
   其中：
   | 参数 | 说明 |
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

1. 从应用程序注册创建 Azure 服务主体，例如：
   ```shell
   az ad sp create-for-rbac --name "wingtiptoysuser"
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `name` | 指定 Azure 服务主体的名称。 |

   Azure CLI 将返回包含 *appId* 和 *password* 的 JSON 状态消息，稍后要使用此信息分别作为客户端 ID 和客户端密码；例如：

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "wingtiptoysuser",
     "name": "http://wingtiptoysuser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

1. 在资源组中创建新的 Key Vault，例如：
   ```azurecli
   az keyvault create --name wingtiptoyskeyvault --resource-group wingtiptoysresources --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `name` | 指定 Key Vault 的唯一名称。 |
   | `location` | 指定要在其中托管资源组的 [Azure 区域](https://azure.microsoft.com/regions/)。 |
   | `enabled-for-deployment` | 指定 [Key Vault 部署选项](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `enabled-for-disk-encryption` | 指定 [Key Vault 加密选项](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `enabled-for-template-deployment` | 指定 [Key Vault 加密选项](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `sku` | 指定 [Key Vault SKU 选项](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `query` | 指定要从响应中检索的值，即完成本教程所需的 Key Vault URI。 |

   Azure CLI 将显示稍后要使用的 Key Vault URI，例如：  

   ```
   "https://wingtiptoyskeyvault.vault.azure.net"
   ```

1. 针对前面创建的 Azure 服务主体设置访问策略，例如：
   ```azurecli
   az keyvault set-policy --name wingtiptoyskeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `name` | 指定前面创建的 Key Vault 名称。 |
   | `secret-permission` | 指定 Key Vault 的[安全策略](https://docs.microsoft.com/en-us/cli/azure/keyvault)。 |
   | `spn` | 指定前面创建的应用程序注册的 GUID。 |

   Azure CLI 将显示安全策略的创建结果，例如：  

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

1. 在新的 Key Vault 中存储一个机密，例如：
   ```azurecli
   az keyvault secret set --vault-name "wingtiptoyskeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `vault-name` | 指定前面创建的 Key Vault 名称。 |
   | `name` | 指定机密的名称。 |
   | `value` | 指定机密的值。 |

   Azure CLI 将显示机密的创建结果，例如：  

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

## <a name="configure-and-compile-your-spring-boot-application"></a>配置并编译 Spring Boot 应用程序

1. 将前面下载的 Spring Boot 项目存档中的文件提取到某个目录中。

1. 导航到项目中的 *src/main/resources* 文件夹，并在文本编辑器中打开 *application.properties* 文件。

1. 使用前面在完成本教程的步骤时创建的值添加 Key Vault 的值，例如：
   ```yaml
   azure.keyvault.uri=https://wingtiptoyskeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```
   其中：
   | 参数 | 说明 |
   |---|---|
   | `azure.keyvault.uri` | 指定创建 Key Vault 时使用的 URI。 |
   | `azure.keyvault.client-id` | 指定创建服务主体时使用的 *appId* GUID。 |
   | `azure.keyvault.client-key` | 指定创建服务主体时使用的 *password* GUID。 |

1. 导航到项目的主源代码文件，例如：*/src/main/java/com/wingtiptoys/secrets*。

1. 在文本编辑器中的某个文件内打开应用程序的主 Java 文件，例如：*SecretsApplication.java*，并将以下行添加到该文件：

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
   此代码示例从 Key Vault 中检索连接字符串，并将其显示到命令行。

1. 保存并关闭该 Java 文件。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 导航到 Spring Boot 应用的 *pom.xml* 文件所在的目录：

1. 使用 Maven 生成 Spring Boot 应用程序，例如：

   ```bash
   mvn clean package
   ```

   Maven 将显示生成结果。

   ![Spring Boot 应用程序生成状态][build-application-01]

1. 使用 Maven 运行 Spring Boot 应用程序；该应用程序将显示 Key Vault 中的连接字符串。 例如：

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot 运行时消息][build-application-02]

## <a name="next-steps"></a>后续步骤

有关使用 Azure Key Vault 的详细信息，请参阅以下文章：

* [Key Vault 文档]。

* [Azure 密钥保管库入门]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

<!-- URL List -->

[Key Vault 文档]: /azure/key-vault/
[Azure 密钥保管库入门]: /azure/key-vault/key-vault-get-started
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
