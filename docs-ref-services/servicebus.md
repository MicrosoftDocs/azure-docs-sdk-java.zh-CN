---
title: "用于 Java 的服务总线库"
description: "用于服务总线的 Java 客户端和管理库的参考文档"
keywords: "Azure, Java, SDK, API, 消息传送, amqp, qpid, JMS, pubsub, 发布-订阅, 消息中转站"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: service-bus
ms.openlocfilehash: 12b5f218008c208bfa85ef02820c56bbcf0b224a
ms.sourcegitcommit: ae39830d5a54fedceac78d8df1718e77741e03fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2017
---
# <a name="service-bus-libraries-for-java"></a>用于 Java 的服务总线库

## <a name="overview"></a>概述

服务总线是一个企业级的事务消息传送平台服务，提供高度可靠的队列和发布/订阅主题，并具有依序传送、会话、分区、计划、复杂订阅以及工作流和事务处理等高级功能。

服务总线的功能与传统的高端本地消息中转站相当，并且往往优于后者。 可通过 AMQP 1.0、HTTPS 等基于标准的协议来使用服务总线功能，所有协议手势均有完备的阐述，从而可以实现广泛的互操作性。 

服务总线高级版注重高度可用和可靠的持久消息传送，即使在大规模的本地数据中心部署中也能提供有竞争力的吞吐量性能，同时可以消除硬件选用和采购流程、部署规划和执行，以及漫长的性能优化会议。 

服务总线高级版是完全托管型的产品，它采用以容量为导向的简单定价模型为每个租户保留专用容量实现可预测的性能，同时与商用本地中转站相比，总体成本极低。 对于许多客户而言，服务总线高级版可以取代眼下的专用本地消息传送群集，即使附加的工作负荷不在云中运行。 

[在消息传送文档部分中](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)详细了解服务总线的概念 

对于 Java 开发人员而言，服务总线提供 Microsoft 支持的本机 API，服务总线还能与符合 AMQP 1.0 规范的库（例如 Apache Qpid Proton 的 JMS 提供程序）配合使用。

[GitHub 上的源代码表单](https://github.com/azure/azure-service-bus-java)中提供了正式的服务总线客户端，[Maven Central 上提供了](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22)二进制文件和打包的源代码。 


## <a name="client-library"></a>客户端库


向 Maven 项目的 `pom.xml` 文件中添加依赖项，以便在自己的项目中使用库。 根据需要指定版本。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="examples"></a>示例

[示例代码存储库](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)包含有关如何处理来自服务总线的 [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)、[TopicClient 和 SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) 以及 [MessageSender 和MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) 消息的示例。


```java
public class BasicSendReceiveWithQueueClient {
    // Connection String for the namespace can be obtained from the Azure portal under the
    // 'Shared Access policies' section.
    private static final String connectionString = "{connection string}";
    private static final String queueName = "{queue name}";
    private static IQueueClient queueClient;
    private static int totalSend = 100;
    private static int totalReceived = 0;

    public static void main(String[] args) throws Exception {

        Log.log("Starting BasicSendReceiveWithQueueClient sample");

        // create client
        Log.log("Create queue client.");
        queueClient = new QueueClient(new ConnectionStringBuilder(connectionString, queueName), ReceiveMode.PeekLock);

        // send and receive
        queueClient.registerMessageHandler(new MessageHandler(queueClient), new MessageHandlerOptions(1, false, Duration.ofMinutes(1)));
        for (int i = 0; i < totalSend; i++) {
            int j = i;
            Log.log("Sending message #%d.", j);
            queueClient.sendAsync(new Message("" + i)).thenRunAsync(() -> { Log.log("Sent message #%d.", j);});
        }

        while(totalReceived != totalSend) {
            Thread.sleep(1000);
        }

        Log.log("Received all messages, exiting the sample.");
        Log.log("Closing queue client.");
        queueClient.close();
    }

    static class MessageHandler implements IMessageHandler {
        private IQueueClient client;

        public MessageHandler(IQueueClient client) {
            this.client = client;
        }

        @Override
        public CompletableFuture<Void> onMessageAsync(IMessage iMessage) {
            Log.log("Received message with sq#: %d and lock token: %s.", iMessage.getSequenceNumber(), iMessage.getLockToken());
            return this.client.completeAsync(iMessage.getLockToken()).thenRunAsync(() -> {
                Log.log("Completed message sq#: %d and locktoken: %s", iMessage.getSequenceNumber(), iMessage.getLockToken());
                totalReceived++;
            });
        }

        @Override
        public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
            Log.log(exceptionPhase + "-" + throwable.getMessage());
        }
    }
}
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/servicebus/clientlibrary)

## <a name="management-api"></a>管理 API

使用管理 API 创建和管理命名空间、主题、队列与订阅。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.2.1</version>
</dependency>
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/servicebus/managementapi)


## <a name="examples"></a>示例

[管理服务总线队列](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
[创建和订阅服务总线主题](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)

详细了解可在应用中使用的 [Azure 服务总线示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=bus)。