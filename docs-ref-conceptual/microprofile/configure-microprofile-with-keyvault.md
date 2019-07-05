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
# <a name="configure-microprofile-by-using-azure-key-vault"></a><span data-ttu-id="b4a03-103">使用 Azure Key Vault 配置 MicroProfile</span><span class="sxs-lookup"><span data-stu-id="b4a03-103">Configure MicroProfile by using Azure Key Vault</span></span>

<span data-ttu-id="b4a03-104">本文演示如何将 [MicroProfile](http://microprofile.io) 应用程序配置为从 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 检索机密。</span><span class="sxs-lookup"><span data-stu-id="b4a03-104">This article demonstrates how to configure a [MicroProfile](http://microprofile.io) application to retrieve secrets from an [Azure key vault](https://azure.microsoft.com/services/key-vault/).</span></span> <span data-ttu-id="b4a03-105">在此过程中，请使用 [MicroProfile 配置 API](https://microprofile.io/project/eclipse/microprofile-config) 与 Azure Key Vault 建立直接连接。</span><span class="sxs-lookup"><span data-stu-id="b4a03-105">In this process, you use the [MicroProfile Config APIs](https://microprofile.io/project/eclipse/microprofile-config) to create a direct connection to an Azure key vault.</span></span> <span data-ttu-id="b4a03-106">作为开发人员，你可以通过 MicroProfile 配置 API 利用标准 API 来检索配置数据，并将其注入微服务。</span><span class="sxs-lookup"><span data-stu-id="b4a03-106">As a developer, by using the MicroProfile Config APIs, you benefit from a standard API for retrieving and injecting configuration data into their microservices.</span></span>

<span data-ttu-id="b4a03-107">在开始之前，请快速了解一下 Azure Key Vault 与 MicroProfile 配置 API 的哪种组合可让你写入自己的代码。</span><span class="sxs-lookup"><span data-stu-id="b4a03-107">Before you start, take a quick look at what a combination of Azure Key Vault and the MicroProfile Config API enables you to write in your code.</span></span> <span data-ttu-id="b4a03-108">下面是某个类中某个字段的代码片段，带有 `@Inject` 和 `@ConfigProperty` 注释。</span><span class="sxs-lookup"><span data-stu-id="b4a03-108">The following code snippet is of a field in a class that's annotated with `@Inject` and `@ConfigProperty`.</span></span> <span data-ttu-id="b4a03-109">注释中指定的 *name* 是要在密钥保管库中查找的属性名称，*defaultValue* 是未发现该密钥时要设置的值。</span><span class="sxs-lookup"><span data-stu-id="b4a03-109">The *name* that's specified in the annotation is the name of the property to look up in your key vault, and the *defaultValue* is what will be set if the key is not discovered.</span></span> <span data-ttu-id="b4a03-110">结果是在运行时自动将密钥保管库中存储的值或默认值注入到该字段。</span><span class="sxs-lookup"><span data-stu-id="b4a03-110">The result is that the value that's stored in the key vault, or the default value, is injected automatically into the field at runtime.</span></span> <span data-ttu-id="b4a03-111">此操作可以简化你的工作，因为你不再需要在构造函数和 setter 方法中的不同位置传递值，</span><span class="sxs-lookup"><span data-stu-id="b4a03-111">This action can simplify your life, because you no longer need to pass values around in constructors and setter methods.</span></span> <span data-ttu-id="b4a03-112">只需让 MicroProfile 来处理该任务。</span><span class="sxs-lookup"><span data-stu-id="b4a03-112">Instead, MicroProfile handles this task.</span></span>

```java
@Inject
@ConfigProperty(name = "key-name", defaultValue = "Unknown")
String keyValue;
```

<span data-ttu-id="b4a03-113">还可以根据需要，直接访问 MicroProfile 配置来请求机密。</span><span class="sxs-lookup"><span data-stu-id="b4a03-113">To request secrets, as necessary, you can also access the MicroProfile config directly.</span></span> <span data-ttu-id="b4a03-114">例如：</span><span class="sxs-lookup"><span data-stu-id="b4a03-114">For example:</span></span>

```java
public class DemoClass {
    @Inject
    Config config;

    public void method() {
        System.out.println("Hello: " + config.getValue("key-name", String.class));
    }
}
```

<span data-ttu-id="b4a03-115">本示例代码使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile](https://microprofile.io/) 创建一个可在计算机本地运行的微型 Java Web 应用程序要求 (WAR) 文件。</span><span class="sxs-lookup"><span data-stu-id="b4a03-115">This sample code uses [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile](https://microprofile.io/) to create a tiny Java web application requirement (WAR) file that you can run locally on your machine.</span></span> <span data-ttu-id="b4a03-116">它不演示如何 Docker 化代码或将代码推送到 Azure，但本文末尾的部分提供了介绍此类操作的其他有用教程的链接。</span><span class="sxs-lookup"><span data-stu-id="b4a03-116">It doesn't demonstrate how to Dockerize or push the code to Azure, but the section at the end of this article has links to other useful tutorials that explain this.</span></span>

<span data-ttu-id="b4a03-117">此示例使用免费开源库在密钥保管库中创建配置源（使用 MicroProfile 配置 API）。</span><span class="sxs-lookup"><span data-stu-id="b4a03-117">The sample uses a free and open source library that creates a config source (using the MicroProfile Config API) in your key vault.</span></span> <span data-ttu-id="b4a03-118">若要详细了解此库和查看代码，请参阅[项目 GitHub 页](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault)。</span><span class="sxs-lookup"><span data-stu-id="b4a03-118">To learn more about this library and review the code, see the [project GitHub page](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault).</span></span> <span data-ttu-id="b4a03-119">如果你使用此库，则本教程中的代码只需专注于库的配置，然后将密钥注入代码即可。</span><span class="sxs-lookup"><span data-stu-id="b4a03-119">If you use the library, the code in this tutorial can simply focus on the configuration of the library and then inject keys into your code.</span></span> <span data-ttu-id="b4a03-120">不需要编写任何特定于 Azure 的代码。</span><span class="sxs-lookup"><span data-stu-id="b4a03-120">You don't need to write any Azure-specific code.</span></span>

<span data-ttu-id="b4a03-121">若要在本地计算机上运行此代码，请从创建密钥保管库资源开始，按后续部分的说明操作。</span><span class="sxs-lookup"><span data-stu-id="b4a03-121">To run this code on your local machine, starting with creating a key vault resource, follow the instructions in the next sections.</span></span>

## <a name="create-a-key-vault-resource"></a><span data-ttu-id="b4a03-122">创建密钥保管库资源</span><span class="sxs-lookup"><span data-stu-id="b4a03-122">Create a key vault resource</span></span>

<span data-ttu-id="b4a03-123">在此部分，请使用 Azure CLI 创建密钥保管库资源并在其中填充一个机密。</span><span class="sxs-lookup"><span data-stu-id="b4a03-123">In this section, you use the Azure CLI to create a key vault resource and populate it with one secret.</span></span>

1. <span data-ttu-id="b4a03-124">创建 Azure 服务主体。</span><span class="sxs-lookup"><span data-stu-id="b4a03-124">Create an Azure service principal.</span></span> <span data-ttu-id="b4a03-125">此步骤提供访问密钥保管库所需的客户端 ID 和密钥：</span><span class="sxs-lookup"><span data-stu-id="b4a03-125">This step gives you the client ID and key that you need to access your key vault:</span></span>

    ```bash
    az login
    az account set --subscription <subscription_id>

    az ad sp create-for-rbac --name <service_principal_name>
    ```

    <span data-ttu-id="b4a03-126">让我们使用 *microprofile-keyvault-service-principal* 来替换上一步骤中的 *\<service_principal_name>* 。</span><span class="sxs-lookup"><span data-stu-id="b4a03-126">Let's use *microprofile-keyvault-service-principal* to replace *\<service_principal_name>* in the preceding step.</span></span> <span data-ttu-id="b4a03-127">来自 Azure 的响应将如下所示：</span><span class="sxs-lookup"><span data-stu-id="b4a03-127">The response from Azure would be similar to the following:</span></span>

    ```json
    {
      "appId": "5292398e-XXXX-40ce-XXXX-d49fXXXX9e79",
      "displayName": "microprofile-keyvault-service-principal",
      "name": "http://microprofile-keyvault-service-principal",
      "password": "9b217777-XXXX-4954-XXXX-deafXXXX790a",
      "tenant": "72f988bf-XXXX-41af-XXXX-2d7cd011db47"
    }
    ```

    <span data-ttu-id="b4a03-128">在这里，需要特别注意的是 *appId* 和 *password* 值。</span><span class="sxs-lookup"><span data-stu-id="b4a03-128">Of particular note here are the *appId* and *password* values.</span></span> <span data-ttu-id="b4a03-129">在本文中，稍后需将其用作 *client ID* 和 *key*。</span><span class="sxs-lookup"><span data-stu-id="b4a03-129">You'll use them later in this article as *client ID* and *key*.</span></span>

1. <span data-ttu-id="b4a03-130">（可选）创建服务主体后，即可创建资源组。</span><span class="sxs-lookup"><span data-stu-id="b4a03-130">(Optional) Now that you've created a service principal, you can create a resource group.</span></span> <span data-ttu-id="b4a03-131">如果已有要使用的资源组，可跳过此步骤。</span><span class="sxs-lookup"><span data-stu-id="b4a03-131">If you already have a resource group that you want to use, you can skip this step.</span></span> <span data-ttu-id="b4a03-132">若要获取资源组位置的列表，可以调用 `az account list-locations`，并使用该列表中的 *name* 值来指定创建资源组的位置。</span><span class="sxs-lookup"><span data-stu-id="b4a03-132">To get a list of resource group locations, you can call `az account list-locations` and use the *name* value from that list to specify where the resource group should be created.</span></span>

    ```bash
    # In this tutorial, "westus" is the resource group location
    # and "jg-test" is the resource group name.
    az group create -l <resource_group_location> -n <resource_group_name>
    ```

1. <span data-ttu-id="b4a03-133">创建密钥保管库资源。</span><span class="sxs-lookup"><span data-stu-id="b4a03-133">Create a key vault resource.</span></span> <span data-ttu-id="b4a03-134">稍后将使用密钥保管库名称来引用密钥保管库，因此务必选择易记的名称。</span><span class="sxs-lookup"><span data-stu-id="b4a03-134">You'll use your key vault name to refer to the key vault later, so be sure to choose a memorable name.</span></span>

    ```bash
    az keyvault create --name <your_keyvault_name>            \
                      --resource-group <your_resource_group> \
                      --location <location>                  \
                      --enabled-for-deployment true          \
                      --enabled-for-disk-encryption true     \
                      --enabled-for-template-deployment true \
                      --sku standard
    ```

1. <span data-ttu-id="b4a03-135">请向前面创建的服务主体授予适当的权限，使它可以访问密钥保管库机密。</span><span class="sxs-lookup"><span data-stu-id="b4a03-135">Grant the appropriate permissions to the service principal that you created earlier, so that it can access the key vault secrets.</span></span> <span data-ttu-id="b4a03-136">以下代码中的 appId 值是步骤 1 中的 *appId* 值。在步骤 1 中，已创建了服务主体。</span><span class="sxs-lookup"><span data-stu-id="b4a03-136">The appId value in the following code is the *appId* value from step 1, where you created the service principal.</span></span> <span data-ttu-id="b4a03-137">也就是说，步骤 1 中的 *appId* 为 *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*，但你应该使用自己的终端输出中的值。</span><span class="sxs-lookup"><span data-stu-id="b4a03-137">That is, the *appId* in step 1 was *5292398e-XXXX-40ce-XXXX-d49fXXXX9e79*, but you should use the value from your own terminal output.</span></span>

    ```bash
    az keyvault set-policy --name <your_keyvault_name>   \
                          --secret-permission get list  \
                          --spn <your_sp_appId_created_in_step1>
    ```

1. <span data-ttu-id="b4a03-138">现在可以将机密推送到密钥保管库中。</span><span class="sxs-lookup"><span data-stu-id="b4a03-138">Now you can push a secret into your key vault.</span></span> <span data-ttu-id="b4a03-139">请使用密钥名称 *demo-key*，并将密钥的值设置为 *demo-value*：</span><span class="sxs-lookup"><span data-stu-id="b4a03-139">Use the key name *demo-key*, and set the value of the key to *demo-value*:</span></span>

    ```bash
    az keyvault secret set --name demo-key      \
                           --value demo-value   \
                           --vault-name <your_keyvault_name>  
    ```

<span data-ttu-id="b4a03-140">就这么简单！</span><span class="sxs-lookup"><span data-stu-id="b4a03-140">That's it!</span></span> <span data-ttu-id="b4a03-141">现在，Azure 中运行了包含一个机密的密钥保管库。</span><span class="sxs-lookup"><span data-stu-id="b4a03-141">You now have a key vault running in Azure with a single secret.</span></span> <span data-ttu-id="b4a03-142">接下来可以克隆此存储库，并将其配置为在应用中使用此资源。</span><span class="sxs-lookup"><span data-stu-id="b4a03-142">You can now clone this repo and configure it to use this resource in your app.</span></span>

## <a name="get-up-and-running-locally"></a><span data-ttu-id="b4a03-143">在本地启动和运行</span><span class="sxs-lookup"><span data-stu-id="b4a03-143">Get up and running locally</span></span>

<span data-ttu-id="b4a03-144">本示例基于 GitHub 上提供的示例应用程序，因此需克隆该应用程序，然后逐步执行代码。</span><span class="sxs-lookup"><span data-stu-id="b4a03-144">This example is based on a sample application that's available on GitHub, so you'll clone that application and then step through the code.</span></span> 

1. <span data-ttu-id="b4a03-145">输入以下命令，将代码克隆到计算机中：</span><span class="sxs-lookup"><span data-stu-id="b4a03-145">Clone the code onto your machine by entering the following commands:</span></span>

    `git clone https://github.com/Azure-Samples/microprofile-configsource-keyvault.git`

    `cd microprofile-configsource-keyvault`

1. <span data-ttu-id="b4a03-146">转到 *src/main/resources/META-INF/microprofile-config.properties*，然后使用上述步骤中的值更改 *microprofile-config.properties* 文件中的属性。</span><span class="sxs-lookup"><span data-stu-id="b4a03-146">Go to *src/main/resources/META-INF/microprofile-config.properties*, and then change the properties in the *microprofile-config.properties* file by using the values from the previous steps.</span></span>

1. <span data-ttu-id="b4a03-147">尝试使用 `mvn clean package payara-micro:start` 运行服务器。</span><span class="sxs-lookup"><span data-stu-id="b4a03-147">Try running the server by using `mvn clean package payara-micro:start`.</span></span>

1. <span data-ttu-id="b4a03-148">尝试在 Web 浏览器中访问 [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config)。</span><span class="sxs-lookup"><span data-stu-id="b4a03-148">Try accessing [http://localhost:8080/keyvault-configsource/api/config](http://localhost:8080/keyvault-configsource/api/config) in your web browser.</span></span> <span data-ttu-id="b4a03-149">应该会看到一个简单的响应，其中展示了正在从密钥保管库读取的值。</span><span class="sxs-lookup"><span data-stu-id="b4a03-149">You should see a simple response that demonstrates values that are being read from your key vault.</span></span>

## <a name="summary"></a><span data-ttu-id="b4a03-150">摘要</span><span class="sxs-lookup"><span data-stu-id="b4a03-150">Summary</span></span>

<span data-ttu-id="b4a03-151">本示例应用程序将 MicroProfile 配置 API、Azure Key Vault 和免费开源 [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) 库组合在一起，这样就能轻松地将配置数据和机密注入 MicroProfile Web 服务。</span><span class="sxs-lookup"><span data-stu-id="b4a03-151">This sample application combines the MicroProfile Config API, Azure Key Vault, and the free and open source [microprofile-config-keyvault](https://github.com/Azure/azure-microprofile/tree/master/microprofile-config-keyvault) library to enable easy injection of configuration data and secrets into your MicroProfile web services.</span></span>
