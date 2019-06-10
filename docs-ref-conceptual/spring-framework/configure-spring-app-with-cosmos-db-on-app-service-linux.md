---
title: 如何将 Spring 和 Cosmos DB 与 Linux 上的应用服务配合使用
description: 本文将详细介绍如何在 Linux 上的 Azure 应用服务中生成、配置、部署和缩放 Java Web 应用并对其进行故障排除。
documentationcenter: java
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 4/24/2019
ms.devlang: java
ms.service: app-service, cosmos-db
ms.topic: article
ms.openlocfilehash: 5d209c0670d6f4265b1237e7b8cff45faa9bb5d8
ms.sourcegitcommit: 04cff6e3c6d3a9c15f7d88d5d3c238f0bdc787fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64568630"
---
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a><span data-ttu-id="b0063-103">如何将 Spring 和 Cosmos DB 与 Linux 上的应用服务配合使用</span><span class="sxs-lookup"><span data-stu-id="b0063-103">How to use Spring and Cosmos DB with App Service on Linux</span></span>

## <a name="overview"></a><span data-ttu-id="b0063-104">概述</span><span class="sxs-lookup"><span data-stu-id="b0063-104">Overview</span></span>

<span data-ttu-id="b0063-105">本文将详细介绍如何在 Linux 上的 Azure 应用服务中生成、配置、部署和缩放 Java Web 应用并对其进行故障排除。</span><span class="sxs-lookup"><span data-stu-id="b0063-105">This article will walk you through the process of building, configuring, deploying, troubleshooting, and scaling Java Web apps in Azure App Service on Linux.</span></span>

<span data-ttu-id="b0063-106">本文将演示如何使用以下组件：</span><span class="sxs-lookup"><span data-stu-id="b0063-106">It will demonstrate the usage of the following components:</span></span>

- [<span data-ttu-id="b0063-107">Spring Boot 起动器与 Azure Cosmos DB SQL API</span><span class="sxs-lookup"><span data-stu-id="b0063-107">Spring Boot Starter with the Azure Cosmos DB SQL API</span></span>](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [<span data-ttu-id="b0063-108">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b0063-108">Azure Cosmos DB</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
- [<span data-ttu-id="b0063-109">应用服务 Linux</span><span class="sxs-lookup"><span data-stu-id="b0063-109">App Service Linux</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a><span data-ttu-id="b0063-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="b0063-110">Prerequisites</span></span>

<span data-ttu-id="b0063-111">为遵循本文介绍的步骤，需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="b0063-111">The following prerequisites are required in order to follow the steps in this article:</span></span>

- <span data-ttu-id="b0063-112">若要将 Java Web 应用部署到云，需要一个 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="b0063-112">In order to deploy a Java Web app to cloud, you need an Azure subscription.</span></span> <span data-ttu-id="b0063-113">如果还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册获取[免费 Azure 帐户]((https://azure.microsoft.com/pricing/free-trial/))。</span><span class="sxs-lookup"><span data-stu-id="b0063-113">If you do not already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free Azure account]((https://azure.microsoft.com/pricing/free-trial/)).</span></span>
- [<span data-ttu-id="b0063-114">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b0063-114">Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [<span data-ttu-id="b0063-115">Java 8 JDK</span><span class="sxs-lookup"><span data-stu-id="b0063-115">Java 8 JDK</span></span>](https://docs.microsoft.com/java/azure/jdk/java-jdk-install)
- [<span data-ttu-id="b0063-116">Maven 3</span><span class="sxs-lookup"><span data-stu-id="b0063-116">Maven 3</span></span>](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a><span data-ttu-id="b0063-117">克隆示例 Java Web 应用存储库</span><span class="sxs-lookup"><span data-stu-id="b0063-117">Clone the Sample Java Web App Repository</span></span>
<span data-ttu-id="b0063-118">在此练习中，我们将使用 Spring Todo 应用，这是一个使用 [Spring Boot](https://spring.io/projects/spring-boot)、[Spring Data for Cosmos DB](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable) 和 [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction) 生成的 Java 应用程序。</span><span class="sxs-lookup"><span data-stu-id="b0063-118">For this exercise you'll be using the Spring Todo app, which is a Java application built using [Spring Boot](https://spring.io/projects/spring-boot), [Spring Data for Cosmos DB](https://docs.microsoft.com/en-us/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable) and [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction).</span></span>
1. <span data-ttu-id="b0063-119">克隆 Spring Todo 应用并复制 **.prep** 文件夹的内容，以便初始化项目：</span><span class="sxs-lookup"><span data-stu-id="b0063-119">Clone the Spring Todo app and copy the contents of the **.prep** folder to initialize the project:</span></span>

    <span data-ttu-id="b0063-120">对于 Bash：</span><span class="sxs-lookup"><span data-stu-id="b0063-120">For bash:</span></span>
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    <span data-ttu-id="b0063-121">对于 Windows：</span><span class="sxs-lookup"><span data-stu-id="b0063-121">For Windows:</span></span>
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. <span data-ttu-id="b0063-122">将目录转到已克隆存储库中的以下文件夹：</span><span class="sxs-lookup"><span data-stu-id="b0063-122">Change the directory to the following folder in the cloned repo:</span></span>

   ```bash
   cd initial\spring-todo-app
   ``` 
## <a name="create-an-azure-cosmos-db-from-azure-cli"></a><span data-ttu-id="b0063-123">通过 Azure CLI 创建 Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b0063-123">Create an Azure Cosmos DB from Azure CLI</span></span>

1. <span data-ttu-id="b0063-124">登录到 Azure CLI 并设置订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="b0063-124">Login to your Azure CLI, and set your subscription id.</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="b0063-125">根据需要设置订阅 ID。</span><span class="sxs-lookup"><span data-stu-id="b0063-125">Set the subscription id if needed.</span></span>
    ```bash
    az account set -s <your-subscription-id>
    ```

3. <span data-ttu-id="b0063-126">创建 Azure 资源组，并记下资源组名称供以后使用。</span><span class="sxs-lookup"><span data-stu-id="b0063-126">Create an Azure resource group, and write down the resource group name for later use.</span></span>

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. <span data-ttu-id="b0063-127">创建 Cosmos DB 并将类型指定为 GlobalDocumentDB。</span><span class="sxs-lookup"><span data-stu-id="b0063-127">Create the Cosmos DB and specify the type as GlobalDocumentDB.</span></span>
<span data-ttu-id="b0063-128">Cosmos DB 的名称只能使用小写字母。</span><span class="sxs-lookup"><span data-stu-id="b0063-128">The name of the Cosmos DB must use only lower case letters.</span></span> <span data-ttu-id="b0063-129">请务必记下响应中的 `documentEndpoint` 字段。</span><span class="sxs-lookup"><span data-stu-id="b0063-129">Make sure to note the `documentEndpoint` field in the response.</span></span> <span data-ttu-id="b0063-130">稍后会需要它。</span><span class="sxs-lookup"><span data-stu-id="b0063-130">You'll need this later.</span></span>

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

4. <span data-ttu-id="b0063-131">获取 Azure Cosmos DB 密钥，记录 `primaryMasterKey` 值供以后使用。</span><span class="sxs-lookup"><span data-stu-id="b0063-131">Get your Azure Cosmos DB keys, record the `primaryMasterKey` value for later use.</span></span>

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a><span data-ttu-id="b0063-132">在本地生成并运行应用</span><span class="sxs-lookup"><span data-stu-id="b0063-132">Build and Run the App Locally</span></span>

1. <span data-ttu-id="b0063-133">在所选控制台中，使用此前在本文中收集的 Azure 和 Cosmos DB 连接信息配置在以下代码部分显示的环境变量。</span><span class="sxs-lookup"><span data-stu-id="b0063-133">Within your console of choice configure the environment variables shown in the following code sections with the Azure and Cosmos DB connection information you gathered previously in this article.</span></span> <span data-ttu-id="b0063-134">需要为 **WEBAPP_NAME** 提供唯一名称，为 **REGION** 变量提供值。</span><span class="sxs-lookup"><span data-stu-id="b0063-134">You'll need to provide a unique name for **WEBAPP_NAME** and value for the **REGION** variables.</span></span>

<span data-ttu-id="b0063-135">对于 Linux (Bash)：</span><span class="sxs-lookup"><span data-stu-id="b0063-135">For Linux (Bash):</span></span>

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

<span data-ttu-id="b0063-136">对于 Windows（命令提示符）：</span><span class="sxs-lookup"><span data-stu-id="b0063-136">For Windows (Command Prompt):</span></span>
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> <span data-ttu-id="b0063-137">若要通过脚本来预配这些变量，可以复制 .prep 目录中一个用于 Bash 的模板，从其着手进行操作。</span><span class="sxs-lookup"><span data-stu-id="b0063-137">If you'd like to provision these variables with a script, there is a template for Bash in the .prep directory that you can copy and use as a starting point.</span></span>

2. <span data-ttu-id="b0063-138">从该目录转到以下目录：</span><span class="sxs-lookup"><span data-stu-id="b0063-138">Change the directory to the following:</span></span>

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. <span data-ttu-id="b0063-139">使用以下命令在本地运行 Spring Todo 应用：</span><span class="sxs-lookup"><span data-stu-id="b0063-139">Run the Spring Todo app locally with the following command:</span></span>

    ```bash
    mvn package spring-boot:run
    ```

4. <span data-ttu-id="b0063-140">待应用程序启动以后，可访问以下 Spring Todo 应用，对部署进行验证：[http://localhost:8080/](http://localhost:8080/)。</span><span class="sxs-lookup"><span data-stu-id="b0063-140">Once the application has started,you can validate the deployment by accessing the Spring Todo app here: [http://localhost:8080/](http://localhost:8080/).</span></span>

 ![在本地运行的 Spring 应用][SCDB01]

## <a name="deploy-to-app-service-linux"></a><span data-ttu-id="b0063-142">部署到应用服务 Linux</span><span class="sxs-lookup"><span data-stu-id="b0063-142">Deploy to App Service Linux</span></span>

1. <span data-ttu-id="b0063-143">打开 pom.xml 文件，该文件以前已复制到存储库的 **initial/spring-todo-app** 目录。</span><span class="sxs-lookup"><span data-stu-id="b0063-143">Open the pom.xml file that you previously copied to the **initial/spring-todo-app** directory of the repository.</span></span> <span data-ttu-id="b0063-144">确保[适用于 Azure 应用服务的 Maven 插件](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)包含在其中，如下面的 pom.xml 文件所示。</span><span class="sxs-lookup"><span data-stu-id="b0063-144">Ensure that the [Maven Plugin for Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) is included as seen in the pom.xml file below.</span></span> <span data-ttu-id="b0063-145">如果未将版本设置为 **1.6.0**，则更新该值。</span><span class="sxs-lookup"><span data-stu-id="b0063-145">If the version is not set to **1.6.0** then update the value.</span></span>

```xml
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.6.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

2. <span data-ttu-id="b0063-146">部署到应用服务 Linux 中的 Java SE</span><span class="sxs-lookup"><span data-stu-id="b0063-146">Deploy to Java SE in App Service Linux</span></span>

    ```bash
    mvn azure-webapp:deploy
    ```

```bash
// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.6.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesnt exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlan11111111-1111-1111'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

3. <span data-ttu-id="b0063-147">浏览到在应用服务 Linux 中的 Java SE 上运行的 Web 应用：</span><span class="sxs-lookup"><span data-stu-id="b0063-147">Browse to your web app running on Java SE in App Service Linux:</span></span>

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![在 Linux 上的应用服务中运行的 Spring 应用][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a><span data-ttu-id="b0063-149">通过查看日志，排查 Azure 上的 Spring Todo 应用的问题</span><span class="sxs-lookup"><span data-stu-id="b0063-149">Troubleshoot Spring Todo App on Azure by Viewing Logs</span></span>

1. <span data-ttu-id="b0063-150">为 Linux 中的 Azure 应用服务中部署的 Java Web 应用配置日志：</span><span class="sxs-lookup"><span data-stu-id="b0063-150">Configure logs for the deployed Java Web app in Azure App Service in Linux:</span></span>

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. <span data-ttu-id="b0063-151">从本地计算机打开 Java Web 应用远程日志流：</span><span class="sxs-lookup"><span data-stu-id="b0063-151">Open Java Web app remote log stream from a local machine:</span></span>

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
     ```

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers needs non existent service dev
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======


```

3. <span data-ttu-id="b0063-152">完成后，可以对照 [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete) 中的代码来查看结果。</span><span class="sxs-lookup"><span data-stu-id="b0063-152">When you are finished, you can check your results against the code in [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete).</span></span>


## <a name="scale-out-the-spring-todo-app"></a><span data-ttu-id="b0063-153">横向扩展 Spring Todo 应用</span><span class="sxs-lookup"><span data-stu-id="b0063-153">Scale out the Spring Todo App</span></span>

1. <span data-ttu-id="b0063-154">使用 Azure CLI 横向扩展 Java Web 应用：</span><span class="sxs-lookup"><span data-stu-id="b0063-154">Scale out Java Web app using Azure CLI:</span></span>

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a><span data-ttu-id="b0063-155">后续步骤</span><span class="sxs-lookup"><span data-stu-id="b0063-155">Next steps</span></span>

- [<span data-ttu-id="b0063-156">Linux 版应用服务中的 Java 开发指南</span><span class="sxs-lookup"><span data-stu-id="b0063-156">Java in App Service Linux dev guide</span></span>](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-java)
- <span data-ttu-id="b0063-157">[面向 Java 开发人员的 Azure](https://docs.microsoft.com/en-us/java/azure/) 若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。</span><span class="sxs-lookup"><span data-stu-id="b0063-157">[Azure for Java Developers](https://docs.microsoft.com/en-us/java/azure/) To learn more about Spring and Azure, continue to the Spring on Azure documentation center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b0063-158">Azure 上的 Spring</span><span class="sxs-lookup"><span data-stu-id="b0063-158">Spring on Azure</span></span>](/java/azure/spring-framework)

### <a name="additional-resources"></a><span data-ttu-id="b0063-159">其他资源</span><span class="sxs-lookup"><span data-stu-id="b0063-159">Additional Resources</span></span>

<span data-ttu-id="b0063-160">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="b0063-160">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="b0063-161">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="b0063-161">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="b0063-162">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="b0063-162">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="b0063-163">有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。</span><span class="sxs-lookup"><span data-stu-id="b0063-163">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].</span></span>

<span data-ttu-id="b0063-164">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序  。</span><span class="sxs-lookup"><span data-stu-id="b0063-164">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="b0063-165">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="b0063-165">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="b0063-166">为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="b0063-166">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="b0063-167">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序  。</span><span class="sxs-lookup"><span data-stu-id="b0063-167">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /java/azure/
[Azure for Java Developers]: /java/azure/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
