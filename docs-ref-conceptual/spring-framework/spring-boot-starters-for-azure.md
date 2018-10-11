---
title: 适用于 Azure 的 Spring Boot 起动器
description: 本文介绍适用于 Azure 的各种 Spring Boot 起动器。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 678d4b279cecb83c95b3bf0f6bcdf1581924aa62
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893498"
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="30ea7-103">适用于 Azure 的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="30ea7-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="30ea7-104">本文介绍适用于 [Spring Initializr] 的，可为 Java 开发人员提供集成功能来使用 Microsoft Azure 的各种 Spring Boot 起动器。</span><span class="sxs-lookup"><span data-stu-id="30ea7-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Azure Spring Boot 起动器][spring-boot-starters]

<span data-ttu-id="30ea7-106">以下 Spring Boot 起动器目前适用于 Azure：</span><span class="sxs-lookup"><span data-stu-id="30ea7-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="30ea7-107">**[Azure 支持](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="30ea7-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="30ea7-108">为服务总线、存储、Active Directory 等 Azure 服务提供自动配置支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="30ea7-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="30ea7-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="30ea7-110">为 Spring 安全性与用于身份验证的 Azure Active Directory 提供集成支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="30ea7-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="30ea7-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="30ea7-112">为 Azure Key Vault 机密集成提供 Spring 值注释支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="30ea7-113">**[Azure 存储](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="30ea7-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="30ea7-114">为 Azure 存储服务提供 Spring Boot 支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="30ea7-115">Azure 支持</span><span class="sxs-lookup"><span data-stu-id="30ea7-115">Azure Support</span></span>

<span data-ttu-id="30ea7-116">此 Spring Boot 起动器为服务总线、存储、Active Directory、Cosmos DB、Key Vault 等 Azure 服务提供自动配置支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="30ea7-117">有关如何使用此起动器提供的各种 Azure 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="30ea7-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

<span data-ttu-id="30ea7-118">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="30ea7-118">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="30ea7-119">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="30ea7-119">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="30ea7-120">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="30ea7-120">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="30ea7-121">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="30ea7-121">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a><span data-ttu-id="30ea7-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="30ea7-122">Azure Active Directory</span></span>

<span data-ttu-id="30ea7-123">此 Spring Boot 起动器为 Spring 安全性提供自动配置支持，以便能够与用于身份验证的 Azure Active Directory 集成。</span><span class="sxs-lookup"><span data-stu-id="30ea7-123">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="30ea7-124">有关如何使用此起动器提供的 Azure Active Directory 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="30ea7-124">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

<span data-ttu-id="30ea7-125">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="30ea7-125">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="30ea7-126">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="30ea7-126">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="30ea7-127">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="30ea7-127">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="30ea7-128">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="30ea7-128">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a><span data-ttu-id="30ea7-129">Azure 密钥保管库</span><span class="sxs-lookup"><span data-stu-id="30ea7-129">Azure Key Vault</span></span>

<span data-ttu-id="30ea7-130">此 Spring Boot 起动器为 Azure Key Vault 机密集成提供 Spring 值注释支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-130">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="30ea7-131">有关如何使用此起动器提供的 Azure Key Vault 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="30ea7-131">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

<span data-ttu-id="30ea7-132">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="30ea7-132">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="30ea7-133">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="30ea7-133">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="30ea7-134">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="30ea7-134">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="30ea7-135">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="30ea7-135">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a><span data-ttu-id="30ea7-136">Azure 存储</span><span class="sxs-lookup"><span data-stu-id="30ea7-136">Azure Storage</span></span>

<span data-ttu-id="30ea7-137">此 Spring Boot 起动器为 Azure 存储服务提供 Spring Boot 集成支持。</span><span class="sxs-lookup"><span data-stu-id="30ea7-137">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="30ea7-138">有关如何使用此起动器提供的 Azure 存储功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="30ea7-138">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="30ea7-139">如何使用适用于 Azure 存储的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="30ea7-139">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

<span data-ttu-id="30ea7-140">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="30ea7-140">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="30ea7-141">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="30ea7-141">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="30ea7-142">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="30ea7-142">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="30ea7-143">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="30ea7-143">The following section is added to the file:</span></span>

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a><span data-ttu-id="30ea7-144">后续步骤</span><span class="sxs-lookup"><span data-stu-id="30ea7-144">Next steps</span></span>

<span data-ttu-id="30ea7-145">有关使用 Azure 上的 [Spring Boot] 应用程序的详细信息，请参阅 [Azure 上的 Spring]。</span><span class="sxs-lookup"><span data-stu-id="30ea7-145">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="30ea7-146">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="30ea7-146">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="30ea7-147">如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。</span><span class="sxs-lookup"><span data-stu-id="30ea7-147">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<!-- URL List -->

[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring on Azure]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
