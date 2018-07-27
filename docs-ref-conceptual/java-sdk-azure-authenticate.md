---
title: 使用用于 Java 的 Azure 管理库进行身份验证
description: 在用于 Java 的 Azure 管理库中使用服务主体进行身份验证
keywords: Azure, Java, SDK, API, Maven, Gradle, 身份验证, active directory, 服务主体
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: 10f457e3-578b-4655-8cd1-51339226ee7d
ms.openlocfilehash: 1d556955fcc5b73f1ba099a0b846b571ba64ccff
ms.sourcegitcommit: 107c3c5ed8c6991c751f95bcaf3757220940df9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050724"
---
# <a name="authenticate-with-the-azure-libraries-for-java"></a>使用用于 Java 的 Azure 库进行身份验证 

## <a name="connect-to-services-with-connection-strings"></a>使用连接字符串连接到服务

大多数 Azure 服务库使用连接字符串或安全密钥进行身份验证。 例如，SQL 数据库在 JDBC 连接字符串中包含用户名和密码信息：

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

Azure 存储使用存储密钥来为应用程序授权：

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

服务连接字符串用于在 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis 缓存](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)和[服务总线](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues)等其他 Azure 服务中进行身份验证。 可以使用 Azure 门户或 CLI 获取连接字符串。  还可以使用用于 Java 的 Azure 管理库来查询资源，以便在代码中生成连接字符串。 

例如，以下代码使用管理库创建存储帐户连接字符串：

```java
// create a new storage account
StorageAccount storage = azure.storageAccounts().getByResourceGroup("myResourceGroup","myStorageAccount");

// create a storage container to hold the file
List<StorageAccountKey> keys = storage.getKeys();
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storage.name()
        + ";AccountKey=" + keys.get(0).value()
        + ";EndpointSuffix=core.windows.net";
```

其他库要求应用程序使用[服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)运行，该服务主体授权应用程序使用授予的凭据来运行。 此配置类似于管理库的下列基于对象的身份验证步骤。

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a>使用用于 Java 的 Azure 管理库进行身份验证

使用 Java 管理库创建和管理资源时，可以借助两个选项在 Azure 中对应用程序进行身份验证。

### <a name="authenticate-with-an-applicationtokencredentials-object"></a>使用 ApplicationTokenCredentials 对象进行身份验证

创建 `ApplicationTokenCredentials` 的实例，以便从代码内部为顶级 `Azure` 对象提供服务主体凭据。

```java
import com.microsoft.azure.credentials.ApplicationTokenCredentials;
import com.microsoft.azure.AzureEnvironment;

// ...

ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(client, 
        tenant,
        key, 
        AzureEnvironment.AZURE);
        
Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credentials)
        .withDefaultSubscription();
```

`client`、`tenant` 和 `key` 是配合[基于文件的身份验证](#mgmt-file)使用的相同服务主体值。 `AzureEnvironment.AZURE` 值针对 Azure 公有云创建凭据。 如需访问其他云（例如 `AzureEnvironment.AZURE_GERMANY`），请将此值更改为其他值。  

 从环境变量或机密管理存储（例如 [Key Vault](/azure/key-vault/key-vault-whatis)）中读取服务主体值。 避免在代码中以明文字符串形式设置这些值，防止在版本控制历史记录中意外公开凭据。   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a>基于文件的身份验证（预览）

最简单的身份验证方法是创建一个采用以下格式的 properties 文件并在其中包含 [Azure 服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)的凭据：

```text
# sample management library properties file
subscription=########-####-####-####-############
client=########-####-####-####-############
key=XXXXXXXXXXXXXXXX
tenant=########-####-####-####-############
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

- subscription：使用在 Azure CLI 2.0 中运行 `az account show` 后返回的 *id* 值。
- client：使用为运行应用程序而创建的服务主体的输出中的 *appId* 值。 如果应用没有服务主体，请[使用 Azure CLI 2.0 创建一个服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)。
- key：使用 service principal create CLI 输出中的 *password* 值 
- tenant：使用 service principal create CLI 输出中的 *tenant* 值

将此文件保存在系统上可供代码读取的安全位置。 在 shell 中将包含完整路径的环境变量设置为此文件：

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

创建入口点 `Azure` 对象以开始使用库。 通过环境变量读取 properties 文件的位置。

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



