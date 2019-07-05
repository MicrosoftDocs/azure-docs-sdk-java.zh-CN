---
title: 使用 Docker 和 Azure 将 MicroProfile 应用部署到云
description: 了解如何使用 Docker 和 Azure 容器实例将 MicroProfile 应用部署到云。
services: container-instances;container-registry
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
ms.openlocfilehash: 6ba12bb183969103676fa988199603df6cf36bba
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533605"
---
# <a name="deploy-a-microprofile-app-to-the-cloud-by-using-docker-and-azure"></a>使用 Docker 和 Azure 将 MicroProfile 应用部署到云

本文演示如何在 Docker 容器中打包 [MicroProfile.io] 应用程序，并在 Azure 容器实例上运行该应用程序。

> [!NOTE]
> 只要 Docker 容器映像可自我执行（即映像包括运行时），此过程就适用于 MicroProfile.io 的任何实现。

## <a name="prerequisites"></a>先决条件

若要完成本教程，需要具备以下先决条件：

* Azure 订阅。 如果没有 Azure 订阅，可以注册[免费的 Azure 帐户]。
* [Azure CLI]，已安装。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅[针对 Azure 和 Azure Stack 的 Java 长期支持](https://aka.ms/azure-jdks)。
* [Apache Maven] 生成工具（版本 3 或更高版本）。
* [Git] 客户端。

## <a name="microprofile-hello-azure-sample"></a>MicroProfile Hello Azure 示例

在本文中，我们使用 [MicroProfile Hello Azure](https://github.com/azure-samples/microprofile-hello-azure) 示例。 通过以下命令在本地克隆、生成和运行它：

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

可以通过调用 `curl` 或通过[浏览器](http://localhost:8080/api/hello)来测试该应用程序：

```bash
$ curl http://localhost:8080/api/hello
Hello, Azure!
```

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

现在，使用 [Azure 容器实例]和 [Azure 容器注册表]服务将此应用程序部署到 Azure。

### <a name="build-a-docker-image"></a>生成 Docker 映像

示例项目提供可用的 Dockerfile。 不过，不需要安装 Docker，因为你将使用 Azure 容器注册表生成功能在云中生成映像。

若要生成映像并准备好在 Azure 中运行它，请执行以下步骤：

1. 安装并登录 Azure CLI。
1. 创建 Azure 资源组。
1. 创建 Azure 容器注册表实例。
1. 生成 Docker 映像。
1. 将 Docker 映像发布到以前创建的容器注册表实例。
1. （可选）通过一个命令生成映像并将其发布到容器注册表实例。


#### <a name="set-up-the-azure-cli"></a>设置 Azure CLI

确保有一个 Azure 订阅、已安装 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)，并已向帐户进行身份验证：

```bash
az login
```

#### <a name="create-a-resource-group"></a>创建资源组

```bash
export ARG=microprofileRG
export ADCL=eastus
az group create --name $ARG --location $ADCL
```

#### <a name="create-a-container-registry-instance"></a>创建容器注册表实例

此命令应该使用基本名称和随机数创建全局唯一的容器注册表实例。

```bash
export RANDINT=`date +"%m%d%y$RANDOM"`
export ACR=mydockerrepo$RANDINT
az acr create --name $ACR -g $ARG --sku Basic --admin-enabled
```

#### <a name="build-the-docker-image"></a>生成 Docker 映像

尽管在本地使用 Docker 本身就能轻松生成 Docker 映像，但可以考虑在云中生成该映像，原因如下：

* 无需在本地安装 Docker。
* 速度要快得多，因为生成在其他位置进行（但需要花费一定的上下文上传时间）。
* 云中进程的 Internet 访问速度更快，因此下载速度也更快。
* 映像直接进入容器注册表实例。

出于上述原因，请使用 [Azure 容器注册表生成]功能来生成映像：

```bash
export IMG_NAME="mympapp:latest"
az acr build -r $ACR -t $IMG_NAME -g $ARG .
...
Build complete
Build ID: aa1 was successful after 1m2.674577892s
```

#### <a name="deploy-the-docker-image-from-the-azure-container-registry-instance-to-container-instances"></a>将 Docker 映像从 Azure 容器注册表实例部署到 Azure 容器实例

在容器注册表实例中提供该映像后，请在 Azure 容器实例中推送并实例化容器实例。 但是，首先请确保可在容器注册表实例中进行身份验证：

```bash
export ACR_REPO=`az acr show --name $ACR -g $ARG --query loginServer -o tsv`
export ACR_PASS=`az acr credential show --name $ACR -g $ARG --query "passwords[0].value" -o tsv`
export ACI_INSTANCE=myapp`date +"%m%d%y$RANDOM"`

az container create --resource-group $ARG --name $ACR --image $ACR_REPO/$IMG_NAME --cpu 1 --memory 1 --registry-login-server $ACR_REPO --registry-username $ACR --registry-password $ACR_PASS --dns-name-label $ACI_INSTANCE --ports 8080
```

#### <a name="test-your-deployed-microprofile-application"></a>测试部署的 MicroProfile 应用程序

该应用程序现在应已启动并运行。 若要从命令行界面测试它，请使用以下命令：

```bash
curl http://$ACI_INSTANCE.$ADCL.azurecontainer.io:8080/api/hello
````

祝贺你！ 现已成功生成一个充当 Docker 容器的 MicroProfile 应用程序，并已将其部署到 Azure。

## <a name="next-steps"></a>后续步骤

有关本文中讨论的各项技术的详细信息，请参阅：

* [从 Azure CLI 登录 Azure](/azure/xplat-cli-connect)

<!-- URL List -->

[Azure 容器注册表生成]: https://docs.microsoft.com/azure/container-registry/container-registry-build-overview
[MicroProfile.io]: https://microprofile.io
[Azure CLI]: /cli/azure/overview
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Azure portal]: https://portal.azure.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Apache Maven]: http://maven.apache.org/
[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->
[Azure 容器实例]: https://docs.microsoft.com/azure/container-instances/
[Azure 容器注册表]:  https://docs.microsoft.com/azure/container-registry
