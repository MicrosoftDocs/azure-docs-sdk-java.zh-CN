---
title: 使用 Maven 和 Azure 将 Spring Boot 应用部署到云中
description: 了解如何使用适用于 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到云中。
services: app-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: robmcm;kevinzha;brborges
ms.date: 06/01/2018
ms.devlang: java
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: ca788354d26964bd9f1e21a0d3a8005ff65ce4bc
ms.sourcegitcommit: 280d13b43cef94177d95e03879a5919da234a23c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "43324343"
---
# <a name="deploy-a-spring-boot-app-to-the-cloud-using-the-maven-plugin-for-azure-app-service"></a>使用适用于 Azure 应用服务的 Maven 插件将 Spring Boot 应用部署到云中

本文演示如何使用适用于 Azure 应用服务 Web 应用的 Maven 插件来部署一个示例 Spring Boot 应用程序。

> [!NOTE]
> 
> 用于 [Apache Maven](http://maven.apache.org/) 的[适用于 Azure 应用服务 Web 应用的 Maven 插件](https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将 Azure 应用服务无缝集成到 Maven 项目，简化了开发人员将 Web 应用部署到 Azure 应用服务的过程。

在使用 Maven 插件之前，请在 Maven Central 中检查该插件的最新可用版本：[![Maven Central](https://img.shields.io/maven-central/v/com.microsoft.azure/azure-webapp-maven-plugin.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-webapp-maven-plugin%22) 

## <a name="prerequisites"></a>先决条件

完成本教程中的步骤需要具备以下先决条件：

* 一个 Azure 订阅；如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。
* [Azure 命令行接口 (CLI)]。
* 最新 [Java 开发工具包 (JDK)] 1.7 版或更高版本。
* Apache 的 [Maven] 生成工具（版本 3）。
* [Git] 客户端。

## <a name="clone-the-sample-spring-boot-web-app"></a>克隆示例 Spring Boot Web 应用

本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。

1. 打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   - 或 -
   ```shell
   md ~/SpringBoot
   cd ~/SpringBoot
   ```

1. 将 [Spring Boot 入门]示例项目克隆到已创建的目录；例如：
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. 将目录更改为已完成项目；例如：
   ```shell
   cd gs-spring-boot/complete
   ```

1. 使用 Maven 生成 JAR 文件；例如：
   ```shell
   mvn clean package
   ```

1. 创建 Web 应用后，使用 Maven 启动 Web 应用；例如：
   ```shell
   mvn spring-boot:run
   ```

1. 使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。 例如，如果有可用的 Curl，可以使用以下命令：
   ```shell
   curl http://localhost:8080
   ```

1. 应会显示以下消息：“来自 Spring Boot 的问候！”

## <a name="adjust-project-for-war-based-deployment-on-azure-app-service"></a>根据 Azure 应用服务中基于 WAR 的部署调整项目

在本部分，我们将快速调整要在 Azure 应用服务中作为 WAR 文件部署的 Spring Boot 项目，该项目默认提供 Tomcat 作为运行时。 为此，需要修改两个文件：

- Maven `pom.xml` 文件
- `Application` Java 类

让我们从 Maven 设置开始：

1. 打开 `pom.xml`

1. 紧接在顶部的 `<artifactId>` 定义后面添加 `<packaging>war</packaging>`：
   ```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>

    <packaging>war</packaging>
   ```

1. 添加以下依赖项：
   ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
   ```

现在请打开类 `Application`（但愿此时 IDE 已拾取新的依赖项），然后继续进行以下修改：

1. 将 Application 类设为 `SpringBootServletInitializer` 的子类：
   ```java
   @SpringBootApplication
   public class Application extends SpringBootServletInitializer {
     // ...
   }
   ```

1. 将以下方法添加到 Application 类：
   ```java
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(Application.class);
       }
   ```
1. 组织导入以确保 `SpringApplicationBuilder` 和 `SpringBootServletInitializer` 正确导入。

现在，可将应用程序部署到 Tomcat 和其他任何 Servlet 运行时（例如 Jetty）。

## <a name="add-the-maven-plugin-for-azure-app-service-web-apps"></a>添加适用于 Azure 应用服务 Web 应用的 Maven 插件

在本部分，我们将添加一个 Maven 插件，用于自动完成将此应用程序部署到 Azure 应用服务 Web 应用的整个过程。

1. 再次打开 `pom.xml`。

1. 在 `<properties>` 中，使用属性 `maven.build.timestamp.format` 设置自定义时间戳格式。 由于 Azure 应用服务将为应用程序创建公共 URL，因此，此设置将用于生成部署名称，并避免与其他用户的实时部署发生冲突。
   ```xml
    <properties>
        <java.version>1.8</java.version>
        <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
    </properties>
   ```

1. 在 `<plugins>` 元素中添加以下内容：
   ```xml
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
    </plugin>
   ```

现在，可以使用这些设置将 Maven 项目实时部署到 Azure 应用服务 Web 应用。

## <a name="install-and-log-in-to-azure-cli"></a>安装并登录到 Azure CLI

获取用于部署 Spring Boot 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](https://docs.microsoft.com/cli/azure/)。 确保已安装 Azure CLI。

1. 通过使用 Azure CLI 登录到 Azure 帐户：
   
   ```shell
   az login
   ```
   
   按照说明完成登录过程。

## <a name="optionally-customize-pomxml-before-deploying"></a>（可选）在部署之前自定义 pom.xml

在文本编辑器中打开 Spring Boot 应用程序的 `pom.xml` 文件，然后找到 `azure-webapp-maven-plugin` 的 `<plugin>` 元素。 该元素应类似于以下示例：

   ```xml
  <plugins>
    <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <!-- Check latest version on Maven Central -->
      <version>1.1.0</version>
      <configuration>
         <resourceGroup>maven-projects</resourceGroup>
         <appName>${project.artifactId}-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>war</deploymentType>
      </configuration>
    </plugin>
  </plugins>
   ```

可以为 Maven 插件修改几个值，[适用于 Azure Web 应用的 Maven 插件]文档中提供了这些元素各自的详细描述。 尽管如此，在本文中有仍几个值得注意的值：

| 元素 | Description |
|---|---|
| `<version>` | 指定[适用于 Azure Web 应用的 Maven 插件]的版本。 验证 [Maven 中央存储库](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22)中列出的版本，确保使用最新版本。 |
| `<resourceGroup>` | 指定目标资源组，在此示例中为 `maven-plugin`。 如果资源组不存在，则会在部署过程中进行创建。 |
| `<appName>` | 指定 Web 应用的目标名称。 在此示例中，目标名称为 `maven-web-app-${maven.build.timestamp}`，此示例附加​​了 `${maven.build.timestamp}` 后缀以避免冲突。 （时间戳是可选项；可为应用名称指定任何唯一的字符串。） |
| `<region>` | 指定目标区域，在此示例中为 `westus`。 （[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。） |
| `<javaVersion>` | 为 Web 应用指定 Java 运行时版本。 （[适用于 Azure Web 应用的 Maven 插件]文档中提供了完整列表。） |
| `<deploymentType>` | 为 Web 应用指定部署类型。 默认为 `war`。 |

## <a name="build-and-deploy-your-web-app-to-azure"></a>生成 Web 应用并将其部署到 Azure

配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。 为此，请按照以下步骤操作：

1. 在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 将 Web 应用部署到 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 会将 Web 应用部署到 Azure；如果 Web 应用不存在，则将创建一个。

Web 部署完成后即可使用 [Azure 门户]进行管理。

* Web 应用将会在“应用服务”中列出：

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* Web 应用的 URL 会在 Web 应用的“概述”中列出：

   ![确定 Web 应用的 URL][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>后续步骤

有关本文中讨论的各项技术的详细信息，请参阅以下文章：

* [适用于 Azure Web 应用的 Maven 插件]

* [通过 Azure CLI 登录到 Azure](/azure/xplat-cli-connect)

* [如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [使用 Azure CLI 2.0 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 设置参考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure 门户]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: https://docs.microsoft.com/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
