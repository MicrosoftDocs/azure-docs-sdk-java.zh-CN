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
---
# <a name="azure-iot-libraries-for-java"></a>用于 Java 的 Azure IoT 库

使用 [Azure IoT 中心](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)连接、监视和控制物联网资产。

若要开始使用 Azure IoT 中心，请参阅[使用 Java 将设备连接到 IoT 中心](/azure/iot-hub/iot-hub-java-java-getstarted)。

## <a name="iot-service-library"></a>IoT 服务库

使用 IoT 服务库注册设备并将消息从云发送到已注册的设备。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-service-client</artifactId>
    <version>1.6.23</version>
</dependency>
```   

## <a name="iot-device-library"></a>Iot 设备库

使用 IoT 设备库将消息发送到云中，以及在设备上接收消息。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。  

```XML
<dependency>
    <groupId>com.microsoft.azure.sdk.iot</groupId>
    <artifactId>iot-device-client</artifactId>
    <version>1.3.31</version>
</dependency>
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/iot/client)   

## <a name="example"></a>示例

将消息从 Azure IoT 中心发送到设备。

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


## <a name="samples"></a>示例

[IoT 设备示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)     
[IoT 服务示例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples)

详细了解可在应用中使用的 [Azure IoT 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=iot)。
