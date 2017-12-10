---
title: "适用于 Azure 的 Spring Boot 起动器"
description: "本文介绍适用于 Azure 的各种 Spring Boot 起动器。"
services: 
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 12/01/2017
ms.author: robmcm
ms.openlocfilehash: ba5829407d03d275784a3a458d88ea47ac8494b8
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2017
---
# <a name="spring-boot-starters-for-azure"></a>适用于 Azure 的 Spring Boot 起动器

本文介绍适用于 [Spring Initializr] 的，可为 Java 开发人员提供集成功能来使用 Microsoft Azure 的各种 Spring Boot 起动器。

![Azure Spring Boot 起动器][spring-boot-starters]

以下 Spring Boot 起动器目前适用于 Azure：

* **[Azure 支持](#azure-support)**

   为服务总线、存储、Active Directory 等 Azure 服务提供自动配置支持。

* **[Azure Active Directory](#azure-active-directory)**

   为 Spring 安全性与用于身份验证的 Azure Active Directory 提供集成支持。

* **[Azure Key Vault](#azure-key-vault)**

   为 Azure Key Vault 机密集成提供 Spring 值注释支持。

* **[Azure 存储](#azure-storage)**

   为 Azure 存储服务提供 Spring Boot 支持。

<a name="azure-support"></a>
## <a name="azure-support"></a>Azure 支持

此 Spring Boot 起动器为服务总线、存储、Active Directory、Cosmos DB、Key Vault 等 Azure 服务提供自动配置支持。

有关如何使用此起动器提供的各种 Azure 功能的示例，请参阅以下文章：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* 将以下节添加到文件：

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
## <a name="azure-active-directory"></a>Azure Active Directory

此 Spring Boot 起动器为 Spring 安全性提供自动配置支持，以便能够与用于身份验证的 Azure Active Directory 集成。

有关如何使用此起动器提供的 Azure Active Directory 功能的示例，请参阅以下文章：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* 将以下节添加到文件：

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
## <a name="azure-key-vault"></a>Azure 密钥保管库

此 Spring Boot 起动器为 Azure Key Vault 机密集成提供 Spring 值注释支持。

有关如何使用此起动器提供的 Azure Key Vault 功能的示例，请参阅以下文章：

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* 将以下节添加到文件：

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
## <a name="azure-storage"></a>Azure 存储空间

此 Spring Boot 起动器为 Azure 存储服务提供 Spring Boot 集成支持。

有关如何使用此起动器提供的 Azure 存储功能的示例，请参阅以下文章：

* [如何使用适用于 Azure 存储的 Spring Boot 起动器](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

将此起动器添加到 Spring Boot 项目时，会对 *pom.xml* 文件进行以下更改：

* 将以下属性添加到 `<properties>` 元素：

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* 将默认的 `spring-boot-starter` 依赖项替换为以下内容：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* 将以下节添加到文件：

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

## <a name="next-steps"></a>后续步骤

有关使用 Azure 上的 [Spring Boot] 应用程序的详细信息，请参阅 [Azure 上的 Spring]。

有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

若要获取 Spring Boot 应用程序入门的相关帮助，请参阅 https://start.spring.io/ 中的“Spring Initializr”。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: https://docs.microsoft.com/java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
