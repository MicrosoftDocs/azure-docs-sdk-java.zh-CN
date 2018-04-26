---
title: 用于 Java 的服务总线库
description: 用于服务总线的 Java 客户端和管理库的参考文档
keywords: Azure, Java, SDK, API, 消息传送, amqp, qpid, JMS, pubsub, 发布-订阅, 消息中转站
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: service-bus
ms.openlocfilehash: 7468d9b920debc778e7e3d298fbcb913add6afdd
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="service-bus-libraries-for-java"></a><span data-ttu-id="00ac9-104">用于 Java 的服务总线库</span><span class="sxs-lookup"><span data-stu-id="00ac9-104">Service Bus libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="00ac9-105">概述</span><span class="sxs-lookup"><span data-stu-id="00ac9-105">Overview</span></span>

<span data-ttu-id="00ac9-106">服务总线是一个企业级的事务消息传送平台服务，提供高度可靠的队列和发布/订阅主题，并具有依序传送、会话、分区、计划、复杂订阅以及工作流和事务处理等高级功能。</span><span class="sxs-lookup"><span data-stu-id="00ac9-106">Service Bus is an enterprise-class, transactional messaging platform service that provides highly reliable queues and publish/subscribe topics with deep feature capabilities such as ordered delivery, sessions, partitioning, scheduling, complex subscriptions, as well as workflow and transaction handling.</span></span>

<span data-ttu-id="00ac9-107">服务总线的功能与传统的高端本地消息中转站相当，并且往往优于后者。</span><span class="sxs-lookup"><span data-stu-id="00ac9-107">The Service Bus feature capabilities are comparable and often exceed those of high-end, on-premises legacy message brokers.</span></span> <span data-ttu-id="00ac9-108">可通过 AMQP 1.0、HTTPS 等基于标准的协议来使用服务总线功能，所有协议手势均有完备的阐述，从而可以实现广泛的互操作性。</span><span class="sxs-lookup"><span data-stu-id="00ac9-108">The Service Bus features are available via standards-based protocols like AMQP 1.0 and HTTPS and all protocol gestures are fully documented, allowing for broad interoperability.</span></span> 

<span data-ttu-id="00ac9-109">服务总线高级版注重高度可用和可靠的持久消息传送，即使在大规模的本地数据中心部署中也能提供有竞争力的吞吐量性能，同时可以消除硬件选用和采购流程、部署规划和执行，以及漫长的性能优化会议。</span><span class="sxs-lookup"><span data-stu-id="00ac9-109">Focusing on highly available and reliable durable messaging, the Service Bus Premium provides competitive throughput performance even with substantial local datacenter deployments, but without hardware selection and acquisition processes, deployment planning and execution, and endless performance optimization sessions.</span></span> 

<span data-ttu-id="00ac9-110">服务总线高级版是完全托管型的产品，它采用以容量为导向的简单定价模型为每个租户保留专用容量实现可预测的性能，同时与商用本地中转站相比，总体成本极低。</span><span class="sxs-lookup"><span data-stu-id="00ac9-110">Service Bus Premium is a fully managed offering with dedicated capacity reserved for each tenant that yields predictable performance with a simple, capacity-oriented pricing model and at extremely lower overall cost than commercial on-premises brokers.</span></span> <span data-ttu-id="00ac9-111">对于许多客户而言，服务总线高级版可以取代眼下的专用本地消息传送群集，即使附加的工作负荷不在云中运行。</span><span class="sxs-lookup"><span data-stu-id="00ac9-111">For many customers, Service Bus Premium can replace dedicated on-premises messaging clusters today, even if the attached workloads do not run in the cloud.</span></span> 

<span data-ttu-id="00ac9-112">[在消息传送文档部分中](https://docs.microsoft.com/azure/service-bus-messaging/)详细了解服务总线的概念</span><span class="sxs-lookup"><span data-stu-id="00ac9-112">Learn more about Service Bus concepts [in the messaging documentation section](https://docs.microsoft.com/azure/service-bus-messaging/)</span></span> 

<span data-ttu-id="00ac9-113">对于 Java 开发人员而言，服务总线提供 Microsoft 支持的本机 API，服务总线还能与符合 AMQP 1.0 规范的库（例如 Apache Qpid Proton 的 JMS 提供程序）配合使用。</span><span class="sxs-lookup"><span data-stu-id="00ac9-113">For Java developers, Service Bus provides a Microsoft supported native API and Service Bus can also be used with AMQP 1.0 compliant libraries such as Apache Qpid Proton's JMS provider.</span></span>

<span data-ttu-id="00ac9-114">[GitHub 上的源代码表单](https://github.com/azure/azure-service-bus-java)中提供了正式的服务总线客户端，[Maven Central 上提供了](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22)二进制文件和打包的源代码。</span><span class="sxs-lookup"><span data-stu-id="00ac9-114">The official Service Bus client is available in [source code form on GitHub](https://github.com/azure/azure-service-bus-java) and binaries and packaged sources [are available on Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-servicebus%22).</span></span> 


## <a name="client-library"></a><span data-ttu-id="00ac9-115">客户端库</span><span class="sxs-lookup"><span data-stu-id="00ac9-115">Client library</span></span>


<span data-ttu-id="00ac9-116">向 Maven 项目的 `pom.xml` 文件中添加依赖项，以便在自己的项目中使用库。</span><span class="sxs-lookup"><span data-stu-id="00ac9-116">Add a dependency to your Maven project's `pom.xml` file to use the library in your own project.</span></span> <span data-ttu-id="00ac9-117">根据需要指定版本。</span><span class="sxs-lookup"><span data-stu-id="00ac9-117">Specify the version as desired.</span></span>

<span data-ttu-id="00ac9-118">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="00ac9-118">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>   

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-servicebus</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="examples"></a><span data-ttu-id="00ac9-119">示例</span><span class="sxs-lookup"><span data-stu-id="00ac9-119">Examples</span></span>

<span data-ttu-id="00ac9-120">[示例代码存储库](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/)包含有关如何处理来自服务总线的 [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java)、[TopicClient 和 SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) 以及 [MessageSender 和MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) 消息的示例。</span><span class="sxs-lookup"><span data-stu-id="00ac9-120">The [sample code repository](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/) contains samples for how to [QueueClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithQueueClient.java) and [TopicClient and SubscriptionClient](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/BasicSendReceiveWithTopicSubscriptionClient.java) and [MessageSender and MessageReceiver](https://github.com/Azure/azure-service-bus/blob/master/samples/Java/src/com/microsoft/azure/servicebus/samples/SendReceiveWithMessageSenderReceiver.java) messages from Service Bus.</span></span>


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
> [<span data-ttu-id="00ac9-121">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="00ac9-121">Explore the Client APIs</span></span>](/java/api/overview/azure/servicebus/client)

## <a name="management-api"></a><span data-ttu-id="00ac9-122">管理 API</span><span class="sxs-lookup"><span data-stu-id="00ac9-122">Management API</span></span>

<span data-ttu-id="00ac9-123">使用管理 API 创建和管理命名空间、主题、队列与订阅。</span><span class="sxs-lookup"><span data-stu-id="00ac9-123">Create and manage namespaces, topics, queues, and subscriptions with the management API.</span></span>

<span data-ttu-id="00ac9-124">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="00ac9-124">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-servicebus</artifactId>
    <version>1.3.0</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="00ac9-125">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="00ac9-125">Explore the Management APIs</span></span>](/java/api/overview/azure/servicebus/management)


## <a name="examples"></a><span data-ttu-id="00ac9-126">示例</span><span class="sxs-lookup"><span data-stu-id="00ac9-126">Examples</span></span>

<span data-ttu-id="00ac9-127">[管理服务总线队列](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
[创建和订阅服务总线主题](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)</span><span class="sxs-lookup"><span data-stu-id="00ac9-127">[Manage Service Bus queues](https://github.com/Azure-Samples/service-bus-java-manage-queue-with-basic-features)
[Create and subscribe to Service Bus topics](https://github.com/Azure-Samples/service-bus-java-manage-publish-subscribe-with-basic-features)</span></span>

<span data-ttu-id="00ac9-128">详细了解可在应用中使用的 [Azure 服务总线示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=bus)。</span><span class="sxs-lookup"><span data-stu-id="00ac9-128">Explore more [sample Java code for Azure Service Bus](https://azure.microsoft.com/resources/samples/?platform=java&term=bus) you can use in your apps.</span></span>
