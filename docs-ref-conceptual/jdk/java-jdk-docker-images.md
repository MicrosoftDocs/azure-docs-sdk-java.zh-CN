---
title: 将 Docker 映像与 JDK 配合使用，以便进行 Azure Java 开发
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 4/9/2019
ms.devlang: java
ms.topic: conceptual
ms.openlocfilehash: ee8df2a08b23d090965cb42e2c15b934d4785e7c
ms.sourcegitcommit: 03379369346974c6e80f86e7129b885112b5c1a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2019
ms.locfileid: "64910227"
---
# <a name="use-docker-with-a-jdk-for-azure"></a><span data-ttu-id="9768f-102">将 Docker 与用于 Azure 的 JDK 配合使用</span><span class="sxs-lookup"><span data-stu-id="9768f-102">Use Docker with a JDK for Azure</span></span> 

<span data-ttu-id="9768f-103">针对 Java 7、8、11 预生成的 Docker 映像可以通过 [Docker Hub](https://hub.docker.com/_/microsoft-java-se) 使用。</span><span class="sxs-lookup"><span data-stu-id="9768f-103">Pre-built Docker images for Java 7, 8, and 11 are available through [Docker Hub](https://hub.docker.com/_/microsoft-java-se).</span></span>

<span data-ttu-id="9768f-104">Docker Hub 上提供适用于 Zulu JDK、JRE 和 JRE-headless 且基于多个基 OS 映像的认证 Docker 容器映像：</span><span class="sxs-lookup"><span data-stu-id="9768f-104">Certified Docker container images for Zulu JDK, JRE, and JRE-headless on multiple base OS images are available at Docker Hub:</span></span>

* [<span data-ttu-id="9768f-105">JDK</span><span class="sxs-lookup"><span data-stu-id="9768f-105">JDK</span></span>](https://hub.docker.com/_/microsoft-java-jdk)
* [<span data-ttu-id="9768f-106">JRE</span><span class="sxs-lookup"><span data-stu-id="9768f-106">JRE</span></span>](https://hub.docker.com/_/microsoft-java-jre)
* [<span data-ttu-id="9768f-107">JRE-headless</span><span class="sxs-lookup"><span data-stu-id="9768f-107">JRE-headless</span></span>](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a><span data-ttu-id="9768f-108">运行 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="9768f-108">Running a Docker image</span></span>

<span data-ttu-id="9768f-109">可以通过语法 `$ docker run mcr.microsoft.com/java/jdk:tag java` 运行 Docker 映像，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="9768f-109">Docker images can be run using the syntax `$ docker run mcr.microsoft.com/java/jdk:tag java` as shown in the following example.</span></span>

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a><span data-ttu-id="9768f-110">创建 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="9768f-110">Creating a Docker image</span></span>

<span data-ttu-id="9768f-111">可以使用 Microsoft 的官方 Docker Hub 映像创建一个映像，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="9768f-111">You can create an image using Microsoft's official Docker Hub images as shown in the following examples.</span></span>

### <a name="create-a-docker-file"></a><span data-ttu-id="9768f-112">创建 Docker 文件</span><span class="sxs-lookup"><span data-stu-id="9768f-112">Create a Docker file</span></span>

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a><span data-ttu-id="9768f-113">生成 Docker 映像</span><span class="sxs-lookup"><span data-stu-id="9768f-113">Build a Docker image</span></span>

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a><span data-ttu-id="9768f-114">运行此新映像</span><span class="sxs-lookup"><span data-stu-id="9768f-114">Run the new image</span></span>

```cli
docker run hello-world
```

<span data-ttu-id="9768f-115">将显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="9768f-115">You will see the following output:</span></span>

```output
Hello World - From Microsoft Azure !!!
```
