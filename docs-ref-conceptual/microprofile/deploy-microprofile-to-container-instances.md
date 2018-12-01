---
title: 使用 Docker 和 Azure 将 MicroProfile 应用部署到云中
description: 了解如何使用 Docker 和 Azure 容器实例将 MicroProfile 应用部署到云中。
services: container-instances;container-retistry
documentationcenter: java
author: brunoborges
manager: routlaw
editor: brunoborges
ms.assetid: ''
ms.author: brborges
ms.date: 11/21/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 22870b7ba32f115e7270c63d1bf42cbfc6531d7e
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338781"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="0d64c-103">使用 Docker 和 Azure 将 MicroProfile 应用程序部署到云中</span><span class="sxs-lookup"><span data-stu-id="0d64c-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="0d64c-104">本文演示如何在 Docker 容器中打包 [MicroProfile.io] 应用程序，并在 Azure 容器实例上运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="0d64c-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0d64c-105">只要 Docker 容器映像可自我执行（即包括运行时），此过程就适用于 MicroProfile.io 的任何实现。</span><span class="sxs-lookup"><span data-stu-id="0d64c-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d64c-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="0d64c-106">Prerequisites</span></span>

<span data-ttu-id="0d64c-107">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="0d64c-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="0d64c-108">一个 Azure 订阅；如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="0d64c-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="0d64c-109">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="0d64c-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="0d64c-110">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="0d64c-110">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="0d64c-111">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="0d64c-111">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="0d64c-112">Apache 的 [Maven] 生成工具（版本 3 以上）。</span><span class="sxs-lookup"><span data-stu-id="0d64c-112">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="0d64c-113">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="0d64c-113">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="0d64c-114">MicroProfile Hello Azure 示例</span><span class="sxs-lookup"><span data-stu-id="0d64c-114">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="0d64c-115">本文使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 示例：</span><span class="sxs-lookup"><span data-stu-id="0d64c-115">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="0d64c-116">克隆、生成和本地运行</span><span class="sxs-lookup"><span data-stu-id="0d64c-116">Clone, build, and run locally</span></span>

```bash
$ git clone https://github.com/Azure-Samples/microprofile-hello-azure.git
$ mvn clean package
...
[INFO] BUILD SUCCESS
...
$ mvn payara-micro:start
...
[2018-07-30T13:34:51.553-0700] [] [INFO] [] [PayaraMicro] [tid: _ThreadID=1 _ThreadName=main] [timeMillis: 1532982891553] [levelValue: 800] Payara Micro  5.182 #badassmicrofish (build 303) ready in 10,304 (ms)
...
```

<span data-ttu-id="0d64c-117">可以通过调用 `curl` 来测试该应用程序，或者通过[浏览器](http://localhost:8080/api/hello)访问该应用程序进行测试：</span><span class="sxs-lookup"><span data-stu-id="0d64c-117">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="0d64c-118">“部署到 Azure”</span><span class="sxs-lookup"><span data-stu-id="0d64c-118">Deploy to Azure</span></span>

<span data-ttu-id="0d64c-119">现在，让我们使用 [Azure 容器实例]和 [Azure 容器注册表]服务将此应用程序部署到云中。</span><span class="sxs-lookup"><span data-stu-id="0d64c-119">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="0d64c-120">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="0d64c-120">Build a Docker image</span></span>

<span data-ttu-id="0d64c-121">示例项目已提供可用的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="0d64c-121">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="0d64c-122">不过，不需要安装 Docker，因为我们将使用 Azure 容器注册表生成功能在云中生成映像。</span><span class="sxs-lookup"><span data-stu-id="0d64c-122">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="0d64c-123">若要生成映像并准备好在 Azure 中运行它，必须执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="0d64c-123">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="0d64c-124">安装 Azure CLI 并使用它登录</span><span class="sxs-lookup"><span data-stu-id="0d64c-124">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="0d64c-125">创建 Azure 资源组</span><span class="sxs-lookup"><span data-stu-id="0d64c-125">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="0d64c-126">创建 Azure 容器注册表 (ACR)</span><span class="sxs-lookup"><span data-stu-id="0d64c-126">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="0d64c-127">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="0d64c-127">Build the Docker image</span></span>
1. <span data-ttu-id="0d64c-128">将 Docker 映像发布到前面创建的 ACR</span><span class="sxs-lookup"><span data-stu-id="0d64c-128">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="0d64c-129">（可选）通过一条命令生成映像并将其发布到 ACR</span><span class="sxs-lookup"><span data-stu-id="0d64c-129">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="0d64c-130">设置 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0d64c-130">Set up Azure CLI</span></span>

<span data-ttu-id="0d64c-131">确保有一个 Azure 订阅、[已安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，并已对帐户进行身份验证：</span><span class="sxs-lookup"><span data-stu-id="0d64c-131">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="0d64c-132">创建资源组。</span><span class="sxs-lookup"><span data-stu-id="0d64c-132">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="0d64c-133">创建 Azure 容器注册表实例</span><span class="sxs-lookup"><span data-stu-id="0d64c-133">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="0d64c-134">此命令使用包含随机数的基本名称创建一个全局唯一（希望如此）的容器注册表。</span><span class="sxs-lookup"><span data-stu-id="0d64c-134">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="0d64c-135">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="0d64c-135">Build the Docker image</span></span>

<span data-ttu-id="0d64c-136">尽管在本地使用 Docker 本身就能轻松生成 Docker 映像，但我们建议在云中生成该映像，原因如下：</span><span class="sxs-lookup"><span data-stu-id="0d64c-136">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="0d64c-137">无需在本地安装 Docker</span><span class="sxs-lookup"><span data-stu-id="0d64c-137">No need to install Docker locally</span></span>
1. <span data-ttu-id="0d64c-138">速度要快得多，因为生成在其他位置进行（但需要花费一定的上下文上传时间）</span><span class="sxs-lookup"><span data-stu-id="0d64c-138">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="0d64c-139">云中进程的 Internet 访问速度更快，因此下载速度也就更快</span><span class="sxs-lookup"><span data-stu-id="0d64c-139">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="0d64c-140">映像将直接进入容器注册表</span><span class="sxs-lookup"><span data-stu-id="0d64c-140">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="0d64c-141">出于上述原因，我们将使用 [Azure 容器注册表生成]功能来生成映像：</span><span class="sxs-lookup"><span data-stu-id="0d64c-141">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="0d64c-142">将 Docker 映像从 Azure 容器注册表 (ACR) 部署到容器实例 (ACI)</span><span class="sxs-lookup"><span data-stu-id="0d64c-142">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="0d64c-143">在 ACR 中提供该映像后，让我们在 ACI 中推送并实例化容器实例。</span><span class="sxs-lookup"><span data-stu-id="0d64c-143">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="0d64c-144">但是，首先需要确保可在 ACR 中进行身份验证：</span><span class="sxs-lookup"><span data-stu-id="0d64c-144">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="0d64c-145">测试部署的 MicroProfile 应用程序</span><span class="sxs-lookup"><span data-stu-id="0d64c-145">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="0d64c-146">该应用程序现在应已启动并运行。</span><span class="sxs-lookup"><span data-stu-id="0d64c-146">Your application should now be up and running.</span></span> <span data-ttu-id="0d64c-147">若要从命令行测试它，请尝试以下命令：</span><span class="sxs-lookup"><span data-stu-id="0d64c-147">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="0d64c-148">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="0d64c-148">Congratulations!</span></span> <span data-ttu-id="0d64c-149">现已成功生成一个 MicroProfile 应用程序，并已将其作为 Docker 容器部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="0d64c-149">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d64c-150">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0d64c-150">Next steps</span></span>

<span data-ttu-id="0d64c-151">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="0d64c-151">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="0d64c-152">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="0d64c-152">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure 容器注册表生成]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[Azure Container Registry Build]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure 容器实例]: https://docs.microsoft.com/azure/container-instances/
[Azure Container Instances]: https://docs.microsoft.com/azure/container-instances/
[Azure 容器注册表]:  https://docs.microsoft.com/azure/container-registry
[Azure Container Registry]:  https://docs.microsoft.com/azure/container-registry