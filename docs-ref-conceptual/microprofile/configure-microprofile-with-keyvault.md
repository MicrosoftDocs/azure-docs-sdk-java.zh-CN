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
ms.openlocfilehash: c405711813176823f2ddee6b4002f75c2b25fdb5
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533614"
---
# <a name="configure-microprofile-by-using-azure-key-vault"></a>使用 Azure Key Vault 配置 MicroProfile

本文演示如何将 [MicroProfile](http://microprofile.io) 应用程序配置为从 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 检索机密。 在此过程中，请使用 [MicroProfile 配置 API](https://microprofile.io/project/eclipse/microprofile-config) 与 Azure Key Vault 建立直接连接。 作为开发人员，你可以通过 MicroProfile 配置 API 利用标准 API 来检索配置数据，并将其注入微服务。

在开始之前，请快速了解一下 Azure Key Vault 与 MicroProfile 配置 API 的哪种组合可让你写入自己的代码。 下面是某个类中某个字段的代码片段，带有 `@Inject` 和 `@ConfigProperty` 注释。 注释中指定的 *name* 是要在密钥保管库中查找的属性名称，*defaultValue* 是未发现该密钥时要设置的值。 结果是在运行时自动将密钥保管库中存储的值或默认值注入到该字段。 此操作可以简化你的工作，因为你不再需要在构造函数和 setter 方法中的不同位置传递值， 只需让 MicroProfile 来处理该任务。

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

还可以根据需要，直接访问 MicroProfile 配置来请求机密。 例如：

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

本示例代码使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 创建一个可在计算机本地运行的微型 Java Web 应用程序要求 (WAR) 文件。 它不演示如何 Docker 化代码或将代码推送到 Azure，但本文末尾的部分提供了介绍此类操作的其他有用教程的链接。

此示例使用免费开源库在密钥保管库中创建配置源（使用 MicroProfile 配置 API）。 若要详细了解此库和查看代码，请参阅[项目 GitHub 页](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)。 如果你使用此库，则本教程中的代码只需专注于库的配置，然后将密钥注入代码即可。 不需要编写任何特定于 Azure 的代码。

若要在本地计算机上运行此代码，请从创建密钥保管库资源开始，按后续部分的说明操作。

## <a name="create-a-key-vault-resource"></a>创建密钥保管库资源

在此部分，请使用 Azure CLI 创建密钥保管库资源并在其中填充一个机密。

1. 创建 Azure 服务主体。 此步骤提供访问密钥保管库所需的客户端 ID 和密钥：

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    让我们使用 *microprofile-keyvault-service-principal* 来替换上一步骤中的 *\<service_principal_name>* 。 来自 Azure 的响应将如下所示：

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    在这里，需要特别注意的是 *appId* 和 *password* 值。 在本文中，稍后需将其用作 *client ID* 和 *key*。

1. （可选）创建服务主体后，即可创建资源组。 如果已有要使用的资源组，可跳过此步骤。 若要获取资源组位置的列表，可以调用 `az account list-locations`，并使用该列表中的 *name* 值来指定创建资源组的位置。

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. 创建密钥保管库资源。 稍后将使用密钥保管库名称来引用密钥保管库，因此务必选择易记的名称。

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. 请向前面创建的服务主体授予适当的权限，使它可以访问密钥保管库机密。 以下代码中的 appId 值是步骤 1 中的 *appId* 值。在步骤 1 中，已创建了服务主体。 也就是说，步骤 1 中的 *appId* 为 *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*，但你应该使用自己的终端输出中的值。

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. 现在可以将机密推送到密钥保管库中。 请使用密钥名称 *demo-key*，并将密钥的值设置为 *demo-value*：

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

就这么简单！ 现在，Azure 中运行了包含一个机密的密钥保管库。 接下来可以克隆此存储库，并将其配置为在应用中使用此资源。

## <a name="get-up-and-running-locally"></a>在本地启动和运行

本示例基于 GitHub 上提供的示例应用程序，因此需克隆该应用程序，然后逐步执行代码。 

1. 输入以下命令，将代码克隆到计算机中：

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. 转到 *src/main/resources/META-INF/microprofile-config.properties*，然后使用上述步骤中的值更改 *microprofile-config.properties* 文件中的属性。

1. 尝试使用 `mvn clean package payara-micro:start` 运行服务器。

1. 尝试在 Web 浏览器中访问 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config)。 应该会看到一个简单的响应，其中展示了正在从密钥保管库读取的值。

## <a name="summary"></a>摘要

本示例应用程序将 MicroProfile 配置 API、Azure Key Vault 和免费开源 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 库组合在一起，这样就能轻松地将配置数据和机密注入 MicroProfile Web 服务。
