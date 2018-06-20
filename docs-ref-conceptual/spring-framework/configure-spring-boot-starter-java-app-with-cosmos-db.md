---
title: 如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm;yungez;kevinzha
ms.date: 02/01/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 6cf999f3db397760709476dae1f5c0fd83503725
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823800"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用

## <a name="overview"></a>概述

Azure Cosmos DB 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API（如 SQL、MongoDB、Graph 和表 API）处理数据。 Microsoft 的 Spring Boot Starter 使开发人员可以使用 Spring Boot 应用程序，这些应用程序通过使用 SQL API 与 Azure Cosmos DB 轻松集成。

本文演示如何使用 Azure 门户创建 Azure Cosmos DB，如何使用 **[Spring Initializr]** 创建自定义 Java 应用程序，以及如何将 Spring Boot Starter 功能添加到自定义应用程序，以使用 SQL API 在 Azure Cosmos DB 中执行数据的存储和检索操作。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。
* [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Cosmos DB

1. 浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“+ 新建”。

   ![Azure 门户][AZ01]

1. 单击“数据库”，然后单击“Azure Cosmos DB”。

   ![Azure 门户][AZ02]

1. 在“Azure Cosmos DB”页面上，输入以下信息：

   * 输入一个唯一的 ID，作为数据库的 URI。 例如：wingtiptoysdata.documents.azure.com。
   * 为 API 选择“SQL(文档 DB)”。
   * 选择要用于数据库的订阅。
   * 指定是否为数据库创建新的“资源组”，或选择现有资源组。
   * 为数据库指定“位置”。
   
   指定这些选项后，单击“创建”以创建数据库。

   ![Azure 门户][AZ03]

1. 创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出。 在任意这些位置单击数据库可打开缓存的属性页面。

   ![Azure 门户][AZ04]

1. 当显示数据库的属性页面时，单击“访问密钥”，然后复制数据库的 URI 和访问密钥，在 Spring Boot 应用程序中会用到这些值。

   ![Azure 门户][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定希望使用 Java 生成 Maven 项目，输入应用程序的“组”名称和“项目”名称，然后单击“生成项目”按钮。

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 使用“组”名称和“项目”名称创建程序包名称，例如：com.example.wintiptoys。
   >

1. 出现提示时，将项目下载到本地计算机中的路径。

   ![下载自定义 Spring Boot 项目][SI02]

1. 在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>配置 Spring Boot 应用，以使用 Azure Spring Boot Starter

1. 在应用的目录中找到 pom.xml 文件，例如：

   `C:\SpringBoot\wingtiptoys\pom.xml`

   -或-

   `/users/example/home/wingtiptoys/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. 在文本编辑器中打开 pom.xml 文件，然后将以下行添加到 `<dependencies>` 列表中：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

1. 保存并关闭 pom.xml 文件。

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>配置 Spring Boot 应用以使用 Azure Cosmos DB

1. 在应用的“资源”目录中找到 application.properties 文件，例如：

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   -或-

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![找到 application.properties 文件][RE01]

1. 在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为数据库的相应属性：

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![编辑 application.properties 文件][RE02]

1. 保存并关闭 application.properties 文件。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>添加示例代码以实现数据库的基本功能

本部分会创建两个存储用户数据的 Java 类，然后修改主应用程序类以创建用户类的实例并将其保存在数据库。

### <a name="define-a-basic-class-for-storing-user-data"></a>定义存储用户数据的基本类

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 User.java 的新文件。

1. 在文本编辑器中打开 User.java 文件，然后将以下行添加到文件中，以定义在数据库中存储和检索值的通用用户类：

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. 保存并关闭 User.java 文件。

### <a name="define-a-data-repository-interface"></a>定义数据存储库接口

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 UserRepository.java 的新文件。

1. 在文本编辑器中打开 UserRepository.java 文件，然后将以下行添加到文件中，以定义可扩展默认 DocumentDB 存储库接口的用户存储库接口：

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. 保存并关闭 UserRepository.java 文件。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   -或-

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. 保存并关闭主应用程序 Java 文件。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   `cd C:\SpringBoot\wingtiptoys`

   -或-

   `cd /users/example/home/wingtiptoys`

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. 你的应用程序将显示多个运行时消息，当显示 `User: testFirstName testLastName` 消息时，表示已成功在数据库中存储和检索值。

   ![成功地从应用程序输出][JV02]

1. 可选：使用 Azure 门户可以从数据库的属性页面查看 Azure Cosmos DB 的内容，方法是单击“文档资源管理器”，然后从现实的列表中选择项目以查看内容。

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a>后续步骤

有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：

* [Azure Cosmos DB 文档]。

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [用于 Azure 的 Spring Boot DocumenDB Starter]

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Build a SQL API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[用于 Azure 的 Spring Boot DocumenDB Starter]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV03.png
