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
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="5e363-103">将基于 Java 的 MicroProfile 服务部署到用于容器的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="5e363-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="5e363-104">MicroProfile 是生成可快速轻松部署到[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)等服务的极小型 Java 应用程序的极佳手段。</span><span class="sxs-lookup"><span data-stu-id="5e363-104">MicroProfile is a great way to build exceedingly tiny Java applications that can be quickly and easily deployed to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="5e363-105">在本教程中，我们将创建一个简单的基于 MicroProfile 的微服务，将其容器化为 Docker 容器并部署到 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)，然后使用用于容器的 Azure Web 应用来托管它。</span><span class="sxs-lookup"><span data-stu-id="5e363-105">In this tutorial we will create a simple MicroProfile-based microservice that is then containerized into a Docker container, deployed into an [Azure Container Registry](https://azure.microsoft.com/services/container-registry/), and then hosted using Azure Web App for Containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5e363-106">只要 Docker 容器映像可自我执行（即包括运行时），此过程就适用于 MicroProfile.io 的任何实现。</span><span class="sxs-lookup"><span data-stu-id="5e363-106">This procedure works with any implementation of MicroProfile.io as long the Docker container image is self-executable (i.e. includes the runtime).</span></span>

<span data-ttu-id="5e363-107">更具体地说，本示例使利用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 创建一个微型 Java war 文件（在创作者的计算机上为 5,085 字节），然后将其打包到 Docker 映像中（大约为 174 MB）。</span><span class="sxs-lookup"><span data-stu-id="5e363-107">More concretely, this sample makes use of [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a tiny Java war file (5,085 bytes on the authors machine), and then packages it up into a Docker image (which is approximately 174 megabytes).</span></span> <span data-ttu-id="5e363-108">此 Docker 映像包含对此 Web 应用进行完全容器化部署所需的一切内容。</span><span class="sxs-lookup"><span data-stu-id="5e363-108">This Docker image contains everything necessary for a fully-containerised deployment of this webapp.</span></span>

<span data-ttu-id="5e363-109">由于 Docker 的工作方式，每当应用程序源代码发生更改时，通常并不需要重新部署 174 MB 的整个 Docker 映像，因为 Docker 只会上传有差异的内容（比原始映像要小得多）。</span><span class="sxs-lookup"><span data-stu-id="5e363-109">Because of the way Docker works, it is often the case that the entire 174 megabyte Docker image does not need to be redeployed whenever the application source code is changed, as Docker will only upload the differences (which is significantly smaller).</span></span> <span data-ttu-id="5e363-110">这样，通过 CI/CD 管道执行 MicroProfile 应用程序新版本的过程就会变得极其高效快速，并可减少冲突，实现快速开发迭代。</span><span class="sxs-lookup"><span data-stu-id="5e363-110">This makes the process of executing a new release of a MicroProfile application via a CI/CD pipeline extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="5e363-111">在学习本教程的过程中，我们首先创建并在本地运行代码，然后将其部署为 Azure 上的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="5e363-111">We will work through this tutorial firstly by creating and running the code locally, and then we will deploy this as a web app on Azure.</span></span> <span data-ttu-id="5e363-112">对于这两项操作，我们将依赖于 Docker 来简化和标准化工作。</span><span class="sxs-lookup"><span data-stu-id="5e363-112">In both cases we will depend on Docker to simplify and standardize our efforts.</span></span> <span data-ttu-id="5e363-113">在开始之前，我们将创建一个 Azure 容器注册表用于存储 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="5e363-113">Before we begin, we will create an Azure Container Registry to store our Docker containers in.</span></span>

## <a name="creating-an-azure-container-registry"></a><span data-ttu-id="5e363-114">创建 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="5e363-114">Creating an Azure Container Registry</span></span>

<span data-ttu-id="5e363-115">我们将使用 [Azure 门户](http://portal.azure.com)来创建 Azure 容器注册表，但请注意，也可以使用其他选项，例如 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="5e363-115">We will use the [Azure Portal](http://portal.azure.com) for creating the Azure Container Registry, but note that there are alternate choices such as the Azure CLI.</span></span> <span data-ttu-id="5e363-116">遵循以下步骤创建新的 Azure 容器注册表：</span><span class="sxs-lookup"><span data-stu-id="5e363-116">Follow the steps below to create a new Azure Container Registry:</span></span>

1. <span data-ttu-id="5e363-117">登录到 [Azure 门户](http://portal.azure.com)并创建新的 Azure 容器注册表资源。</span><span class="sxs-lookup"><span data-stu-id="5e363-117">Log in to the [Azure Portal](http://portal.azure.com) and create a new Azure Container Registry resource.</span></span> <span data-ttu-id="5e363-118">提供注册表名称（请注意，这是应在 `pom.xml` 中设置为 `docker.registry` 属性的名称）。</span><span class="sxs-lookup"><span data-stu-id="5e363-118">Provide a registry name (note that this is the name that should be set as the `docker.registry` property in `pom.xml`).</span></span> <span data-ttu-id="5e363-119">根据需要更改默认值，然后单击“创建”。</span><span class="sxs-lookup"><span data-stu-id="5e363-119">Change the defaults as you wish, and then click 'create'.</span></span>

1. <span data-ttu-id="5e363-120">激活容器注册表后（单击“创建”后等待大约 30 秒），单击该容器注册表，然后单击左侧菜单区域中的“访问密钥”链接。</span><span class="sxs-lookup"><span data-stu-id="5e363-120">Once the container registry is live (which is about 30 seconds after clicking 'create'), click on the container registry, and click on the 'Access keys' link in the left-menu area.</span></span> <span data-ttu-id="5e363-121">在这里，需要启用“管理员用户”设置，以便能够从计算机（Docker 容器将推送到其中）访问此容器注册表；另外，需要启用从稍后将要设置的用于容器的 Azure Web 应用实例进行的访问。</span><span class="sxs-lookup"><span data-stu-id="5e363-121">In here, you need to enable the 'admin user' setting, so that this container registry can be accessed from our machines (to push docker containers into), and also to enable access from the Azure Web Apps for Containers instance we will setup soon.</span></span>

1. <span data-ttu-id="5e363-122">在“访问密钥”区域中，记下 `username` 和 `password`值。</span><span class="sxs-lookup"><span data-stu-id="5e363-122">Whilst you are in the 'Access keys' area, note the `username` and `password` values.</span></span> <span data-ttu-id="5e363-123">稍后需要将这些值复制/粘贴到全局 Maven `settings.xml` 文件（有关 Maven 设置的详细信息，请参阅 [Apache Maven 项目](https://maven.apache.org/settings.html)网站）。</span><span class="sxs-lookup"><span data-stu-id="5e363-123">We will copy / paste these into our global Maven `settings.xml` file  (for more information on Maven settings, refer to the [Apache Maven Project](https://maven.apache.org/settings.html) website).</span></span> <span data-ttu-id="5e363-124">此处提供了创作者系统上 `${user.home}/.m2/settings.xml` 文件的模糊版本供参考：</span><span class="sxs-lookup"><span data-stu-id="5e363-124">For reference, here is an obfuscated version of the `${user.home}/.m2/settings.xml` file on the authors system:</span></span>

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

<span data-ttu-id="5e363-125">完成此操作后，可以继续生成 MicroProfile 应用程序并在本地运行。</span><span class="sxs-lookup"><span data-stu-id="5e363-125">Now that this is complete, we can move on with building and running our MicroProfile application locally.</span></span>

## <a name="creating-our-microprofile-application"></a><span data-ttu-id="5e363-126">创建 MicroProfile 应用程序</span><span class="sxs-lookup"><span data-stu-id="5e363-126">Creating our MicroProfile application</span></span>

<span data-ttu-id="5e363-127">本示例基于 GitHub 上提供的示例应用程序，因此我们将克隆该应用程序，然后逐步执行代码。</span><span class="sxs-lookup"><span data-stu-id="5e363-127">This example is based on a sample application available on GitHub, so we will clone that and then step through the code.</span></span> <span data-ttu-id="5e363-128">遵循以下步骤将克隆的代码提取到计算机：</span><span class="sxs-lookup"><span data-stu-id="5e363-128">Follow the steps below to get the code cloned onto your machine:</span></span>

1. `git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git`
1. `cd microprofile-docker-helloworld`

<span data-ttu-id="5e363-129">此目录中包含一个 `pom.xml` 文件，此文件用于以 Maven 生成工具使用的格式指定项目。</span><span class="sxs-lookup"><span data-stu-id="5e363-129">In this directory there is a `pom.xml` file that is used to specify the project in the format used by the Maven build tool.</span></span> <span data-ttu-id="5e363-130">可根据自己的需要编辑此文件。</span><span class="sxs-lookup"><span data-stu-id="5e363-130">This file can be edited to suit your own needs.</span></span> <span data-ttu-id="5e363-131">具体而言，应将 `docker.registry` 和 `docker.name` 属性更改为在设置 Azure 容器注册表时创建的 `docker.registry` 和 `docker.name`。</span><span class="sxs-lookup"><span data-stu-id="5e363-131">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` created when the Azure Container Registry was setup.</span></span>

<span data-ttu-id="5e363-132">在此目录中，需要注意的另一个文件是 Dockerfile，下面再现了此文件：</span><span class="sxs-lookup"><span data-stu-id="5e363-132">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="5e363-133">此 Dockerfile 只是基于 Payara Micro Docker 容器创建新的 Docker 容器，并在其中复制生成过程中创建的 .war 文件。</span><span class="sxs-lookup"><span data-stu-id="5e363-133">This Dockerfile simply creates a new Docker container based on the Payara Micro Docker Container, and copies in the .war file that is created as part of our build process.</span></span> <span data-ttu-id="5e363-134">此外，它还公开端口 8080，以便在 Docker 容器中启动并运行服务后，我们可以访问该服务。</span><span class="sxs-lookup"><span data-stu-id="5e363-134">It also exposes port 8080 so that we may access the service once it is up and running within a Docker container.</span></span>

<span data-ttu-id="5e363-135">深入到 `src` 目录，最终会发现下面再现的 `Application` 类：</span><span class="sxs-lookup"><span data-stu-id="5e363-135">Diving into the `src` directory, we will eventually discover the `Application` class reproduced below:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="5e363-136">`@ApplicationPath("/api")` 注释指定此微服务的基础终结点 - 即，所有终结点的 `/api` 将出现在访问任何特定 REST 终结点所需的 URL 的其余部分的前面。</span><span class="sxs-lookup"><span data-stu-id="5e363-136">The `@ApplicationPath("/api")` annotation specifies the base endpoint for this microservice - that is, that all endpoints will have `/api` preceed the rest of the URL required to access any specific REST endpoint.</span></span>

<span data-ttu-id="5e363-137">`api` 包的内部是名为 `API` 的类，其中包含以下代码：</span><span class="sxs-lookup"><span data-stu-id="5e363-137">Inside the `api` package is a class named `API`, which contains the following code:</span></span>

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

<span data-ttu-id="5e363-138">使用 `@Path("/helloworld")` 注释可以看到，此 REST 终结点与 `Application` 类中指定的 `/api` 结合后变成了 `/api/helloworld`。</span><span class="sxs-lookup"><span data-stu-id="5e363-138">Through the use of the `@Path("/helloworld")` annotation, we can see that this REST endpoint, when combined with the `/api` specified in the `Application` class, will be `/api/helloworld`.</span></span> <span data-ttu-id="5e363-139">使用 HTTP GET 请求调用此终结点时，可以看到此方法会生成文本/html，事实上它只是一个硬编码的字符串“Hello, world!”。</span><span class="sxs-lookup"><span data-stu-id="5e363-139">When this endpoint is called using an HTTP GET request, we can see that the method will produce text/html, and in fact it is simply a hard-coded string "Hello, world!".</span></span>

<span data-ttu-id="5e363-140">现在，我们已了解使用 MicroProfile 创建微服务所需的全部代码。</span><span class="sxs-lookup"><span data-stu-id="5e363-140">We have now covered all the code required to create a microservice using MicroProfile.</span></span> <span data-ttu-id="5e363-141">接下来，我们可以使用 Maven 来生成它，将它容器化为 Docker 容器，然后在本地运行。</span><span class="sxs-lookup"><span data-stu-id="5e363-141">We can now use Maven to build it, containerize it into a Docker container, and run it locally.</span></span> <span data-ttu-id="5e363-142">可使用以下步骤执行该操作：</span><span class="sxs-lookup"><span data-stu-id="5e363-142">We can do that with the following steps:</span></span>

1. <span data-ttu-id="5e363-143">运行 `mvn clean package` 并等到该命令成功完成。</span><span class="sxs-lookup"><span data-stu-id="5e363-143">Run `mvn clean package` and wait until it successfully completes.</span></span>

1. <span data-ttu-id="5e363-144">运行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`，例如，如果 `docker.registry` 是 `jogilescr.azurecr.io`，`docker.name` 是 `samples/docker-helloworld`，则运行 `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`。</span><span class="sxs-lookup"><span data-stu-id="5e363-144">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`, for example, `docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest`, if your `docker.registry` is `jogilescr.azurecr.io` and `docker.name` is `samples/docker-helloworld`.</span></span>

1. <span data-ttu-id="5e363-145">尝试在 Web 浏览器中访问 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。</span><span class="sxs-lookup"><span data-stu-id="5e363-145">Try accessing [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="5e363-146">如果看到了预期的“Hello, world!”</span><span class="sxs-lookup"><span data-stu-id="5e363-146">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="5e363-147">响应（以及 [/health](http://localhost:8080/health) 终结点的运行状况相关信息），则表示已在本地计算机上成功部署了该 MicroProfile 应用程序。</span><span class="sxs-lookup"><span data-stu-id="5e363-147">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you have successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="pushing-to-the-azure-container-registry"></a><span data-ttu-id="5e363-148">推送到 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="5e363-148">Pushing to the Azure Container Registry</span></span>

<span data-ttu-id="5e363-149">在本地计算机上成功生成并运行 MicroProfile 应用程序后，下一步是将此容器推送到容器注册表中。</span><span class="sxs-lookup"><span data-stu-id="5e363-149">Now that we have successfully built and run our MicroProfile application on our local machine, the next step is to push this container into our container registry.</span></span> <span data-ttu-id="5e363-150">本教程使用 Azure 容器注册表，但可以使用任何容器注册表（前提是将 `pom.xml` 文件编辑为指向相关位置）。</span><span class="sxs-lookup"><span data-stu-id="5e363-150">In this tutorial we are using the Azure Container Registry, but any container registry will work (as long as the `pom.xml` file is edited to point to the relevant location).</span></span>

1. <span data-ttu-id="5e363-151">运行 `mvn clean package` 来清理、编译并创建本地 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="5e363-151">Run `mvn clean package` to clean, compile, and create a local docker image.</span></span>
2. <span data-ttu-id="5e363-152">运行 `mvn dockerfile:push` 以推送到 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="5e363-152">Run `mvn dockerfile:push` to push to the Azure Container Registry.</span></span>

<span data-ttu-id="5e363-153">在此阶段，Docker 容器映像已上传到 Azure 容器注册表但尚未运行，因为我们必须将其部署到用于容器的 Azure Web 应用实例。</span><span class="sxs-lookup"><span data-stu-id="5e363-153">At this stage you now have your docker container image uploaded to the Azure Container Registry, but it is not yet running as we now have to deploy it into an Azure Web App for Containers instance.</span></span> <span data-ttu-id="5e363-154">现在我们就这样做。</span><span class="sxs-lookup"><span data-stu-id="5e363-154">We will now do that.</span></span>

## <a name="creating-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="5e363-155">创建用于容器的 Azure Web 应用实例</span><span class="sxs-lookup"><span data-stu-id="5e363-155">Creating an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="5e363-156">返回 [Azure 门户](http://portal.azure.com)，创建新的用于容器的 Web 应用实例（在菜单中的“Web + 移动”标题下）。</span><span class="sxs-lookup"><span data-stu-id="5e363-156">Return to the [Azure Portal](http://portal.azure.com) and create a new Web App for Containers instance (located under the 'Web + Mobile' heading in the menu).</span></span> <span data-ttu-id="5e363-157">几个要点：</span><span class="sxs-lookup"><span data-stu-id="5e363-157">A few pointers:</span></span>

   1. <span data-ttu-id="5e363-158">此处指定的名称将是 Web 应用的公共 URL（不过稍后可根据需要添加自定义域），因此最好选择一个容易记住的名称。</span><span class="sxs-lookup"><span data-stu-id="5e363-158">The name you specify here will be the public URL of the web app (although a custom domain can be added later if desired), so it is a good idea to pick a name that you can easily remember.</span></span>

   1. <span data-ttu-id="5e363-159">转到“配置容器”部分时，可为“映像源”选择“Azure 容器注册表”，然后从下拉列表中选择正确的映像。</span><span class="sxs-lookup"><span data-stu-id="5e363-159">When you get to the 'Configure container' section, you can select 'Azure Container Registry' for the 'Image source', and then select the correct image from the drop-down lists.</span></span>

   1. <span data-ttu-id="5e363-160">不需要在“启动文件”字段中指定任何值。</span><span class="sxs-lookup"><span data-stu-id="5e363-160">You do not need to specify any value in the 'Startup File' field.</span></span>

1. <span data-ttu-id="5e363-161">创建实例后（同样，此过程也很快速），请单击它，然后单击“应用程序设置”菜单项。</span><span class="sxs-lookup"><span data-stu-id="5e363-161">Once the instance is created (again, it is very quick), click on it and then click on the 'Application Settings' menu item.</span></span> <span data-ttu-id="5e363-162">此处需要添加一个新的应用程序设置，其中的键为 `WEBSITES_PORT`，值为 `8080`。</span><span class="sxs-lookup"><span data-stu-id="5e363-162">In here you need to add a new application setting, where the key is `WEBSITES_PORT` and the value is `8080`.</span></span> <span data-ttu-id="5e363-163">这会告知 Azure 你要在容器中公开哪个端口，该端口将在外部映射到端口 80。</span><span class="sxs-lookup"><span data-stu-id="5e363-163">This tells Azure which port you want to expose in the container, and it will be mapped to port 80 externally.</span></span>

1. <span data-ttu-id="5e363-164">（可选）单击“Docker 容器”链接并启用“持续部署”，以便每次更新 Azure 容器注册表映像时，会在用于容器的 Azure Web 应用实例中更新该映像。</span><span class="sxs-lookup"><span data-stu-id="5e363-164">Optionally, click on the 'Docker Container' link, and enable 'Continuous Deployment', so that whenever you update the Azure Container Registry image it is automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="5e363-165">现在，应该可以访问 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上的 Azure 托管实例。</span><span class="sxs-lookup"><span data-stu-id="5e363-165">You should be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="5e363-166">摘要</span><span class="sxs-lookup"><span data-stu-id="5e363-166">Summary</span></span>

<span data-ttu-id="5e363-167">本教程逐步讲解了创建简单的基于 MicroProfile 的微服务、将其容器化为 Docker 容器、在本地运行并将其发布到 Azure 的整个过程。</span><span class="sxs-lookup"><span data-stu-id="5e363-167">Through this tutorial we have stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, and we have run it locally and published it to Azure.</span></span> <span data-ttu-id="5e363-168">扩展微服务以提供更多有用功能的过程超出了本教程的范畴，但是，Internet 上提供了大量有关 MicroProfile 的教程和建议，我们建议读者查看 [MicroProfile.io](https://microprofile.io/) 来获取更多内容。</span><span class="sxs-lookup"><span data-stu-id="5e363-168">Extending our microservice to provide more useful functionality is outside the scope of this tutorial, but there is a wealth of tutorials and advice on MicroProfile on the internet, and readers are encouraged to review [MicroProfile.io](https://microprofile.io/) for more content.</span></span>
