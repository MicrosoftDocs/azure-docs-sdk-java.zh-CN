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
# <a name="deploy-a-microprofile-application-to-the-cloud-with-docker-and-azure"></a>使用 Docker 和 Azure 将 MicroProfile 应用程序部署到云中

本文演示如何在 Docker 容器中打包 [MicroProfile.io] 应用程序，并在 Azure 容器实例上运行该应用程序。

> [!NOTE]
>
> 只要 Docker 容器映像可自我执行（即包括运行时），此过程就适用于 MicroProfile.io 的任何实现。

## <a name="prerequisites"></a>先决条件

完成本教程中的步骤需要具备以下先决条件：

* 一个 Azure 订阅；如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。
* [Azure 命令行接口 (CLI)]。
* 最新的 [Java 开发工具包 (JDK)] 1.8 或更高版本。
* Apache 的 [Maven] 生成工具（版本 3 以上）。
* [Git] 客户端。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure 示例

本文使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 示例：

### <a name="clone-build-and-run-locally"></a>克隆、生成和本地运行

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

可以通过调用 `curl` 来测试该应用程序，或者通过[浏览器](http://localhost:8080/api/hello)访问该应用程序进行测试：

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-to-azure"></a>“部署到 Azure”

现在，让我们使用 [Azure 容器实例] 和 [Azure 容器注册表] 服务将此应用程序部署到云中。

### <a name="build-a-docker-image"></a>生成 Docker 映像

示例项目已提供可用的 Dockerfile。 不过，不需要安装 Docker，因为我们将使用 Azure 容器注册表生成功能在云中生成映像。

若要生成映像并准备好在 Azure 中运行它，必须执行以下步骤：

1. 安装 Azure CLI 并使用它登录
1. 创建 Azure 资源组
1. 创建 Azure 容器注册表 (ACR)
1. 生成 Docker 映像
1. 将 Docker 映像发布到前面创建的 ACR
1. （可选）通过一条命令生成映像并将其发布到 ACR


#### <a name="set-up-azure-cli"></a>设置 Azure CLI

确保有一个 Azure 订阅、[已安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，并已对帐户进行身份验证：

```bash
az login
```

#### <a name="create-a-resource-group"></a>创建资源组。

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-an-azure-container-registry-instance"></a>创建 Azure 容器注册表实例

此命令使用包含随机数的基本名称创建一个全局唯一（希望如此）的容器注册表。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>生成 Docker 映像

尽管在本地使用 Docker 本身就能轻松生成 Docker 映像，但我们建议在云中生成该映像，原因如下：

1. 无需在本地安装 Docker
1. 速度要快得多，因为生成在其他位置进行（但需要花费一定的上下文上传时间）
1. 云中进程的 Internet 访问速度更快，因此下载速度也就更快
1. 映像将直接进入容器注册表

出于上述原因，我们将使用 [Azure 容器注册表生成]功能来生成映像：

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-docker-image-from-azure-container-registry-acr-into-container-instances-aci"></a>将 Docker 映像从 Azure 容器注册表 (ACR) 部署到容器实例 (ACI)

在 ACR 中提供该映像后，让我们在 ACI 中推送并实例化容器实例。 但是，首先需要确保可在 ACR 中进行身份验证：

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>测试部署的 MicroProfile 应用程序

该应用程序现在应已启动并运行。 若要从命令行测试它，请尝试以下命令：

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

祝贺你！ 现已成功生成一个 MicroProfile 应用程序，并已将其作为 Docker 容器部署到 Microsoft Azure。

## <a name="next-steps"></a>后续步骤

有关本文中讨论的各项技术的详细信息，请参阅以下文章：

* [通过 Azure CLI 登录到 Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure 容器注册表生成]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Maven]: http://maven.apache.org/
