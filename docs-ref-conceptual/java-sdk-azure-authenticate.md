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
# <a name="authenticate-with-the-azure-libraries-for-java"></a><span data-ttu-id="97d5d-104">使用用于 Java 的 Azure 库进行身份验证</span><span class="sxs-lookup"><span data-stu-id="97d5d-104">Authenticate with the Azure libraries for Java</span></span> 

## <a name="connect-to-services-with-connection-strings"></a><span data-ttu-id="97d5d-105">使用连接字符串连接到服务</span><span class="sxs-lookup"><span data-stu-id="97d5d-105">Connect to services with connection strings</span></span>

<span data-ttu-id="97d5d-106">大多数 Azure 服务库使用连接字符串或安全密钥进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="97d5d-106">Most Azure service libraries use a connection string or secure key for authentication.</span></span> <span data-ttu-id="97d5d-107">例如，SQL 数据库在 JDBC 连接字符串中包含用户名和密码信息：</span><span class="sxs-lookup"><span data-stu-id="97d5d-107">For example, SQL Database includes username and password information in the JDBC connection string:</span></span>

```java
String url = "jdbc:sqlserver://myazuredb.database.windows.net:1433;" + 
        "database=testjavadb;" + 
        "user=myazdbuser;" +
        "password=myazdbpass;" +
        "encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";
        Connection conn = DriverManager.getConnection(url);
```

<span data-ttu-id="97d5d-108">Azure 存储使用存储密钥来为应用程序授权：</span><span class="sxs-lookup"><span data-stu-id="97d5d-108">Azure Storage uses a storage key to authorize the application:</span></span>

```java
final String storageConnection = "DefaultEndpointsProtocol=https;"
        + "AccountName=" + storageName 
        + ";AccountKey=" + storageKey
        + ";EndpointSuffix=core.windows.net";
```

<span data-ttu-id="97d5d-109">服务连接字符串用于在 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService)、[Redis 缓存](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started)和[服务总线](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues)等其他 Azure 服务中进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="97d5d-109">Service connection strings are used to authenticate to other Azure services like [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-java-application#UseService), [Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-java-get-started), and [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).</span></span> <span data-ttu-id="97d5d-110">可以使用 Azure 门户或 CLI 获取连接字符串。</span><span class="sxs-lookup"><span data-stu-id="97d5d-110">You can get the connection strings using the Azure portal or the CLI.</span></span>  <span data-ttu-id="97d5d-111">还可以使用用于 Java 的 Azure 管理库来查询资源，以便在代码中生成连接字符串。</span><span class="sxs-lookup"><span data-stu-id="97d5d-111">You can also use the Azure management libraries for Java to query resources to build connection strings in your code.</span></span> 

<span data-ttu-id="97d5d-112">例如，以下代码使用管理库创建存储帐户连接字符串：</span><span class="sxs-lookup"><span data-stu-id="97d5d-112">For example, this code uses the management libraries to create a storage account connection string:</span></span>

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

<span data-ttu-id="97d5d-113">其他库要求应用程序使用[服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)运行，该服务主体授权应用程序使用授予的凭据来运行。</span><span class="sxs-lookup"><span data-stu-id="97d5d-113">Other libraries require your application to run with a [service prinicpal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) authorizing the application to run with granted credentials.</span></span> <span data-ttu-id="97d5d-114">此配置类似于管理库的下列基于对象的身份验证步骤。</span><span class="sxs-lookup"><span data-stu-id="97d5d-114">This configuration is similar to the object-based authentication steps for the management library listed below.</span></span>

<a name="mgmt-auth"></a>

##  <a name="authenticate-with-the-azure-management-libraries-for-java"></a><span data-ttu-id="97d5d-115">使用用于 Java 的 Azure 管理库进行身份验证</span><span class="sxs-lookup"><span data-stu-id="97d5d-115">Authenticate with the Azure management libraries for Java</span></span>

<span data-ttu-id="97d5d-116">使用 Java 管理库创建和管理资源时，可以借助两个选项在 Azure 中对应用程序进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="97d5d-116">Two options are available to authenticate your application with Azure when using the Java management libraries to create and manage resources.</span></span>

### <a name="authenticate-with-an-applicationtokencredentials-object"></a><span data-ttu-id="97d5d-117">使用 ApplicationTokenCredentials 对象进行身份验证</span><span class="sxs-lookup"><span data-stu-id="97d5d-117">Authenticate with an ApplicationTokenCredentials object</span></span>

<span data-ttu-id="97d5d-118">创建 `ApplicationTokenCredentials` 的实例，以便从代码内部为顶级 `Azure` 对象提供服务主体凭据。</span><span class="sxs-lookup"><span data-stu-id="97d5d-118">Create an instance of `ApplicationTokenCredentials` to supply the service principal credentials to the top-level `Azure` object from inside your code.</span></span>

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

<span data-ttu-id="97d5d-119">`client`、`tenant` 和 `key` 是配合[基于文件的身份验证](#mgmt-file)使用的相同服务主体值。</span><span class="sxs-lookup"><span data-stu-id="97d5d-119">The `client`, `tenant` and `key` are the same service principal values used with [file-based authentication](#mgmt-file).</span></span> <span data-ttu-id="97d5d-120">`AzureEnvironment.AZURE` 值针对 Azure 公有云创建凭据。</span><span class="sxs-lookup"><span data-stu-id="97d5d-120">The `AzureEnvironment.AZURE` value creates credentials against the Azure public cloud.</span></span> <span data-ttu-id="97d5d-121">如需访问其他云（例如 `AzureEnvironment.AZURE_GERMANY`），请将此值更改为其他值。</span><span class="sxs-lookup"><span data-stu-id="97d5d-121">Change this to a different value if you need to access another cloud (for example, `AzureEnvironment.AZURE_GERMANY`).</span></span>  

 <span data-ttu-id="97d5d-122">从环境变量或机密管理存储（例如 [Key Vault](/azure/key-vault/key-vault-whatis)）中读取服务主体值。</span><span class="sxs-lookup"><span data-stu-id="97d5d-122">Read the service principal values from environment variables or a secret management store like [Key Vault](/azure/key-vault/key-vault-whatis).</span></span> <span data-ttu-id="97d5d-123">避免在代码中以明文字符串形式设置这些值，防止在版本控制历史记录中意外公开凭据。</span><span class="sxs-lookup"><span data-stu-id="97d5d-123">Avoid setting these values as cleartext strings in your code to prevent accidentally exposing credentials in your version control history.</span></span>   

<a name="mgmt-file"></a>

### <a name="file-based-authentication-preview"></a><span data-ttu-id="97d5d-124">基于文件的身份验证（预览）</span><span class="sxs-lookup"><span data-stu-id="97d5d-124">File based authentication (Preview)</span></span>

<span data-ttu-id="97d5d-125">最简单的身份验证方法是创建一个采用以下格式的 properties 文件并在其中包含 [Azure 服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)的凭据：</span><span class="sxs-lookup"><span data-stu-id="97d5d-125">The simplest way to authenticate is to create a properties file that contains credentials for an [Azure service principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects) using the following format:</span></span>

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

- <span data-ttu-id="97d5d-126">subscription：使用在 Azure CLI 2.0 中运行 `az account show` 后返回的 *id* 值。</span><span class="sxs-lookup"><span data-stu-id="97d5d-126">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="97d5d-127">client：使用为运行应用程序而创建的服务主体的输出中的 *appId* 值。</span><span class="sxs-lookup"><span data-stu-id="97d5d-127">client: use the *appId* value from the output taken from a service principal created to run the application.</span></span> <span data-ttu-id="97d5d-128">如果应用没有服务主体，请[使用 Azure CLI 2.0 创建一个服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="97d5d-128">If you don't have a service principal for your app, [create one with the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>
- <span data-ttu-id="97d5d-129">key：使用 service principal create CLI 输出中的 *password* 值</span><span class="sxs-lookup"><span data-stu-id="97d5d-129">key: use the *password* value from the service principal create CLI output</span></span> 
- <span data-ttu-id="97d5d-130">tenant：使用 service principal create CLI 输出中的 *tenant* 值</span><span class="sxs-lookup"><span data-stu-id="97d5d-130">tenant: use the *tenant* value from the service principal create CLI output</span></span>

<span data-ttu-id="97d5d-131">将此文件保存在系统上可供代码读取的安全位置。</span><span class="sxs-lookup"><span data-stu-id="97d5d-131">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="97d5d-132">在 shell 中将包含完整路径的环境变量设置为此文件：</span><span class="sxs-lookup"><span data-stu-id="97d5d-132">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="97d5d-133">创建入口点 `Azure` 对象以开始使用库。</span><span class="sxs-lookup"><span data-stu-id="97d5d-133">Create the entry point `Azure` object to start working with the libraries.</span></span> <span data-ttu-id="97d5d-134">通过环境变量读取 properties 文件的位置。</span><span class="sxs-lookup"><span data-stu-id="97d5d-134">Read the location of the properties file through the environment variable.</span></span>

```java
// pull in the location of the authenticaiton properties file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```



