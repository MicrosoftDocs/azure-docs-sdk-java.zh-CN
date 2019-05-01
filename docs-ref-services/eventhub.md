---
title: 用于 Java 的 Azure 事件中心库
description: Java 事件中心库的参考文档
keywords: Azure, Java, SDK, API, 事件中心, IoT, 流处理
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: event-hub
ms.openlocfilehash: 9e2ae18f98083cb35bb076a5f84726b669cbf81e
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593352"
---
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="b2970-104">用于 Java 的 Azure 事件中心库</span><span class="sxs-lookup"><span data-stu-id="b2970-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="b2970-105">概述</span><span class="sxs-lookup"><span data-stu-id="b2970-105">Overview</span></span>

<span data-ttu-id="b2970-106">使用 [Azure 事件中心](/azure/event-hubs/event-hubs-what-is-event-hubs)从已连接的 IoT 设备和应用程序每秒收集数百万个事件并对其进行管理。</span><span class="sxs-lookup"><span data-stu-id="b2970-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="b2970-107">若要开始使用 Azure 事件中心，请参阅[使用 Java 从 Azure 事件中心接收事件](/azure/event-hubs/event-hubs-java-get-started-receive-eph)。</span><span class="sxs-lookup"><span data-stu-id="b2970-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="b2970-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="b2970-108">Client library</span></span>

<span data-ttu-id="b2970-109">使用事件中心客户端库将事件发送到 Azure 事件中心，并从事件中心使用和处理事件。</span><span class="sxs-lookup"><span data-stu-id="b2970-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="b2970-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用[客户端库](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs)。</span><span class="sxs-lookup"><span data-stu-id="b2970-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the [client library](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventhubs) in your project.</span></span>
 

## <a name="example"></a><span data-ttu-id="b2970-111">示例</span><span class="sxs-lookup"><span data-stu-id="b2970-111">Example</span></span>

<span data-ttu-id="b2970-112">将事件发送到事件中心。</span><span class="sxs-lookup"><span data-stu-id="b2970-112">Send an event to an event hub.</span></span>

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                                            .setNamespaceName(namespaceName)
                                            .setEventHubName(eventHubName)
                                            .setSasKeyName(sasKeyName)
                                            .setSasKey(sasKey);
final EventHubClient ehClient = EventHubClient.createSync(connStr.toString());

final byte[] payloadBytes = "Test AMQP message".getBytes("UTF-8");
final EventData sendEvent = new EventData(payloadBytes);

ehClient.sendSync(sendEvent);
```


> [!div class="nextstepaction"]
> [<span data-ttu-id="b2970-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="b2970-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhubs/client)



## <a name="samples"></a><span data-ttu-id="b2970-114">示例</span><span class="sxs-lookup"><span data-stu-id="b2970-114">Samples</span></span>

<span data-ttu-id="b2970-115">[使用示例探究事件中心数据平面 API][1]</span><span class="sxs-lookup"><span data-stu-id="b2970-115">[Explore Event Hub data-plane API using samples][1]</span></span>

<span data-ttu-id="b2970-116">[通过 JMS 写入事件中心以及从 Apache Storm 读取][2]</span><span class="sxs-lookup"><span data-stu-id="b2970-116">[Write to Event Hub via JMS and read from Apache Storm][2]</span></span>

<span data-ttu-id="b2970-117">[使用混合 .NET/Java 拓扑从事件中心读取和写入][3]</span><span class="sxs-lookup"><span data-stu-id="b2970-117">[Read and write from EventHubs using a hybrid .NET/Java topology][3]</span></span> 

[1]: https://github.com/Azure/azure-event-hubs/tree/master/samples/Java
[2]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[3]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="b2970-118">详细了解可在应用中使用的 [Azure 事件中心示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=event)。</span><span class="sxs-lookup"><span data-stu-id="b2970-118">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

