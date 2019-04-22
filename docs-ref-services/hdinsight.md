---
title: 用于 Java 的 Azure HDInsight SDK
description: 用于 Java 的 Azure HDInsight SDK 参考。 用于 Java 的 HDInsight SDK 提供用于管理 HDInsight 群集的类和方法。
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: reference
ms.devlang: java
ms.date: 04/15/2019
ms.openlocfilehash: fe87c9214e2a620230cf2f1f52261fd66a2b8857
ms.sourcegitcommit: f33befab25a66a252b4c91c7aeb1b77cb32821bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705115"
---
# <a name="hdinsight-sdk-for-java"></a><span data-ttu-id="3e7ff-104">用于 Java 的 HDInsight SDK</span><span class="sxs-lookup"><span data-stu-id="3e7ff-104">HDInsight SDK for Java</span></span>

## <a name="overview"></a><span data-ttu-id="3e7ff-105">概述</span><span class="sxs-lookup"><span data-stu-id="3e7ff-105">Overview</span></span>

<span data-ttu-id="3e7ff-106">用于 Java 的 HDInsight SDK 提供用于管理 HDInsight 群集的类和方法。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-106">The HDInsight SDK for Java provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="3e7ff-107">该 SDK 包含用于创建、删除、更新、列出、调整大小、执行脚本操作，以及监视、获取 HDInsight 群集属性等操作。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e7ff-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="3e7ff-108">Prerequisites</span></span>

* <span data-ttu-id="3e7ff-109">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-109">An Azure account.</span></span> <span data-ttu-id="3e7ff-110">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="3e7ff-111">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-111">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="3e7ff-112">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-112">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* [<span data-ttu-id="3e7ff-113">Maven</span><span class="sxs-lookup"><span data-stu-id="3e7ff-113">Maven</span></span>](https://maven.apache.org/download.cgi)

## <a name="sdk-installation"></a><span data-ttu-id="3e7ff-114">SDK 安装</span><span class="sxs-lookup"><span data-stu-id="3e7ff-114">SDK Installation</span></span>

<span data-ttu-id="3e7ff-115">若要通过 Maven 获取用于 Java 的 HDInsight SDK，请单击[此处](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-115">The HDInsight SDK for Java is available through Maven [here](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight).</span></span> <span data-ttu-id="3e7ff-116">将以下依赖项添加到 pom.xml：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-116">Add the following dependency to your pom.xml:</span></span>

```
<dependency>
    <groupId>com.microsoft.azure.hdinsight.v2018_06_01_preview</groupId>
    <artifactId>azure-mgmt-hdinsight</artifactId>
    <version>1.0.0-beta-1</version>
</dependency>
```

<span data-ttu-id="3e7ff-117">此外还需将以下依赖项添加到 pom.xml：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-117">You will also need to add the following dependencies to your pom.xml:</span></span>

* [<span data-ttu-id="3e7ff-118">Azure 客户端身份验证库：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-118">Azure Client Authentication Library:</span></span>](https://search.maven.org/artifact/com.microsoft.azure/azure-client-authentication)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-client-authentication</artifactId>
    <version>1.6.5</version>
  </dependency>
  ```

* [<span data-ttu-id="3e7ff-119">适用于 ARM 的 Azure Java 客户端运行时：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-119">Azure Java Client Runtime For ARM:</span></span>](https://search.maven.org/artifact/com.microsoft.azure/azure-arm-client-runtime)
  ```
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-arm-client-runtime</artifactId>
    <version>1.6.5</version>
  </dependency>
  ```

## <a name="authentication"></a><span data-ttu-id="3e7ff-120">Authentication</span><span class="sxs-lookup"><span data-stu-id="3e7ff-120">Authentication</span></span>

<span data-ttu-id="3e7ff-121">首先需要使用 Azure 订阅对该 SDK 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-121">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="3e7ff-122">请遵循以下示例创建服务主体，然后使用该服务主体进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-122">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="3e7ff-123">完成此操作后，将会获得 `HDInsightManagementClientImpl` 的实例，其中包含可用于执行管理操作的多个方法（以下部分将概述这些方法）。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-123">After this is done, you will have an instance of an `HDInsightManagementClientImpl`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="3e7ff-124">除了以下示例中所示的方法以外，还有其他一些身份验证方法可能更符合你的需要。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-124">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="3e7ff-125">此处概述了所有方法：[使用用于 Java 的 Azure 管理库进行身份验证](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span><span class="sxs-lookup"><span data-stu-id="3e7ff-125">All methods are outlined here: [Authenticate with the Azure management libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-authenticate?view=azure-java-stable)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="3e7ff-126">使用服务主体的身份验证示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-126">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="3e7ff-127">首先登录到 [Azure Cloud Shell](https://shell.azure.com/bash)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-127">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="3e7ff-128">验证当前使用的是要在其中创建服务主体的订阅。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-128">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="3e7ff-129">订阅信息将显示为 JSON。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-129">Your subscription information is displayed as JSON.</span></span>

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

<span data-ttu-id="3e7ff-130">如果尚未登录到正确的订阅，请运行以下命令选择正确的订阅：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-130">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="3e7ff-131">如果尚未通过其他方法（例如，通过 Azure 门户创建 HDInsight 群集）注册 HDInsight 资源提供程序，则需要先执行此操作一次，然后才能进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-131">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="3e7ff-132">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来完成此操作：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-132">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="3e7ff-133">接下来，选择服务主体的名称，然后使用以下命令创建服务主体：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-133">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="3e7ff-134">服务主体信息将以 JSON 格式显示。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-134">The service principal information is displayed as JSON.</span></span>

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
<span data-ttu-id="3e7ff-135">复制以下代码片段，并在 `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET` 和 `SUBSCRIPTION_ID` 中填写运行创建服务主体的命令后返回的 JSON 中的字符串。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-135">Copy the below snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

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

        HDInsightManagementClientImpl client = new HDInsightManagementClientImpl(credentials)
                .withSubscriptionId(SUBSCRIPTION_ID);
```

## <a name="cluster-management"></a><span data-ttu-id="3e7ff-136">群集管理</span><span class="sxs-lookup"><span data-stu-id="3e7ff-136">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="3e7ff-137">本部分假设你已完成身份验证，已构造 `HDInsightManagementClientImpl` 实例并已将其存储在名为 `client` 的变量中。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-137">This section assumes you have already authenticated and constructed an `HDInsightManagementClientImpl` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="3e7ff-138">在前面的“身份验证”部分可以找到有关身份验证和获取 `HDInsightManagementClientImpl` 的说明。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-138">Instructions for authenticating and obtaining an `HDInsightManagementClientImpl` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="3e7ff-139">创建群集</span><span class="sxs-lookup"><span data-stu-id="3e7ff-139">Create a Cluster</span></span>

<span data-ttu-id="3e7ff-140">可以通过调用 `client.clusters().create()` 来创建新群集。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-140">A new cluster can be created by calling `client.clusters().create()`.</span></span>

#### <a name="samples"></a><span data-ttu-id="3e7ff-141">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-141">Samples</span></span>

<span data-ttu-id="3e7ff-142">用于创建几个常见类型的 HDInsight 群集的代码示例可供使用：[HDInsight Java 示例](https://github.com/Azure-Samples/hdinsight-java-sdk-samples)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-142">Code samples for creating several common types of HDInsight clusters are available: [HDInsight Java Samples](https://github.com/Azure-Samples/hdinsight-java-sdk-samples).</span></span>

#### <a name="example"></a><span data-ttu-id="3e7ff-143">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-143">Example</span></span>

<span data-ttu-id="3e7ff-144">本示例演示如何创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-144">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="3e7ff-145">首先需要创建一个资源组和存储帐户，下面将予以介绍。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-145">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="3e7ff-146">如果已创建资源组和存储帐户，则可以跳过这些步骤。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-146">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="3e7ff-147">创建资源组</span><span class="sxs-lookup"><span data-stu-id="3e7ff-147">Creating a Resource Group</span></span>

<span data-ttu-id="3e7ff-148">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建资源组</span><span class="sxs-lookup"><span data-stu-id="3e7ff-148">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="3e7ff-149">创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="3e7ff-149">Creating a Storage Account</span></span>

<span data-ttu-id="3e7ff-150">可以在 [Azure Cloud Shell](https://shell.azure.com/bash) 中运行以下命令来创建存储帐户</span><span class="sxs-lookup"><span data-stu-id="3e7ff-150">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="3e7ff-151">现在，运行以下命令获取存储帐户的密钥（创建群集时需要用到）：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-151">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="3e7ff-152">以下 Java 代码片段创建包含 2 个头节点和 1 个工作节点的 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-152">The below Java snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="3e7ff-153">按照注释中所述填写空白变量，并根据具体的需要任意更改其他参数。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-153">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

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

### <a name="get-cluster-details"></a><span data-ttu-id="3e7ff-154">获取群集详细信息</span><span class="sxs-lookup"><span data-stu-id="3e7ff-154">Get Cluster Details</span></span>

<span data-ttu-id="3e7ff-155">获取给定群集的属性：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-155">To get properties for a given cluster:</span></span>

```java
client.clusters.getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="3e7ff-156">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-156">Example</span></span>

<span data-ttu-id="3e7ff-157">可以使用 `get` 来确认已成功创建群集。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-157">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```java
ClusterInner cluster = client.clusters().getByResourceGroup("<Resource Group Name>", "<Cluster Name>");
System.out.println(cluster.name()); //Prints the name of the cluster
System.out.println(cluster.id()); //Prints the resource Id of the cluster
```

<span data-ttu-id="3e7ff-158">输出应如下所示：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-158">The output should look like:</span></span>

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a><span data-ttu-id="3e7ff-159">列出群集</span><span class="sxs-lookup"><span data-stu-id="3e7ff-159">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="3e7ff-160">列出订阅下的群集</span><span class="sxs-lookup"><span data-stu-id="3e7ff-160">List Clusters Under The Subscription</span></span>

```java
client.clusters.list();
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="3e7ff-161">按资源组列出群集</span><span class="sxs-lookup"><span data-stu-id="3e7ff-161">List Clusters By Resource Group</span></span>

```java
client.clusters.listByResourceGroup("<Resource Group Name>");
```
> [!NOTE]
> <span data-ttu-id="3e7ff-162">`List()` 和 `ListByResourceGroup()` 都返回 `PagedList<ClusterInner>` 对象。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-162">Both `List()` and `ListByResourceGroup()` return a `PagedList<ClusterInner>` object.</span></span> <span data-ttu-id="3e7ff-163">调用 `loadNext()` 会在该页上返回群集列表，并会将 `ClusterPaged` 对象推到下一页。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-163">Calling `loadNext()` returns a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="3e7ff-164">此操作可以一直重复，直至 `hasNextPage()` 返回 `false`，这表明没有其他页。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-164">This can be repeated until `hasNextPage()` return `false`, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="3e7ff-165">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-165">Example</span></span>

<span data-ttu-id="3e7ff-166">以下示例列显当前订阅的所有群集的属性：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-166">The following example prints the properties of all clusters for the current subscription:</span></span>

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

### <a name="delete-a-cluster"></a><span data-ttu-id="3e7ff-167">删除群集</span><span class="sxs-lookup"><span data-stu-id="3e7ff-167">Delete a Cluster</span></span>

<span data-ttu-id="3e7ff-168">删除群集：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-168">To delete a cluster:</span></span>

```java
client.clusters.delete("<Resource Group Name>", "<Cluster Name>");
```

### <a name="update-cluster-tags"></a><span data-ttu-id="3e7ff-169">更新群集标记</span><span class="sxs-lookup"><span data-stu-id="3e7ff-169">Update Cluster Tags</span></span>

<span data-ttu-id="3e7ff-170">可按如下所示更新给定群集的标记：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-170">You can update the tags of a given cluster like so:</span></span>

```java
client.clusters.update("<Resource Group Name>", "<Cluster Name>", <Map<String,String> of Tags>);
```

### <a name="resize-cluster"></a><span data-ttu-id="3e7ff-171">调整群集大小</span><span class="sxs-lookup"><span data-stu-id="3e7ff-171">Resize Cluster</span></span>

<span data-ttu-id="3e7ff-172">可以通过指定新大小来调整给定群集的工作节点数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-172">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```java
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", <Num of Worker Nodes (int)>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="3e7ff-173">群集监视</span><span class="sxs-lookup"><span data-stu-id="3e7ff-173">Cluster Monitoring</span></span>

<span data-ttu-id="3e7ff-174">使用 HDInsight 管理 SDK 还可以通过 Operations Management Suite (OMS) 来管理群集的监视。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-174">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="3e7ff-175">启用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="3e7ff-175">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="3e7ff-176">若要启用 OMS 监视，必须已有一个 Log Analytics 工作区。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-176">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="3e7ff-177">如果尚未创建工作区，可在此了解创建方法：[在 Azure 门户中创建 Log Analytics 工作区](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-177">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="3e7ff-178">在群集上启用 OMS 监视：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-178">To enable OMS Monitoring on your cluster:</span></span>

```java
client.extensions().enableMonitoring("<Resource Group Name", "<Cluster Name>", new ClusterMonitoringRequest().withWorkspaceId("<Workspace Id>"));
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="3e7ff-179">查看 OMS 监视状态</span><span class="sxs-lookup"><span data-stu-id="3e7ff-179">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="3e7ff-180">获取群集上的 OMS 状态：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-180">To get the status of OMS on your cluster:</span></span>

```java
client.extensions().getMonitoringStatus("<Resource Group Name", "Cluster Name");
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="3e7ff-181">禁用 OMS 监视</span><span class="sxs-lookup"><span data-stu-id="3e7ff-181">Disable OMS Monitoring</span></span>

<span data-ttu-id="3e7ff-182">在群集上禁用 OMS：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-182">To disable OMS on your cluster:</span></span>

```java
client.extensions().disableMonitoring("<Resource Group Name>", "<Cluster Name>");
```

## <a name="script-actions"></a><span data-ttu-id="3e7ff-183">脚本操作</span><span class="sxs-lookup"><span data-stu-id="3e7ff-183">Script Actions</span></span>

<span data-ttu-id="3e7ff-184">HDInsight 提供一个称为“脚本操作”的配置方法，该方法可调用用于自定义群集的自定义脚本。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-184">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="3e7ff-185">有关如何使用脚本操作的详细信息见此处：[使用脚本操作自定义基于 Linux 的 HDInsight 群集](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="3e7ff-185">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="3e7ff-186">执行脚本操作</span><span class="sxs-lookup"><span data-stu-id="3e7ff-186">Execute Script Actions</span></span>

<span data-ttu-id="3e7ff-187">可按如下所示在给定的群集上执行脚本操作：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-187">You can execute script actions on a given cluster like so:</span></span>

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

### <a name="delete-script-action"></a><span data-ttu-id="3e7ff-188">删除脚本操作</span><span class="sxs-lookup"><span data-stu-id="3e7ff-188">Delete Script Action</span></span>

<span data-ttu-id="3e7ff-189">删除给定群集上指定的持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-189">To delete a specified persisted script action on a given cluster:</span></span>

```java
client.scriptActions().delete("<Resource Group Name>", "<Cluster Name>", "<Script Name>");
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="3e7ff-190">列出持久化脚本操作</span><span class="sxs-lookup"><span data-stu-id="3e7ff-190">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="3e7ff-191">`listByCluster()` 返回 `PagedList<RuntimeScriptActionDetailInner>` 对象。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-191">Both `listByCluster()` returns a `PagedList<RuntimeScriptActionDetailInner>` object.</span></span> <span data-ttu-id="3e7ff-192">调用 `currentPage().items()` 会返回 `RuntimeScriptActionDetailInner` 的列表，而 `loadNextPage()` 会进入下一页。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-192">Calling `currentPage().items()` returns a list of `RuntimeScriptActionDetailInner`, and `loadNextPage()` advances to the next page.</span></span> <span data-ttu-id="3e7ff-193">此操作可以一直重复，直至 `hasNextPage()` 返回 `false`，这表明没有其他页。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-193">This can be repeated until `hasNextPage()` returns `false`, indicating that there are no more pages.</span></span>

<span data-ttu-id="3e7ff-194">列出指定群集的所有持久化脚本操作：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-194">To list all persisted script actions for the specified cluster:</span></span>
```java
client.scriptActions().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="3e7ff-195">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-195">Example</span></span>

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

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="3e7ff-196">列出所有脚本的执行历史记录</span><span class="sxs-lookup"><span data-stu-id="3e7ff-196">List All Scripts' Execution History</span></span>

<span data-ttu-id="3e7ff-197">列出指定群集的所有脚本的执行历史记录：</span><span class="sxs-lookup"><span data-stu-id="3e7ff-197">To list all scripts' execution history for the specified cluster:</span></span>

```java
client.scriptExecutionHistorys().listByCluster("<Resource Group Name>", "<Cluster Name>");
```

#### <a name="example"></a><span data-ttu-id="3e7ff-198">示例</span><span class="sxs-lookup"><span data-stu-id="3e7ff-198">Example</span></span>

<span data-ttu-id="3e7ff-199">此示例列显以往所有脚本执行活动的所有详细信息。</span><span class="sxs-lookup"><span data-stu-id="3e7ff-199">This example prints all the details for all past script executions.</span></span>

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
