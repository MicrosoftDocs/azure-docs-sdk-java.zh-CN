---
title: "用于 Java 的 Azure 管理库用法概念和模式"
description: 
keywords: "Azure, Java, SDK, API, Maven, Gradle, 身份验证, active directory, 服务主体"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: f452468b-7aae-4944-abad-0b1aaf19170d
ms.openlocfilehash: 052c4de1e8f9ff0ece5f36d1c3514bad8c04cfec
ms.sourcegitcommit: 1500f341a96d9da461c288abf4baf79f494ae662
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="azure-management-library-concepts"></a><span data-ttu-id="cbac7-103">Azure 管理库的概念</span><span class="sxs-lookup"><span data-stu-id="cbac7-103">Azure management library concepts</span></span>

## <a name="build-resources-through-a-fluent-interface"></a><span data-ttu-id="cbac7-104">通过 Fluent 界面生成资源</span><span class="sxs-lookup"><span data-stu-id="cbac7-104">Build resources through a fluent interface</span></span>

<span data-ttu-id="cbac7-105">Fluent 界面是使用方法链（可正确配置对象的属性）创建对象的模式。</span><span class="sxs-lookup"><span data-stu-id="cbac7-105">A fluent interface is a pattern that creates objects using a method chain that correctly configures the object's attributes.</span></span> <span data-ttu-id="cbac7-106">例如，创建新的 Azure 存储帐户</span><span class="sxs-lookup"><span data-stu-id="cbac7-106">For example, to create a new Azure Storage account</span></span>

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

<span data-ttu-id="cbac7-107">在操作方法链的过程中，IDE 会在 Fluent 对话中建议下一个要调用的方法。</span><span class="sxs-lookup"><span data-stu-id="cbac7-107">As you go through the method chain, your IDE suggests the next method to call in the fluent conversation.</span></span>   

![通过 Fluent 链进行的 IntelliJ GIF 命令完成](media/intelliJFluent.gif)

<span data-ttu-id="cbac7-109">只要 IDE 建议的方法对于所定义的 Azure 资源有用，就会链接这些方法。</span><span class="sxs-lookup"><span data-stu-id="cbac7-109">Chain the methods suggested by the IDE as long as they make sense for the Azure resource being defined.</span></span> <span data-ttu-id="cbac7-110">如果链中缺少所需的方法，IDE 会在错误消息中突出显示该方法。</span><span class="sxs-lookup"><span data-stu-id="cbac7-110">If you are missing a required method in the chain your IDE will highlight it with an error.</span></span>

## <a name="resource-collections"></a><span data-ttu-id="cbac7-111">资源集合</span><span class="sxs-lookup"><span data-stu-id="cbac7-111">Resource collections</span></span>

<span data-ttu-id="cbac7-112">管理库通过顶级 `com.microsoft.azure.management.Azure` 对象提供单一入口点用于创建和更新资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-112">The management library has a single point of entry through the top-level `com.microsoft.azure.management.Azure` object to create and update resources.</span></span> <span data-ttu-id="cbac7-113">使用 `Azure` 对象中定义的资源集合方法来选择要处理的资源类型。</span><span class="sxs-lookup"><span data-stu-id="cbac7-113">Select which type of resources to work with using the resource collection methods defined in the `Azure` object.</span></span> <span data-ttu-id="cbac7-114">例如，对于 SQL 数据库：</span><span class="sxs-lookup"><span data-stu-id="cbac7-114">For example, SQL Database:</span></span>

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a><span data-ttu-id="cbac7-115">列表和迭代</span><span class="sxs-lookup"><span data-stu-id="cbac7-115">Lists and iterations</span></span>

<span data-ttu-id="cbac7-116">每个资源集合提供一个 `list()` 方法来返回当前订阅中该资源的每个实例。</span><span class="sxs-lookup"><span data-stu-id="cbac7-116">Each resource collection has a `list()` method to return every instance of that resource in your current subscription.</span></span> <span data-ttu-id="cbac7-117">例如，`azure.sqlServers().list()` 返回订阅中的所有 SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="cbac7-117">For example, `azure.sqlServers().list()` returns all SQL databases in the subscription.</span></span>

<span data-ttu-id="cbac7-118">使用 `listByResourceGroup(String groupname)` 方法可将返回列表的范围限定为特定的 [Azure 资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。</span><span class="sxs-lookup"><span data-stu-id="cbac7-118">Use the `listByResourceGroup(String groupname)` method to scope the returned List to a specific [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span>  

<span data-ttu-id="cbac7-119">可以像执行普通的 `List<T>` 一样搜索和循环访问返回的 `PagedList<T>` 集合：</span><span class="sxs-lookup"><span data-stu-id="cbac7-119">Search and iterate over the returned `PagedList<T>` collection just as you would a normal `List<T>`:</span></span>

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a><span data-ttu-id="cbac7-120">从查询返回的集合</span><span class="sxs-lookup"><span data-stu-id="cbac7-120">Collections returned from queries</span></span>

<span data-ttu-id="cbac7-121">管理库根据结果的结构从查询返回特定的集合类型。</span><span class="sxs-lookup"><span data-stu-id="cbac7-121">The management libraries return specific collection types from queries based on the structure of the results.</span></span>

- <span data-ttu-id="cbac7-122">`List<T>`：易于搜索和循环访问的无序数据。</span><span class="sxs-lookup"><span data-stu-id="cbac7-122">`List<T>`: Unordered data that is easy to search and iterate over.</span></span>
- <span data-ttu-id="cbac7-123">`Map<T>`：映射是具有唯一键的键/值对，但不一定是唯一值。</span><span class="sxs-lookup"><span data-stu-id="cbac7-123">`Map<T>`: Maps are key/value pairs with unique keys, but not necessarily unique values.</span></span> <span data-ttu-id="cbac7-124">映射的示例包括应用服务 Web 应用的应用设置。</span><span class="sxs-lookup"><span data-stu-id="cbac7-124">An example of a Map would be app settings for an App Service Web App.</span></span>
- <span data-ttu-id="cbac7-125">`Set<T>`：集具有唯一的键和值。</span><span class="sxs-lookup"><span data-stu-id="cbac7-125">`Set<T>`: Sets have unique keys and values.</span></span> <span data-ttu-id="cbac7-126">集的示例包括附加到虚拟机的网络，它们具有唯一的标识符（键）和唯一的网络配置（值）。</span><span class="sxs-lookup"><span data-stu-id="cbac7-126">An example of a Set would be networks attached to a virtual machine, which would have both an unique identifier (the key) and a unique network configuration (the value).</span></span>

## <a name="actionable-verbs"></a><span data-ttu-id="cbac7-127">可操作的谓词</span><span class="sxs-lookup"><span data-stu-id="cbac7-127">Actionable verbs</span></span>

<span data-ttu-id="cbac7-128">名称中包含谓词的方法在 Azure 中立即执行。</span><span class="sxs-lookup"><span data-stu-id="cbac7-128">Methods with verbs in their names take immediate action in Azure.</span></span> <span data-ttu-id="cbac7-129">这些方法以同步方式工作，在完成之前会阻塞当前线程中的执行。</span><span class="sxs-lookup"><span data-stu-id="cbac7-129">These methods work synchronously and block execution in the current thread until they complete.</span></span> 

| <span data-ttu-id="cbac7-130">Verb</span><span class="sxs-lookup"><span data-stu-id="cbac7-130">Verb</span></span>   |  <span data-ttu-id="cbac7-131">示例用法</span><span class="sxs-lookup"><span data-stu-id="cbac7-131">Sample Usage</span></span> |
|--------|---------------|
| <span data-ttu-id="cbac7-132">create</span><span class="sxs-lookup"><span data-stu-id="cbac7-132">create</span></span> | `azure.virtualMachines().create(listOfVMCreatables)` |
| <span data-ttu-id="cbac7-133">apply</span><span class="sxs-lookup"><span data-stu-id="cbac7-133">apply</span></span>  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| <span data-ttu-id="cbac7-134">删除</span><span class="sxs-lookup"><span data-stu-id="cbac7-134">delete</span></span> | `azure.disks().deleteById(id)` | 
| <span data-ttu-id="cbac7-135">list</span><span class="sxs-lookup"><span data-stu-id="cbac7-135">list</span></span>   | `azure.sqlServers().list()` | 
| <span data-ttu-id="cbac7-136">get</span><span class="sxs-lookup"><span data-stu-id="cbac7-136">get</span></span>    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> <span data-ttu-id="cbac7-137">`define()` 和 `update()` 是谓词，但除非后接 `create()` 或 `apply()`，否则不会阻塞。</span><span class="sxs-lookup"><span data-stu-id="cbac7-137">`define()` and `update()` are verbs but do not block unless followed by a `create()` or `apply()`.</span></span>
 
<span data-ttu-id="cbac7-138">其中某些方法的异步版本使用[反应式扩展](https://github.com/ReactiveX/RxJava)连同 `Async` 后缀一起存在。</span><span class="sxs-lookup"><span data-stu-id="cbac7-138">Asynchronous versions of some of these  methods exist with the `Async` suffix using [Reactive extensions](https://github.com/ReactiveX/RxJava).</span></span> 

<span data-ttu-id="cbac7-139">某些对象的其他方法会更改 Azure 中资源的状态。</span><span class="sxs-lookup"><span data-stu-id="cbac7-139">Some objects have other methods with that change the state of the resource in Azure.</span></span> <span data-ttu-id="cbac7-140">例如，`VirtualMachine` 上的 `restart()`。</span><span class="sxs-lookup"><span data-stu-id="cbac7-140">For example, `restart()` on a `VirtualMachine`.</span></span>

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
<span data-ttu-id="cbac7-141">这些方法并一定总有异步版本，在完成之前，会阻塞其线程中的执行。</span><span class="sxs-lookup"><span data-stu-id="cbac7-141">These methods do not always have asynchronous versions and will block execution on their thread until they complete.</span></span>

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a><span data-ttu-id="cbac7-142">延缓资源创建</span><span class="sxs-lookup"><span data-stu-id="cbac7-142">Lazy resource creation</span></span>

<span data-ttu-id="cbac7-143">如果某个新资源依赖于尚不存在的另一个资源，创建 Azure 资源时会出现难题。</span><span class="sxs-lookup"><span data-stu-id="cbac7-143">A challenge when creating Azure resources arises when a new resource depends on another resource that doesn't yet exist.</span></span> <span data-ttu-id="cbac7-144">例如，在创建新虚拟机时保留公共 IP 地址和设置磁盘就会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="cbac7-144">An example of this scenario is reserving a public IP address and setting up a disk when creating a new virtual machine.</span></span> <span data-ttu-id="cbac7-145">我们不需要确认是否保留了地址或创建了磁盘，而只需确保在创建虚拟机时，虚拟机上存在这些资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-145">You don't want to verify reserving the address or the creating the disk, you just want to ensure the virtual machine has those resources when it is created.</span></span>

<span data-ttu-id="cbac7-146">通过 `Creatable<T>` 对象可以定义要在代码中使用的 Azure 资源，而无需等待在订阅中创建这些资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-146">`Creatable<T>` objects let you define Azure resources for use in your code without waiting around for them to be created in your subscription.</span></span> <span data-ttu-id="cbac7-147">管理库会将 `Creatable<T>` 对象的创建推迟到需要这些对象时。</span><span class="sxs-lookup"><span data-stu-id="cbac7-147">The management libraries defer creating  `Creatable<T>` objects until they are needed.</span></span>

<span data-ttu-id="cbac7-148">通过 `define()` 谓词为 Azure 资源生成 `Creatable<T>` 对象：</span><span class="sxs-lookup"><span data-stu-id="cbac7-148">Generate `Creatable<T>` objects for Azure resources through the `define()` verb:</span></span>

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

<span data-ttu-id="cbac7-149">执行此代码时，此示例中的 `Creatable<PublicIPAddress>` 定义的 Azure 资源尚不在订阅中存在。</span><span class="sxs-lookup"><span data-stu-id="cbac7-149">The Azure resource defined by the `Creatable<PublicIPAddress>` in this example does not yet exist in your subscription when this code executes.</span></span>  <span data-ttu-id="cbac7-150">使用 `publicIPAddressCreatable` 对象可创建具有此 IP 地址的其他 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-150">Use the `publicIPAddressCreatable` object to create other Azure resources with this IP address.</span></span> 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

<span data-ttu-id="cbac7-151">在 Azure 中使用 `create()` 生成通过该对象定义的任何资源时，会在订阅中生成 `Creatable<T>` 资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-151">The `Creatable<T>` resources are generated in your subscription when any resource that is defined using the object is built in Azure using `create()`.</span></span> <span data-ttu-id="cbac7-152">沿用前面的 IP 地址和虚拟机示例：</span><span class="sxs-lookup"><span data-stu-id="cbac7-152">Continuing the IP address and virtual machine example:</span></span>

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

<span data-ttu-id="cbac7-153">将 `Creatable<T>` 传递给 `create()` 调用会返回 `CreatedResources` 对象，而不是单个资源对象。</span><span class="sxs-lookup"><span data-stu-id="cbac7-153">Passing the `Creatable<T>` to `create()` calls returns a `CreatedResources` object instead of a single resource object.</span></span>  <span data-ttu-id="cbac7-154">使用 `CreatedResources<T>` 对象可以访问 `create()` 调用创建的所有资源，而不仅仅是调用中的类型化资源。</span><span class="sxs-lookup"><span data-stu-id="cbac7-154">The `CreatedResources<T>` object lets you access all resources created by the `create()` call, not just the typed resource in the call.</span></span> <span data-ttu-id="cbac7-155">访问 Azure 中针对上述示例中创建的虚拟机所创建的公共 IP 地址：</span><span class="sxs-lookup"><span data-stu-id="cbac7-155">To access the public IP address created in Azure for the virtual machine created in the above example:</span></span>

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a><span data-ttu-id="cbac7-156">异常处理</span><span class="sxs-lookup"><span data-stu-id="cbac7-156">Exception handling</span></span>

<span data-ttu-id="cbac7-157">管理库的异常类扩展了 `com.microsoft.rest.RestException`。</span><span class="sxs-lookup"><span data-stu-id="cbac7-157">The management libraries' Exception classes extend `com.microsoft.rest.RestException`.</span></span> <span data-ttu-id="cbac7-158">在相关的 `try` 语句后面使用 `catch (RestException exception)` 块捕获管理库生成的异常。</span><span class="sxs-lookup"><span data-stu-id="cbac7-158">Catch exceptions generated by the management libraries with a `catch (RestException exception)` block after the relevant `try` statement.</span></span>

## <a name="logs-and-trace"></a><span data-ttu-id="cbac7-159">日志和跟踪</span><span class="sxs-lookup"><span data-stu-id="cbac7-159">Logs and trace</span></span>

<span data-ttu-id="cbac7-160">使用 `withLogLevel()` 来配置生成入口点 `Azure` 对象时管理库要提供的日志记录量。</span><span class="sxs-lookup"><span data-stu-id="cbac7-160">Configure the amount of logging from the management library when you build the entry point `Azure` object using `withLogLevel()`.</span></span> <span data-ttu-id="cbac7-161">存在以下跟踪级别：</span><span class="sxs-lookup"><span data-stu-id="cbac7-161">The following trace levels exist:</span></span>

| <span data-ttu-id="cbac7-162">跟踪级别</span><span class="sxs-lookup"><span data-stu-id="cbac7-162">Trace level</span></span> | <span data-ttu-id="cbac7-163">日志记录已启用</span><span class="sxs-lookup"><span data-stu-id="cbac7-163">Logging enabled</span></span> 
| ------------ | ---------------
| <span data-ttu-id="cbac7-164">com.microsoft.rest.LogLevel.NONE</span><span class="sxs-lookup"><span data-stu-id="cbac7-164">com.microsoft.rest.LogLevel.NONE</span></span> | <span data-ttu-id="cbac7-165">无输出</span><span class="sxs-lookup"><span data-stu-id="cbac7-165">No output</span></span>
| <span data-ttu-id="cbac7-166">com.microsoft.rest.LogLevel.BASIC</span><span class="sxs-lookup"><span data-stu-id="cbac7-166">com.microsoft.rest.LogLevel.BASIC</span></span> | <span data-ttu-id="cbac7-167">记录基础 REST 调用的 URL、响应代码和时间</span><span class="sxs-lookup"><span data-stu-id="cbac7-167">Logs the URLs to underlying REST calls, response codes and times</span></span>
| <span data-ttu-id="cbac7-168">com.microsoft.rest.LogLevel.BODY</span><span class="sxs-lookup"><span data-stu-id="cbac7-168">com.microsoft.rest.LogLevel.BODY</span></span> | <span data-ttu-id="cbac7-169">BASIC 中的所有内容，加上 REST 调用的请求和响应正文</span><span class="sxs-lookup"><span data-stu-id="cbac7-169">Everything in BASIC plus request and response bodies for the REST calls</span></span>
| <span data-ttu-id="cbac7-170">com.microsoft.rest.LogLevel.HEADERS</span><span class="sxs-lookup"><span data-stu-id="cbac7-170">com.microsoft.rest.LogLevel.HEADERS</span></span> | <span data-ttu-id="cbac7-171">BASIC 中的所有内容，加上请求和响应标头 REST 调用</span><span class="sxs-lookup"><span data-stu-id="cbac7-171">Everything in BASIC plus the request and response headers REST calls</span></span>
| <span data-ttu-id="cbac7-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span><span class="sxs-lookup"><span data-stu-id="cbac7-172">com.microsoft.rest.LogLevel.BODY_AND_HEADERS</span></span> | <span data-ttu-id="cbac7-173">BODY 和 HEADERS 日志级别中的所有内容</span><span class="sxs-lookup"><span data-stu-id="cbac7-173">Everything in both BODY and HEADERS log level</span></span>

<span data-ttu-id="cbac7-174">如果需要将输出记录到 [Log4J 2](https://logging.apache.org/log4j/2.x/) 等记录框架，请绑定 [SLF4J 日志记录实现](https://www.slf4j.org/manual.html)。</span><span class="sxs-lookup"><span data-stu-id="cbac7-174">Bind a [SLF4J logging implementation](https://www.slf4j.org/manual.html) if you need to log output to a logging framework like [Log4J 2](https://logging.apache.org/log4j/2.x/).</span></span>