---
title: 使用 Maven 和 Azure 将 Spring Boot JAR 文件应用部署到云中
description: 了解如何使用适用于 Linux 的 Azure Web 应用的 Maven 插件将 Spring Boot 应用部署到云中。
services: app-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: brborges
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.openlocfilehash: b133290d1f14429cbf36d6ed5a67d27e1a637593
ms.sourcegitcommit: 599405a9ce892d75073ef0776befa2fa22407b4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67237596"
---
# <a name="deploy-a-spring-boot-jar-file-web-app-to-azure-app-service-on-linux"></a>在 Linux 上将 Spring Boot JAR 文件 Web 应用部署到 Azure 应用服务

本文演示如何使用[适用于 Azure 应用服务 Web 应用的 Maven 插件](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)将打包为 Java SE JAR 的 Spring Boot 应用程序部署到 [Linux 上的 Azure 应用服务](/azure/app-service/containers/)。 如果要将应用的依赖关系、运行时和配置合并到单个可部署项目中，请选择通过 [Tomcat 和 WAR 文件](/azure/app-service/containers/quickstart-java)进行 Java SE 部署。


如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

若要完成本教程中的步骤，需要安装和配置以下内容：

* [Azure CLI](/cli/azure/)，在本地或通过 [Azure Cloud Shell](https://shell.azure.com)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](https://maven.apache.org/) 版本 3）。
* [Git](https://git-scm.com/downloads) 客户端。

## <a name="install-and-sign-in-to-azure-cli"></a>安装并登录 Azure CLI

获取用于部署 Spring Boot 应用程序的 Maven 插件的最简单方法是使用 [Azure CLI](/cli/azure/)。

通过使用 Azure CLI 登录到 Azure 帐户：
   
   ```shell
   az login
   ```
   
按照说明完成登录过程。

## <a name="clone-the-sample-app"></a>克隆示例应用

本部分将克隆完整的 Spring Boot 应用程序并进行本地测试。

1. 打开命令提示符或终端窗口，创建本地目录以存放 Spring Boot 应用程序，并更改为以下目录；例如：
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   \- 或 -
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

1. 应当会看到显示了以下消息：**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>配置适用于 Azure 应用服务的 Maven 插件

在本部分中，将配置 Spring Boot 项目 `pom.xml`，以便 Maven 可以将应用部署到 Linux 上的 Azure 应用服务。

1. 在代码编辑器中打开 `pom.xml`。

2. 在 pom.xml 的 `<build>` 节的 `<plugins>` 标记内添加以下 `<plugin>` 条目。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.6.0</version>
   </plugin>
   ```

3. 然后可以配置部署，在命令提示符下运行 maven 命令 `mvn azure-webapp:config`，并使用**数字**选择提示中的这些选项：
    * **OS**：linux  
    * **javaVersion**：jre8
    * **runtimeStack**：jre8

显示 **Confirm (Y/N)** 提示时，请按 **'y'** ，配置将完成。

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. jre8 [*]
2. java11
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. jre8
3. TOMCAT 8.5 [*]
4. WILDFLY 14
Enter index to use: 2
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. 将 `<appSettings>` 部分添加到 `<azure-webapp-maven-plugin>` 的 `<configuration>` 部分，以便在 *80* 端口上侦听。

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.6.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>

          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          ...
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。 为此，请按照以下步骤操作：

1. 在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如  ：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 将 Web 应用部署到 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 会将 Web 应用部署到 Azure；如果 Web 应用或 Web 应用计划尚不存在，则将为你创建一个。

Web 部署完成后即可通过 [Azure 门户]进行管理。

* Web 应用将会在“应用服务”中列出  ：

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* Web 应用的 URL 会在 Web 应用的“概述”中列出  ：

   ![确定 Web 应用的 URL][AP02]

通过与以前相同的 cURL 命令（使用来自门户的 Web 应用 URL而不是 `localhost`）验证部署是否成功。 应当会看到显示了以下消息：**Greetings from Spring Boot!** 

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关本文中讨论的各项技术的详细信息，请参阅以下文章：

* [适用于 Azure Web 应用的 Maven 插件]

* [如何使用适用于 Azure Web 应用的 Maven 插件将容器化 Spring Boot 应用部署到 Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [使用 Azure CLI 2.0 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 设置参考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /java/azure/
[Azure 门户]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[适用于 Azure Web 应用的 Maven 插件]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP01.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/AP02.png
