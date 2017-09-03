---
title: "用于 Java 的 Azure Data Lake Store 库"
description: "Azure Data Lake Store 库的参考文档"
keywords: "Azure, Java, SDK, API, 大数据, data lake"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 06/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: data-lake-store
ms.openlocfilehash: 66ff566e74203d3b5a8e9bcc170f4c21cf310645
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-data-lake-store-libraries-for-java"></a>用于 Java 的 Azure Data Lake Store 库

## <a name="overview"></a>概述

使用 [Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview) 在单个位置捕获任意大小、类型和集成速度的数据以进行分析。

若要开始使用 Data Lake Store，请参阅[通过 Java 开始使用 Azure Data Lake Store](/azure/data-lake-store/data-lake-store-get-started-java-sdk)。


## <a name="client-library"></a>客户端库

使用客户端库在 Data Lake Store 中读取和写入文件、设置权限和元数据，以及管理文件和目录。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>azure-data-lake-store-sdk</artifactId>
   <version>2.1.5</version>
</dependency>
```   

## <a name="example"></a>示例

通过完全限定的域名和 OAuth2 访问令牌创建 Data Lake 客户端，然后在 Data Lake 中创建文件和写入数据。

```java
// AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);
ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

// create directory
client.createDirectory("/a/b/w");
        
// create file and write some content
String filename = "/a/b/c.txt";
OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
PrintStream out = new PrintStream(stream);
for (int i = 1; i <= 10; i++) {
    out.println("This is line #" + i);
    out.format("This is the same line (%d), but using formatted output. %n", i);
}
out.close();
```

> [!div class="nextstepaction"]
> [了解客户端 API](/java/api/overview/azure/datalakestore/clientlibrary)


## <a name="management-api"></a>管理 API

使用管理 API 来管理 Data Lake Store 帐户、防火墙规则和受信任的标识提供者。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。


```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-datalake-store</artifactId>
    <version>1.0.0-beta1.3</version>
</dependency>
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/datalakestore/managementapi)

## <a name="samples"></a>示例

[Azure Data Lake 入门][1] 

[1]: https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started

详细了解可在应用中使用的 [Azure Data Lake Store 示例 Java 代码](https://azure.microsoft.com/resources/samples/?platform=java&term=lake)。
