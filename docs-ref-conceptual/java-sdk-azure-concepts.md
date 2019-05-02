---
title: 面向 Java 开发人员的 Azure 管理库指南
description: 有关使用用于 Java 的 Java 管理库管理 Azure 中的云资源的模式和概念。
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
ms.assetid: f452468b-7aae-4944-abad-0b1aaf19170d
ms.openlocfilehash: 4e7d5ea8b796733ab9386ea5ee37f935a4347a18
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593204"
---
# <a name="patterns-and-best-practices-for-development-with-the-azure-libraries-for-java"></a>有关使用用于 Java 的 Azure 库进行开发的模式和最佳做法 

本文列出有关在项目中使用用于 Java 的 Azure 库时的一系列模式和最佳做法。 使用这些模式和指导原则进行开发可减少要维护的代码量，最可以在管理库的将来更新中更轻松地添加或配置其他资源。

## <a name="build-resources-through-a-fluent-interface"></a>通过 Fluent 界面生成资源

Fluent 界面是使用方法链（可正确配置对象的属性）创建对象的模式。 例如，创建新的 Azure 存储帐户

```java
StorageAccount storage = azure.storageAccounts().define(storageAccountName)
    .withRegion(region)
    .withNewResourceGroup(resourceGroup)
    .create();
```

在操作方法链的过程中，IDE 会在 Fluent 对话中建议下一个要调用的方法。   

![Fluent 链中 IntelliJ GIF 命令完成的 GIF](media/intelliJFluent.gif)

只要 IDE 建议的方法对于正在定义的 Azure 资源合理，就可以将这些方法加入链中。 如果链中缺少所需的方法，IDE 会在错误消息中突出显示该方法。

## <a name="resource-collections"></a>资源集合

管理库通过顶级 `com.microsoft.azure.management.Azure` 对象提供单一入口点用于创建和更新资源。 使用 `Azure` 对象中定义的资源集合方法来选择要处理的资源类型。 例如，对于 SQL 数据库：

```java
SqlServer sqlServer = azure.sqlServers().define(sqlServerName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName)
    .withAdministratorLogin(administratorLogin)
    .withAdministratorPassword(administratorPassword)
    .create();
```

## <a name="lists-and-iterations"></a>列表和迭代

每个资源集合提供一个 `list()` 方法来返回当前订阅中该资源的每个实例。 例如，`azure.sqlServers().list()` 返回订阅中的所有 SQL 数据库。

使用 `listByResourceGroup(String groupname)` 方法可将返回列表的范围限定为特定的 [Azure 资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。  

可以像操作普通的 `List<T>` 一样搜索和循环访问返回的 `PagedList<T>` 集合：

```java
PagedList<VirtualMachine> vms = azure.virtualMachines().list();
for (VirtualMachine vm : vms) {
    System.out.println("Found virtual machine with ID " + vm.id());
}
```   

## <a name="collections-returned-from-queries"></a>从查询返回的集合

管理库根据结果的结构从查询返回特定的集合类型。

- `List<T>`：易于搜索和循环访问的无序数据。
- `Map<T>`：映射是具有唯一键的键/值对，但不一定是唯一值。 映射的示例包括应用服务 Web 应用的应用设置。
- `Set<T>`：集具有唯一的键和值。 集的示例包括附加到虚拟机的网络，它们具有唯一的标识符（键）和唯一的网络配置（值）。

## <a name="actionable-verbs"></a>可操作的谓词

名称中包含谓词的方法在 Azure 中立即执行。 这些方法以同步方式工作，在完成之前会阻止当前线程中的执行。 

| Verb   |  示例用法 |
|--------|---------------|
| create | `azure.virtualMachines().create(listOfVMCreatables)` |
| apply  | `virtualMachineScaleSet.update().withCapacity(6).apply()` |
| delete | `azure.disks().deleteById(id)` | 
| list   | `azure.sqlServers().list()` | 
| get    | `VirtualMachine vm  = azure.virtualMachines().getByResourceGroup(group, vmName)` |

>[!NOTE]
> `define()` 和 `update()` 是谓词，但除非后接 `create()` 或 `apply()`，否则不会阻止执行。
 
其中某些方法存在使用 [Reactive Extensions](https://github.com/ReactiveX/RxJava) 获得的异步版本，异步版本带有 `Async` 后缀。 

某些对象的其他方法会更改 Azure 中资源的状态。 例如，`VirtualMachine` 上的 `restart()`。

```java
VirtualMachine vmToRestart = azure.getVirtualMachines().getById(id);
vmToRestart.restart();
```
这些方法并不一定总有异步版本，在完成之前，会阻止其线程中的执行。

<a name="Creatables"></a>

## <a name="lazy-resource-creation"></a>延缓资源创建

如果某个新资源依赖于尚不存在的另一个资源，创建 Azure 资源时会出现难题。 例如，在创建新虚拟机时保留公共 IP 地址和设置磁盘就会出现这种情况。 我们不需要确认是否保留了地址或创建了磁盘，而只需确保在创建虚拟机时，虚拟机上存在这些资源。

通过 `Creatable<T>` 对象可以定义要在代码中使用的 Azure 资源，而无需等待在订阅中创建这些资源。 管理库会将 `Creatable<T>` 对象的创建推迟到需要这些对象时。

通过 `define()` 谓词为 Azure 资源生成 `Creatable<T>` 对象：

```java
Creatable<PublicIPAddress> publicIPAddressCreatable = azure.publicIPAddresses().define(publicIPAddressName)
    .withRegion(Region.US_EAST)
    .withNewResourceGroup(rgName);
```

执行此代码时，此示例中的 `Creatable<PublicIPAddress>` 定义的 Azure 资源尚不在订阅中存在。  使用 `publicIPAddressCreatable` 对象可创建具有此 IP 地址的其他 Azure 资源。 

```java
Creatable<VirtualMachine> vmCreatable = azure.virtualMachines().define("creatableVM")
    .withNewPrimaryPublicIPAddress(publicIPAddressCreatable)
```

在 Azure 中使用 `create()` 生成通过该对象定义的任何资源时，会在订阅中生成 `Creatable<T>` 资源。 沿用前面的 IP 地址和虚拟机示例：

```java
CreatedResources<VirtualMachine> virtualMachine = azure.virtualMachines().create(vmCreatable);
```

将 `Creatable<T>` 传递给 `create()` 调用会返回 `CreatedResources` 对象，而不是单个资源对象。  使用 `CreatedResources<T>` 对象可以访问 `create()` 调用创建的所有资源，而不仅仅是调用中的类型化资源。 访问 Azure 中针对上述示例中创建的虚拟机所创建的公共 IP 地址：

```java
PublicIPAddress pip = (PublicIPAddress) virtualMachine.createdRelatedResource(publicIPAddressCreatable.key());
```    

## <a name="exception-handling"></a>异常处理

管理库的异常类扩展了 `com.microsoft.rest.RestException`。 在相关的 `try` 语句后面使用 `catch (RestException exception)` 块捕获管理库生成的异常。

## <a name="logs-and-trace"></a>日志和跟踪

使用 `withLogLevel()` 来配置生成入口点 `Azure` 对象时管理库要提供的日志记录量。 存在以下跟踪级别：

| 跟踪级别 | 日志记录已启用 
| ------------ | ---------------
| com.microsoft.rest.LogLevel.NONE | 无输出
| com.microsoft.rest.LogLevel.BASIC | 记录基础 REST 调用的 URL、响应代码和时间
| com.microsoft.rest.LogLevel.BODY | BASIC 中的所有内容，加上 REST 调用的请求和响应正文
| com.microsoft.rest.LogLevel.HEADERS | BASIC 中的所有内容，加上请求和响应标头 REST 调用
| com.microsoft.rest.LogLevel.BODY_AND_HEADERS | BODY 和 HEADERS 日志级别中的所有内容

如果需要将输出记录到 [Log4J 2](https://logging.apache.org/log4j/2.x/) 等记录框架，请绑定 [SLF4J 日志记录实现](https://www.slf4j.org/manual.html)。