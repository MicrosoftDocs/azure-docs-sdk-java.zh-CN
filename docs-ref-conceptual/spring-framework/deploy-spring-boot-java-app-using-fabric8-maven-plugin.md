---
title: "使用 Fabric8 Maven 插件部署 Spring Boot 应用"
description: "本教程介绍使用适用于 Apache Maven 的 Fabric8 插件在 Microsoft Azure 中部署 Spring Boot 应用程序的步骤。"
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
keywords: Spring, Spring Boot, Spring Framework, Maven
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: java
ms.topic: article
ms.date: 11/01/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6c0e0e233b44b2b54e869b443edf3a255e7fbfb4
ms.sourcegitcommit: 613c1ffd2e0279fc7a96fca98aa1809563f52ee1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a><span data-ttu-id="b0db5-104">使用 Fabric8 Maven 插件部署 Spring Boot 应用</span><span class="sxs-lookup"><span data-stu-id="b0db5-104">Deploy a Spring Boot app using the Fabric8 Maven Plugin</span></span>

<span data-ttu-id="b0db5-105">**[Fabric8]** 是一种基于 **[Kubernetes]** 构建的开源解决方案，可帮助开发人员在 Linux 容器中创建应用程序。</span><span class="sxs-lookup"><span data-stu-id="b0db5-105">**[Fabric8]** is an open-source solution that is built on **[Kubernetes]**, which helps developers create applications in Linux containers.</span></span>

<span data-ttu-id="b0db5-106">本教程介绍如何使用用于 Maven 的 Fabric8 插件在 [Azure 容器服务 (AKS)] 中开发应用程序并将其部署到 Linux 主机。</span><span class="sxs-lookup"><span data-stu-id="b0db5-106">This tutorial walks you through using the Fabric8 plugin for Maven to develop to deploy an application to a Linux host in the [Azure Container Service (AKS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0db5-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="b0db5-107">Prerequisites</span></span>

<span data-ttu-id="b0db5-108">完成本教程中的步骤需要具备以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-108">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="b0db5-109">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-109">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="b0db5-110">[Azure 命令行接口 (CLI)]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-110">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="b0db5-111">最新的 [Java 开发人员工具包 (JDK)]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-111">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="b0db5-112">Apache 的 [Maven] 生成工具（版本 3）。</span><span class="sxs-lookup"><span data-stu-id="b0db5-112">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="b0db5-113">[Git] 客户端。</span><span class="sxs-lookup"><span data-stu-id="b0db5-113">A [Git] client.</span></span>
* <span data-ttu-id="b0db5-114">[Docker] 客户端。</span><span class="sxs-lookup"><span data-stu-id="b0db5-114">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="b0db5-115">由于本教程中的虚拟化要求，无法在虚拟机上执行本文中的步骤；必须使用启用了虚拟化功能的物理计算机。</span><span class="sxs-lookup"><span data-stu-id="b0db5-115">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="b0db5-116">创建 Docker 上的 Spring Boot 入门 Web 应用</span><span class="sxs-lookup"><span data-stu-id="b0db5-116">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="b0db5-117">以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。</span><span class="sxs-lookup"><span data-stu-id="b0db5-117">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="b0db5-118">打开命令提示符，创建本地目录以存放应用程序，并更改为以下目录；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-118">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   <span data-ttu-id="b0db5-119">- 或 -</span><span class="sxs-lookup"><span data-stu-id="b0db5-119">-- or --</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. <span data-ttu-id="b0db5-120">将 [Docker 上的 Spring Boot 启动入门]示例项目克隆到目录。</span><span class="sxs-lookup"><span data-stu-id="b0db5-120">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="b0db5-121">将目录更改为已完成项目；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-121">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   <span data-ttu-id="b0db5-122">- 或 -</span><span class="sxs-lookup"><span data-stu-id="b0db5-122">-- or --</span></span>
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. <span data-ttu-id="b0db5-123">使用 Maven 生成和运行示例应用。</span><span class="sxs-lookup"><span data-stu-id="b0db5-123">Use Maven to build and run the sample app.</span></span>
   ```shell
   mvn clean package spring-boot:run
   ```

1. <span data-ttu-id="b0db5-124">通过浏览到 http://localhost:8080 或使用以下 `curl` 命令测试 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="b0db5-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="b0db5-125">应该会显示“Hello Docker World”消息。</span><span class="sxs-lookup"><span data-stu-id="b0db5-125">You should see a **Hello Docker World** message displayed.</span></span>

   ![在本地浏览示例应用程序][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a><span data-ttu-id="b0db5-127">安装 Kubernetes 命令行接口并使用 Azure CLI 创建 Azure 资源组</span><span class="sxs-lookup"><span data-stu-id="b0db5-127">Install the Kubernetes command-line interface and create an Azure resource group using the Azure CLI</span></span>

1. <span data-ttu-id="b0db5-128">打开命令提示符。</span><span class="sxs-lookup"><span data-stu-id="b0db5-128">Open a command prompt.</span></span>

1. <span data-ttu-id="b0db5-129">键入以下命令登录到 Azure 帐户：</span><span class="sxs-lookup"><span data-stu-id="b0db5-129">Type the following command to log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="b0db5-130">按照说明完成登录过程</span><span class="sxs-lookup"><span data-stu-id="b0db5-130">Follow the instructions to complete the login process</span></span>
   
   <span data-ttu-id="b0db5-131">Azure CLI 将显示帐户列表；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-131">The Azure CLI will display a list of your accounts; for example:</span></span>

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. <span data-ttu-id="b0db5-132">如果尚未安装 Kubernetes 命令行接口 (`kubectl`)，可以使用 Azure CLI 安装；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-132">If you do not already have the Kubernetes command-line interface (`kubectl`) installed, you can install using the Azure CLI; for example:</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="b0db5-133">Linux 用户可能必须将 `sudo` 作为此命令的前缀，因为它将 Kubernetes CLI 部署到 `/usr/local/bin`。</span><span class="sxs-lookup"><span data-stu-id="b0db5-133">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   >
   > <span data-ttu-id="b0db5-134">如果已经安装 `kubectl`，但 `kubectl` 的版本太旧，那么在尝试完成本文后面列出的步骤时，可能会显示类似于以下示例的错误消息：</span><span class="sxs-lookup"><span data-stu-id="b0db5-134">If you already have `kubectl`) installed and your version of `kubectl` is too old, you may see an error message similar to the following example when you attempt to complete the steps listed later in this article:</span></span>
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > <span data-ttu-id="b0db5-135">如果发生这种情况，则需重新安装 `kubectl` 以更新版本。</span><span class="sxs-lookup"><span data-stu-id="b0db5-135">If this happens, you will need to reinstall `kubectl` to update your version.</span></span>
   >

1. <span data-ttu-id="b0db5-136">为本教程中要使用的 Azure 资源创建资源组；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-136">Create a resource group for the Azure resources that you will use in this tutorial; for example:</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   <span data-ttu-id="b0db5-137">其中：</span><span class="sxs-lookup"><span data-stu-id="b0db5-137">Where:</span></span>  
      * <span data-ttu-id="b0db5-138">*wingtiptoys-kubernetes* 是资源组的唯一名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-138">*wingtiptoys-kubernetes* is a unique name for your resource group</span></span>  
      * <span data-ttu-id="b0db5-139">*westeurope* 是应用程序的相应地理位置</span><span class="sxs-lookup"><span data-stu-id="b0db5-139">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="b0db5-140">Azure CLI 将显示资源组创建的结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-140">The Azure CLI will display the results of your resource group creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a><span data-ttu-id="b0db5-141">使用 Azure CLI 创建 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="b0db5-141">Create a Kubernetes cluster using the Azure CLI</span></span>

1. <span data-ttu-id="b0db5-142">在新资源组中创建 Kubernetes 群集；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-142">Create a Kubernetes cluster in your new resource group; for example:</span></span>  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   <span data-ttu-id="b0db5-143">其中：</span><span class="sxs-lookup"><span data-stu-id="b0db5-143">Where:</span></span>  
      * <span data-ttu-id="b0db5-144">*wingtiptoys-kubernetes* 是本文前面的资源组的名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-144">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="b0db5-145">*wingtiptoys-cluster* 是 Kubernetes 群集的唯一名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-145">*wingtiptoys-cluster* is a unique name for your Kubernetes cluster</span></span>
      * <span data-ttu-id="b0db5-146">*wingtiptoys* 是应用程序的唯一名称 DNS 名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-146">*wingtiptoys* is a unique name DNS name for your application</span></span>

   <span data-ttu-id="b0db5-147">Azure CLI 将显示群集创建的结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-147">The Azure CLI will display the results of your cluster creation; for example:</span></span>  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
       "parameters": {
         "clientSecret": {
           "type": "SecureString"
         }
       },
       "parametersLink": null,
       "providers": [
         {
           "id": null,
           "namespace": "Microsoft.ContainerService",
           "registrationState": null,
           "resourceTypes": [
             {
               "aliases": null,
               "apiVersions": null,
               "locations": [
                 "westeurope"
               ],
               "properties": null,
               "resourceType": "containerServices"
             }
           ]
         }
       ],
       "provisioningState": "Succeeded",
       "template": null,
       "templateLink": null,
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. <span data-ttu-id="b0db5-148">下载新 Kubernetes 群集的凭据；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-148">Download your credentials for your new Kubernetes cluster; for example:</span></span>  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. <span data-ttu-id="b0db5-149">使用以下命令验证连接：</span><span class="sxs-lookup"><span data-stu-id="b0db5-149">Verify your connection with the following command:</span></span>
   ```shell 
   kubectl get nodes
   ```

   <span data-ttu-id="b0db5-150">应显示节点和状态列表，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="b0db5-150">You should see a list of nodes and statuses like the following example:</span></span>

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="b0db5-151">使用 Azure CLI 创建专用 Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="b0db5-151">Create a private Azure container registry using the Azure CLI</span></span>

1. <span data-ttu-id="b0db5-152">在资源组中创建专用 Azure 容器注册表来承载 Docker 映像；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-152">Create a private Azure container registry in your resource group to host your Docker image; for example:</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="b0db5-153">其中：</span><span class="sxs-lookup"><span data-stu-id="b0db5-153">Where:</span></span>  
      * <span data-ttu-id="b0db5-154">*wingtiptoys-kubernetes* 是本文前面的资源组的名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-154">*wingtiptoys-kubernetes* is the name of your resource group from earlier in this article</span></span>  
      * <span data-ttu-id="b0db5-155">*wingtiptoysregistry* 是专用注册表的唯一名称</span><span class="sxs-lookup"><span data-stu-id="b0db5-155">*wingtiptoysregistry* is a unique name for your private registry</span></span>
      * <span data-ttu-id="b0db5-156">*westeurope* 是应用程序的相应地理位置</span><span class="sxs-lookup"><span data-stu-id="b0db5-156">*westeurope* is an appropriate geographic location for your application</span></span>  

   <span data-ttu-id="b0db5-157">Azure CLI 将显示注册表创建的结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-157">The Azure CLI will display the results of your registry creation; for example:</span></span>  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. <span data-ttu-id="b0db5-158">从 Azure CLI 检索容器注册表的密码。</span><span class="sxs-lookup"><span data-stu-id="b0db5-158">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   <span data-ttu-id="b0db5-159">Azure CLI 将显示注册表密码；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-159">The Azure CLI will display the password for your registry; for example:</span></span>  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="b0db5-160">导航到 Maven 安装的配置目录 (default ~/.m2/ or C:\Users\username\.m2)，并使用文本编辑器打开 settings.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="b0db5-160">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="b0db5-161">将 Azure 容器注册表 URL、用户名和密码添加到 *settings.xml* 文件中新的 `<server>` 集合。</span><span class="sxs-lookup"><span data-stu-id="b0db5-161">Add your Azure Container Registry URL, username and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="b0db5-162">`id` 和 `username` 是注册表的名称。</span><span class="sxs-lookup"><span data-stu-id="b0db5-162">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="b0db5-163">使用上一个命令中的 `password` 值（不带引号）。</span><span class="sxs-lookup"><span data-stu-id="b0db5-163">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="b0db5-164">导航到 Spring Boot 应用程序的已完成项目目录（例如，“*C:\SpringBoot\gs-spring-boot-docker\complete*”或“*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*”），并使用文本编辑器打开 *pom.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="b0db5-164">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="b0db5-165">使用 Azure 容器注册表的登录服务器值更新 pom.xml 文件中的 `<properties>` 集合。</span><span class="sxs-lookup"><span data-stu-id="b0db5-165">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="b0db5-166">更新 pom.xml 文件中的 `<plugins>` 集合，使 `<plugin>` 包含 Azure 容器注册表的登录服务器地址和注册表名称。</span><span class="sxs-lookup"><span data-stu-id="b0db5-166">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="b0db5-167">导航到 Spring Boot 应用程序的已完成项目目录，然后运行以下 Maven 命令生成 Docker 容器并将映像推送到注册表：</span><span class="sxs-lookup"><span data-stu-id="b0db5-167">Navigate to the completed project directory for your Spring Boot application, and run the following Maven command to build the Docker container and push the image to your registry:</span></span>

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   <span data-ttu-id="b0db5-168">Maven 将显示生成结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-168">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a><span data-ttu-id="b0db5-169">将 Spring Boot 应用配置为使用 Fabric8 Maven 插件</span><span class="sxs-lookup"><span data-stu-id="b0db5-169">Configure your Spring Boot app to use the Fabric8 Maven plugin</span></span>

1. <span data-ttu-id="b0db5-170">导航到 Spring Boot 应用程序的已完成项目目录（例如，“*C:\SpringBoot\gs-spring-boot-docker\complete*”或“*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*”），并使用文本编辑器打开 *pom.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="b0db5-170">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="b0db5-171">更新 *pom.xml* 文件中的 `<plugins>` 集合以添加 Fabric8 Maven 插件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-171">Update the `<plugins>` collection in the *pom.xml* file to add the Fabric8 Maven plugin:</span></span>

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. <span data-ttu-id="b0db5-172">导航到 Spring Boot 应用程序的主要源目录（例如：“*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*”或“*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*”），并创建一个名为“*fabric8*”的新文件夹。</span><span class="sxs-lookup"><span data-stu-id="b0db5-172">Navigate to the main source directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*" or "*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete/src/main*"), and create a new folder named "*fabric8*".</span></span>

1. <span data-ttu-id="b0db5-173">在新的 *fabric8* 文件夹中创建三个 YAML 片段文件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-173">Create three YAML fragment files in the new *fabric8* folder:</span></span>

   <span data-ttu-id="b0db5-174">a.</span><span class="sxs-lookup"><span data-stu-id="b0db5-174">a.</span></span> <span data-ttu-id="b0db5-175">使用以下内容创建名为 **deployment.yml** 的文件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-175">Create a file named **deployment.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   <span data-ttu-id="b0db5-176">b.</span><span class="sxs-lookup"><span data-stu-id="b0db5-176">b.</span></span> <span data-ttu-id="b0db5-177">使用以下内容创建名为 **secrets.yml** 的文件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-177">Create a file named **secrets.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   <span data-ttu-id="b0db5-178">c.</span><span class="sxs-lookup"><span data-stu-id="b0db5-178">c.</span></span> <span data-ttu-id="b0db5-179">使用以下内容创建名为 **service.yml** 的文件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-179">Create a file named **service.yml** with the following contents:</span></span>
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. <span data-ttu-id="b0db5-180">运行以下 Maven 命令，生成 Kubernetes 资源列表文件：</span><span class="sxs-lookup"><span data-stu-id="b0db5-180">Run the following Maven command to build the Kubernetes resource list file:</span></span>

   ```shell
   mvn fabric8:resource
   ```

   <span data-ttu-id="b0db5-181">此命令会将 *src/main/fabric8* 文件夹下的所有 Kubernetes 资源 yaml 文件合并到一个包含 Kubernetes 资源列表的 YAML 文件，该文件可以直接应用于 Kubernetes 群集或导出到 Helm 图表。</span><span class="sxs-lookup"><span data-stu-id="b0db5-181">This command merges all Kubernetes resource yaml files under the *src/main/fabric8* folder to a YAML file that contains a Kubernetes resource list, which can be applied to Kubernetes cluster directly or export to a helm chart.</span></span>

   <span data-ttu-id="b0db5-182">Maven 将显示生成结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-182">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="b0db5-183">运行以下 Maven 命令，将资源列表文件应用于 Kubernetes 群集：</span><span class="sxs-lookup"><span data-stu-id="b0db5-183">Run the following Maven command to apply the resource list file to your Kubernetes cluster:</span></span>

   ```shell
   mvn fabric8:apply
   ```

   <span data-ttu-id="b0db5-184">Maven 将显示生成结果；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-184">Maven will display the results of your build; for example:</span></span>  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. <span data-ttu-id="b0db5-185">将应用部署到群集后，使用 `kubectl` 应用程序查询外部 IP 地址；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-185">Once the app is deployed to the cluster, query the external IP address using the `kubectl` application; for example:</span></span>
   ```shell
   kubectl get svc -w
   ```

   <span data-ttu-id="b0db5-186">`kubectl` 将显示内部和外部 IP 地址；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-186">`kubectl` will display your internal and external IP addresses; for example:</span></span>
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   <span data-ttu-id="b0db5-187">外部 IP 地址可用于在 Web 浏览器中打开应用程序。</span><span class="sxs-lookup"><span data-stu-id="b0db5-187">You can use the external IP address to open your application in a web browser.</span></span>

   ![从外部浏览示例应用程序][SB02]

## <a name="delete-your-kubernetes-cluster"></a><span data-ttu-id="b0db5-189">删除 Kubernetes 群集</span><span class="sxs-lookup"><span data-stu-id="b0db5-189">Delete your Kubernetes cluster</span></span>

<span data-ttu-id="b0db5-190">不再需要 Kubernetes 群集时，可以使用 `az group delete` 命令删除资源组，这将删除其所有相关资源；例如：</span><span class="sxs-lookup"><span data-stu-id="b0db5-190">When your Kubernetes cluster is no longer needed, you can use the `az group delete` command to remove the resource group, which will remove all of its related resources; for example:</span></span>

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a><span data-ttu-id="b0db5-191">后续步骤</span><span class="sxs-lookup"><span data-stu-id="b0db5-191">Next steps</span></span>

<span data-ttu-id="b0db5-192">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="b0db5-192">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="b0db5-193">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="b0db5-193">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)
* [<span data-ttu-id="b0db5-194">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Linux 上</span><span class="sxs-lookup"><span data-stu-id="b0db5-194">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-linux.md)
* [<span data-ttu-id="b0db5-195">在 Azure 容器服务中将 Spring Boot 应用程序部署于 Kubernetes 群集上</span><span class="sxs-lookup"><span data-stu-id="b0db5-195">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="b0db5-196">有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-196">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="b0db5-197">有关 Docker 上的 Spring Boot 示例项目的详细信息，请参阅 [Docker 上的 Spring Boot 启动入门]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-197">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="b0db5-198">若要获取 Spring Boot 应用程序入门的相关帮助，请参阅 <https://start.spring.io/> 中的 **Spring Initializr**。</span><span class="sxs-lookup"><span data-stu-id="b0db5-198">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at <https://start.spring.io/>.</span></span>

<span data-ttu-id="b0db5-199">有关创建简单 Spring Boot 应用程序入门的详细信息，请参阅 <https://start.spring.io/> 中的 Spring Initializr。</span><span class="sxs-lookup"><span data-stu-id="b0db5-199">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at <https://start.spring.io/>.</span></span>

<span data-ttu-id="b0db5-200">有关如何使用 Azure 的自定义 Docker 映像的其他示例，请参阅[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]。</span><span class="sxs-lookup"><span data-stu-id="b0db5-200">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure 命令行接口 (CLI)]: /cli/azure/overview
[Azure 容器服务 (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[使用 Linux 上 Azure Web 应用的自定义 Docker 映像]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[免费 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java 开发人员工具包 (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker 上的 Spring Boot 启动入门]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-using-fabric8-maven-plugin/SB02.png
