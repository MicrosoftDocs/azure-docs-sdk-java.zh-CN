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
ms.sourcegitcommit: 151aaa6ccc64d94ed67f03e846bab953bde15b4a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
ms.locfileid: "28954438"
---
# <a name="spring-boot-starters-for-azure"></a><span data-ttu-id="7e155-103">适用于 Azure 的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="7e155-103">Spring Boot Starters for Azure</span></span>

<span data-ttu-id="7e155-104">本文介绍适用于 [Spring Initializr] 的，可为 Java 开发人员提供集成功能来使用 Microsoft Azure 的各种 Spring Boot 起动器。</span><span class="sxs-lookup"><span data-stu-id="7e155-104">This article describes the various Spring Boot Starters for the [Spring Initializr] that provide Java developers with integration features for working with Microsoft Azure.</span></span>

![Azure Spring Boot 起动器][spring-boot-starters]

<span data-ttu-id="7e155-106">以下 Spring Boot 起动器目前适用于 Azure：</span><span class="sxs-lookup"><span data-stu-id="7e155-106">The following Spring Boot Starters are currently available for Azure:</span></span>

* <span data-ttu-id="7e155-107">**[Azure 支持](#azure-support)**</span><span class="sxs-lookup"><span data-stu-id="7e155-107">**[Azure Support](#azure-support)**</span></span>

   <span data-ttu-id="7e155-108">为服务总线、存储、Active Directory 等 Azure 服务提供自动配置支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-108">Provides auto-configuration support for Azure Services; e.g. Service Bus, Storage, Active Directory, etc.</span></span>

* <span data-ttu-id="7e155-109">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="7e155-109">**[Azure Active Directory](#azure-active-directory)**</span></span>

   <span data-ttu-id="7e155-110">为 Spring 安全性与用于身份验证的 Azure Active Directory 提供集成支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-110">Provides integration support for Spring Security with Azure Active Directory for authentication.</span></span>

* <span data-ttu-id="7e155-111">**[Azure Key Vault](#azure-key-vault)**</span><span class="sxs-lookup"><span data-stu-id="7e155-111">**[Azure Key Vault](#azure-key-vault)**</span></span>

   <span data-ttu-id="7e155-112">为 Azure Key Vault 机密集成提供 Spring 值注释支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-112">Provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

* <span data-ttu-id="7e155-113">**[Azure 存储](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="7e155-113">**[Azure Storage](#azure-storage)**</span></span>

   <span data-ttu-id="7e155-114">为 Azure 存储服务提供 Spring Boot 支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-114">Provides Spring Boot support for Azure Storage services.</span></span>

<a name="azure-support"></a>
## <a name="azure-support"></a><span data-ttu-id="7e155-115">Azure 支持</span><span class="sxs-lookup"><span data-stu-id="7e155-115">Azure Support</span></span>

<span data-ttu-id="7e155-116">此 Spring Boot 起动器为服务总线、存储、Active Directory、Cosmos DB、Key Vault 等 Azure 服务提供自动配置支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-116">This Spring Boot Starter provides auto-configuration support for Azure Services; for example: Service Bus, Storage, Active Directory, Cosmos DB, Key Vault, etc.</span></span>

<span data-ttu-id="7e155-117">有关如何使用此起动器提供的各种 Azure 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="7e155-117">For examples of how to use the various Azure features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="7e155-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span><span class="sxs-lookup"><span data-stu-id="7e155-118"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples></span></span>

<span data-ttu-id="7e155-119">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="7e155-119">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="7e155-120">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="7e155-120">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="7e155-121">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="7e155-121">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* <span data-ttu-id="7e155-122">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="7e155-122">The following section is added to the file:</span></span>

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
## <a name="azure-active-directory"></a><span data-ttu-id="7e155-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e155-123">Azure Active Directory</span></span>

<span data-ttu-id="7e155-124">此 Spring Boot 起动器为 Spring 安全性提供自动配置支持，以便能够与用于身份验证的 Azure Active Directory 集成。</span><span class="sxs-lookup"><span data-stu-id="7e155-124">This Spring Boot Starter provides auto-configuration support for Spring Security in order to provide integration with Azure Active Directory for authentication.</span></span>

<span data-ttu-id="7e155-125">有关如何使用此起动器提供的 Azure Active Directory 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="7e155-125">For examples of how to use the Azure Active Directory features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="7e155-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="7e155-126"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample></span></span>

<span data-ttu-id="7e155-127">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="7e155-127">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="7e155-128">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="7e155-128">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="7e155-129">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="7e155-129">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="7e155-130">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="7e155-130">The following section is added to the file:</span></span>

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
## <a name="azure-key-vault"></a><span data-ttu-id="7e155-131">Azure 密钥保管库</span><span class="sxs-lookup"><span data-stu-id="7e155-131">Azure Key Vault</span></span>

<span data-ttu-id="7e155-132">此 Spring Boot 起动器为 Azure Key Vault 机密集成提供 Spring 值注释支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-132">This Spring Boot Starter provides Spring value annotation support for integration with Azure Key Vault Secrets.</span></span>

<span data-ttu-id="7e155-133">有关如何使用此起动器提供的 Azure Key Vault 功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="7e155-133">For examples of how to use the Azure Key Vault features that are provided by this starter, see the following:</span></span>

* <span data-ttu-id="7e155-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="7e155-134"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample></span></span>

<span data-ttu-id="7e155-135">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="7e155-135">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="7e155-136">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="7e155-136">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="7e155-137">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="7e155-137">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="7e155-138">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="7e155-138">The following section is added to the file:</span></span>

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
## <a name="azure-storage"></a><span data-ttu-id="7e155-139">Azure 存储</span><span class="sxs-lookup"><span data-stu-id="7e155-139">Azure Storage</span></span>

<span data-ttu-id="7e155-140">此 Spring Boot 起动器为 Azure 存储服务提供 Spring Boot 集成支持。</span><span class="sxs-lookup"><span data-stu-id="7e155-140">This Spring Boot Starter provides Spring Boot integration support for Azure Storage services.</span></span>

<span data-ttu-id="7e155-141">有关如何使用此起动器提供的 Azure 存储功能的示例，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="7e155-141">For examples of how to use the Azure Storage features that are provided by this starter, see the following:</span></span>

* [<span data-ttu-id="7e155-142">如何使用适用于 Azure 存储的 Spring Boot 起动器</span><span class="sxs-lookup"><span data-stu-id="7e155-142">How to use the Spring Boot Starter for Azure Storage</span></span>](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <span data-ttu-id="7e155-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span><span class="sxs-lookup"><span data-stu-id="7e155-143"><https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample></span></span>

<span data-ttu-id="7e155-144">将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="7e155-144">When you add this starter to a Spring Boot project, the following changes are made to the *pom.xml* file:</span></span>

* <span data-ttu-id="7e155-145">将以下属性添加到 `<properties>` 元素：</span><span class="sxs-lookup"><span data-stu-id="7e155-145">The following property is added to `<properties>` element:</span></span>

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* <span data-ttu-id="7e155-146">将默认的 `spring-boot-starter` 依赖项替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="7e155-146">The default `spring-boot-starter` dependency is replaced with the following:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* <span data-ttu-id="7e155-147">将以下节添加到文件：</span><span class="sxs-lookup"><span data-stu-id="7e155-147">The following section is added to the file:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7e155-148">后续步骤</span><span class="sxs-lookup"><span data-stu-id="7e155-148">Next steps</span></span>

<span data-ttu-id="7e155-149">有关使用 Azure 上的 [Spring Boot] 应用程序的详细信息，请参阅 [Azure 上的 Spring]。</span><span class="sxs-lookup"><span data-stu-id="7e155-149">For more information about using [Spring Boot] applications on Azure, see [Spring on Azure].</span></span>

<span data-ttu-id="7e155-150">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="7e155-150">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="7e155-151">若要获取 Spring Boot 应用程序入门的相关帮助，请参阅 https://start.spring.io/ 中的“Spring Initializr”。</span><span class="sxs-lookup"><span data-stu-id="7e155-151">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

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
