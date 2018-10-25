---
title: 如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用
description: 了解如何配置使用 Spring Boot Initializer 创建的应用程序，以便将 Apache Kafka 与 Azure 事件中心配合使用。
services: event-hubs
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 09/10/2018
ms.devlang: java
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: na
ms.openlocfilehash: 85fe1d9c56530b716a1f1750713f4c87d43dfad3
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799953"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a>如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用

## <a name="overview"></a>概述

本文介绍如何配置基于 Java 的 Spring Cloud Stream Binder，它是使用 Spring Boot Initializer 创建的，目的是将 [Apache Kafka] 与 Azure 事件中心配合使用。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/) 1.7 版或更高版本。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

> [!IMPORTANT]
>
> 完成本文中的步骤需要 Spring Boot 2.0 或更高版本。
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>使用 Azure 门户创建 Azure 事件中心

### <a name="create-an-azure-event-hub-namespace"></a>创建 Azure 事件中心命名空间

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次单击“+创建资源”、“物联网”、“事件中心”。

   ![创建 Azure 事件中心命名空间][IMG01]

1. 在“创建命名空间”页上，输入以下信息：

   * 输入一个唯一**名称**，该名称将成为事件中心命名空间 URI 的一部分。 例如，如果输入 **wingtiptoys** 作为**名称**，则 URI 将为 *wingtiptoys.servicebus.windows.net*。
   * 为事件中心命名空间选择一个“定价层”。
   * 为命名空间指定“启用 Kafka”设置。
   * 选择需要用于命名空间的“订阅”。
   * 指定是为命名空间创建新的“资源组”，还是选择现有资源组。
   * 指定事件中心命名空间的“位置”。

   ![指定 Azure 事件中心命名空间选项][IMG02]

1. 指定上面列出的选项后，请单击“创建”以创建命名空间。

### <a name="create-an-azure-event-hub-in-your-namespace"></a>在命名空间中创建 Azure 事件中心

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户。

1. 单击“所有资源”，然后单击已创建的命名空间名称。

   ![选择 Azure 事件中心命名空间][IMG03]

1. 单击“事件中心”，然后单击“+事件中心”。

   ![添加新的 Azure 事件中心][IMG04]

1. 在“创建事件中心”页上，为事件中心输入唯一的**名称**，然后单击“创建”。

   ![创建 Azure 事件中心][IMG05]

1. 事件中心在创建后会列在“事件中心”页上。

   ![创建 Azure 事件中心][IMG06]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定以下选项：

   * 使用 **Java** 生成一个 **Maven** 项目。
   * 指定一个其值大于或等于 2.0 的 **Spring Boot** 版本。
   * 指定应用程序的“组”和“项目”名称。
   * 添加 **Web** 依赖项。

      ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.wingtiptoys.kafka。
   >

1. 指定上面列出的选项后，请单击“生成项目”。

1. 出现提示时，将项目下载到本地计算机中的路径。

   ![下载 Spring 项目][SI02]

1. 在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a>配置 Spring Boot 应用，以便使用 Spring Cloud Kafka Stream和 Azure Event Hub Starter

1. 在应用的根目录中找到 pom.xml 文件，例如：

   `C:\SpringBoot\kafka\pom.xml`

   -或-

   `/users/example/home/kafka/pom.xml`

1. 在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Kafka Stream Starter 和 Azure Event Hub Starter 添加到 `<dependencies>` 列表：

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![编辑 pom.xml 文件][SI03]

1. 保存并关闭 pom.xml 文件。

## <a name="create-an-azure-credential-file"></a>创建 Azure 凭据文件

1. 打开命令提示符。

1. 导航到 Spring Boot 应用的 *resources* 目录，例如：

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   -或-

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. 请登录到 Azure 帐户：

   ```azurecli
   az login
   ```

1. 列出订阅：

   ```azurecli
   az account list
   ```
   Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]

1. Specify the GUID for the subscription you want to use with Azure; for example:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. 创建 Azure 凭据文件：

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>配置 Spring Boot 应用以使用 Azure 事件中心

1. 在应用的 *resources* 目录中找到 *application.properties*，例如：

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   -或-

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. 在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为事件中心的相应属性：

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   其中：

   |                       字段                       |                                                                                   Description                                                                                    |
   |---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `spring.cloud.azure.credential-file-path`     |                                                    指定之前在本教程中创建的 Azure 凭据文件。                                                    |
   |        `spring.cloud.azure.resource-group`        |                                                      指定包含 Azure 事件中心的 Azure 资源组。                                                      |
   |            `spring.cloud.azure.region`            |                                           指定你在创建 Azure 事件中心时指定的地理区域。                                            |
   |      `spring.cloud.azure.eventhub.namespace`      |                                          指定你在创建 Azure 事件中心命名空间时指定的唯一名称。                                           |
   | `spring.cloud.stream.bindings.input.destination`  |                            指定输入目标 Azure 事件中心。在本教程中，它是你此前在本教程中创建的中心。                            |
   |    `spring.cloud.stream.bindings.input.group `    | 在 Azure 事件中心指定一个使用者组，该组可以设置为“$Default”，以便使用你在创建 Azure 事件中心时创建的基本使用者组。 |
   | `spring.cloud.stream.bindings.output.destination` |                               指定输出目标 Azure 事件中心。在本教程中，它与输入目标相同。                               |


3. 保存并关闭 application.properties 文件。

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>添加示例代码以实现事件中心的基本功能

在本部分，请创建所需的 Java 类，以便将事件发送到事件中心。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   -或-

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. 保存并关闭主应用程序 Java 文件。


### <a name="create-a-new-class-for-the-source-connector"></a>为源连接器创建新类

1. 在应用的包目录中创建名为 *KafkaSource.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. 保存并关闭 *KafkaSource.java* 文件。

### <a name="create-a-new-class-for-the-sink-connector"></a>为接收器连接器创建新类

1. 在应用的包目录中创建名为 *KafkaSink.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.wingtiptoys.kafka;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;

   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. 保存并关闭 *KafkaSink.java* 文件。

## <a name="build-and-test-your-application"></a>生成和测试应用程序

1. 打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   `cd C:\SpringBoot\kafka`

   -或-

   `cd /users/example/home/kafka`

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 应用程序运行以后，即可使用 *curl* 对其进行测试，例如：

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   此时会看到“hello”发布到应用程序的日志中。 例如：

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> 若要进行测试，可以修改 *KafkaSource.java*，使之包含简单的 HTML 窗体，如以下示例所示：
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> 这样就可以使用 Web 浏览器来测试应用程序：
> 
> ![使用 Web 浏览器测试应用程序][TB01]
> 
> 提交窗体后，应用程序会显示结果：
> 
> ![Web 浏览器中的应用程序响应][TB02]
> 

## <a name="next-steps"></a>后续步骤

有关 Event Hub Stream Binder 和 Apache Kafka 的 Azure 支持的详细信息，请参阅以下文章：

* [什么是 Azure 事件中心？](/azure/event-hubs/event-hubs-about)

* [适用于 Apache Kafka 的 Azure 事件中心](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [使用 Azure 门户创建事件中心命名空间和事件中心](/azure/event-hubs/event-hubs-create)

* [创建启用了 Apache Kafka 的事件中心](/azure/event-hubs/event-hubs-create-kafka-enabled)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
