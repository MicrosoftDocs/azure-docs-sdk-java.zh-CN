---
title: "在 Azure 中使用 Maven 花费片刻时间部署 Java Web 应用 | Microsoft Docs"
description: "使用 Maven 创建 Java 应用并将其部署到 Azure"
services: app-service\web
documentationcenter: 
author: rloutlaw
manager: douge
editor: 
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
---
# <a name="create-and-deploy-a-java-app-to-azure-with-maven"></a>使用 Maven 创建 Java 应用并将其部署到 Azure

本快速入门介绍如何使用 [Apache Maven](http://maven.apache.org) 和 Azure CLI，在几分钟内创建一个简单的 Java Web 应用并将其部署到 [Azure 应用服务](/azure/app-service/app-service-value-prop-what-is)。

## <a name="before-you-begin"></a>开始之前

在开始之前，请先安装以下软件：

- [Git](https://git-scm.com/)
- [Java 8](https://www.azul.com/downloads/zulu/)
- [Maven 3](http://maven.apache.org/download.cgi)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)

如果还没有 Azure 订阅，可以在开始前创建一个 [免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="get-the-sample-code"></a>获取示例代码

将示例应用存储库克隆到本地计算机。 本示例是一个简单的 JSP 应用程序，它在项目 `pom.xml` 中包含附加的 Maven 配置用于在本地测试应用，并帮助将示例部署到 Azure 应用服务。

```
git clone https://github.com/Azure-Samples/app-service-maven
```

## <a name="run-the-app-locally"></a>在本地运行应用

打开命令提示符，使用 Maven 生成应用，并在本地 [Tomcat](https://tomcat.apache.org) Web 容器中运行该应用。 

```
cd app-service-maven
mvn package
mvn tomcat7:run-war
```

打开 Web 浏览器并导航到 http://localhost:8080 以预览该应用：

  ![示例 Java 应用的 Hello World 输出](media/maven-quickstart/java-app-hello-world-output.png)

## <a name="log-in-to-azure"></a>登录 Azure

使用 `az login` 登录到 Azure CLI。 本快速入门教程使用 CLI 来查询和创建用于托管应用的 Azure 资源。
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a>创建资源组   

使用 [az group create](/cli/azure/group#create) 创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。

```azurecli
az group create --location "East US" --name myResourceGroup
```

若要查看可对 `---location` 使用的其他可能值，请使用 [az appservice list-locations](/cli/azure/appservice#list-locations)

## <a name="create-an-app-service-plan"></a>创建应用服务计划

使用 [az appservice plan create](/cli/azure/appservice/plan#create) 创建**免费**的[应用服务计划](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。 应用服务计划分配在计划中运行的所有 Web 应用之间共享的资源。


```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

应用服务计划准备就绪后，Azure CLI 会显示类似于以下示例的信息：

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


## <a name="create-a-web-app"></a>创建 Web 应用

运行 Azure CLI [az webapp create](/cli/azure/appservice/web#create) 命令创建使用计划资源的 Web 应用。 Web 应用定义提供可在其中部署代码的空间，以及在运行应用后用于访问该应用的 URL。

在以下命令中，请将出现的 <appname> 占位符替换成自己的唯一应用名称。 <appname> 在 Web 应用的默认主机名中使用。 如果 <appname> 不唯一，会出现友好错误消息“具有给定名称的网站已存在”。

```azurecli
az webapp create --name appname --resource-group myResourceGroup --plan my-free-appservice-plan
```

创建 Web 应用后，Azure CLI 会显示类似于以下示例的信息：1

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

## <a name="configure-java-and-tomcat"></a>配置 Java 和 Tomcat

使用 Azure CLI 的 [az webapp config](/cli/azure/appservice/web#config) 命令将 Web 应用配置为使用 Java 和 Tomcat。 以下示例将应用服务配置为使用 Java 8 和 [Apache Tomcat 8.5](http://tomcat.apache.org/) 来运行应用。

```azurecli
az webapp config set \ 
--name appname \
--resource-group myResourceGroup \
--java-container TOMCAT \
--java-version 1.8.0_73  \
--java-container-version 8.5
```

## <a name="configure-maven"></a>配置 Maven 

示例的 Maven `pom.xml` 包含用于通过 FTP 将示例传输到 Azure 的配置，但需要自定义该配置，以便部署到自己的 web 应用。 使用 [az appservice web deployment list-publishing-profiles](/cli/azure/appservice/web/deployment#list-publishing-profiles) 检索应用服务凭据：

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

将 `az-settings.xml` 文件中的占位符替换为输出中的密码、用户名和 FTP 主机名。 请务必只在用户名中使用一个 `\` 字符，而不要使用 CLI 输出中的转义值。
   
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

## <a name="deploy-the-sample-application"></a>部署示例应用程序

将示例部署到 Azure，并使用 Maven 的 `-s` 参数来使用 `az-settings.xml` 文件中的配置。

```
mvn install -s az-settings.xml
```

## <a name="browse-to-web-app"></a>浏览到 Web 应用

查看 Azure 中运行的示例：

```azurecli
az webapp browse --name appname --resource-group myResourceGroup
```

## <a name="update-the-app"></a>更新应用

使用文本编辑器打开 `src/main/webapp/index.jsp`，并将现有的 JSP 替换为以下内容。

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

使用 Maven 部署更新：

```
mvn clean package
mvn install -s az-settings.xml
```

重新部署应用后，刷新浏览器查看更改。

## <a name="manage-your-new-azure-app"></a>管理新的 Azure 应用

转到 Azure 门户，查看刚刚创建的 Web 应用。

为此，请登录到 [https://portal.azure.com](https://portal.azure.com)。

从左侧菜单中单击“应用服务”，并单击 Azure Web 应用的名称。

 ![从门户中的 Web 应用列表中选择自己的应用](media/azure-app-service-portal.png)

针对应用运行 `curl` 来发送一些流量，以测试监视情况。

```bash
curl http://<appname>.azurewebsites.net/?[1-30]
```

几分钟后，监视屏幕中会出现请求活动。

 ![在 Azure 门户中监视](media/java-app-monitoring.png)

## <a name="clean-up-resources"></a>清理资源

若要删除本指南中创建的所有资源，请运行以下命令：

```azurecli
az group delete --name myResrouceGroup
```

## <a name="next-steps"></a>后续步骤

浏览 [Azure Java 示例](https://azure.microsoft.com/resources/samples/?term=java)的完整列表。