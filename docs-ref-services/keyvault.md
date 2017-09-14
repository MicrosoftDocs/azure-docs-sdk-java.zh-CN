---
title: "用于 Java 的 Azure Key Vault 库"
description: "用于 Java 的 Azure Key Vault 库的概述"
keywords: "Azure, Java, SDK, API, keyvault, 安全, 密钥, 机密, 保管库"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: keyvault
ms.openlocfilehash: d87ad458eeefb090a23529662695c4faa4a75b38
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="azure-key-vault-libraries-for-java"></a><span data-ttu-id="0dc63-104">用于 Java 的 Azure Key Vault 库</span><span class="sxs-lookup"><span data-stu-id="0dc63-104">Azure Key Vault libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="0dc63-105">概述</span><span class="sxs-lookup"><span data-stu-id="0dc63-105">Overview</span></span>

<span data-ttu-id="0dc63-106">使用 [Azure Key Vault](/azure/key-vault/) 保护及管理云应用程序与服务使用的加密密钥和机密。</span><span class="sxs-lookup"><span data-stu-id="0dc63-106">Safeguard and manage cryptographic keys and secrets used by cloud applications and services with [Azure Key Vault](/azure/key-vault/).</span></span>

<span data-ttu-id="0dc63-107">若要开始使用 Azure Key Vault，请参阅 [Azure Key Vault 入门](/azure/key-vault/key-vault-get-started)。</span><span class="sxs-lookup"><span data-stu-id="0dc63-107">To get started with Azure Key Vault, see [Get started with Azure Key Vault](/azure/key-vault/key-vault-get-started).</span></span>

## <a name="client-library"></a><span data-ttu-id="0dc63-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="0dc63-108">Client library</span></span>

<span data-ttu-id="0dc63-109">使用客户端库在 Azure Key Vault 中创建、更新和删除密钥与机密。</span><span class="sxs-lookup"><span data-stu-id="0dc63-109">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="0dc63-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="0dc63-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.0.0</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="0dc63-111">示例</span><span class="sxs-lookup"><span data-stu-id="0dc63-111">Example</span></span>

<span data-ttu-id="0dc63-112">从 Key Vault 检索[JSON Web 密钥](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)。</span><span class="sxs-lookup"><span data-stu-id="0dc63-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```java
KeyVaultClient kvc = new KeyVaultClient(credentials);
KeyBundle returnedKeyBundle = getKey(vaultUrl, keyName);
JsonWebKey jsonKey = returnedKeyBundle.key();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="0dc63-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="0dc63-113">Explore the Client APIs</span></span>](/java/api/overview/azure/keyvault/clientlibrary)


## <a name="management-api"></a><span data-ttu-id="0dc63-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="0dc63-114">Management API</span></span>

<span data-ttu-id="0dc63-115">使用 Azure Key Vault 管理库来创建 Key Vault、授权应用程序及管理权限。</span><span class="sxs-lookup"><span data-stu-id="0dc63-115">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="0dc63-116">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="0dc63-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-keyvault</artifactId>
    <version>1.2.1</version>
</dependency>
```

## <a name="example"></a><span data-ttu-id="0dc63-117">示例</span><span class="sxs-lookup"><span data-stu-id="0dc63-117">Example</span></span>

<span data-ttu-id="0dc63-118">使用[服务主体](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` 授权运行的应用程序，以便从 Key Vault 中检索机密。</span><span class="sxs-lookup"><span data-stu-id="0dc63-118">Authorize and application running with [service principal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) `clientId` to list and retrieve secrets from a key vault.</span></span> 

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
> [<span data-ttu-id="0dc63-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="0dc63-119">Explore the Management APIs</span></span>](/java/api/overview/azure/keyvault/managementapi)


## <a name="samples"></a><span data-ttu-id="0dc63-120">示例</span><span class="sxs-lookup"><span data-stu-id="0dc63-120">Samples</span></span>

<span data-ttu-id="0dc63-121">[管理 Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="0dc63-121">[Manage Key Vaults][1]</span></span>   

[1]: https://github.com/Azure-Samples/key-vault-java-manage-key-vaults

<span data-ttu-id="0dc63-122">详细了解可在应用中使用的 [Azure Key Vault 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault)。</span><span class="sxs-lookup"><span data-stu-id="0dc63-122">Explore more [sample Java code for Azure Key Vault](https://azure.microsoft.com/resources/samples/?platform=java&term=key+vault) you can use in your apps.</span></span>
