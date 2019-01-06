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
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 69c0381313994796af31d5301ceadb9f6f40dcb5
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991551"
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

此 Spring Boot 起动器为 Azure 服务提供了自动配置支持，这些服务包括：服务总线、存储、Active Directory、Cosmos DB、Key Vault，等等。

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
## <a name="azure-storage"></a>Azure 存储

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

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure 上的 [Spring Boot] 应用程序的详细信息，请参阅 [Azure 上的 Spring]。

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /java/azure/
[使用 Azure DevOps 和 Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure 上的 Spring]: /java/azure/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
