---
title: 在用于容器的 Azure 应用服务上部署 Spring Boot Web 应用
description: 本教程将指导用户完成在 Microsoft Azure 中将 Spring Boot 应用程序部署为 Linux Web 应用的步骤。
services: azure app service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: azure app service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 407b852e24ef88d2fb075bd064f1acf2b107ddc1
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270868"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a>在用于容器的 Azure 应用服务上部署 Spring Boot 应用程序

本教程介绍如何使用 [Docker] 将 [Spring Boot] 应用程序容器化并将自己的 docker 映像部署到 [Azure 应用服务](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)中的 Linux 主机。

## <a name="prerequisites"></a>先决条件

完成本教程中的步骤需要具备以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* [Azure 命令行接口 (CLI)]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven] 生成工具（版本 3）。
* [Git] 客户端。
* [Docker] 客户端。

> [!NOTE]
>
> 由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>创建 Docker 上的 Spring Boot 入门 Web 应用

以下步骤将引导你完成以下步骤：创建一个简单的 Spring Boot Web 应用程序并对其进行本地测试。

1. 打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   - 或 -
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 将 [Docker 上的 Spring Boot 入门]示例项目克隆到创建的目录中；例如：
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 将目录更改为已完成项目；例如：
   ```
   cd gs-spring-boot-docker/complete
   ```

1. 使用 Maven 生成 JAR 文件；例如：
   ```
   mvn package
   ```

1. 创建 Web 应用后，将目录更改为 JAR 文件所在的 `target` 目录并启动 Web 应用；例如：
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. 使用 Web 浏览器在本地浏览到 Web 应用并对其进行测试。 例如，如果你有可用的 curl，并且已将 Tomcat 服务器配置为在端口 80 上运行：
   ```
   curl http://localhost
   ```

1. 应当会看到显示了以下消息：**Hello Docker World!**

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>创建 Azure 容器注册表以用作专用 Docker 注册表

以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。

> [!NOTE]
>
> 如果你想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表](/azure/container-registry/container-registry-get-started-azure-cli)中的步骤操作。
>

1. 浏览到 [Azure 门户]并登录。

   登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。

1. 单击“+ 新建”  菜单图标，然后依次单击“容器”、  “Azure 容器注册表”  。
   
   ![创建新的 Azure 容器注册表][AR01]

1. 显示 Azure 容器注册表模板的信息页面时，请单击“创建”  。 

   ![创建新的 Azure 容器注册表][AR02]

1. 当显示“创建容器注册表”  页时，输入你的“注册表名称”  和“资源组”  ，为“管理员用户”  选择“启用”  ，然后单击“创建”  。

   ![配置 Azure 容器注册表设置][AR03]

1. 创建容器注册表后，在 Azure 门户中导航到你的容器注册表，然后单击“访问密钥”  。 记下用户名和密码，以供后续步骤中使用。

   ![Azure 容器注册表访问密钥][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>配置 Maven 以使用你的 Azure 容器注册表访问密钥

1. 导航到 Spring Boot 应用程序的已完成项目目录（例如：“C:\SpringBoot\gs-spring-boot-docker\complete”  或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”  ），并使用文本编辑器打开 pom.xml  文件。

1. 使用本教程上一部分提供的最新版 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)、登录服务器值以及 Azure 容器注册表的访问设置更新 pom.xml  文件中的 `<properties>` 集合。 例如：

   ```xml
   <properties>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. 将 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 添加到 *pom.xml* 文件中的 `<plugins>` 集合，在 `<from>/<image>` 中指定基础映像，指定最终映像名称 `<to>/<image>`，并在 `<to>/<auth>` 中指定上一部分提供的用户名和密码。 例如：

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. 导航到 Spring Boot 应用程序的已完成项目目录，然后运行以下命令以重新生成应用程序，并将容器推送到 Azure 容器注册表：

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> 使用 Jib 将映像推送到 Azure 容器注册表时，该映像不会认可 *Dockerfile*。有关详细信息，请参阅[此](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html)文档。
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>在 Azure 应用服务中使用容器映像创建 Linux 上的 Web 应用

1. 浏览到 [Azure 门户]并登录。

2. 依次单击“+ 创建资源”  菜单图标、“Web”、  “用于容器的 Web 应用”  。
   
   ![在 Azure 门户中创建新的 Web 应用][LX01]

3. 当显示“Linux 上的 Web 应用”  页时，输入以下信息：

   a. 为“应用名称”输入唯一名称；例如：“wingtiptoyslinux”   。

   b. 从下拉列表中选择一个订阅  。

   c. 选择现有资源组  ，或指定名称以创建新资源组。

   d. 选择 *Linux* 作为 **OS**。

   e. 单击“应用服务计划/位置”，选择现有的应用服务计划；或者单击“新建”，创建新的应用服务计划。  

   f. 单击“配置容器”  边栏选项卡，然后输入以下信息：

   * 选择“单一容器”和“Azure 容器注册表”。  

   * **注册表**：选择此前创建的容器名称，例如“*wingtiptoysregistry*”

   * **映像**：选择映像名称，例如“*gs-spring-boot-docker*”
   
   * **标记**：选择映像的标记，例如“*latest*”
   
   * **启动文件**：将此项留空，因为映像已经有启动命令
   
   e. 输入上述所有信息后，请单击“应用”  。

   ![配置 Web 应用设置][LX02]

4. 单击“创建”。 

> [!NOTE]
>
> Azure 会将 Internet 请求自动映射到在标准端口 80 或 8080 上运行的嵌入式 Tomcat 服务器。 但是，如果已将嵌入式 Tomcat 服务器配置为在自定义端口上运行，则需要将一个环境变量添加到为嵌入式 Tomcat 服务器定义端口的 Web 应用。 为此，请按照以下步骤操作：
>
> 1. 浏览到 [Azure 门户]并登录。
> 
> 2. 单击**应用服务**的图标，然后从列表中选择你的 Web 应用。
>
> 4. 单击“配置”  。 （下图中的第 1 项。）
>
> 5. 在“应用程序设置”  部分中，添加一个名为 **PORT** 的新设置，并为该值输入自定义端口号。 （下图中的第 2、3、4 项。）
>
> 6. 单击“ **保存**”。 （请参阅下图中的第 5 项。）
>
> ![在 Azure 门户中保存自定义端口号][LX03]
>

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

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)
* [在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅 [Docker 上的 Spring Boot 入门]。

如果在开始使用自己的 Spring Boot 应用程序时需要帮助，请参阅 https://start.spring.io/ 上的 **Spring Initializr**。

有关开始创建简单 Spring Boot 应用程序入门的详细信息，请参阅 https://start.spring.io/ 上的“Spring Initializr”。

有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure 门户]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-linux/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png

[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
