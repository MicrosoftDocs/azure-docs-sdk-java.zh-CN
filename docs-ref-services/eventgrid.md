---
title: 用于 Java 的 Azure 事件网格库
description: Azure 事件网格 Java 库的参考文档
keywords: Azure, Java, SDK, API, 事件网格, 消息传送, 事件驱动
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 07/11/2017
ms.topic: managed-reference
ms.devlang: java
ms.service: event-grid
ms.openlocfilehash: 35acbdeede2fbc541128029ad43f149209a057c4
ms.sourcegitcommit: db36eeb667a0bfe6d113953bb0583240db61b0f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134623"
---
# <a name="azure-event-grid-libraries-for-java"></a><span data-ttu-id="d0c3a-104">用于 Java 的 Azure 事件网格库</span><span class="sxs-lookup"><span data-stu-id="d0c3a-104">Azure Event Grid libraries for Java</span></span>

<span data-ttu-id="d0c3a-105">将简单的基于 HTTP 的事件处理与 Azure 事件网格配合使用，开发事件驱动型应用程序，以便侦听并响应来自 Azure 服务和自定义源的事件。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-105">Build event-driven applications that listen and react to events from Azure services and custom sources using simple HTTP-based event handling with Azure Event Grid.</span></span>

<span data-ttu-id="d0c3a-106">[详细了解](/azure/event-grid/overview) Azure 事件网格，通过 [Azure Blob 存储事件教程](/azure/storage/blobs/storage-blob-event-quickstart)来入门。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-106">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="client-sdk"></a><span data-ttu-id="d0c3a-107">客户端 SDK</span><span class="sxs-lookup"><span data-stu-id="d0c3a-107">Client SDK</span></span>

<span data-ttu-id="d0c3a-108">使用 Azure 事件网格客户端 SDK 创建事件、进行身份验证以及发布到主题。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-108">Create events, authenticate, and post to topics using the Azure Event Grid Client SDK.</span></span>

<span data-ttu-id="d0c3a-109">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventgrid</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="publish-events"></a><span data-ttu-id="d0c3a-110">发布事件</span><span class="sxs-lookup"><span data-stu-id="d0c3a-110">Publish events</span></span>

<span data-ttu-id="d0c3a-111">以下代码通过 Azure 进行身份验证，然后将自定义类型（在此示例中为 `Contoso.Items.ItemsReceived`）的 `EventGridEvent` 事件的 `List` 发布到某个主题。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-111">The following code authenticates with Azure and publishes a `List` of  `EventGridEvent` events of a custom type (in this example, `Contoso.Items.ItemsReceived` ) to a topic.</span></span> <span data-ttu-id="d0c3a-112">示例中使用的主题密钥和终结点地址可以从 Azure CLI 检索：</span><span class="sxs-lookup"><span data-stu-id="d0c3a-112">The topic key and endpoint address used in the sample can be retrieved from the Azure CLI:</span></span>

```azurecli-interactive
az eventgrid topic show -g gridResourceGroup -n topicName
az eventgrid topic key list -g gridResourceGroup -n topicName
```

```java
// Create an event grid client.
TopicCredentials topicCredentials = new TopicCredentials(System.getenv("EVENTGRID_TOPIC_KEY"));
EventGridClient client = new EventGridClientImpl(topicCredentials);

// Publish custom events to the EventGrid.
System.out.println("Publish custom events to the EventGrid");
List<EventGridEvent> eventsList = new ArrayList<>();
for (int i = 0; i < 5; i++) {
    eventsList.add(new EventGridEvent(
        UUID.randomUUID().toString(),
        String.format("Door%d", i),
        new ContosoItemReceivedEventData("Contoso Item SKU #1"),
        "Contoso.Items.ItemReceived",
        DateTime.now(),
        "2.0"
    ));
}

String eventGridEndpoint = String.format("https://%s/", new URI(System.getenv("EVENTGRID_TOPIC_ENDPOINT")).getHost());

client.publishEvents(eventGridEndpoint, eventsList);
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d0c3a-113">探索事件网格客户端 API</span><span class="sxs-lookup"><span data-stu-id="d0c3a-113">Explore the Event Grid Client APIs</span></span>](/java/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="d0c3a-114">管理 SDK</span><span class="sxs-lookup"><span data-stu-id="d0c3a-114">Management SDK</span></span>

<span data-ttu-id="d0c3a-115">使用事件网格管理 SDK 创建、更新或删除事件网格实例、主题和订阅。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the Event Grid management SDK.</span></span>

<span data-ttu-id="d0c3a-116">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="d0c3a-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.16.0</version>
</dependency>
```   

<span data-ttu-id="d0c3a-117">以下示例创建一个事件网格订阅，取自 [EventGrid Java 示例](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)</span><span class="sxs-lookup"><span data-stu-id="d0c3a-117">The following example creates an Event Grid subscription, taken from the [EventGrid Java samples](https://github.com/Azure-Samples/event-grid-java-publish-consume-events)</span></span>

```java
// Create an event grid subscription.
//
System.out.println("Creating an Azure EventGrid Subscription");

EventSubscription eventSubscription = eventGridManager.eventSubscriptions().define(eventSubscriptionName)
    .withScope(eventGridTopic.id())
    .withDestination(new EventHubEventSubscriptionDestination()
        .withResourceId(eventHub.id()))
    .withFilter(new EventSubscriptionFilter()
        .withIsSubjectCaseSensitive(false)
        .withSubjectBeginsWith("")
        .withSubjectEndsWith(""))
    .create();
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d0c3a-118">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="d0c3a-118">Explore the management APIs</span></span>](/java/api/overview/azure/eventgrid/management)