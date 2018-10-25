---
title: 用于 Java 的 Azure Key Vault 库
description: 用于 Java 的 Azure Key Vault 库的概述
keywords: Azure, Java, SDK, API, keyvault, 安全, 密钥, 机密, 保管库
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: 84ea77a19c326409f453f62359cf46c90398daeb
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799923"
---
# <a name="azure-key-vault-libraries-for-java"></a>用于 Java 的 Azure Key Vault 库

## <a name="overview"></a>概述

使用 [Azure Key Vault](/azure/key-vault/) 保护及管理云应用程序与服务使用的加密密钥和机密。

若要开始使用 Azure Key Vault，请参阅 [Azure Key Vault 入门](/azure/key-vault/key-vault-get-started)。

## <a name="client-library"></a>客户端库

使用客户端库在 Azure Key Vault 中创建、更新和删除密钥与机密。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.1.0</version>
</dependency>
```   

## <a name="example"></a>示例

从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = kvc.getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/keyvault/client)


## <a name="management-api"></a>管理 API

使用 Azure Key Vault 管理库来创建 Key Vault、授权应用程序及管理权限。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.15.0</version>
</dependency>
```

## <a name="example"></a>示例

使用[服务主体](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` 授权运行的应用程序，以便从 Key Vault 中检索机密。 

```java
vault1 = vault1.update()
            .defineAccessPolicy()
                .forServicePrincipal(clientId)
                .allowKeyAllPermissions()
                .allowSecretPermissions(SecretPermissions.GET)
                .allowSecretPermissions(SecretPermissions.LIST)
                .attach()
            .apply();
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/keyvault/management)


## <a name="samples"></a>示例

详细了解可在应用中使用的 [Azure Key Vault 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)。
