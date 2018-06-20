---
title: 用于 Java 的 Azure IoT 中心库
description: Java Azure IoT 中心库的参考文档
keywords: Azure, Java, SDK, API, 事件, IoT, 流, 设备, iot 中心
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/20/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: iot-hub
ms.openlocfilehash: 5e6a102b062b2fff6b297c7e3dda423d1448bcb0
ms.sourcegitcommit: 49b17bbf34732512f836ee634818f1058147ff5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31823610"
---
# <a name="azure-iot-libraries-for-java"></a><span data-ttu-id="5059c-104">用于 Java 的 Azure IoT 库</span><span class="sxs-lookup"><span data-stu-id="5059c-104">Azure IoT libraries for Java</span></span>

<span data-ttu-id="5059c-105">使用 [Azure IoT 中心](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)连接、监视和控制物联网资产。</span><span class="sxs-lookup"><span data-stu-id="5059c-105">Connect, monitor, and control Internet of Things assets with [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).</span></span>

<span data-ttu-id="5059c-106">若要开始使用 Azure IoT 中心，请参阅[使用 Java 将设备连接到 IoT 中心](/azure/iot-hub/iot-hub-java-java-getstarted)。</span><span class="sxs-lookup"><span data-stu-id="5059c-106">To get started with Azure IoT Hub, see [Connect your device to your IoT hub using Java](/azure/iot-hub/iot-hub-java-java-getstarted).</span></span>

## <a name="iot-service-library"></a><span data-ttu-id="5059c-107">IoT 服务库</span><span class="sxs-lookup"><span data-stu-id="5059c-107">IoT Service library</span></span>

<span data-ttu-id="5059c-108">使用 IoT 服务库注册设备并将消息从云发送到已注册的设备。</span><span class="sxs-lookup"><span data-stu-id="5059c-108">Register devices and send messages from the cloud to registered devices using the IoT Service library.</span></span>

<span data-ttu-id="5059c-109">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="5059c-109">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a><span data-ttu-id="5059c-110">Iot 设备库</span><span class="sxs-lookup"><span data-stu-id="5059c-110">Iot Device library</span></span>

<span data-ttu-id="5059c-111">使用 IoT 设备库将消息发送到云中，以及在设备上接收消息。</span><span class="sxs-lookup"><span data-stu-id="5059c-111">Send messages to the cloud and receive messages on devices using the IoT Device library.</span></span>

<span data-ttu-id="5059c-112">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="5059c-112">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5059c-113">了解客户端 API</span><span class="sxs-lookup"><span data-stu-id="5059c-113">Explore the Client APIs</span></span>](/java/api/overview/azure/iot/client)   

## <a name="example"></a><span data-ttu-id="5059c-114">示例</span><span class="sxs-lookup"><span data-stu-id="5059c-114">Example</span></span>

<span data-ttu-id="5059c-115">将消息从 Azure IoT 中心发送到设备。</span><span class="sxs-lookup"><span data-stu-id="5059c-115">Send a message from Azure IoT Hub to a device.</span></span>

```java
Message messageToSend = new Message(messageText);
messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
messageToSend.setMessageId(java.util.UUID.randomUUID().toString());

// set message properties
Map<String, String> propertiesToSend = new HashMap<String, String>();
propertiesToSend.put(messagePropertyKey,messagePropertyKey);
messageToSend.setProperties(propertiesToSend);

CompletableFuture<Void> future = serviceClient.sendAsync(deviceId, messageToSend);
try {
    future.get();
}
catch (ExecutionException e) {
    System.out.println("Exception : " + e.getMessage());
}
```


## <a name="samples"></a><span data-ttu-id="5059c-116">示例</span><span class="sxs-lookup"><span data-stu-id="5059c-116">Samples</span></span>

<span data-ttu-id="5059c-117">[IoT 设备示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span><span class="sxs-lookup"><span data-stu-id="5059c-117">[IoT Device samples](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)   </span></span>  
[<span data-ttu-id="5059c-118">IoT 服务示例</span><span class="sxs-lookup"><span data-stu-id="5059c-118">IoT Service samples</span></span>](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

<span data-ttu-id="5059c-119">详细了解可在应用中使用的 [Azure IoT 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)。</span><span class="sxs-lookup"><span data-stu-id="5059c-119">Explore more [sample Java code for Azure IoT](https://azure.microsoft.com/resources/samples/?platform=java&term=iot) you can use in your apps.</span></span>
