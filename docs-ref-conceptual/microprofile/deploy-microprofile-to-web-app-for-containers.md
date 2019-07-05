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
# <a name="deploy-a-java-based-microprofile-service-to-azure-web-app-for-containers"></a><span data-ttu-id="fad2f-103">将基于 Java 的 MicroProfile 服务部署到用于容器的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="fad2f-103">Deploy a Java-based MicroProfile service to Azure Web App for Containers</span></span>

<span data-ttu-id="fad2f-104">MicroProfile 是生成可快速轻松部署到[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)等服务的极小型 Java 应用程序的极佳手段。</span><span class="sxs-lookup"><span data-stu-id="fad2f-104">MicroProfile is a great way to build exceedingly small Java applications that you can quickly and easily deploy to services such as [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/).</span></span> <span data-ttu-id="fad2f-105">在本教程中，我们将创建一个简单的基于 MicroProfile 的微服务，将其容器化为 Docker 容器并部署到 [Azure 容器注册表实例](https://azure.microsoft.com/services/container-registry/)，然后使用用于容器的 Azure Web 应用来托管它。</span><span class="sxs-lookup"><span data-stu-id="fad2f-105">In this tutorial, you create a simple, MicroProfile-based microservice that you containerize into a Docker container, deploy to an [Azure container registry instance](https://azure.microsoft.com/services/container-registry/), and then host by using Azure Web App for Containers.</span></span>

> [!NOTE]
> <span data-ttu-id="fad2f-106">只要 Docker 容器映像可自我执行（即映像包括运行时），此过程就适用于 MicroProfile.io 的任何实现。</span><span class="sxs-lookup"><span data-stu-id="fad2f-106">This procedure works with any implementation of MicroProfile.io, as long the Docker container image is self-executable (that is, the image includes the runtime).</span></span>

<span data-ttu-id="fad2f-107">此示例使用 [Payara Micro](https://www.payara.fish/payara_micro) 和 [MicroProfile 1.3](https://microprofile.io/) 创建一个小型（大约 5,000 字节）Java Web 应用程序要求 (WAR) 文件，然后将其打包到大约 174 兆字节 (MB) 的 Docker 映像中。</span><span class="sxs-lookup"><span data-stu-id="fad2f-107">In this sample, you use [Payara Micro](https://www.payara.fish/payara_micro) and [MicroProfile 1.3](https://microprofile.io/) to create a small (approximately 5,000 bytes) Java web application requirement (WAR) file, and then package it into a Docker image of approximately 174 megabytes (MB).</span></span> <span data-ttu-id="fad2f-108">此 Docker 映像包含对此 Web 应用进行完全容器化部署所需的一切内容。</span><span class="sxs-lookup"><span data-stu-id="fad2f-108">The Docker image contains everything necessary for a fully containerized deployment of the web app.</span></span>

<span data-ttu-id="fad2f-109">每当应用程序源代码发生更改时，并不需要重新部署 174 MB 的整个 Docker 映像，因为 Docker 只会上传有差异的内容（比原始映像要小得多）。</span><span class="sxs-lookup"><span data-stu-id="fad2f-109">An entire 174 MB Docker image doesn't need to be redeployed whenever the application source code is changed, because Docker uploads only the differences (which are significantly smaller).</span></span> <span data-ttu-id="fad2f-110">因此，通过 CI/CD 管道执行 MicroProfile 应用程序新版本的过程就会变得极其高效快速，并可减少冲突，实现快速开发迭代。</span><span class="sxs-lookup"><span data-stu-id="fad2f-110">Consequently, the process of executing a new release of a MicroProfile application via a CI/CD pipeline is extremely efficient and quick, reducing friction and enabling rapid development iteration.</span></span>

<span data-ttu-id="fad2f-111">在学习本教程的过程中，我们首先创建并在本地运行代码，然后将其部署为 Azure 上的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="fad2f-111">You'll work through this tutorial by first creating and running the code locally and then deploying it as a web app on Azure.</span></span> <span data-ttu-id="fad2f-112">在这两个阶段，我们都可以依赖 Docker 来简化和标准化工作。</span><span class="sxs-lookup"><span data-stu-id="fad2f-112">In both phases, you can depend on Docker to simplify and standardize your efforts.</span></span> <span data-ttu-id="fad2f-113">在开始之前，我们将创建一个 Azure 容器注册表实例来存储 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="fad2f-113">Before you begin, you'll create an Azure container registry instance for storing your Docker containers.</span></span>

## <a name="create-an-azure-container-registry-instance"></a><span data-ttu-id="fad2f-114">创建 Azure 容器注册表实例</span><span class="sxs-lookup"><span data-stu-id="fad2f-114">Create an Azure container registry instance</span></span>

<span data-ttu-id="fad2f-115">可使用 [Azure 门户](http://portal.azure.com)来创建 Azure 容器注册表实例，但也可使用其他选项，例如 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="fad2f-115">You can use the [Azure portal](http://portal.azure.com) to create the Azure container registry instance, but there are other choices also, such as the Azure CLI.</span></span> <span data-ttu-id="fad2f-116">若要创建新的 Azure 容器注册表实例，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="fad2f-116">To create a new Azure container registry instance, do the following:</span></span>

1. <span data-ttu-id="fad2f-117">登录到 [Azure 门户](http://portal.azure.com)并创建新的 Azure 容器注册表资源。</span><span class="sxs-lookup"><span data-stu-id="fad2f-117">Sign in to the [Azure portal](http://portal.azure.com) and create a new Azure container registry resource.</span></span> <span data-ttu-id="fad2f-118">提供注册表名称（应在 *pom.xml* 文件中将此名称设置为 `docker.registry` 属性）。</span><span class="sxs-lookup"><span data-stu-id="fad2f-118">Provide a registry name (this name should be set as the `docker.registry` property in the *pom.xml* file).</span></span> <span data-ttu-id="fad2f-119">根据需要更改默认值，然后选择“创建”。 </span><span class="sxs-lookup"><span data-stu-id="fad2f-119">Change the defaults as you want, and then select **Create**.</span></span>

1. <span data-ttu-id="fad2f-120">在大约 30 秒的时间内，当容器注册表实例处于激活状态后，请将其选中，然后在左窗格中选择“访问密钥”链接。 </span><span class="sxs-lookup"><span data-stu-id="fad2f-120">In about 30 seconds, when the container registry instance is live, select the container registry instance and then, in the left pane, select the **Access keys** link.</span></span> 

    <span data-ttu-id="fad2f-121">请务必启用“管理员用户”设置，以便能够从计算机访问此容器注册表实例并将 Docker 容器推送到其中。 </span><span class="sxs-lookup"><span data-stu-id="fad2f-121">Be sure to enable the *admin user* setting, so that you can access this container registry instance from your machines and push Docker containers into it.</span></span> <span data-ttu-id="fad2f-122">这样做还可以从很快就要设置的用于容器的 Azure Web 应用实例进行访问。</span><span class="sxs-lookup"><span data-stu-id="fad2f-122">Doing so also enables access from the Azure Web App for Containers instance that you'll set up soon.</span></span>

1. <span data-ttu-id="fad2f-123">在“访问密钥”窗格中复制“用户名”和“密码”值，然后将其粘贴到全局 Maven *settings.xml* 文件中。   </span><span class="sxs-lookup"><span data-stu-id="fad2f-123">In the **Access keys** pane, copy the **username** and **password** values and paste them into your global Maven *settings.xml* file.</span></span> <span data-ttu-id="fad2f-124">有关 Maven 设置的详细信息，请访问 [Apache Maven 项目](https://maven.apache.org/settings.html)网站。</span><span class="sxs-lookup"><span data-stu-id="fad2f-124">For more information about Maven settings, go to the [Apache Maven Project](https://maven.apache.org/settings.html) website.</span></span> 

   <span data-ttu-id="fad2f-125">下面是 *${user.home}/.m2/settings.xml* 文件的示例，供参考：</span><span class="sxs-lookup"><span data-stu-id="fad2f-125">For your reference, here's an example of the *${user.home}/.m2/settings.xml* file:</span></span>

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

<span data-ttu-id="fad2f-126">创建容器注册表实例以后，即可继续生成 MicroProfile 应用程序并在本地运行它。</span><span class="sxs-lookup"><span data-stu-id="fad2f-126">Now that you've created your container registry instance, you can move on to building and running your MicroProfile application locally.</span></span>

## <a name="create-your-microprofile-application"></a><span data-ttu-id="fad2f-127">创建 MicroProfile 应用程序</span><span class="sxs-lookup"><span data-stu-id="fad2f-127">Create your MicroProfile application</span></span>

<span data-ttu-id="fad2f-128">本示例代码基于 GitHub 上提供的示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="fad2f-128">This example code is based on a sample application that's available on GitHub.</span></span> <span data-ttu-id="fad2f-129">若要将代码克隆到计算机中，请输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="fad2f-129">To clone the code onto your machine, enter the following commands:</span></span>

```
git clone https://github.com/Azure-Samples/microprofile-docker-helloworld.git

cd microprofile-docker-helloworld
```

<span data-ttu-id="fad2f-130">此目录包含一个 *pom.xml* 文件，该文件可以用来以 Maven 生成工具所使用的格式指定项目。</span><span class="sxs-lookup"><span data-stu-id="fad2f-130">This directory contains a *pom.xml* file that you use to specify the project in the format that's used by the Maven build tool.</span></span> <span data-ttu-id="fad2f-131">可根据自己的需要编辑此文件。</span><span class="sxs-lookup"><span data-stu-id="fad2f-131">You can edit the file to suit your own needs.</span></span> <span data-ttu-id="fad2f-132">具体而言，应将 `docker.registry` 和 `docker.name` 属性更改为在设置 Azure 容器注册表实例时创建的 `docker.registry` 和 `docker.name` 属性。</span><span class="sxs-lookup"><span data-stu-id="fad2f-132">In particular, the `docker.registry` and `docker.name` properties should be changed to the `docker.registry` and `docker.name` properties that were created when you set up the Azure container registry instance.</span></span>

<span data-ttu-id="fad2f-133">在此目录中，需要注意的另一个文件是 Dockerfile，下面再现了此文件：</span><span class="sxs-lookup"><span data-stu-id="fad2f-133">Another file of note in this directory is the Dockerfile, which is reproduced below:</span></span>

```dockerfile
FROM payara/micro

ARG WAR_FILE
COPY target/${WAR_FILE} $DEPLOY_DIR

EXPOSE 8080
```

<span data-ttu-id="fad2f-134">此 Dockerfile 基于 Payara Micro Docker 容器创建新的 Docker 容器，</span><span class="sxs-lookup"><span data-stu-id="fad2f-134">The Dockerfile creates a new Docker container that's based on the Payara Micro Docker Container.</span></span> <span data-ttu-id="fad2f-135">并在 WAR 文件（已在生成过程中创建）中进行复制。</span><span class="sxs-lookup"><span data-stu-id="fad2f-135">It copies in the WAR file that was created as part of your build process.</span></span> <span data-ttu-id="fad2f-136">它还公开端口 8080，这样，当服务在 Docker 容器中启动并运行以后，我们便可以访问服务。</span><span class="sxs-lookup"><span data-stu-id="fad2f-136">It also exposes port 8080 so that you can access the service after it's up and running within a Docker container.</span></span>

<span data-ttu-id="fad2f-137">打开 *src* 目录以后，最终会发现下面再现的 `Application` 类：</span><span class="sxs-lookup"><span data-stu-id="fad2f-137">When you open the *src* directory, you'll eventually discover the `Application` class that's reproduced here:</span></span>

```java
package com.microsoft.azure.samples.microprofile.docker.helloworld;

import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")
public class Application extends javax.ws.rs.core.Application { }
```

<span data-ttu-id="fad2f-138">*@ApplicationPath("/api")* 注释指定此微服务的基础终结点。</span><span class="sxs-lookup"><span data-stu-id="fad2f-138">The *@ApplicationPath("/api")* annotation specifies the base endpoint for this microservice.</span></span> <span data-ttu-id="fad2f-139">即，所有终结点的 */api* 将出现在访问任何特定 REST 终结点所需的 URL 的其余部分的前面。</span><span class="sxs-lookup"><span data-stu-id="fad2f-139">That is, for all endpoints, */api* precedes the rest of the URL that's required to access any specific REST endpoint.</span></span>

<span data-ttu-id="fad2f-140">*api* 包的内部是名为 `API` 的类，其中包含以下代码：</span><span class="sxs-lookup"><span data-stu-id="fad2f-140">Inside the *api* package is a class named `API`, which contains the following code:</span></span>

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

<span data-ttu-id="fad2f-141">使用 *@Path("/helloworld")* 注释可以看到，此 REST 终结点与 `Application` 类中指定的 */api* 结合后变成了 */api/helloworld*。</span><span class="sxs-lookup"><span data-stu-id="fad2f-141">Through the use of the *@Path("/helloworld")* annotation, you can see that this REST endpoint, when it's combined with the */api* that's specified in the `Application` class, will be */api/helloworld*.</span></span> <span data-ttu-id="fad2f-142">使用 HTTP GET 请求调用此终结点时，可以看到此方法会生成文本/html，事实上它只是一个硬编码的字符串“Hello, world!”</span><span class="sxs-lookup"><span data-stu-id="fad2f-142">When you call this endpoint by using an HTTP GET request, you can see that the method produces text/html and, in fact, it's simply a hard-coded string, "Hello, world!"</span></span>

<span data-ttu-id="fad2f-143">到目前为止，本文已介绍使用 MicroProfile 创建微服务所需的全部代码。</span><span class="sxs-lookup"><span data-stu-id="fad2f-143">So far, this article has covered all the code that's required for you to create a microservice by using MicroProfile.</span></span> <span data-ttu-id="fad2f-144">现在可以执行以下步骤，使用 Maven 来生成它，将它容器化为 Docker 容器，然后在本地运行它：</span><span class="sxs-lookup"><span data-stu-id="fad2f-144">You can now use Maven to build it, containerize it into a Docker container, and run it locally by doing the following:</span></span>

1. <span data-ttu-id="fad2f-145">运行 `mvn clean package` 并等到该命令完成。</span><span class="sxs-lookup"><span data-stu-id="fad2f-145">Run `mvn clean package`, and wait until it is complete.</span></span>

1. <span data-ttu-id="fad2f-146">运行 `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`。</span><span class="sxs-lookup"><span data-stu-id="fad2f-146">Run `docker run -it --rm -p 8080:8080 <docker.registry>/<docker.name>:latest`.</span></span> <span data-ttu-id="fad2f-147">例如 *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*，其中 *\<docker.registry>* 为 *jogilescr.azurecr.io*， *\<docker.name>* 为 *samples/docker-helloworld*。</span><span class="sxs-lookup"><span data-stu-id="fad2f-147">For example, *docker run -it --rm -p 8080:8080 jogilescr.azurecr.io/samples/docker-helloworld:latest*, where *\<docker.registry>* is *jogilescr.azurecr.io* and *\<docker.name>* is *samples/docker-helloworld*.</span></span>

1. <span data-ttu-id="fad2f-148">尝试在 Web 浏览器中访问 [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) 和 [http://localhost:8080/health](http://localhost:8080/health)。</span><span class="sxs-lookup"><span data-stu-id="fad2f-148">Try to access [http://localhost:8080/microprofile/api/helloworld](http://localhost:8080/microprofile/api/helloworld) and [http://localhost:8080/health](http://localhost:8080/health) in your web browser.</span></span> <span data-ttu-id="fad2f-149">如果看到了预期的“Hello, world!”</span><span class="sxs-lookup"><span data-stu-id="fad2f-149">If you see the expected "Hello, world!"</span></span> <span data-ttu-id="fad2f-150">响应（以及 [/health](http://localhost:8080/health) 终结点的运行状况相关信息），则表示已在本地计算机上成功部署了该 MicroProfile 应用程序。</span><span class="sxs-lookup"><span data-stu-id="fad2f-150">response (and health-related information for the [/health](http://localhost:8080/health) endpoint), you've successfully deployed the MicroProfile application on your local machine.</span></span>

## <a name="push-the-container-to-the-azure-container-registry-instance"></a><span data-ttu-id="fad2f-151">向 Azure 容器注册表实例推送容器</span><span class="sxs-lookup"><span data-stu-id="fad2f-151">Push the container to the Azure container registry instance</span></span>

<span data-ttu-id="fad2f-152">在本地计算机上成功生成并运行 MicroProfile 应用程序后，即可将此容器推送到容器注册表实例中。</span><span class="sxs-lookup"><span data-stu-id="fad2f-152">Now that you've successfully built and run your MicroProfile application on your local machine, push this container to your container registry instance.</span></span> 

> [!NOTE]
> <span data-ttu-id="fad2f-153">虽然本文使用 Azure 容器注册表实例，但任何容器注册表实例均可使用，只要将 *pom.xml* 文件编辑为指向相关位置即可。</span><span class="sxs-lookup"><span data-stu-id="fad2f-153">Although this article uses an Azure container registry instance, any container registry instance should work, as long as the *pom.xml* file is edited to point to the relevant location.</span></span>

1. <span data-ttu-id="fad2f-154">若要清理、编译并创建本地 Docker 映像，请运行 `mvn clean package`。</span><span class="sxs-lookup"><span data-stu-id="fad2f-154">To clean, compile, and create a local Docker image, run `mvn clean package`.</span></span>
2. <span data-ttu-id="fad2f-155">若要向 Azure 容器注册表实例推送容器，请运行 `mvn dockerfile:push`。</span><span class="sxs-lookup"><span data-stu-id="fad2f-155">To push the container to the Azure container registry instance, run `mvn dockerfile:push`.</span></span>

<span data-ttu-id="fad2f-156">现在已将 Docker 容器映像上传到 Azure 容器注册表实例。</span><span class="sxs-lookup"><span data-stu-id="fad2f-156">You now have your Docker container image uploaded to the Azure container registry instance.</span></span> <span data-ttu-id="fad2f-157">但是，该映像尚未运行。</span><span class="sxs-lookup"><span data-stu-id="fad2f-157">However, it's not yet running.</span></span> <span data-ttu-id="fad2f-158">现在必须将它部署到用于容器的 Azure Web 应用实例。</span><span class="sxs-lookup"><span data-stu-id="fad2f-158">You now have to deploy it to an Azure Web App for Containers instance.</span></span> 

## <a name="create-an-azure-web-app-for-containers-instance"></a><span data-ttu-id="fad2f-159">创建用于容器的 Azure Web 应用实例</span><span class="sxs-lookup"><span data-stu-id="fad2f-159">Create an Azure Web App for Containers instance</span></span>

1. <span data-ttu-id="fad2f-160">在 [Azure 门户](http://portal.azure.com)的左窗格中选择“Web + 移动”，然后执行以下操作： </span><span class="sxs-lookup"><span data-stu-id="fad2f-160">In the [Azure portal](http://portal.azure.com), in the left pane, select **Web + Mobile**, and then do the following:</span></span>

   <span data-ttu-id="fad2f-161">a.</span><span class="sxs-lookup"><span data-stu-id="fad2f-161">a.</span></span> <span data-ttu-id="fad2f-162">指定名称。</span><span class="sxs-lookup"><span data-stu-id="fad2f-162">Specify a name.</span></span> <span data-ttu-id="fad2f-163">此名称将成为 Web 应用的公共 URL，因此最好选择一个容易记住的名称。</span><span class="sxs-lookup"><span data-stu-id="fad2f-163">The name will become the public URL of the web app, so it's a good idea to pick a name that you can easily remember.</span></span> <span data-ttu-id="fad2f-164">可以稍后根据需要添加自定义域名。</span><span class="sxs-lookup"><span data-stu-id="fad2f-164">You can add a custom domain name later, if you want.</span></span>

   <span data-ttu-id="fad2f-165">b.</span><span class="sxs-lookup"><span data-stu-id="fad2f-165">b.</span></span> <span data-ttu-id="fad2f-166">在“配置容器”部分的“映像源”下选择“Azure 容器注册表”，然后在下拉列表中选择正确的映像。   </span><span class="sxs-lookup"><span data-stu-id="fad2f-166">In the **Configure container** section, under **Image source**, select **Azure Container Registry** and then, in the drop-down list, select the correct image.</span></span>

   <span data-ttu-id="fad2f-167">c.</span><span class="sxs-lookup"><span data-stu-id="fad2f-167">c.</span></span> <span data-ttu-id="fad2f-168">不需在“启动文件”字段中指定值。 </span><span class="sxs-lookup"><span data-stu-id="fad2f-168">You don't need to specify a value in the **Startup File** field.</span></span>

1. <span data-ttu-id="fad2f-169">在创建实例后将其选中，然后选择“应用程序设置”  。</span><span class="sxs-lookup"><span data-stu-id="fad2f-169">After you've created the instance, select it, and then select **Application Settings**.</span></span> <span data-ttu-id="fad2f-170">输入 **WEBSITES_PORT** 作为**键**，然后输入 **8080** 作为**值**。</span><span class="sxs-lookup"><span data-stu-id="fad2f-170">For **Key**, enter **WEBSITES_PORT**, and for **Value**, enter **8080**.</span></span> <span data-ttu-id="fad2f-171">这些设置告知 Azure 要在容器中公开哪个端口，然后在外部将该端口映射到端口 80。</span><span class="sxs-lookup"><span data-stu-id="fad2f-171">These settings tell Azure which port to expose in the container and to map it to port 80 externally.</span></span>

1. <span data-ttu-id="fad2f-172">（可选）选择“Docker 容器”链接，然后启用“持续部署”   。</span><span class="sxs-lookup"><span data-stu-id="fad2f-172">(Optional) Select the **Docker Container** link, and then enable **Continuous Deployment**.</span></span> <span data-ttu-id="fad2f-173">这样，每次更新 Azure 容器注册表实例映像时，就会在用于容器的 Azure Web 应用实例中自动更新该映像。</span><span class="sxs-lookup"><span data-stu-id="fad2f-173">By doing so, whenever you update the Azure container registry instance image, it's automatically updated in the Azure Web App for Containers instance.</span></span>

1. <span data-ttu-id="fad2f-174">现在，应该可以访问 `http://<appname>.azurewebsites.net/microprofile/api/helloworld` 和 `http://<appname>.azurewebsites.net/health` 上的 Azure 托管实例了。</span><span class="sxs-lookup"><span data-stu-id="fad2f-174">You should now be able to access the Azure-hosted instances at `http://<appname>.azurewebsites.net/microprofile/api/helloworld` and `http://<appname>.azurewebsites.net/health`.</span></span>

## <a name="summary"></a><span data-ttu-id="fad2f-175">摘要</span><span class="sxs-lookup"><span data-stu-id="fad2f-175">Summary</span></span>

<span data-ttu-id="fad2f-176">本教程逐步讲解了创建简单的基于 MicroProfile 的微服务、将其容器化为 Docker 容器、在本地运行并将其发布到 Azure 的整个过程。</span><span class="sxs-lookup"><span data-stu-id="fad2f-176">In this tutorial, you've stepped through the process of creating a simple MicroProfile-based microservice, containerized it into a Docker container, run it locally, and published it to Azure.</span></span> <span data-ttu-id="fad2f-177">可以扩展微服务，让其提供其他有用的功能。</span><span class="sxs-lookup"><span data-stu-id="fad2f-177">You can extend your microservice to provide additional useful functionality.</span></span> <span data-ttu-id="fad2f-178">有关详细信息，请访问 [MicroProfile.io](https://microprofile.io/)。</span><span class="sxs-lookup"><span data-stu-id="fad2f-178">For more information, go to [MicroProfile.io](https://microprofile.io/).</span></span>
