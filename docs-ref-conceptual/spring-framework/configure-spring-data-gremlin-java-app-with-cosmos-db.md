---
title: 如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.date: 12/19/2018
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 70bed5696048af1de857f1064bf98e83ab96ca53
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53991561"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用

## <a name="overview"></a>概述

Spring Data Gremlin Starter 为 Apache 中的 Gremlin 查询语言提供 Spring Data 支持，开发人员可对 Gremlin 兼容的任何数据存储使用这项支持。

本文演示如何使用 Azure 门户创建可与 Gremlin API 配合使用的 Azure Cosmos DB，使用 **[Spring Initializr]** 创建自定义的 Java 应用程序，然后将 Spring Data Gremlin Starter 功能添加到自定义应用程序用于存储数据，并使用 Gremlin 从 Azure Cosmos DB 检索数据。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

> [!IMPORTANT]
>
> 完成本文中的步骤需要 Spring Boot 2.0 或更高版本。
>

## <a name="create-an-azure-cosmos-db-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Cosmos DB

### <a name="create-your-azure-cosmos-database-for-use-with-gremlin-api"></a>创建与 Gremlin API 配合使用的 Azure Cosmos 数据库

1. 浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“创建资源”。

   ![创建资源][AZ01]

1. 单击“数据库”，然后单击“Azure Cosmos DB”。

   ![创建 Azure Cosmos DB][AZ02]

1. 在“Azure Cosmos DB”页面上，输入以下信息：

   * 输入唯一的 **ID**，用作数据库 Gremlin URI 的一部分。 例如：如果输入了 **wingtiptoysdata** 作为 **ID**，则 Gremlin URI 将是 *wingtiptoysdata.gremlin.cosmosdb.azure.com*。
   * 选择“Gremlin (图形)”作为 API。
   * 选择要用于数据库的订阅。
   * 指定是否为数据库创建新的“资源组”，或选择现有资源组。
   * 为数据库指定“位置”。
   
   指定这些选项后，单击“创建”以创建数据库。

   ![指定 Azure Cosmos DB 选项][AZ03]

1. 创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出。 在任意这些位置单击数据库可打开缓存的属性页面。

   ![所有资源][AZ04]

1. 当显示数据库的属性页面时，单击“访问密钥”，然后复制数据库的 URI 和访问密钥，在 Spring Boot 应用程序中会用到这些值。

   ![访问密钥][AZ05]

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>将图形添加到 Azure Cosmos 数据库

1. 依次单击“数据资源管理器”、“新建图形”。

   ![新建图形][AZ06]

1. 显示“添加图形”后，输入以下信息：

   * 指定数据库的唯一“数据库 ID”。
   * 指定图形的唯一“图形 ID”。
   * 可以选择指定“存储容量”，或者接受默认值。
   * 指定“吞吐量”，对于本示例，可以选择 400 个请求单位 (RU)。
   
   指定这些选项后，单击“确定”以创建图形。

   ![添加图形][AZ07]

1. 创建图形后，可以使用“数据资源管理器”来查看它。

   ![图形属性界面][AZ08]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定想要使用 Java 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，指定 **Spring Boot** 版本（2.0 或以上），然后单击“生成项目”。

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：*com.example.wintiptoysdata。
   >

1. 出现提示时，将项目下载到本地计算机中的路径。

   ![下载自定义 Spring Boot 项目][SI02]

1. 在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

   ![自定义 Spring Boot 项目文件][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>将 Spring Boot 应用配置为使用 Spring Data Gremlin Starter

1. 在应用的目录中找到 pom.xml 文件，例如：

   `C:\SpringBoot\wingtiptoysdata\pom.xml`

   -或-

   `/users/example/home/wingtiptoysdata/pom.xml`

   ![找到 pom.xml 文件][PM01]

1. 在文本编辑器中打开 *pom.xml* 文件，将 Spring Data Gremlin Starter 添加到 `<dependencies>` 列表：

   ```xml
   <dependency>
      <groupId>com.microsoft.spring.data.gremlin</groupId>
      <artifactId>spring-data-gremlin</artifactId>
      <version>2.0.0</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][PM02]

1. 保存并关闭 pom.xml 文件。

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>配置 Spring Boot 应用以使用 Azure Cosmos DB

1. 找到应用的 *resources* 目录，并创建名为 *application.yml* 的新文件。 例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.yml`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/resources/application.yml`

   ![创建 application.yml 文件][RE01]

1. 在文本编辑器中打开 *application.yml* 文件，将以下行添加到该文件，然后将示例值替换为数据库的相应属性：

   ```yaml
   gremlin:
      endpoint: wingtiptoys.gremlin.cosmosdb.azure.com
      port: 443
      username: /dbs/wingtiptoysdb/colls/wingtiptoysgraph
      password: 57686f6120447564652c20426f6220526f636b73==
      telemetryAllowed: false
   ```
   
   其中：
   
   | 字段 | 说明 |
   |---|---|
   | `endpoint` | 指定数据库的 Gremlin URI，此 URI 派生自前面在本教程中创建 Azure Cosmos DB 时指定的唯一“ID”。 |
   | `port` | 指定 TCP/IP 端口，对于 HTTPS 通信，应是 **443**。 |
   | `username` | 指定前面在本教程中添加图形时使用的唯一“数据库 ID”和“图形 ID”；必须使用以下语法输入这些值："/dbs/**{数据库ID}**/colls/**{图形 ID}**"。 |
   | `password` | 指定前面在本教程中复制的主要或辅助“访问密钥”。 |
   | `telemetryAllowed` | 若要启用遥测，请指定 **true**；否则指定 **false**。

1. 保存并关闭 application.yml 文件。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>添加示例代码以实现数据库的基本功能

在本部分，我们将创建所需的 Java 类，用于在数据库中存储数据。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

   ![找到应用程序 Java 文件][JV01]

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

   ```java
   package com.example.wingtiptoysdata;
   
   // These imports are required for the application.
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.example.wingtiptoysdata.domain.Network;
   import com.example.wingtiptoysdata.domain.Person;
   import com.example.wingtiptoysdata.domain.Relation;
   import com.example.wingtiptoysdata.repository.NetworkRepository;
   import com.example.wingtiptoysdata.repository.PersonRepository;
   import com.example.wingtiptoysdata.repository.RelationRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   @SpringBootApplication
   public class WingtiptoysdataApplication {
   
       // Define several person classes to store personal data.
       private final Person person1 = new Person("01", "Nellie Hughes", "23");
       private final Person person2 = new Person("02", "Delmar Alfred", "34");
       private final Person person3 = new Person("03", "Kelley Hunter", "45");
       private final Person person4 = new Person("04", "Roscoe Guerin", "56");
       private final Person person5 = new Person("05", "Gracie Chavez", "67");
   
       // Define relationship classes to define the relationships between some of the persons.
       private final Relation relation1 = new Relation("0102", "siblings", person1, person2);
       private final Relation relation2 = new Relation("0203", "coworkers", person2, person3);
       private final Relation relation3 = new Relation("0301", "parent", person3, person1);
       private final Relation relation4 = new Relation("0302", "parent", person3, person2);
       private final Relation relation5 = new Relation("0501", "grandparent", person5, person1);
       private final Relation relation6 = new Relation("0502", "grandparent", person5, person2);
   
       // Define the network.
       private final Network network = new Network();
   
       // Autowire the repositories and factory.
       @Autowired
       private PersonRepository personRepo;
       @Autowired
       private RelationRepository relationRepo;
       @Autowired
       private NetworkRepository networkRepo;
       @Autowired
       private GremlinFactory factory;
   
       // Run the Spring application and exit.
    public static void main(String[] args) {
           SpringApplication.run(WingtiptoysdataApplication.class, args);
           System.exit(0);
    }
   
       // Perform post-construct operations.    
       @PostConstruct
       public void setup() {
           // Delete any existing data from the database.
           this.networkRepo.deleteAll();
   
           // Add the relationship classes as edges.
           this.network.getEdges().add(this.relation1);
           this.network.getEdges().add(this.relation2);
           this.network.getEdges().add(this.relation3);
           this.network.getEdges().add(this.relation4);
           this.network.getEdges().add(this.relation5);
           this.network.getEdges().add(this.relation6);
   
           // Add the person classes as vertices.
           this.network.getVertexes().add(this.person1);
           this.network.getVertexes().add(this.person2);
           this.network.getVertexes().add(this.person3);
           this.network.getVertexes().add(this.person4);
           this.network.getVertexes().add(this.person5);
   
           // Save the network.
           this.networkRepo.save(this.network);
       }
   }
   ```

1. 保存并关闭主应用程序 Java 文件。

### <a name="define-a-basic-class-for-storing-configuration-information"></a>定义用于存储配置信息的基本类

1. 在应用的 package 目录下创建名为 *config* 的文件夹；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\config`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/config`

1. 在 *config* 目录中创建名为 *UserRepositoryConfiguration.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.config;

   import com.microsoft.spring.data.gremlin.common.GremlinConfiguration;
   import com.microsoft.spring.data.gremlin.common.GremlinFactory;
   import com.microsoft.spring.data.gremlin.config.AbstractGremlinConfiguration;
   import com.microsoft.spring.data.gremlin.repository.config.EnableGremlinRepositories;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   
   @Configuration
   @EnableGremlinRepositories(basePackages = "com.example.wingtiptoysdata.repository")
   @EnableConfigurationProperties(GremlinConfiguration.class)
   @PropertySource("classpath:application.yml")
   public class UserRepositoryConfiguration extends AbstractGremlinConfiguration {
   
       @Autowired
       private GremlinConfiguration config;
   
       @Override
       public GremlinConfiguration getGremlinConfiguration() {
           return this.config;
       }
   }
   ```

1. 保存并关闭 *UserRepositoryConfiguration.java* 文件。

### <a name="define-a-set-of-classes-that-define-the-elements-of-your-graph-database"></a>定义一组类用于定义图形数据库的元素

1. 在应用的 package 目录下创建名为 *domain* 的文件夹；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\domain`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/domain`

1. 在 *domain* 目录中创建名为 *Person.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Vertex;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Vertex
   @AllArgsConstructor
   @NoArgsConstructor
   public class Person {
   
       @Id
       private String id;
   
       private String name;
   
       private String age;
   }
   ```

1. 保存并关闭 *Person.java* 文件。

1. 在 *domain* 目录中创建名为 *Relation.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.Edge;
   import com.microsoft.spring.data.gremlin.annotation.EdgeFrom;
   import com.microsoft.spring.data.gremlin.annotation.EdgeTo;
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import org.springframework.data.annotation.Id;
   
   @Data
   @Edge
   @AllArgsConstructor
   @NoArgsConstructor
   public class Relation {
   
       @Id
       private String id;
   
       private String name;
   
       @EdgeFrom
       private Person personFrom;
   
       @EdgeTo
       private Person personTo;
   }
   ```

1. 保存并关闭 *Relation.java* 文件。

1. 在 *domain* 目录中创建名为 *Network.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.domain;
   
   import com.microsoft.spring.data.gremlin.annotation.EdgeSet;
   import com.microsoft.spring.data.gremlin.annotation.Graph;
   import com.microsoft.spring.data.gremlin.annotation.VertexSet;
   import lombok.Getter;
   import org.springframework.data.annotation.Id;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Graph
   public class Network {
   
       @Id
       private String id;
   
       public Network() {
           this.edges = new ArrayList<Object>();
           this.vertexes = new ArrayList<Object>();
       }
   
       @EdgeSet
       @Getter
       private List<Object> edges;
   
       @VertexSet
       @Getter
       private List<Object> vertexes;
   }
   ```

1. 保存并关闭 *Network.java* 文件。

### <a name="define-a-set-of-classes-that-define-the-repositories-for-your-graph-database"></a>定义一组类用于定义图形数据库的存储库

1. 在应用的 package 目录下创建名为 *repository* 的文件夹；例如：

   `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\repository`

   -或-

   `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/repository`

1. 在 *repository* 目录中创建名为 *NetworkRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Network;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface NetworkRepository extends GremlinRepository<Network, String> {
   }
   ```

1. 保存并关闭 *NetworkRepository.java* 文件。

1. 在 *repository* 目录中创建名为 *PersonRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Person;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface PersonRepository extends GremlinRepository<Person, String> {
   }
   ```

1. 保存并关闭 *PersonRepository.java* 文件。

1. 在 *repository* 目录中创建名为 *RelationRepository.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.example.wingtiptoysdata.repository;
   
   import com.microsoft.spring.data.gremlin.repository.GremlinRepository;
   import com.example.wingtiptoysdata.domain.Relation;
   import org.springframework.stereotype.Repository;
   
   @Repository
   public interface RelationRepository extends GremlinRepository<Relation, String> {
   }
   ```

1. 保存并关闭 *RelationRepository.java* 文件。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   `cd C:\SpringBoot\wingtiptoysdata`

   -或-

   `cd /users/example/home/wingtiptoysdata`

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 应用程序将显示多个运行时消息，如果未出错，则你可以使用 Azure 门户来查看 Azure Cosmos DB 的内容。 为此，请在数据库的属性页上单击“数据资源管理器”，单击“执行 Gremlin 查询”，然后从结果列表中选择一个项以查看数据。

   ![使用文档资源管理器查看数据][JV03]

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关 Gremlin 和图形 API 的 Azure 支持的详细信息，请参阅以下文章：

* [Azure Cosmos DB 简介：图形 API](/azure/cosmos-db/graph-introduction)

* [Azure Cosmos DB Gremlin 图形支持](/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建图形数据库](/azure/cosmos-db/create-graph-java)

* [教程：使用 Gremlin 查询 Azure Cosmos DB 图形 API](/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：

* [Azure Cosmos DB 文档]。

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ05.png
[AZ06]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ06.png
[AZ07]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ07.png
[AZ08]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/AZ08.png

[SI01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI01.png
[SI02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI02.png
[SI03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/SI03.png

[RE01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE01.png
[RE02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/RE02.png

[PM01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM01.png
[PM02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/PM02.png

[JV01]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV01.png
[JV02]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV02.png
[JV03]: ./media/configure-spring-data-gremlin-java-app-with-cosmos-db/JV03.png
