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
ms.openlocfilehash: 076906ff3cafcb4eba97b0a022e5214d7834517c
ms.sourcegitcommit: 02b70b9f5d34415c337601f0b818f7e0985fd884
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2018
---
# <a name="azure-event-hub-libraries-for-java"></a><span data-ttu-id="9dcd3-104">用于 Java 的 Azure 事件中心库</span><span class="sxs-lookup"><span data-stu-id="9dcd3-104">Azure Event Hub libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="9dcd3-105">概述</span><span class="sxs-lookup"><span data-stu-id="9dcd3-105">Overview</span></span>

<span data-ttu-id="9dcd3-106">使用 [Azure 事件中心](/azure/event-hubs/event-hubs-what-is-event-hubs)从已连接的 IoT 设备和应用程序每秒收集数百万个事件并对其进行管理。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-106">Collect and manage millions of events per second from connected IoT devices and applications with [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs).</span></span>

<span data-ttu-id="9dcd3-107">若要开始使用 Azure 事件中心，请参阅[使用 Java 从 Azure 事件中心接收事件](/azure/event-hubs/event-hubs-java-get-started-receive-eph)。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-107">To get started with Azure Event Hubs, see [Receive events from Azure Event Hubs using Java](/azure/event-hubs/event-hubs-java-get-started-receive-eph).</span></span>


## <a name="client-library"></a><span data-ttu-id="9dcd3-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="9dcd3-108">Client library</span></span>

<span data-ttu-id="9dcd3-109">使用事件中心客户端库将事件发送到 Azure 事件中心，并从事件中心使用和处理事件。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-109">Send events to an Azure Event Hub and consume and process events from an Event Hub using the Event Hubs client library.</span></span>

<span data-ttu-id="9dcd3-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>0.14.3</version>
</dependency>
```   

## <a name="example"></a><span data-ttu-id="9dcd3-111">示例</span><span class="sxs-lookup"><span data-stu-id="9dcd3-111">Example</span></span>

<span data-ttu-id="9dcd3-112">将事件发送到事件中心。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-112">Send an event to an event hub.</span></span>

```java
ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName,sasKeyName, sasKey);

byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);
EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
ehClient.sendSync(sendEvent);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9dcd3-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="9dcd3-113">Explore the Client APIs</span></span>](/java/api/overview/azure/eventhub/client)


## <a name="samples"></a><span data-ttu-id="9dcd3-114">示例</span><span class="sxs-lookup"><span data-stu-id="9dcd3-114">Samples</span></span>

<span data-ttu-id="9dcd3-115">[通过 JMS 向事件中心写入数据以及从 Apache Storm 读取数据][1]
[使用混合 .NET/Java 拓扑来读取和写入事件中心][2]</span><span class="sxs-lookup"><span data-stu-id="9dcd3-115">[Write to Event Hub via JMS and read from Apache Storm][1]
[Read and write from EventHubs using a hybrid .NET/Java topology][2]</span></span> 

[1]: https://github.com/Azure-Samples/event-hubs-java-storm-sender-jms-receiver
[2]: https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub

<span data-ttu-id="9dcd3-116">详细了解可在应用中使用的 [Azure 事件中心示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=event)。</span><span class="sxs-lookup"><span data-stu-id="9dcd3-116">Explore more [sample Java code for Azure Event Hubs](https://azure.microsoft.com/resources/samples/?platform=java&term=event) you can use in your apps.</span></span>

