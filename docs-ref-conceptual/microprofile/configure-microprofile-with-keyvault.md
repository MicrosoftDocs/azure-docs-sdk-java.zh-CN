---
title: 使用 Azure Key Vault 配置 MicroProfile
description: 了解如何使用 Azure Key Vault 将机密注入 MicroProfile Web 服务
services: key-vault
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: fa93788f9b600d963f35ea599a58d309d3a3ee52
ms.sourcegitcommit: 77dc6c03a2e6264df688d91a04fc6b40950779ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43240820"
---
# <a name="configure-microprofile-with-azure-key-vault"></a>使用 Azure Key Vault 配置 MicroProfile

本教程演示如何将 [MicroProfile](http://microprofile.io) 应用程序配置为使用 [MicroProfile Config API](https://microprofile.io/project/eclipse/microprofile-config) 从 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 检索机密，以便与 Azure Key Vault 建立直接连接。 使用 MicroProfile Config API，开发人员可以利用标准 API 来检索配置数据，并将其注入微服务。

在深入探讨之前，让我们快速了解一下 Azure Key Vault 与 MicroProfile Config API 的哪种组合可让我们写入自己的代码。 下面是某个类中某个字段的代码片段，其带有 `@Inject` 和 `@ConfigProperty` 注释。 注释中指定的 `name` 是要在 Azure Key Vault 中查找的属性名称，`defaultValue` 是未发现该密钥时要设置的值。 最终结果是在运行时自动将 Azure Key Vault 中存储的值或默认值注入到该字段，这可以简化开发人员的工作，因为他们不再需要在构造函数和 setter 方法中的不同位置传递值，只需让 MicroProfile 来处理此工作。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

还可以根据需要，直接访问 MicroProfile 配置来请求机密，例如：

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

本示例利用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 创建一个可在计算机本地运行的微型 Java war 文件。 其中并未演示如何 Docker 容器化代码或将代码推送到 Azure，但本教程末尾的“链接”部分提供了介绍此类操作的其他有用教程的链接。

本示例利用免费开源库来为 Azure Key Vault 创建配置源（使用 MicroProfile Config API）。 可在[项目 GitHub 页](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)上详细了解此库和查看代码。 使用此库时，本教程中的代码只需专注于库的配置，然后将密钥注入代码，我们不需要编写任何特定于 Azure 的代码。

下面是在本地计算机上运行此代码所要执行的步骤，从创建 Azure Key Vault 资源开始。

## <a name="creating-an-azure-key-vault-resource"></a>创建 Azure Key Vault 资源

我们将使用 Azure CLI 创建 Azure Key Vault 资源并在其中填充一个机密。

1. 首先创建 Azure 服务主体。 这会提供访问 Key Vault 时所需的客户端 ID 和密钥：

```bash
az login
az account set --subscription <subscription_id>

az ad sp create-for-rbac --name <service_principal_name>
```

假设我们在上一步骤中使用了 `microprofile-keyvault-service-principal` 作为服务主体名称。 为此，来自 Azure 的响应将采用以下格式（略有删改）：

```json
{
  "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
  "displayName": "microprofile-keyvault-service-principal",
  "name": "http://microprofile-keyvault-service-principal",
  "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
  "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
}
```

此处需特别注意 `appID` 和 `password` 值 - 稍后我们要将这些值分别用作 `client ID` 和 `key`。

创建服务主体后，可根据需要创建一个资源组（如果已有一个可用的资源组，则可以跳过此步骤）。 请注意，若要获取资源组位置的列表，可以调用 `az account list-locations`，并使用该列表中的 `name` 值来指定创建资源组的位置。

```bash
# For this tutorial, the author chose to use `westus`
# and `jg-test` for the resource group name.
az group create -l <resource_group_location> -n <resource_group_name>
```

现在创建 Azure Key Vault 资源。 请注意，稍后要使用 Key Vault 名称来引用该 Key Vault，因此请选择易记的名称。

```bash
az keyvault create --name <your_keyvault_name>            \
                   --resource-group <your_resource_group> \
                   --location <location>                  \
                   --enabled-for-deployment true          \
                   --enabled-for-disk-encryption true     \
                   --enabled-for-template-deployment true \
                   --sku standard
```

我们还需要向前面创建的服务主体授予适当的权限，使它可以访问 Key Vault 机密。 请注意，appID 值是前面创建服务主体时指定的 `appId` 值（即 `5292398e-XXXX-40ce-XXXX-d49fXXXX9e79` - 但应使用终端输出中的值）。

```bash
az keyvault set-policy --name <your_keyvault_name>   \
                       --secret-permission get list  \
                       --spn <your_sp_appId_created_in_step1>
```

现在，可将机密推送到 Key Vault。 让我们使用密钥名称 `demo-key`，并将密钥值设置为 `demo-value`：

```bash
az keyvault secret set --name demo-key      \
                       --value demo-value   \
                       --vault-name <your_keyvault_name>  
```

就这么简单！ 现在，Azure 中运行了包含一个机密的 Key Vault。 接下来可以克隆此存储库，并将其配置为在应用中使用此资源。

## <a name="getting-up-and-running-locally"></a>在本地启动和运行

本示例基于 GitHub 上提供的示例应用程序，因此我们将克隆该应用程序，然后逐步执行代码。 遵循以下步骤将克隆的代码提取到计算机：

1. `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

1. `cd microprofile-configsource-keyvault`

1. 导航到 `src/main/resources/META-INF/microprofile-config.properties`，使用上述详细信息更改 microprofile-config.properties 文件中的属性。

1. 尝试使用 `mvn clean package payara-micro:start` 运行服务器

1. 尝试在 Web 浏览器中访问 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) - 应会看到一个简单的响应，其中演示了正在从 Azure Key Vault 读取值。

## <a name="summary"></a>摘要

本示例应用程序将 MicroProfile Config API、Azure Key Vault 和免费开源 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 库融合在一起，使我们能够轻松将配置数据和机密注入到 MicroProfile Web 服务。
