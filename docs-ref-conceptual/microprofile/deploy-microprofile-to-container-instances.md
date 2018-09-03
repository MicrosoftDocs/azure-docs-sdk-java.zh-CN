---
title: 使用 Docker 和 Azure 将 MicroProfile 应用部署到云中
description: 了解如何使用 Docker 和 Azure 容器实例将 MicroProfile 应用部署到云中。
services: container-instances;container-retistry
documentationcenter: java
author: brborges
manager: routlaw
editor: brborges
ms.assetid: ''
ms.author: brborges
ms.date: 07/30/2018
ms.devlang: java
ms.service: container-instances
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: c6254d11ee1596a23076931c9a2a2370b5f52409
ms.sourcegitcommit: 3d0896f821907278547c283c54b53fbd7f4f30f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43153817"
---
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a><span data-ttu-id="7b386-103">使用 Docker 和 Azure 将 MicroProfile 应用程序部署到云中</span><span class="sxs-lookup"><span data-stu-id="7b386-103">Deploy a MicroProfile application to the cloud with Docker and Azure</span></span>

<span data-ttu-id="7b386-104">本文演示如何在 Docker 容器中打包 [MicroProfile.io] 应用程序，并在 Azure 容器实例上运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b386-104">This article demonstrates how to pack a [MicroProfile.io] application in a Docker container and run it on Azure Container Instances.</span></span>

> [!NOTE]
>
> <span data-ttu-id="7b386-105">只要 Docker 容器映像可自我执行（即包括运行时），此过程就适用于 MicroProfile.io 的任何实现。</span><span class="sxs-lookup"><span data-stu-id="7b386-105">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b386-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="7b386-106">Prerequisites</span></span>

<span data-ttu-id="7b386-107">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="7b386-107">In order to complete the steps in this tutorial, you will need to have the following prerequisites:</span></span>

* <span data-ttu-id="7b386-108">一个 Azure 订阅；如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="7b386-108">An Azure subscription; if you don't already have an Azure subscription, you can sign up for a [free Azure account].</span></span>
* <span data-ttu-id="7b386-109">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="7b386-109">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="7b386-110">最新的 [Java 开发工具包 (JDK)] 1.8 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="7b386-110">An up-to-date [Java Development Kit (JDK)], version 1.8 or later.</span></span>
* <span data-ttu-id="7b386-111">Apache 的 [Maven] 生成工具（版本 3 以上）。</span><span class="sxs-lookup"><span data-stu-id="7b386-111">Apache's [Maven] build tool (version 3+).</span></span>
* <span data-ttu-id="7b386-112">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="7b386-112">A [Git] client.</span></span>

## <a name="microprofile-hello-azure-sample"></a><span data-ttu-id="7b386-113">MicroProfile Hello Azure 示例</span><span class="sxs-lookup"><span data-stu-id="7b386-113">MicroProfile Hello Azure sample</span></span>

<span data-ttu-id="7b386-114">本文使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 示例：</span><span class="sxs-lookup"><span data-stu-id="7b386-114">For this article, we will use the [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) sample:</span></span>

### <a name="clone-build-and-run-locally"></a><span data-ttu-id="7b386-115">克隆、生成和本地运行</span><span class="sxs-lookup"><span data-stu-id="7b386-115">Clone, build, and run locally</span></span>

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

<span data-ttu-id="7b386-116">可以通过调用 `curl` 来测试该应用程序，或者通过[浏览器](http://localhost:8080/api/hello)访问该应用程序进行测试：</span><span class="sxs-lookup"><span data-stu-id="7b386-116">You can test the application by calling `curl` or visiting through a [browser](http://localhost:8080/api/hello):</span></span>

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a><span data-ttu-id="7b386-117">“部署到 Azure”</span><span class="sxs-lookup"><span data-stu-id="7b386-117">Deploy to Azure</span></span>

<span data-ttu-id="7b386-118">现在，让我们使用 [Azure 容器实例] 和 [Azure 容器注册表] 服务将此应用程序部署到云中。</span><span class="sxs-lookup"><span data-stu-id="7b386-118">Now let's bring this application to the cloud using [Azure Container Instances] and [Azure Container Registry] services.</span></span>

### <a name="build-a-docker-image"></a><span data-ttu-id="7b386-119">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="7b386-119">Build a Docker image</span></span>

<span data-ttu-id="7b386-120">示例项目已提供可用的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="7b386-120">The sample project already provides a Dockerfile you can use.</span></span> <span data-ttu-id="7b386-121">不过，不需要安装 Docker，因为我们将使用 Azure 容器注册表生成功能在云中生成映像。</span><span class="sxs-lookup"><span data-stu-id="7b386-121">You don't need Docker installed though, as we will use Azure Container Registry Build feature to build the image in the cloud.</span></span>

<span data-ttu-id="7b386-122">若要生成映像并准备好在 Azure 中运行它，必须执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="7b386-122">To build the image and be ready to run on Azure, you will have to follow these steps:</span></span>

1. <span data-ttu-id="7b386-123">安装 Azure CLI 并使用它登录</span><span class="sxs-lookup"><span data-stu-id="7b386-123">Install and log in with Azure CLI</span></span>
1. <span data-ttu-id="7b386-124">创建 Azure 资源组</span><span class="sxs-lookup"><span data-stu-id="7b386-124">Create an Azure Resource Group</span></span>
1. <span data-ttu-id="7b386-125">创建 Azure 容器注册表 (ACR)</span><span class="sxs-lookup"><span data-stu-id="7b386-125">Create an Azure Container Registry (ACR)</span></span>
1. <span data-ttu-id="7b386-126">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="7b386-126">Build the Docker image</span></span>
1. <span data-ttu-id="7b386-127">将 Docker 映像发布到前面创建的 ACR</span><span class="sxs-lookup"><span data-stu-id="7b386-127">Publish the Docker image to the ACR created before</span></span>
1. <span data-ttu-id="7b386-128">（可选）通过一条命令生成映像并将其发布到 ACR</span><span class="sxs-lookup"><span data-stu-id="7b386-128">(Optionally) Build and publish to ACR in one command</span></span>


#### <a name="set-up-azure-cli"></a><span data-ttu-id="7b386-129">设置 Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7b386-129">Set up Azure CLI</span></span>

<span data-ttu-id="7b386-130">确保有一个 Azure 订阅、[已安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，并已对帐户进行身份验证：</span><span class="sxs-lookup"><span data-stu-id="7b386-130">Make sure you have an Azure subscription, [Azure CLI installed](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), and that you are authenticated to your account:</span></span>

```bash
az login
```

#### <a name="create-a-resource-group"></a><span data-ttu-id="7b386-131">创建资源组。</span><span class="sxs-lookup"><span data-stu-id="7b386-131">Create a Resource Group</span></span>

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="7b386-132">创建 Azure 容器注册表实例</span><span class="sxs-lookup"><span data-stu-id="7b386-132">Create an Azure Container Registry instance</span></span>

<span data-ttu-id="7b386-133">此命令使用包含随机数的基本名称创建一个全局唯一（希望如此）的容器注册表。</span><span class="sxs-lookup"><span data-stu-id="7b386-133">This command will create a globally unique (hopefully) container registry using a basic name with a random number.</span></span>

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a><span data-ttu-id="7b386-134">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="7b386-134">Build the Docker image</span></span>

<span data-ttu-id="7b386-135">尽管在本地使用 Docker 本身就能轻松生成 Docker 映像，但我们建议在云中生成该映像，原因如下：</span><span class="sxs-lookup"><span data-stu-id="7b386-135">While you can easily build the Docker image locally using Docker itself, you may want to consider building it in the Cloud for few reasons:</span></span>

1. <span data-ttu-id="7b386-136">无需在本地安装 Docker</span><span class="sxs-lookup"><span data-stu-id="7b386-136">No need to install Docker locally</span></span>
1. <span data-ttu-id="7b386-137">速度要快得多，因为生成在其他位置进行（但需要花费一定的上下文上传时间）</span><span class="sxs-lookup"><span data-stu-id="7b386-137">Much faster since build will happen elsewhere (except for context upload time)</span></span>
1. <span data-ttu-id="7b386-138">云中进程的 Internet 访问速度更快，因此下载速度也就更快</span><span class="sxs-lookup"><span data-stu-id="7b386-138">Process in the Cloud has access to faster Internet, therefore faster downloads</span></span>
1. <span data-ttu-id="7b386-139">映像将直接进入容器注册表</span><span class="sxs-lookup"><span data-stu-id="7b386-139">Image goes directly into the Container Registry</span></span>

<span data-ttu-id="7b386-140">出于上述原因，我们将使用 [Azure 容器注册表生成]功能来生成映像：</span><span class="sxs-lookup"><span data-stu-id="7b386-140">Because of these reasons, we will build the image using the [Azure Container Registry Build] feature:</span></span>

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a><span data-ttu-id="7b386-141">将 Docker 映像从 Azure 容器注册表 (ACR) 部署到容器实例 (ACI)</span><span class="sxs-lookup"><span data-stu-id="7b386-141">Deploy Docker Image from Azure Container Registry (ACR) into Container Instances (ACI)</span></span>

<span data-ttu-id="7b386-142">在 ACR 中提供该映像后，让我们在 ACI 中推送并实例化容器实例。</span><span class="sxs-lookup"><span data-stu-id="7b386-142">Now that the image is available on your ACR, let's push and instanciate a container instance on ACI.</span></span> <span data-ttu-id="7b386-143">但是，首先需要确保可在 ACR 中进行身份验证：</span><span class="sxs-lookup"><span data-stu-id="7b386-143">But first, we need to make sure we can authenticate into the ACR:</span></span>

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a><span data-ttu-id="7b386-144">测试部署的 MicroProfile 应用程序</span><span class="sxs-lookup"><span data-stu-id="7b386-144">Test Your Deployed MicroProfile Application</span></span>

<span data-ttu-id="7b386-145">该应用程序现在应已启动并运行。</span><span class="sxs-lookup"><span data-stu-id="7b386-145">Your application should now be up and running.</span></span> <span data-ttu-id="7b386-146">若要从命令行测试它，请尝试以下命令：</span><span class="sxs-lookup"><span data-stu-id="7b386-146">To test it from the command-line, try the following command:</span></span>

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

<span data-ttu-id="7b386-147">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="7b386-147">Congratulations!</span></span> <span data-ttu-id="7b386-148">现已成功生成一个 MicroProfile 应用程序，并已将其作为 Docker 容器部署到 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="7b386-148">You have successfuly built and deployed a MicroProfile application as a Docker container onto Microsoft Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b386-149">后续步骤</span><span class="sxs-lookup"><span data-stu-id="7b386-149">Next steps</span></span>

<span data-ttu-id="7b386-150">有关本文中讨论的各项技术的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="7b386-150">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* [<span data-ttu-id="7b386-151">通过 Azure CLI 登录到 Azure</span><span class="sxs-lookup"><span data-stu-id="7b386-151">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure 容器注册表生成]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[Azure Container Registry Build]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
