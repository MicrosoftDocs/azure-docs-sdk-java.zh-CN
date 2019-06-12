---
title: 如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: f00afbdd09ce617f863ed758f4bdddcb40701e27
ms.sourcegitcommit: 5bbf64121a99019207ed8cca29280fc5183c7314
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2019
ms.locfileid: "66840845"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用

## <a name="overview"></a>概述

Azure Cosmos DB 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API（如 SQL、MongoDB、Graph 和表 API）处理数据。 Microsoft 的 Spring Boot Starter 使开发人员可以使用 Spring Boot 应用程序，这些应用程序通过使用 SQL API 与 Azure Cosmos DB 轻松集成。

本文演示如何使用 Azure 门户创建 Azure Cosmos DB，如何使用 **[Spring Initializr]** 创建自定义 Spring Boot 应用程序，以及如何将[用于 Azure 的 Spring Boot Cosmos DB Starter] 添加到自定义应用程序，以使用 Spring Data 和 Cosmos DB SQL API 在 Azure Cosmos DB 中执行数据的存储和检索操作。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Cosmos DB

1. 浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“创建资源”  。

   ![Azure 门户][AZ01]

1. 单击“数据库”，然后单击“Azure Cosmos DB”   。

   ![Azure 门户][AZ02]

1. 在“Azure Cosmos DB”页面上，输入以下信息  ：

   * 选择要用于数据库的订阅  。
   * 指定是否为数据库创建新的“资源组”，或选择现有资源组  。
   * 输入一个唯一的帐户名，作为数据库的 URI  。 例如：*wingtiptoysdata*。
   * 为 API 选择 **Core (SQL)** 。
   * 为数据库指定“位置”  。

   指定这些选项后，单击“查看 + 创建”以创建数据库  。

   ![Azure 门户][AZ03]

1. 创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出    。 在任意这些位置单击数据库可打开缓存的属性页面。

   ![Azure 门户][AZ04]

1. 当显示数据库的属性页面时，单击“密钥”  ，然后复制数据库的 URI 和访问密钥；在 Spring Boot 应用程序中会用到这些值。

   ![Azure 门户][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定你希望使用 Java 生成 Maven 项目，指定 Spring Boot 版本，输入应用程序的“组”名称和“项目”名称，在依赖项中添加“Azure 支持”，然后单击“生成项目”按钮        。

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 使用“组”名称和“项目”名称创建程序包名称，例如：com.example.wintiptoysdata    。
   >

1. 出现提示时，将项目下载到本地计算机中的路径，然后提取文件。

   ![提取自定义 Spring Boot 项目][SI02]

1. 在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>配置 Spring Boot 应用程序，以使用 Azure Spring Boot Starter

1. 在应用的目录中找到 pom.xml 文件，例如  ：

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -或-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. 在文本编辑器中打开 pom.xml 文件，然后将以下行添加到 `<dependencies>` 列表中  ：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

1. 验证 Spring Boot 版本是否是使用 Spring Initializr 创建应用程序时选择的版本；例如：

   ```xml
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. 验证你使用的是否是最新 [Azure Spring Boot Starter](https://github.com/microsoft/azure-spring-boot) 版本，例如：

   ```xml
   <azure.version>2.1.6</azure.version>
   ```

1. 保存并关闭 pom.xml 文件  。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>配置 Spring Boot 应用程序以使用 Azure Cosmos DB

1. 在应用的“资源”目录中找到 application.properties 文件，例如   ：

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

   ![找到 application.properties 文件][RE01]

1. 在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为数据库的相应属性  ：

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.cosmosdb.database=wingtiptoysdata
   ```

   ![编辑 application.properties 文件][RE02]

1. 保存并关闭 application.properties 文件  。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>添加示例代码以实现数据库的基本功能

本部分会创建两个存储用户数据的 Java 类，然后修改主应用程序类以创建 *User* 类的实例并将其保存到数据库。

### <a name="define-a-base-class-for-storing-user-data"></a>定义一个用于存储用户数据的基类

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 User.java 的新文件  。

1.  在文本编辑器中打开 User.java 文件，然后将以下行添加到文件中，以定义在数据库中存储和检索值的通用用户类：

   ```java
   package com.example.wingtiptoysdata;

   // Define a generic User class.
   public class User {
      private String id;
      private String firstName;
      private String lastName;

      public User() {
      }

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
         return String.format("User: %s %s %s", id, firstName, lastName);
      }
   }
   ```

1. 保存并关闭 User.java 文件  。

### <a name="define-a-data-repository-interface"></a>定义数据存储库接口

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 UserRepository.java 的新文件  。

1.  在文本编辑器中打开 UserRepository.java 文件，然后将以下行添加到文件中，以定义可扩展默认 DocumentDB 存储库接口的用户存储库接口：

   ```java
   package com.example.wingtiptoysdata;

   import com.microsoft.azure.spring.data.cosmosdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> { }
   ```

1. 保存并关闭 UserRepository.java 文件  。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用程序的程序包目录中找到主应用程序 Java 文件，例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

   ```java
    package com.example.wingtiptoysdata;

    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    import java.util.Optional;
    import java.util.UUID;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private final UserRepository repository;

        public WingtiptoysdataApplication(UserRepository repository) {
            this.repository = repository;
        }

        public static void main(String[] args) {
            // Execute the command line runner.
            SpringApplication.run(WingtiptoysdataApplication.class, args);
            System.exit(0);
        }

        public void run(String... args) throws Exception {
            // Create a unique identifier.
            String uuid = UUID.randomUUID().toString();

            // Create a new User class.
            final User testUser = new User(uuid, "John", "Doe");

            // For this example, remove all of the existing records.
            repository.deleteAll();

            // Save the User class to the Azure database.
            repository.save(testUser);

            // Retrieve the database record for the User class you just saved by ID.
            Optional<User> result = repository.findById(testUser.getId());

            // Display the results of the database record retrieval.
            System.out.println("\nSaved user is: " + result + "\n")
        }
    }
   ```

1. 保存并关闭主应用程序 Java 文件。

## <a name="build-and-test-your-app"></a>生成并测试应用

1.  打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   `cd C:\SpringBoot\wingtiptoysdata`

   -或-

   `cd /users/example/home/wingtiptoysdata`

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvnw clean spring-boot:run
   ```

1. 应用程序将显示多个运行时消息，并显示一条类似于以下示例的消息，指示已成功在数据库中存储和检索值。

   ```shell
   Saved user is: Optional[User: 24093cb5-55fe-4d2c-b459-cb8bafdd39fe John Doe]
   ```

   ![成功地从应用程序输出][JV02]

1. 可选：可以使用 Azure 门户从数据库的属性页查看 Azure Cosmos DB 的内容，方法是单击“数据资源管理器”，然后从显示的列表中选择项目以查看内容  。

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：

* [Azure Cosmos DB 文档]。

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [用于 Azure 的 Spring Boot Cosmos DB Starter]

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序  。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序  。

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[用于 Azure 的 Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
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
