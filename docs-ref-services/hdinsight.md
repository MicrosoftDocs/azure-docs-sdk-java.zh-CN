---
title: Azure HDInsight Java SDK
description: Azure HDInsight Java SDK 参考。 HDInsight Java SDK 提供用于管理 HDInsight 群集的类和方法。
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 9/20/2018
ms.openlocfilehash: 5e3887341ddb2fdcab336f0a8a232e6e8bfbe0f2
ms.sourcegitcommit: bb7286fad75a2bb43e6ce1a8f1b09e701147c9f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "48047154"
---
# <a name="hdinsight-java-management-sdk-preview"></a><span data-ttu-id="a3a8d-104">HDInsight Java 管理 SDK（预览版）</span><span class="sxs-lookup"><span data-stu-id="a3a8d-104">HDInsight Java Management SDK (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="a3a8d-105">概述</span><span class="sxs-lookup"><span data-stu-id="a3a8d-105">Overview</span></span>

<span data-ttu-id="a3a8d-106">HDInsight Java SDK 提供用于管理 HDInsight 群集的类和方法。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-106">The HDInsight Java SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="a3a8d-107">该 SDK 包含用于创建、删除、更新、列出、缩放、执行脚本操作，以及监视、获取 HDInsight 群集属性等的操作。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-107">It includes operations to create, delete, update, list, scale, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3a8d-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="a3a8d-108">Prerequisites</span></span>

* <span data-ttu-id="a3a8d-109">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-109">An Azure account.</span></span> <span data-ttu-id="a3a8d-110">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* [<span data-ttu-id="a3a8d-111">Java JDK</span><span class="sxs-lookup"><span data-stu-id="a3a8d-111">Java JDK</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [<span data-ttu-id="a3a8d-112">Maven</span><span class="sxs-lookup"><span data-stu-id="a3a8d-112">Maven</span></span>](https://maven.apache.org/install.html)

## <a name="sdk-installation"></a><span data-ttu-id="a3a8d-113">SDK 安装</span><span class="sxs-lookup"><span data-stu-id="a3a8d-113">SDK Installation</span></span>

<span data-ttu-id="a3a8d-114">若要通过 Maven 获取 HDInsight Java SDK，请单击[此处](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-114">The HDInsight Java SDK is available through Maven [here](https://mvnrepository.com/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span></span> <span data-ttu-id="a3a8d-115">将以下依赖项添加到 pom.xml：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-115">Add the following dependency to your pom.xml:</span></span>

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

<span data-ttu-id="a3a8d-116">此外还需将以下依赖项添加到 pom.xml：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-116">You will also need to add the following dependencies to your pom.xml:</span></span>

* [<span data-ttu-id="a3a8d-117">Azure 客户端身份验证库：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-117">Azure Client Authentication Library:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-client-authentication/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.2</version>
    <scope>test</scope>
</dependency>
```

* [<span data-ttu-id="a3a8d-118">适用于 ARM 的 Azure Java 客户端运行时：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-118">Azure Java Client Runtime For ARM:</span></span>](https://mvnrepository.com/artifact/com.microsoft.azure/azure-arm-client-runtime/1.6.2)
```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.2</version>
</dependency>
```

## <a name="authentication"></a><span data-ttu-id="a3a8d-119">身份验证</span><span class="sxs-lookup"><span data-stu-id="a3a8d-119">Authentication</span></span>

<span data-ttu-id="a3a8d-120">首先需要使用 Azure 订阅对该 SDK 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-120">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="a3a8d-121">请遵循以下示例创建服务主体，然后使用该服务主体进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-121">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="a3a8d-122">完成此操作后，将会获得 `HDInsightManagementClientImpl` 的实例，其中包含可用于执行管理操作的多个方法（以下部分将概述这些方法）。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-122">After this is done, you will have an instance of an `HDInsightManagementClientImpl`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="a3a8d-123">除了以下示例中所示的方法以外，还有其他一些身份验证方法可能更符合你的需要。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-123">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="a3a8d-124">[使用用于 Java 的 Azure 管理库进行身份验证](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)中概述了所有方法</span><span class="sxs-lookup"><span data-stu-id="a3a8d-124">All methods are outlined here: [Authenticate with the Azure management libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="a3a8d-125">使用服务主体的身份验证示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-125">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="a3a8d-126">首先登录到 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-126">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="a3a8d-127">验证当前使用的是要在其中创建服务主体的订阅。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-127">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="a3a8d-128">订阅信息将显示为 JSON。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-128">Your subscription information is displayed as JSON.</span></span>

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

<span data-ttu-id="a3a8d-129">如果尚未登录到正确的订阅，请运行以下命令选择正确的订阅：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-129">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="a3a8d-130">如果尚未通过其他方法（例如，通过 Azure 门户创建 HDInsight 群集）注册 HDInsight 资源提供程序，则需要先执行此操作一次，然后才能进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-130">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="a3a8d-131">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来完成此操作：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-131">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="a3a8d-132">接下来，选择服务主体的名称，然后使用以下命令创建服务主体：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-132">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="a3a8d-133">服务主体信息将以 JSON 格式显示。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-133">The service principal information is displayed as JSON.</span></span>

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
<span data-ttu-id="a3a8d-134">复制以下代码片段，并在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填写运行创建服务主体的命令后返回的 JSON 中的字符串。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-134">Copy the below snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

```java
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.*;
import com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.HDInsightManagementClientImpl;

public class Main {
    public static void main (String[] args) {

        // Tenant ID for your Azure Subscription
        String TENANT_ID = "";
        // Your Service Principal App Client ID
        String CLIENT_ID = "";
        // Your Service Principal Client Secret
        String CLIENT_SECRET = "";
        // Azure Subscription ID
        String SUBSCRIPTION_ID = "";

        ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(
                CLIENT_ID,
                TENANT_ID,
                CLIENT_SECRET,
                AzureEnvironment.AZURE);

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials);
```


## <a name="cluster-management"></a><span data-ttu-id="a3a8d-135">群集管理</span><span class="sxs-lookup"><span data-stu-id="a3a8d-135">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="a3a8d-136">本部分假设你已完成身份验证，已构造 `HDInsightManagementClientImpl` 实例并已将其存储在名为 `client` 的变量中。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-136">This section assumes you have already authenticated and constructed an `HDInsightManagementClientImpl` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="a3a8d-137">在前面的“身份验证”部分可以找到有关身份验证和获取 `HDInsightManagementClientImpl` 的说明。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-137">Instructions for authenticating and obtaining an `HDInsightManagementClientImpl` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="a3a8d-138">创建群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-138">Create a Cluster</span></span>

<span data-ttu-id="a3a8d-139">可以通过调用 `client.clusters().create()` 来创建新群集。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-139">A new cluster can be created by calling `client.clusters().create()`.</span></span>

#### <a name="example"></a><span data-ttu-id="a3a8d-140">示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-140">Example</span></span>

<span data-ttu-id="a3a8d-141">本示例演示如何创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-141">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="a3a8d-142">首先需要创建一个资源组和存储帐户，下面将予以介绍。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-142">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="a3a8d-143">如果已创建资源组和存储帐户，则可以跳过这些步骤。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-143">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="a3a8d-144">创建资源组</span><span class="sxs-lookup"><span data-stu-id="a3a8d-144">Creating a Resource Group</span></span>

<span data-ttu-id="a3a8d-145">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建资源组</span><span class="sxs-lookup"><span data-stu-id="a3a8d-145">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="a3a8d-146">创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="a3a8d-146">Creating a Storage Account</span></span>

<span data-ttu-id="a3a8d-147">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="a3a8d-147">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="a3a8d-148">现在，运行以下命令获取存储帐户的密钥（创建群集时需要用到）：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-148">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="a3a8d-149">以下 Java 代码片段创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-149">The below Java snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="a3a8d-150">按照注释中所述填写空白变量，并根据具体的需要任意更改其他参数。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-150">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

```java
// The name for the cluster you are creating
String clusterName = "";
// The name of your existing Resource Group
String resourceGroupName = "";
// Choose a username
String username = "";
// Choose a password
String password = "";
// Replace <> with the name of your storage account
String storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
String storageAccountKey = "";
// Choose a region
String location = "";
String container = "default";

HashMap<String, HashMap<String, String>> configurations = new HashMap<String, HashMap<String, String>>();
        HashMap<String, String> gateway = new HashMap<String, String>();
        gateway.put("restAuthCredential.enabled_credential", "True");
        gateway.put("restAuthCredential.username", username);
        gateway.put("restAuthCredential.password", password);
        configurations.put("gateway", gateway);

        ClusterCreateParametersExtended parameters = new ClusterCreateParametersExtended()
            .withLocation(location)
            .withTags(Collections.EMPTY_MAP)
            .withProperties(
                new ClusterCreateProperties()
                    .withClusterVersion("3.6")
                    .withOsType(OSType.LINUX)
                    .withClusterDefinition(new ClusterDefinition()
                            .withKind("spark")
                            .withConfigurations(configurations)
                    )
                    .withTier(Tier.STANDARD)
                    .withComputeProfile(new ComputeProfile()
                        .withRoles(List.of(
                            new Role()
                                .withName("headnode")
                                .withTargetInstanceCount(2)
                                .withHardwareProfile(new HardwareProfile()
                                    .withVmSize("Large")
                                )
                                .withOsProfile(new OsProfile()
                                    .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                    )
                                ),
                            new Role()
                                    .withName("workernode")
                                    .withTargetInstanceCount(1)
                                    .withHardwareProfile(new HardwareProfile()
                                        .withVmSize("Large")
                                    )
                                    .withOsProfile(new OsProfile()
                                        .withLinuxOperatingSystemProfile(new LinuxOperatingSystemProfile()
                                            .withUsername(username)
                                            .withPassword(password)
                                        )
                                    )
                        ))
                    )
                    .withStorageProfile(new StorageProfile()
                        .withStorageaccounts(List.of(
                                new StorageAccount()
                                    .withName(storageAccount)
                                    .withKey(storageAccountKey)
                                    .withContainer(container)
                                    .withIsDefault(true)
                        ))
                    )
            );
        client.clusters().create(resourceGroupName, clusterName, parameters);
```

### <a name="get-cluster-details"></a><span data-ttu-id="a3a8d-151">获取群集详细信息</span><span class="sxs-lookup"><span data-stu-id="a3a8d-151">Get Cluster Details</span></span>

<span data-ttu-id="a3a8d-152">获取给定群集的属性：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-152">To get properties for a given cluster:</span></span>

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="a3a8d-153">示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-153">Example</span></span>

<span data-ttu-id="a3a8d-154">可以使用 `get` 来确认已成功创建群集。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-154">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

<span data-ttu-id="a3a8d-155">输出应如下所示：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-155">The output should look like:</span></span>

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a><span data-ttu-id="a3a8d-156">列出群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-156">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="a3a8d-157">列出订阅下的群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-157">List Clusters Under The Subscription</span></span>

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="a3a8d-158">按资源组列出群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-158">List Clusters By Resource Group</span></span>

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> <span data-ttu-id="a3a8d-159">`List()` 和 `ListByResourceGroup()` 都返回 `PagedList<ClusterInner>` 对象。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-159">Both `List()` and `ListByResourceGroup()` return a `PagedList<ClusterInner>` object.</span></span> <span data-ttu-id="a3a8d-160">调用 `loadNext()` 会在该页上返回群集列表，并会将 `ClusterPaged` 对象推到下一页。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-160">Calling `loadNext()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="a3a8d-161">此操作可以一直重复，直至 `hasNextPage()` 返回 `false`，这表明没有其他页。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-161">This can be repeated until `hasNextPage()` return `false`, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="a3a8d-162">示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-162">Example</span></span>

<span data-ttu-id="a3a8d-163">以下示例列显当前订阅的所有群集的属性：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-163">The following example prints the properties of all clusters for the current subscription:</span></span>

```java
PagedList<ClusterInner> clusterPages = client.clusters().list();
while (true) {
    for (ClusterInner cluster : clusterPages.currentPage().items()) {
        System.out.println(cluster.name());
    }
    if (clusterPages.hasNextPage()) {
        clusterPages.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="delete-a-cluster"></a><span data-ttu-id="a3a8d-164">删除群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-164">Delete a Cluster</span></span>

<span data-ttu-id="a3a8d-165">删除群集：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-165">To delete a cluster:</span></span>

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a><span data-ttu-id="a3a8d-166">更新群集标记</span><span class="sxs-lookup"><span data-stu-id="a3a8d-166">Update Cluster Tags</span></span>

<span data-ttu-id="a3a8d-167">可按如下所示更新给定群集的标记：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-167">You can update the tags of a given cluster like so:</span></span>

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="scale-cluster"></a><span data-ttu-id="a3a8d-168">缩放群集</span><span class="sxs-lookup"><span data-stu-id="a3a8d-168">Scale Cluster</span></span>

<span data-ttu-id="a3a8d-169">可以通过指定新大小来缩放给定群集的工作节点数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-169">You can scale a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="a3a8d-170">群集监视</span><span class="sxs-lookup"><span data-stu-id="a3a8d-170">Cluster Monitoring</span></span>

<span data-ttu-id="a3a8d-171">使用 HDInsight 管理 SDK 还可以通过 Operations Management Suite (OMS) 来管理群集的监视。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-171">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="a3a8d-172">启用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="a3a8d-172">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="a3a8d-173">若要启用 OMS 监视，必须已有一个 Log Analytics 工作区。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-173">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="a3a8d-174">如果尚未创建此工作区，可以参阅[在 Azure 门户中创建 Log Analytics 工作区](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)了解相关操作。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-174">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="a3a8d-175">在群集上启用 OMS 监视：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-175">To enable OMS Monitoring on your cluster:</span></span>

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="a3a8d-176">查看 OMS 监视状态</span><span class="sxs-lookup"><span data-stu-id="a3a8d-176">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="a3a8d-177">获取群集上的 OMS 状态：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-177">To get the status of OMS on your cluster:</span></span>

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="a3a8d-178">禁用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="a3a8d-178">Disable OMS Monitoring</span></span>

<span data-ttu-id="a3a8d-179">在群集上禁用 OMS：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-179">To disable OMS on your cluster:</span></span>

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a><span data-ttu-id="a3a8d-180">脚本操作</span><span class="sxs-lookup"><span data-stu-id="a3a8d-180">Script Actions</span></span>

<span data-ttu-id="a3a8d-181">HDInsight 提供一个称为“脚本操作”的配置方法，该方法可调用用于自定义群集的自定义脚本。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-181">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="a3a8d-182">有关如何使用脚本操作的详细信息，请参阅[使用脚本操作自定义基于 Linux 的 HDInsight 群集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="a3a8d-182">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="a3a8d-183">执行脚本操作</span><span class="sxs-lookup"><span data-stu-id="a3a8d-183">Execute Script Actions</span></span>

<span data-ttu-id="a3a8d-184">可按如下所示在给定的群集上执行脚本操作：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-184">You can execute script actions on a given cluster like so:</span></span>

```java
RuntimeScriptAction scriptAction1 = new RuntimeScriptAction()
    .withName("<Script Name>")
    .withUri("<URL To Script>")
    .withRoles(<List<String> of roles>);
client.clusters().executeScriptActions(
    resourceGroupName, 
    clusterName, 
    new ExecuteScriptActionParameters().withPersistOnSuccess(false).withScriptActions(new LinkedList<>(Arrays.asList(scriptAction1)))); //add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="a3a8d-185">删除脚本操作</span><span class="sxs-lookup"><span data-stu-id="a3a8d-185">Delete Script Action</span></span>

<span data-ttu-id="a3a8d-186">删除给定群集上指定的持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-186">To delete a specified persisted script action on a given cluster:</span></span>

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="a3a8d-187">列出持久化脚本操作</span><span class="sxs-lookup"><span data-stu-id="a3a8d-187">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="a3a8d-188">`listByCluster()` 返回 `PagedList<RuntimeScriptActionDetailInner>` 对象。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-188">Both `listByCluster()` returns a `PagedList<RuntimeScriptActionDetailInner>` object.</span></span> <span data-ttu-id="a3a8d-189">调用 `currentPage().items()` 会返回 `RuntimeScriptActionDetailInner` 的列表，而 `loadNextPage()` 会进入下一页。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-189">Calling `currentPage().items()` returns a list of `RuntimeScriptActionDetailInner`, and `loadNextPage()` advances to the next page.</span></span> <span data-ttu-id="a3a8d-190">此操作可以一直重复，直至 `hasNextPage()` 返回 `false`，这表明没有其他页。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-190">This can be repeated until `hasNextPage()` returns `false`, indicating that there are no more pages.</span></span>

<span data-ttu-id="a3a8d-191">列出指定群集的所有持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-191">To list all persisted script actions for the specified cluster:</span></span>
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="a3a8d-192">示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-192">Example</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptsPaged = client.scriptActions().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptsPaged.hasNextPage()) {
        scriptsPaged.loadNextPage();
    } else {
        break;
    }
}
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="a3a8d-193">列出所有脚本的执行历史记录</span><span class="sxs-lookup"><span data-stu-id="a3a8d-193">List All Scripts' Execution History</span></span>

<span data-ttu-id="a3a8d-194">列出指定群集的所有脚本的执行历史记录：</span><span class="sxs-lookup"><span data-stu-id="a3a8d-194">To list all scripts' execution history for the specified cluster:</span></span>

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="a3a8d-195">示例</span><span class="sxs-lookup"><span data-stu-id="a3a8d-195">Example</span></span>

<span data-ttu-id="a3a8d-196">此示例列显以往所有脚本执行活动的所有详细信息。</span><span class="sxs-lookup"><span data-stu-id="a3a8d-196">This example prints all the details for all past script executions.</span></span>

```java
PagedList<RuntimeScriptActionDetailInner> scriptExecutionsPaged = client.scriptExecutionHistorys().listByCluster(resourceGroupName, clusterName);
while (true) {
    for (RuntimeScriptActionDetailInner script : scriptExecutionsPaged.currentPage().items()) {
        System.out.println(script.name()); //There are methods to get other properties of RuntimeScriptActionDetail besides name(), such as status(), operation(), startTime(), endTime(), etc. See reference documentation.
    }
    if (scriptExecutionsPaged.hasNextPage()) {
        scriptExecutionsPaged.loadNextPage();
    } else {
        break;
    }
}
```
