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
ms.openlocfilehash: 950b360eb525b0c6b97daad0798c27ded0582b8b
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991341"
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

1. 应当会看到显示了以下消息：**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>配置适用于 Azure 应用服务的 Maven 插件

在本部分中，将配置 Spring Boot 项目 `pom.xml`，以便 Maven 可以将应用部署到 Linux 上的 Azure 应用服务。

1. 在代码编辑器中打开 `pom.xml`。

2. 在 pom.xml 的 `<build>` 节的 `<plugins>` 标记内添加以下 `<plugin>` 条目。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>
      <deploymentType>jar</deploymentType>

      <!-- configure app to run on port 80, required by App Service -->
      <appSettings>
        <property> 
          <name>JAVA_OPTS</name> 
          <value>-Dserver.port=80</value> 
        </property> 
      </appSettings>

      <!-- Web App information -->
      <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
      <appName>${WEBAPP_NAME}</appName>
      <region>${REGION}</region>  

      <!-- Java Runtime Stack for Web App on Linux-->
      <linuxRuntime>jre8</linuxRuntime>
    </configuration>
   </plugin>
   ```

3. 更新插件配置中的以下占位符：

| 占位符 | 说明 |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | 要在其中创建 Web 应用的新资源组的名称。 通过将应用的所有资源都放在一个组中，可以一起管理它们。 例如，删除资源组会删除与该应用关联的所有资源。 使用唯一的新资源组名称（例如 *TestResources*）更新此值。 将在后面的部分使用此资源组名称来清除所有 Azure 资源。 |
| `WEBAPP_NAME` | 应用名称将成为部署到 Azure 时的 Web 应用 (WEBAPP_NAME.azurewebsites.net) 的主机名的一部分。 使用将用于托管 Java 应用的新 Azure Web 应用的唯一名称（例如 *contoso*）更新此值。 |
| `REGION` | 托管着 Web 应用的 Azure 区域，例如 `westus2`。 可以从 Cloud Shell 或 CLI 使用 `az account list-locations` 命令获取区域列表。 |

可以在 [GitHub 上的 Maven 插件参考](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)中找到完整的配置选项列表。

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

配置了本文前面部分中的所有设置后，就可以将 Web 应用部署到 Azure。 为此，请按照以下步骤操作：

1. 在之前使用的命令提示符或终端窗口中，如果对 pom.xml 文件进行了任何更改，请使用 Maven 重新生成 JAR 文件；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 将 Web 应用部署到 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 会将 Web 应用部署到 Azure；如果 Web 应用或 Web 应用计划尚不存在，则将为你创建一个。

Web 部署完成后即可通过 [Azure 门户]进行管理。

* Web 应用将会在“应用服务”中列出：

   ![Azure 门户应用服务中列出的 Web 应用][AP01]

* Web 应用的 URL 会在 Web 应用的“概述”中列出：

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
