---
title: 如何将 Spring Data JDBC 用于 Azure MySQL
description: 了解如何将 Spring Data JDBC 用于 Azure MySQL 数据库。
services: mysql
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 5ec89194c72cef81c79d21382e41b33ebe91d3d0
ms.sourcegitcommit: f0f140b0862ca5338b1b7e5c33cec3e58a70b8fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992020"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-mysql"></a>如何将 Spring Data JDBC 用于 Azure MySQL

## <a name="overview"></a>概述

本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Java 数据库连接 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 在 Azure [MySQL](https://www.mysql.com/) 数据库中存储和检索信息。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* 用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。
* [mysql](https://dev.mysql.com/downloads/) 命令行实用工具。
* [Git](https://git-scm.com/downloads) 客户端。

## <a name="create-a-mysql-database-for-azure"></a>创建 MySQL database for Azure

### <a name="create-a-mysql-database-server-using-the-azure-portal"></a>使用 Azure 门户创建 MySQL 数据库服务器

> [!NOTE]
> 
> 可以在[在 Azure 门户中创建 Azure Database for MySQL 服务器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)中阅读有关创建 MySQL 数据库的更多详细信息。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次单击“+创建资源”、“数据库”和“Azure Database for MySQL”。

   ![创建 MySQL 数据库][MYSQL01]

1. 输入以下信息：

   - **服务器名称**：为 MySQL 服务器选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoysmysql.mysql.database.azure.com*。
   - **订阅**：指定要使用的 Azure 订阅。
   - **资源组**：指定是要创建新资源组，还是选择现有资源组。
   - **选择源**：对于本教程，请选择 `Blank` 以创建新数据库。
   - **服务器管理员登录名**：指定数据库管理员名称。
   - **密码**和**确认密码**：指定数据库管理员的密码。
   - **位置**：指定最靠近你的数据库的地理区域。
   - **版本**：指定最新的数据库版本。
   - **定价层**：对于本教程，请指定最经济的定价层。

   ![创建 MySQL 数据库属性][MYSQL02]

1. 输入上述所有信息后，单击“创建”。

### <a name="configure-a-firewall-rule-for-your-mysql-database-server-using-the-azure-portal"></a>使用 Azure 门户为 MySQL 数据库服务器配置防火墙规则

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 单击“所有资源”，然后单击你刚才创建的 MySQL 数据库。

   ![选择 MySQL 数据库][MYSQL03]

1. 单击“连接安全性”，在“防火墙规则”中通过为规则指定一个唯一名称来创建新规则，输入将需要访问你的数据库的 IP 地址范围，然后单击“保存”。

   ![配置连接安全性][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-mysql-server-using-the-azure-portal"></a>使用 Azure 门户检索 MySQL 服务器的连接字符串

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 单击“所有资源”，然后单击你刚才创建的 MySQL 数据库。

   ![选择 MySQL 数据库][MYSQL03]

1. 单击“连接字符串”，然后复制“JDBC”文本字段中的值。

   ![检索 JDBC 连接字符串][MYSQL05]

### <a name="create-mysql-database-using-the-mysql-command-line-utility"></a>使用 `mysql` 命令行实用工具创建 MySQL 数据库

1. 打开一个命令 shell，通过输入 `mysql` 命令连接到 MySQL 服务器，如以下示例所示：

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `host` | 指定本文上文中所述的完全限定的 MySQL 服务器名称。 |
   | `user` | 指定本文上文中所述的 MySQL 管理员和缩短的服务器名称。 |
   | `p` | 指定等待至提示输入密码。 |

   MySQL 服务器应当如以下示例所示进行响应：

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

1. 通过输入 `mysql` 命令创建名为 *mysqldb* 的数据库，如以下示例所示：

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   MySQL 服务器应当如以下示例所示进行响应：

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. 可选：可以通过输入 `mysql` 命令验证数据库是否已创建，如以下示例所示：

   ```SQL
   SHOW DATABASES;
   ```

   MySQL 服务器应当如以下示例所示进行响应：

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. 输入 `\q` 以退出 `mysql` 实用工具。

## <a name="configure-the-sample-application"></a>配置示例应用程序

1. 打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。

1. 在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：

   ```yaml
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false&serverTimezone=UTC
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   其中：

   | 参数 | 说明 |
   |---|---|
   | `spring.datasource.url` | 指定本文上文中所述的 MySQL JDBC 字符串并添加时区。 |
   | `spring.datasource.username` | 指定本文上文中所述的 MySQL 管理员名称，并将缩短的服务器名称追加到其末尾。 |
   | `spring.datasource.password` | 指定本文上文中所述的 MySQL 管理员密码。 |

1. 保存并关闭 application.properties 文件。

## <a name="package-and-test-the-sample-application"></a>打包并测试示例应用程序 

1. 使用 Maven 生成示例应用程序，例如：

   ```shell
   mvn clean package -P mysql
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

## <a name="summary"></a>摘要

在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 JDBC 在 Azure MySQL 数据库中存储和检索信息。

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

[MYSQL01]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png
[MYSQL03]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-03.png
[MYSQL04]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-05.png
