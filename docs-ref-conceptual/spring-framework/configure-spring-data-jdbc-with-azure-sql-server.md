---
title: 如何将 Spring Data JDBC 用于 Azure SQL 数据库
description: 了解如何将 Spring Data JDBC 用于 Azure SQL 数据库。
services: sql-database
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 92f489b513eeb05efaacdf07af2b976de8f84868
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992018"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-sql-database"></a>如何将 Spring Data JDBC 用于 Azure SQL 数据库

## <a name="overview"></a>概述

本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Java 数据库连接 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 在 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)中存储和检索信息。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* 用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。
* [Git](https://git-scm.com/downloads) 客户端。

## <a name="create-an-azure-sql-satabase"></a>创建 Azure SQL 数据库

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>使用 Azure 门户创建 SQL 数据库服务器

> [!NOTE]
> 
> 可以在[在 Azure 门户中创建 Azure SQL 数据库](/azure/sql-database/sql-database-get-started-portal)中阅读有关创建 Azure SQL 数据库的更多详细信息。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次单击“+创建资源”、“数据库”和“SQL 数据库”。

   ![创建 SQL 数据库][SQL01]

1. 指定以下信息：

   - **数据库名称**：为 SQL 数据库选择一个唯一名称；这将在你稍后指定的 SQL 服务器中创建。
   - **订阅**：指定要使用的 Azure 订阅。
   - **资源组**：指定是要创建新资源组，还是选择现有资源组。
   - **选择源**：对于本教程，请选择 `Blank database` 以创建新数据库。

   ![指定 SQL 数据库属性][SQL02]
   
1. 依次单击“服务器”、“创建新服务器”，然后指定以下信息：

   - **服务器名称**：为 SQL 服务器选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoyssql.database.windows.net*。
   - **服务器管理员登录名**：指定数据库管理员名称。
   - **密码**和**确认密码**：指定数据库管理员的密码。
   - **位置**：指定最靠近你的数据库的地理区域。

   ![指定 SQL Server][SQL03]

1. 输入上述所有信息后，单击“选择”。

1. 对于本教程，请指定价格最低的**定价层**，然后单击“创建”。

   ![创建 SQL 数据库][SQL04]

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>使用 Azure 门户为 SQL Server 配置防火墙规则

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 单击“所有资源”，然后单击你刚才创建的 SQL Server。

   ![选择 SQL Server][SQL05]

1. 在“概述”部分中，单击“显示防火墙设置”

   ![显示防火墙设置][SQL06]

1. 在“防火墙和虚拟网络”部分中，通过为规则指定一个唯一名称来创建新规则，输入将需要访问你的数据库的 IP 地址范围，然后单击“保存”。

   ![配置防火墙设置][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>使用 Azure 门户检索 SQL Server 的连接字符串

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 单击“所有资源”，然后单击你刚才创建的 SQL 数据库。

   ![选择 SQL 数据库][SQL08]

1. 单击“连接字符串”，然后单击“JDBC”并复制 JDBC 文本字段中的值。

   ![检索 JDBC 连接字符串][SQL09]

## <a name="configure-the-sample-application"></a>配置示例应用程序

1. 打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。

1. 在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `spring.datasource.url` | 指定上文中所述的 SQL JDBC 字符串的编辑后版本。 |
   | `spring.datasource.username` | 指定本文上文中所述的 SQL 管理员名称，并将缩短的服务器名称追加到其末尾。 |
   | `spring.datasource.password` | 指定本文上文中所述的 SQL 管理员密码。 |

1. 保存并关闭 application.properties 文件。

## <a name="package-and-test-the-sample-application"></a>打包并测试示例应用程序 

1. 使用 Maven 生成示例应用程序，例如：

   ```shell
   mvn clean package -P sql
   ```

1. 启动示例应用程序；例如：

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 在命令提示符下使用 `curl` 创建新记录，如以下示例所示：

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   你的应用程序应返回如下所示的值：

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   你的应用程序应返回如下所示的值：

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>总结

在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 JDBC 在 Azure SQL 数据库中存储和检索信息。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SQL01]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-09.png
