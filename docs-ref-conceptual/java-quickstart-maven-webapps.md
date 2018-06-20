---
title: 在 Azure 中使用 Maven 花费片刻时间部署 Java Web 应用 | Microsoft Docs
description: 使用 Maven 创建 Java 应用并将其部署到 Azure
services: app-service\web
documentationcenter: ''
author: rloutlaw
manager: douge
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: routlaw
ms.openlocfilehash: 1adc0a104ba22bcd353664e68323165890e46c64
ms.sourcegitcommit: 30d502b3150fa14bcc1251f5f88c7c0dd83e531e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
ms.locfileid: "22033629"
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a><span data-ttu-id="32375-103">使用 Maven 创建 Java 应用并将其部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="32375-103">Create and deploy a Java app to Azure with Maven</span></span>

<span data-ttu-id="32375-104">本快速入门介绍如何使用 [Apache Maven](http://maven.apache.org) 和 Azure CLI，在几分钟内创建一个简单的 Java Web 应用并将其部署到 [Azure 应用服务](/azure/app-service/app-service-value-prop-what-is)。</span><span class="sxs-lookup"><span data-stu-id="32375-104">This quickstart helps you create and deploy a simple Java web app to [Azure App Service](/azure/app-service/app-service-value-prop-what-is) in just a few minutes using [Apache Maven](http://maven.apache.org) and the Azure CLI.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="32375-105">开始之前</span><span class="sxs-lookup"><span data-stu-id="32375-105">Before you begin</span></span>

<span data-ttu-id="32375-106">在开始之前，请先安装以下软件：</span><span class="sxs-lookup"><span data-stu-id="32375-106">Before you start, set up the following:</span></span>

- [<span data-ttu-id="32375-107">Git</span><span class="sxs-lookup"><span data-stu-id="32375-107">Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="32375-108">Java 8</span><span class="sxs-lookup"><span data-stu-id="32375-108">Java 8</span></span>](https://www.azul.com/downloads/zulu/)
- [<span data-ttu-id="32375-109">Maven 3</span><span class="sxs-lookup"><span data-stu-id="32375-109">Maven 3</span></span>](http://maven.apache.org/download.cgi)
- [<span data-ttu-id="32375-110">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="32375-110">Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-az-cli2)

<span data-ttu-id="32375-111">如果还没有 Azure 订阅，可以在开始前创建一个 [免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="32375-111">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="get-the-sample-code"></a><span data-ttu-id="32375-112">获取示例代码</span><span class="sxs-lookup"><span data-stu-id="32375-112">Get the sample code</span></span>

<span data-ttu-id="32375-113">将示例应用存储库克隆到本地计算机。</span><span class="sxs-lookup"><span data-stu-id="32375-113">Clone the sample app repository to your local machine.</span></span> <span data-ttu-id="32375-114">本示例是一个简单的 JSP 应用程序，它在项目 `pom.xml` 中包含附加的 Maven 配置用于在本地测试应用，并帮助将示例部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="32375-114">The sample is a simple JSP application with additional Maven configuration in the project `pom.xml` to test the app locally and help deploy the sample to Azure App Service.</span></span>

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a><span data-ttu-id="32375-115">在本地运行应用</span><span class="sxs-lookup"><span data-stu-id="32375-115">Run the app locally</span></span>

<span data-ttu-id="32375-116">打开命令提示符，使用 Maven 生成应用，并在本地 [Tomcat](https://tomcat.apache.org) Web 容器中运行该应用。</span><span class="sxs-lookup"><span data-stu-id="32375-116">Open a command prompt and use Maven to build the app and run it in a local [Tomcat](https://tomcat.apache.org) web container.</span></span> 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

<span data-ttu-id="32375-117">打开 Web 浏览器并导航到 http://localhost:8080 以预览该应用：</span><span class="sxs-lookup"><span data-stu-id="32375-117">Open a web browser and navigate to http://localhost:8080 to preview the app:</span></span>

  ![示例 Java 应用的 Hello World 输出](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="32375-119">登录 Azure</span><span class="sxs-lookup"><span data-stu-id="32375-119">Log in to Azure</span></span>

<span data-ttu-id="32375-120">使用 `az login` 登录到 Azure CLI。</span><span class="sxs-lookup"><span data-stu-id="32375-120">Log in to the Azure CLI with `az login`.</span></span> <span data-ttu-id="32375-121">本快速入门教程使用 CLI 来查询和创建用于托管应用的 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="32375-121">This quickstart uses the CLI to query and create the Azure resources needed to host the app.</span></span>
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="32375-122">创建资源组</span><span class="sxs-lookup"><span data-stu-id="32375-122">Create a resource group</span></span>   

<span data-ttu-id="32375-123">使用 [az group create](/cli/azure/group#create) 创建资源组。</span><span class="sxs-lookup"><span data-stu-id="32375-123">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="32375-124">Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。</span><span class="sxs-lookup"><span data-stu-id="32375-124">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

```azurecli
az group create --location "East US" --name myResourceGroup
```

<span data-ttu-id="32375-125">若要查看可对 `---location` 使用的其他可能值，请使用 [az appservice list-locations](/cli/azure/appservice#list-locations)</span><span class="sxs-lookup"><span data-stu-id="32375-125">To see other possible values you can use for `---location`, use [az appservice list-locations](/cli/azure/appservice#list-locations)</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="32375-126">创建应用服务计划</span><span class="sxs-lookup"><span data-stu-id="32375-126">Create an App Service plan</span></span>

<span data-ttu-id="32375-127">使用 [az appservice plan create](/cli/azure/appservice/plan#create) 创建**免费**的[应用服务计划](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。</span><span class="sxs-lookup"><span data-stu-id="32375-127">Create a **FREE** [App Service plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) using [az appservice plan create](/cli/azure/appservice/plan#create).</span></span> <span data-ttu-id="32375-128">应用服务计划分配在计划中运行的所有 Web 应用之间共享的资源。</span><span class="sxs-lookup"><span data-stu-id="32375-128">App Service plans allocate resources shared across all web apps running in the plan.</span></span>


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="32375-129">应用服务计划准备就绪后，Azure CLI 会显示类似于以下示例的信息：</span><span class="sxs-lookup"><span data-stu-id="32375-129">When the App Service Plan is ready, the Azure CLI shows information similar to the following example:</span></span>

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "location": "East US",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```


## <a name="create-a-web-app"></a><span data-ttu-id="32375-130">创建 Web 应用</span><span class="sxs-lookup"><span data-stu-id="32375-130">Create a web app</span></span>

<span data-ttu-id="32375-131">运行 Azure CLI [az webapp create](/cli/azure/appservice/web#create) 命令创建使用计划资源的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="32375-131">Create a web app that uses the plan's resources using the Azure CLI [az webapp create](/cli/azure/appservice/web#create) command.</span></span> <span data-ttu-id="32375-132">Web 应用定义提供可在其中部署代码的空间，以及在运行应用后用于访问该应用的 URL。</span><span class="sxs-lookup"><span data-stu-id="32375-132">The web app definition provides a space to deploy the code to and a URL to access the app once it's running.</span></span>

<span data-ttu-id="32375-133">在以下命令中，请将出现的 <appname> 占位符替换成自己的唯一应用名称。</span><span class="sxs-lookup"><span data-stu-id="32375-133">In the command below substitute your own unique app name where you see the <appname> placeholder.</span></span> <span data-ttu-id="32375-134"><appname> 在 Web 应用的默认主机名中使用。</span><span class="sxs-lookup"><span data-stu-id="32375-134">The <appname> is used in the default hostname for the web app.</span></span> <span data-ttu-id="32375-135">如果 <appname> 不唯一，会出现友好错误消息“具有给定名称的网站已存在”。</span><span class="sxs-lookup"><span data-stu-id="32375-135">If <appname> is not unique, you get the friendly error message "Website with given name already exists."</span></span>

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

<span data-ttu-id="32375-136">创建 Web 应用后，Azure CLI 会显示类似于以下示例的信息：1</span><span class="sxs-lookup"><span data-stu-id="32375-136">When the Web App has been created, the Azure CLI shows information similar to the following example.1</span></span>

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<appname>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<appname>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "East US",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/my-free-appservice-plan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

## <a name="configure-java-and-tomcat"></a><span data-ttu-id="32375-137">配置 Java 和 Tomcat</span><span class="sxs-lookup"><span data-stu-id="32375-137">Configure Java and Tomcat</span></span>

<span data-ttu-id="32375-138">使用 Azure CLI 的 [az webapp config](/cli/azure/appservice/web#config) 命令将 Web 应用配置为使用 Java 和 Tomcat。</span><span class="sxs-lookup"><span data-stu-id="32375-138">Configure the web app to use Java and Tomcat using the Azure CLI's [az webapp config](/cli/azure/appservice/web#config) command.</span></span> <span data-ttu-id="32375-139">以下示例将应用服务配置为使用 Java 8 和 [Apache Tomcat 8.5](http://tomcat.apache.org/) 来运行应用。</span><span class="sxs-lookup"><span data-stu-id="32375-139">The example below configures App Service to run Java 8 and [Apache Tomcat 8.5](http://tomcat.apache.org/) to run the app.</span></span>

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a><span data-ttu-id="32375-140">配置 Maven</span><span class="sxs-lookup"><span data-stu-id="32375-140">Configure Maven</span></span> 

<span data-ttu-id="32375-141">示例的 Maven `pom.xml` 包含用于通过 FTP 将示例传输到 Azure 的配置，但需要自定义该配置，以便部署到自己的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="32375-141">The sample's Maven `pom.xml` includes configuration to FTP the sample into Azure, but you'll need to customize it to deploy to your own web app.</span></span> <span data-ttu-id="32375-142">使用 [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) 检索应用服务凭据：</span><span class="sxs-lookup"><span data-stu-id="32375-142">Retreive your App Seevice credentials with [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles):</span></span>

```azurecli
az webapp deployment list-publishing-profiles  \
--name appname \ 
--resource-group myResourceGroup  \
--query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" 
```

```json
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "appname\\$appname"
  }
]
```

<span data-ttu-id="32375-143">将 `az-settings.xml` 文件中的占位符替换为输出中的密码、用户名和 FTP 主机名。</span><span class="sxs-lookup"><span data-stu-id="32375-143">Replace the placeholders in the `az-settings.xml` file with password, username, and FTP hostname from the output.</span></span> <span data-ttu-id="32375-144">请务必只在用户名中使用一个 `\` 字符，而不要使用 CLI 输出中的转义值。</span><span class="sxs-lookup"><span data-stu-id="32375-144">Make sure to only use one `\` character in the username instead of the escaped value from the CLI output.</span></span>
   
```XML
...
    <server>
      <id>azure-hello-world</id>
      <username>appname\$appname</username>
      <password>aBcDeFgHiJkLmNoPqRsTuVwXyZ</password>
    </server>
...
   <configuration>
     <fromFile>${basedir}/target/AzureAppDemo-0.0.1-SNAPSHOT.war</fromFile>
     <url>ftp://waws-prod-blu-045.ftp.azurewebsites.windows.net/site/wwwroot</url>
     <toFile>webapps/ROOT.war</toFile>
     <serverId>azure-hello-world</serverId>
   </configuration>
...
```

## <a name="deploy-the-sample-application"></a><span data-ttu-id="32375-145">部署示例应用程序</span><span class="sxs-lookup"><span data-stu-id="32375-145">Deploy the sample application</span></span>

<span data-ttu-id="32375-146">将示例部署到 Azure，并使用 Maven 的 `-s` 参数来使用 `az-settings.xml` 文件中的配置。</span><span class="sxs-lookup"><span data-stu-id="32375-146">Deploy with sample to Azure, using Maven's `-s` parameter to use the configuration in the `az-settings.xml` file.</span></span>

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a><span data-ttu-id="32375-147">浏览到 Web 应用</span><span class="sxs-lookup"><span data-stu-id="32375-147">Browse to web app</span></span>

<span data-ttu-id="32375-148">查看 Azure 中运行的示例：</span><span class="sxs-lookup"><span data-stu-id="32375-148">View the sample running in Azure:</span></span>

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a><span data-ttu-id="32375-149">更新应用</span><span class="sxs-lookup"><span data-stu-id="32375-149">Update the app</span></span>

<span data-ttu-id="32375-150">使用文本编辑器打开 `src/main/webapp/index.jsp`，并将现有的 JSP 替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="32375-150">Using a text editor, open `src/main/webapp/index.jsp`, and replace the existing JSP with the one below.</span></span>

```html
<%--
  Copyright (c) Microsoft Corporation. All rights reserved.
  Licensed under the MIT License. See License.txt in the project root for
  license information.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java"  import="java.util.Date" %>
<html>
<head>
  <title>Azure Samples Hello World</title>
</head>
<body>
  <H1>Hello Azure!</H1>
   Current time is: <%= new Date() %>
</body>
</html>
```

<span data-ttu-id="32375-151">使用 Maven 部署更新：</span><span class="sxs-lookup"><span data-stu-id="32375-151">Deploy the update with Maven:</span></span>

```
mvn clean package
mvn install -s az-settings.xml
```

<span data-ttu-id="32375-152">重新部署应用后，刷新浏览器查看更改。</span><span class="sxs-lookup"><span data-stu-id="32375-152">Refresh your browser after the app redeploys to view your changes.</span></span>

## <a name="manage-your-new-azure-app"></a><span data-ttu-id="32375-153">管理新的 Azure 应用</span><span class="sxs-lookup"><span data-stu-id="32375-153">Manage your new Azure app</span></span>

<span data-ttu-id="32375-154">转到 Azure 门户，查看刚刚创建的 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="32375-154">Go to the Azure portal to take a look at the web app you just created.</span></span>

<span data-ttu-id="32375-155">为此，请登录到 [https://portal.azure.com](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="32375-155">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="32375-156">从左侧菜单中单击“应用服务”，并单击 Azure Web 应用的名称。</span><span class="sxs-lookup"><span data-stu-id="32375-156">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

 ![从门户中的 Web 应用列表中选择自己的应用](media/azure-app-service-portal.png)

<span data-ttu-id="32375-158">针对应用运行 `curl` 来发送一些流量，以测试监视情况。</span><span class="sxs-lookup"><span data-stu-id="32375-158">Test the monitoring by running `curl` against the app to send some traffic.</span></span>

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

<span data-ttu-id="32375-159">几分钟后，监视屏幕中会出现请求活动。</span><span class="sxs-lookup"><span data-stu-id="32375-159">You'll see the request activity in the monitoring after a couple of minutes.</span></span>

 ![在 Azure 门户中监视](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a><span data-ttu-id="32375-161">清理资源</span><span class="sxs-lookup"><span data-stu-id="32375-161">Clean up resources</span></span>

<span data-ttu-id="32375-162">若要删除本指南中创建的所有资源，请运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="32375-162">To remove all the resources created in this guide, run the following command:</span></span>

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a><span data-ttu-id="32375-163">后续步骤</span><span class="sxs-lookup"><span data-stu-id="32375-163">Next steps</span></span>

<span data-ttu-id="32375-164">浏览 [Azure Java 示例](https://azure.microsoft.com/resources/samples/?term=java)的完整列表。</span><span class="sxs-lookup"><span data-stu-id="32375-164">Browse the full list of [Azure Java samples](https://azure.microsoft.com/resources/samples/?term=java).</span></span>