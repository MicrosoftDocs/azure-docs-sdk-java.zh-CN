---
title: 在 Azure Kubernetes 服务中将 Spring Boot 应用部署于 Kubernetes
description: 本教程将指导用户完成在 Microsoft Azure 的 Kubernetes 群集中部署 Spring Boot 应用程序的步骤。
services: container-service
documentationcenter: java
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.custom: mvc
ms.openlocfilehash: 9ab781d27e8968ab867efc65f3ac422ac6253a6a
ms.sourcegitcommit: 394521c47ac9895d00d9f97535cc9d1e27d08fe9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66270850"
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-kubernetes-service"></a>在 Azure Kubernetes 服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上

[Kubernetes] 和 [Docker] 是开源解决方案，可帮助开发人员自动部署、缩放和管理在容器中运行的应用程序   。

本教程将指导用户将这两种常用的开源技术进行结合，从而将 Spring Boot 应用程序开发和部署到 Microsoft Azure。 具体而言，将使用 [Spring Boot] 进行应用程序开发，使用 [Kubernetes] 进行容器部署，使用 [Azure Kubernetes 服务 (AKS)] 来托管应用程序   。

### <a name="prerequisites"></a>先决条件

* Azure 订阅；若尚未拥有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册获取[免费的 Azure 帐户]。
* [Azure 命令行接口 (CLI)]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven] 生成工具（版本 3）。
* [Git] 客户端。
* [Docker] 客户端。

> [!NOTE]
>
> 由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>创建 Docker 上的 Spring Boot 入门 Web 应用

以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。

1. 打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   - 或 -
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. 将 [Docker 上的 Spring Boot 入门]示例项目克隆到目录。
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 将目录更改为已完成项目。
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. 使用 Maven 生成和运行示例应用。
   ```
   mvn package spring-boot:run
   ```

1. 通过浏览到 `http://localhost:8080` 或使用以下 `curl` 命令测试 Web 应用：
   ```
   curl http://localhost:8080
   ```

1. 应当会看到显示了以下消息：**Hello Docker World**

   ![本地浏览示例应用][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 创建 Azure 容器注册表

1. 打开命令提示符。

1. 登录到 Azure 帐户：
   ```azurecli
   az login
   ```

1. 选择自己的 Azure 订阅：
   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. 为本教程中使用的 Azure 资源创建资源组。
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. 在资源组中创建私有 Azure 容器注册表。 本教程的后续步骤会将示例应用作为 Docker 映像推送到此注册表。 用注册表的唯一名称替换 `wingtiptoysregistry`。
   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>通过 Jib 将应用推送到容器注册表

1. 通过 Azure CLI 登录到 Azure 容器注册表。
   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. 导航到 Spring Boot 应用程序的完整项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件    。

1. 将 *pom.xml* 文件中的 `<properties>` 集合更新为你的 Azure 容器注册表的注册表名称和 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 的最新版本。

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.2.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. 更新 *pom.xml* 文件中的 `<plugins>` 集合，使 `<plugin>` 包含 `jib-maven-plugin`。

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. 导航到 Spring Boot 应用程序的完成项目目录，然后运行以下命令以生成映像并将映像推送到注册表：

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> 出于 Azure Cli 和 Azure 容器注册表的安全考虑，通过 `az acr login` 创建的凭据的有效时间为 1 小时。如果遇到“401 未授权”错误，  则可再次运行 `az acr login -n <your registry name>` 命令，以便重新进行身份验证。
>

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>使用 Azure CLI 在 AKS 上创建 Kubernetes 群集

1. 在 Azure Kubernetes 服务中创建 Kubernetes 群集。 以下命令在 wingtiptoys-kubernetes 资源组中创建 kubernetes 群集，将 wingtiptoys-akscluster 作为群集名称，wingtiptoys-kubernetes 作为 DNS 前缀     ：
   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \ 
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```
   此命令可能需要一段时间才能完成。

1. 将 Azure 容器注册表 (ACR) 与 Azure Kubernetes 服务 (AKS) 配合使用时，需要向 Azure Kubernetes 服务授予对 Azure 容器注册表的拉取访问权限。 创建 Azure Kubernetes 服务时，Azure 会创建一个默认的服务主体。 请在 bash 或 Powershell 中运行以下脚本以授予 AKS 对 ACR 的访问权限，可以在[使用 Azure 容器注册表从 Azure Kubernetes 服务进行身份验证]中查看更多详细信息。

```bash
   # Get the id of the service principal configured for AKS
   CLIENT_ID=$(az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv)
   
   # Get the ACR registry resource id
   ACR_ID=$(az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv)
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

  - 或 -

```PowerShell
   # Get the id of the service principal configured for AKS
   $CLIENT_ID = az aks show -g wingtiptoys-kubernetes -n wingtiptoys-akscluster --query "servicePrincipalProfile.clientId" --output tsv
   
   # Get the ACR registry resource id
   $ACR_ID = az acr show -g wingtiptoys-kubernetes -n wingtiptoysregistry --query "id" --output tsv
   
   # Create role assignment
   az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

1. 使用 Azure CLI 安装 `kubectl`。 Linux 用户可能必须将 `sudo` 作为此命令的前缀，因为它将 Kubernetes CLI 部署到 `/usr/local/bin`。
   ```azurecli
   az aks install-cli
   ```

1. 下载群集配置信息，以便从 Kubernetes Web 界面和 `kubectl` 管理群集。 
   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>将映像部署到 Kubernetes 群集

本教程使用 `kubectl` 部署应用，以便让你通过 Kubernetes Web 界面浏览部署。

### <a name="deploy-with-kubectl"></a>使用 kubectl 进行部署

1. 打开命令提示符。

1. 使用 `kubectl run` 命令在 Kubernetes 群集中运行容器。 指定 Kubernetes 中应用的服务名称和完整映像名称。 例如：
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   在此命令中：

   * 容器名称 `gs-spring-boot-docker` 会立即在 `run` 命令后指定

   * `--image` 参数将组合的登录服务器和映像名称指定为 `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. 使用 `kubectl expose` 命令外部公开你的 Kubernetes 群集。 指定你的服务名称、用于访问应用的公开 TCP 端口以及应用侦听的内部目标端口。 例如：
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   在此命令中：

   * 容器名称 `gs-spring-boot-docker` 会立即在 `expose deployment` 命令后指定

   * `--type` 参数指定此群集使用负载均衡器

   * `--port` 参数指定面向公众的 TCP 端口 80。 在此端口访问应用。

   * `--target-port` 参数指定内部 TCP 端口 8080。 负载均衡器在此端口将请求转发到你的应用。

1. 将应用部署到群集后，请查询外部 IP 地址并在 Web 浏览器中打开：

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=default
   ```

   ![在 Azure 上浏览示例应用][SB02]



### <a name="deploy-with-the-kubernetes-web-interface"></a>使用 Kubernetes Web 界面进行部署

1. 打开命令提示符。

1. 在默认浏览器中打开 Kubernetes 群集的配置网站：
   ```
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

1. 在浏览器中打开 Kubernetes 配置网站后，单击“部署容器化应用”的链接  ：

   ![Kubernetes 配置网站][KB01]

1. 显示“资源创建”页时，请指定以下选项  ：

   a. 选择“创建应用”。 

   b. 在“应用名称”中输入 Spring Boot 应用程序名称；例如：gs-spring-boot-docker   。

   c. 在容器映像中输入之前的登录服务器和容器映像；例如：“wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest”   。

   d. 选择“外部”服务   。

   e. 在“端口”和“目标端口”文本框中指定外部和内部端口   。

   ![Kubernetes 配置网站][KB02]


1. 单击“部署”对容器进行部署  。

   ![Kubernetes 部署][KB05]

1. 部署应用程序后，Spring Boot 应用程序将列在“服务”下方  。

   ![Kubernetes 服务][KB06]

1. 若单击“外部终结点”的链接，则可看见 Spring Boot 应用程序在 Azure 上运行  。

   ![Kubernetes 服务][KB07]

   ![在 Azure 上浏览示例应用][SB02]

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)

### <a name="additional-resources"></a>其他资源

有关在 Azure 上使用 Spring Boot 的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-web-app-on-azure.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

有关使用 Visual Studio Code 将 Java 应用程序部署到 Kubernetes 的详细信息，请参阅 [Visual Studio Code Java 教程]。

有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅[Docker 上的 Spring Boot 入门]。

以下链接提供了创建 Spring Boot 应用程序的详细信息：

* 有关创建简单 Spring Boot 应用程序的详细信息，请参阅 https://start.spring.io/ 中的 Spring Initializr。

以下链接提供了将 Kubernetes 与 Azure 配合使用的详细信息：

* [Azure Kubernetes 服务中的 Kubernetes 群集入门](/azure/aks/intro-kubernetes)

有关使用 Kubernetes 命令行接口的详细信息，请在 <https://kubernetes.io/docs/user-guide/kubectl/> 处参阅 **kubectl** 用户指南。

Kubernetes 网站中有多篇文章讨论有关在私有注册表中使用映像的信息：

* [为 Pod 配置服务帐户]
* [命名空间]
* [从私有注册表拉取映像]

有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。

有关使用 Azure Dev Spaces 直接在 Azure Kubernetes 服务 (AKS) 中迭代运行和调试容器的详细信息，请参阅[通过 Java 开始使用 Azure Dev Spaces]

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure Kubernetes 服务 (AKS)]: https://azure.microsoft.com/services/kubernetes-service/
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[为 Pod 配置服务帐户]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[命名空间]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[从私有注册表拉取映像]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[使用 Azure 容器注册表从 Azure Kubernetes 服务进行身份验证]: /azure/container-registry/container-registry-auth-aks/
[Visual Studio Code Java 教程]: https://code.visualstudio.com/docs/java/java-kubernetes/
[通过 Java 开始使用 Azure Dev Spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: ./media/deploy-spring-boot-java-app-on-kubernetes/KB07.png
