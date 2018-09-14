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
ms.openlocfilehash: 1eb0e7d7a718a1c106adebbf89011f6e3fa1504e
ms.sourcegitcommit: c2019ba6da6c7c28b17b5a85f89e49bb5e570ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040245"
---
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a>将基于 Java 的 MicroProfile 服务部署到用于容器的 Azure Web 应用

MicroProfile 是生成可快速轻松部署到[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)等服务的极小型 Java 应用程序的极佳手段。 在本教程中，我们将创建一个简单的基于 MicroProfile 的微服务，将其容器化为 Docker 容器并部署到 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)，然后使用用于容器的 Azure Web 应用来托管它。

> [!NOTE]
>
> 只要 Docker 容器映像可自我执行（即包括运行时），此过程就适用于 MicroProfile.io 的任何实现。

更具体地说，本示例使利用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 创建一个微型 Java war 文件（在创作者的计算机上为 5,085 字节），然后将其打包到 Docker 映像中（大约为 174 MB）。 此 Docker 映像包含对此 Web 应用进行完全容器化部署所需的一切内容。

由于 Docker 的工作方式，每当应用程序源代码发生更改时，通常并不需要重新部署 174 MB 的整个 Docker 映像，因为 Docker 只会上传有差异的内容（比原始映像要小得多）。 这样，通过 CI/CD 管道执行 MicroProfile 应用程序新版本的过程就会变得极其高效快速，并可减少冲突，实现快速开发迭代。

在学习本教程的过程中，我们首先创建并在本地运行代码，然后将其部署为 Azure 上的 Web 应用。 对于这两项操作，我们将依赖于 Docker 来简化和标准化工作。 在开始之前，我们将创建一个 Azure 容器注册表用于存储 Docker 容器。

## <a name="creating-an-azure-container-registry"></a>创建 Azure 容器注册表

我们将使用 [Azure 门户](http://portal.azure.com)来创建 Azure 容器注册表，但请注意，也可以使用其他选项，例如 Azure CLI。 遵循以下步骤创建新的 Azure 容器注册表：

1. 登录到 [Azure 门户](http://portal.azure.com)并创建新的 Azure 容器注册表资源。 提供注册表名称（请注意，这是应在 `pom.xml` 中设置为 `docker.registry` 属性的名称）。 根据需要更改默认值，然后单击“创建”。

1. 激活容器注册表后（单击“创建”后等待大约 30 秒），单击该容器注册表，然后单击左侧菜单区域中的“访问密钥”链接。 在这里，需要启用“管理员用户”设置，以便能够从计算机（Docker 容器将推送到其中）访问此容器注册表；另外，需要启用从稍后将要设置的用于容器的 Azure Web 应用实例进行的访问。

1. 在“访问密钥”区域中，记下 `username` 和 `password`值。 稍后需要将这些值复制/粘贴到全局 Maven `settings.xml` 文件（有关 Maven 设置的详细信息，请参阅 [Apache Maven 项目](https://maven.apache.org/settings.html)网站）。 此处提供了创作者系统上 `${user.home}/.m2/settings.xml` 文件的模糊版本供参考：

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

完成此操作后，可以继续生成 MicroProfile 应用程序并在本地运行。

## <a name="creating-our-microprofile-application"></a>创建 MicroProfile 应用程序

本示例基于 GitHub 上提供的示例应用程序，因此我们将克隆该应用程序，然后逐步执行代码。 遵循以下步骤将克隆的代码提取到计算机：

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

此目录中包含一个 `pom.xml` 文件，此文件用于以 Maven 生成工具使用的格式指定项目。 可根据自己的需要编辑此文件。 具体而言，应将 `docker.registry` 和 `docker.name` 属性更改为在设置 Azure 容器注册表时创建的 `docker.registry` 和 `docker.name`。

在此目录中，需要注意的另一个文件是 Dockerfile，下面再现了此文件：

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

此 Dockerfile 只是基于 Payara Micro Docker 容器创建新的 Docker 容器，并在其中复制生成过程中创建的 .war 文件。 此外，它还公开端口 8080，以便在 Docker 容器中启动并运行服务后，我们可以访问该服务。

深入到 `src` 目录，最终会发现下面再现的 `Application` 类：

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

`@ApplicationPath("/api")` 注释指定此微服务的基础终结点 - 即，所有终结点的 `/api` 将出现在访问任何特定 REST 终结点所需的 URL 的其余部分的前面。

`api` 包的内部是名为 `API` 的类，其中包含以下代码：

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

使用 `@Path("/helloworld")` 注释可以看到，此 REST 终结点与 `Application` 类中指定的 `/api` 结合后变成了 `/api/helloworld`。 使用 HTTP GET 请求调用此终结点时，可以看到此方法会生成文本/html，事实上它只是一个硬编码的字符串“Hello, world!”。

现在，我们已了解使用 MicroProfile 创建微服务所需的全部代码。 接下来，我们可以使用 Maven 来生成它，将它容器化为 Docker 容器，然后在本地运行。 可使用以下步骤执行该操作：

1. 运行 `mvn clean package` 并等到该命令成功完成。

1. 运行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`，例如，如果 `docker.registry` 是 `jogilescr.azurecr.io`，`docker.name` 是 `samples/docker-helloworld`，则运行 `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`。

1. 尝试在 Web 浏览器中访问 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。 如果看到了预期的“Hello, world!” 响应（以及 [/health](http://localhost:8080/health) 终结点的运行状况相关信息），则表示已在本地计算机上成功部署了该 MicroProfile 应用程序。

## <a name="pushing-to-the-azure-container-registry"></a>推送到 Azure 容器注册表

在本地计算机上成功生成并运行 MicroProfile 应用程序后，下一步是将此容器推送到容器注册表中。 本教程使用 Azure 容器注册表，但可以使用任何容器注册表（前提是将 `pom.xml` 文件编辑为指向相关位置）。

1. 运行 `mvn clean package` 来清理、编译并创建本地 Docker 映像。
2. 运行 `mvn dockerfile:push` 以推送到 Azure 容器注册表。

在此阶段，Docker 容器映像已上传到 Azure 容器注册表但尚未运行，因为我们必须将其部署到用于容器的 Azure Web 应用实例。 现在我们就这样做。

## <a name="creating-an-azure-web-app-for-containers-instance"></a>创建用于容器的 Azure Web 应用实例

1. 返回 [Azure 门户](http://portal.azure.com)，创建新的用于容器的 Web 应用实例（在菜单中的“Web + 移动”标题下）。 几个要点：

   1. 此处指定的名称将是 Web 应用的公共 URL（不过稍后可根据需要添加自定义域），因此最好选择一个容易记住的名称。

   1. 转到“配置容器”部分时，可为“映像源”选择“Azure 容器注册表”，然后从下拉列表中选择正确的映像。

   1. 不需要在“启动文件”字段中指定任何值。

1. 创建实例后（同样，此过程也很快速），请单击它，然后单击“应用程序设置”菜单项。 此处需要添加一个新的应用程序设置，其中的键为 `WEBSITES_PORT`，值为 `8080`。 这会告知 Azure 你要在容器中公开哪个端口，该端口将在外部映射到端口 80。

1. （可选）单击“Docker 容器”链接并启用“持续部署”，以便每次更新 Azure 容器注册表映像时，会在用于容器的 Azure Web 应用实例中更新该映像。

1. 现在，应该可以访问 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上的 Azure 托管实例。

## <a name="summary"></a>摘要

本教程逐步讲解了创建简单的基于 MicroProfile 的微服务、将其容器化为 Docker 容器、在本地运行并将其发布到 Azure 的整个过程。 扩展微服务以提供更多有用功能的过程超出了本教程的范畴，但是，Internet 上提供了大量有关 MicroProfile 的教程和建议，我们建议读者查看 [MicroProfile.io](https://microprofile.io/) 来获取更多内容。
