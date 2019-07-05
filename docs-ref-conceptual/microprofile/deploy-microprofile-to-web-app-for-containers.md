---
title: 将基于 Java 的 MicroProfile 服务部署到用于容器的 Azure Web 应用
description: 了解如何使用 Docker 和用于容器的 Azure Web 应用部署 MicroProfile 服务
services: container-registry;app-service
documentationcenter: java
author: jonathangiles
manager: routlaw
editor: jonathangiles
ms.assetid: ''
ms.author: jogiles
ms.date: 09/07/2018
ms.devlang: java
ms.service: container-registry;app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.openlocfilehash: 4ecfbf92bc30bc491c991e30111cce8375da7f1a
ms.sourcegitcommit: f8faa4a14c714e148c513fd46f119524f3897abf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533592"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>将基于 Java 的 MicroProfile 服务部署到用于容器的 Azure Web 应用

MicroProfile 是生成可快速轻松部署到[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)等服务的极小型 Java 应用程序的极佳手段。 在本教程中，我们将创建一个简单的基于 MicroProfile 的微服务，将其容器化为 Docker 容器并部署到 [Azure 容器注册表实例](https://azure.microsoft.com/services/container-registry/)，然后使用用于容器的 Azure Web 应用来托管它。

> [!NOTE]
> 只要 Docker 容器映像可自我执行（即映像包括运行时），此过程就适用于 MicroProfile.io 的任何实现。

此示例使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 创建一个小型（大约 5,000 字节）Java Web 应用程序要求 (WAR) 文件，然后将其打包到大约 174 兆字节 (MB) 的 Docker 映像中。 此 Docker 映像包含对此 Web 应用进行完全容器化部署所需的一切内容。

每当应用程序源代码发生更改时，并不需要重新部署 174 MB 的整个 Docker 映像，因为 Docker 只会上传有差异的内容（比原始映像要小得多）。 因此，通过 CI/CD 管道执行 MicroProfile 应用程序新版本的过程就会变得极其高效快速，并可减少冲突，实现快速开发迭代。

在学习本教程的过程中，我们首先创建并在本地运行代码，然后将其部署为 Azure 上的 Web 应用。 在这两个阶段，我们都可以依赖 Docker 来简化和标准化工作。 在开始之前，我们将创建一个 Azure 容器注册表实例来存储 Docker 容器。

## <a name="create-an-azure-container-registry-instance"></a>创建 Azure 容器注册表实例

可使用 [Azure 门户](http://portal.azure.com)来创建 Azure 容器注册表实例，但也可使用其他选项，例如 Azure CLI。 若要创建新的 Azure 容器注册表实例，请执行以下操作：

1. 登录到 [Azure 门户](http://portal.azure.com)并创建新的 Azure 容器注册表资源。 提供注册表名称（应在 *pom.xml* 文件中将此名称设置为 `docker.registry` 属性）。 根据需要更改默认值，然后选择“创建”。 

1. 在大约 30 秒的时间内，当容器注册表实例处于激活状态后，请将其选中，然后在左窗格中选择“访问密钥”链接。  

    请务必启用“管理员用户”设置，以便能够从计算机访问此容器注册表实例并将 Docker 容器推送到其中。  这样做还可以从很快就要设置的用于容器的 Azure Web 应用实例进行访问。

1. 在“访问密钥”窗格中复制“用户名”和“密码”值，然后将其粘贴到全局 Maven *settings.xml* 文件中。    有关 Maven 设置的详细信息，请访问 [Apache Maven 项目](https://maven.apache.org/settings.html)网站。 

   下面是 *${user.home}/.m2/settings.xml* 文件的示例，供参考：

    ```xml
    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
          <server>
            <id>jogilescr.azurecr.io</id>
            <username>jogilescr</username>
            <password>ojoirshois.this-isn't-real.hrihslirhlishrglih</password>
          </server>
        </servers>
    </settings>
    ```

创建容器注册表实例以后，即可继续生成 MicroProfile 应用程序并在本地运行它。

## <a name="create-your-microprofile-application"></a>创建 MicroProfile 应用程序

本示例代码基于 GitHub 上提供的示例应用程序。 若要将代码克隆到计算机中，请输入以下命令：

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

此目录包含一个 *pom.xml* 文件，该文件可以用来以 Maven 生成工具所使用的格式指定项目。 可根据自己的需要编辑此文件。 具体而言，应将 `docker.registry` 和 `docker.name` 属性更改为在设置 Azure 容器注册表实例时创建的 `docker.registry` 和 `docker.name` 属性。

在此目录中，需要注意的另一个文件是 Dockerfile，下面再现了此文件：

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

此 Dockerfile 基于 Payara Micro Docker 容器创建新的 Docker 容器， 并在 WAR 文件（已在生成过程中创建）中进行复制。 它还公开端口 8080，这样，当服务在 Docker 容器中启动并运行以后，我们便可以访问服务。

打开 *src* 目录以后，最终会发现下面再现的 `Application` 类：

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

*@ApplicationPath("/api")* 注释指定此微服务的基础终结点。 即，所有终结点的 */api* 将出现在访问任何特定 REST 终结点所需的 URL 的其余部分的前面。

*api* 包的内部是名为 `API` 的类，其中包含以下代码：

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld.api;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

import static javax.ws.rs.core.MediaType.TEXT_HTML;

@ApplicationScoped
@Path("/")
public class API {

    @GET
    @Path("/helloworld")
    @Produces(TEXT_HTML)
    public String info() {
        return "Hello, world!";
    }
}
```

使用 *@Path("/helloworld")* 注释可以看到，此 REST 终结点与 `Application` 类中指定的 */api* 结合后变成了 */api/helloworld*。 使用 HTTP GET 请求调用此终结点时，可以看到此方法会生成文本/html，事实上它只是一个硬编码的字符串“Hello, world!”

到目前为止，本文已介绍使用 MicroProfile 创建微服务所需的全部代码。 现在可以执行以下步骤，使用 Maven 来生成它，将它容器化为 Docker 容器，然后在本地运行它：

1. 运行 `mvn clean package` 并等到该命令完成。

1. 运行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`。 例如 *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*，其中 *\<docker.registry>* 为 *jogilescr.azurecr.io*， *\<docker.name>* 为 *samples/docker-helloworld*。

1. 尝试在 Web 浏览器中访问 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。 如果看到了预期的“Hello, world!” 响应（以及 [/health](http://localhost:8080/health) 终结点的运行状况相关信息），则表示已在本地计算机上成功部署了该 MicroProfile 应用程序。

## <a name="push-the-container-to-the-azure-container-registry-instance"></a>向 Azure 容器注册表实例推送容器

在本地计算机上成功生成并运行 MicroProfile 应用程序后，即可将此容器推送到容器注册表实例中。 

> [!NOTE]
> 虽然本文使用 Azure 容器注册表实例，但任何容器注册表实例均可使用，只要将 *pom.xml* 文件编辑为指向相关位置即可。

1. 若要清理、编译并创建本地 Docker 映像，请运行 `mvn clean package`。
2. 若要向 Azure 容器注册表实例推送容器，请运行 `mvn dockerfile:push`。

现在已将 Docker 容器映像上传到 Azure 容器注册表实例。 但是，该映像尚未运行。 现在必须将它部署到用于容器的 Azure Web 应用实例。 

## <a name="create-an-azure-web-app-for-containers-instance"></a>创建用于容器的 Azure Web 应用实例

1. 在 [Azure 门户](http://portal.azure.com)的左窗格中选择“Web + 移动”，然后执行以下操作： 

   a. 指定名称。 此名称将成为 Web 应用的公共 URL，因此最好选择一个容易记住的名称。 可以稍后根据需要添加自定义域名。

   b. 在“配置容器”部分的“映像源”下选择“Azure 容器注册表”，然后在下拉列表中选择正确的映像。   

   c. 不需在“启动文件”字段中指定值。 

1. 在创建实例后将其选中，然后选择“应用程序设置”  。 输入 **WEBSITES_PORT** 作为**键**，然后输入 **8080** 作为**值**。 这些设置告知 Azure 要在容器中公开哪个端口，然后在外部将该端口映射到端口 80。

1. （可选）选择“Docker 容器”链接，然后启用“持续部署”   。 这样，每次更新 Azure 容器注册表实例映像时，就会在用于容器的 Azure Web 应用实例中自动更新该映像。

1. 现在，应该可以访问 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上的 Azure 托管实例了。

## <a name="summary"></a>摘要

本教程逐步讲解了创建简单的基于 MicroProfile 的微服务、将其容器化为 Docker 容器、在本地运行并将其发布到 Azure 的整个过程。 可以扩展微服务，让其提供其他有用的功能。 有关详细信息，请访问 [MicroProfile.io](https://microprofile.io/)。
