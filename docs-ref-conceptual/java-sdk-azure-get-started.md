---
title: "用于 Java 的 Azure 库入门"
description: "结合自己的 Azure 订阅开始了解用于 Java 的 Azure 库的基本用法。"
keywords: "Azure, Java, SDK, API, 身份验证, 入门"
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 04/16/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.openlocfilehash: c9b654ea927563e8255f5d189ddc84733a1202e2
ms.sourcegitcommit: 30d502b3150fa14bcc1251f5f88c7c0dd83e531e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="get-started-with-the-azure-libraries-for-java"></a><span data-ttu-id="42d35-104">用于 Java 的 Azure 库入门</span><span class="sxs-lookup"><span data-stu-id="42d35-104">Get started with the Azure libraries for Java</span></span>

<span data-ttu-id="42d35-105">本指南逐步讲解如何使用 Azure 服务主体设置开发环境，并运行一段可以通过用于 Java 的 Azure 库在 Azure 订阅中创建和使用资源的示例代码。</span><span class="sxs-lookup"><span data-stu-id="42d35-105">This guide walks you through setting up a development environment with an Azure service principal and running sample code that creates and uses resources in your Azure subscription using the Azure libraries for Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42d35-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="42d35-106">Prerequisites</span></span>

- <span data-ttu-id="42d35-107">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="42d35-107">An Azure account.</span></span> <span data-ttu-id="42d35-108">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="42d35-108">If you don't have one , [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="42d35-109">[Azure Cloud Shell](https://docs.microsoft.coms/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="42d35-109">[Azure Cloud Shell](https://docs.microsoft.coms/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="42d35-110">[Java 8](https://www.azul.com/downloads/zulu/)（Azure Cloud Shell 中已随附）</span><span class="sxs-lookup"><span data-stu-id="42d35-110">[Java 8](https://www.azul.com/downloads/zulu/) (included in Azure Cloud Shell)</span></span>
- <span data-ttu-id="42d35-111">[Maven 3](http://maven.apache.org/download.cgi)（Azure Cloud Shell 中已随附）</span><span class="sxs-lookup"><span data-stu-id="42d35-111">[Maven 3](http://maven.apache.org/download.cgi) (included in Azure Cloud Shell)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="42d35-112">设置身份验证</span><span class="sxs-lookup"><span data-stu-id="42d35-112">Set up authentication</span></span>

<span data-ttu-id="42d35-113">Java 应用程序需要 Azure 订阅中的读取和创建权限才能运行本教程中的示例代码。</span><span class="sxs-lookup"><span data-stu-id="42d35-113">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="42d35-114">创建一个服务主体，并将应用程序配置为使用该服务主体的凭据运行。</span><span class="sxs-lookup"><span data-stu-id="42d35-114">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="42d35-115">通过服务主体可以创建一个与标识关联的非交互式帐户，该帐户仅拥有运行应用所需的特权。</span><span class="sxs-lookup"><span data-stu-id="42d35-115">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="42d35-116">[使用 Azure CLI 2.0 创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)并捕获输出。</span><span class="sxs-lookup"><span data-stu-id="42d35-116">[Create a service principal using the Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) and capture the output.</span></span> <span data-ttu-id="42d35-117">需要在密码参数而非 `MY_SECURE_PASSWORD` 中提供[安全密码](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)。</span><span class="sxs-lookup"><span data-stu-id="42d35-117">You'll need to provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```

<span data-ttu-id="42d35-118">接下来，将以下内容复制到系统上的某个文本文件中：</span><span class="sxs-lookup"><span data-stu-id="42d35-118">Next, copy the following into a text file on your system:</span></span>

```text
# sample management library properties file
subscription=########-####-####-####-############
client=########-####-####-####-############
key=XXXXXXXXXXXXXXXX
tenant=########-####-####-####-############
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

<span data-ttu-id="42d35-119">将前四个值替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="42d35-119">Replace the top four values with the following:</span></span>

- <span data-ttu-id="42d35-120">subscription：使用在 Azure CLI 2.0 中运行 `az account show` 后返回的 *id* 值。</span><span class="sxs-lookup"><span data-stu-id="42d35-120">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="42d35-121">client：使用从服务主体输出中获取的输出中的 *appId* 值。</span><span class="sxs-lookup"><span data-stu-id="42d35-121">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="42d35-122">key：使用服务主体输出中的 *password* 值。</span><span class="sxs-lookup"><span data-stu-id="42d35-122">key: use the *password* value from the service principal output .</span></span>
- <span data-ttu-id="42d35-123">tenant：使用服务主体输出中的 *tenant* 值。</span><span class="sxs-lookup"><span data-stu-id="42d35-123">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="42d35-124">将此文件保存在系统上可供代码读取的安全位置。</span><span class="sxs-lookup"><span data-stu-id="42d35-124">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="42d35-125">在 shell 中将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为身份验证文件。</span><span class="sxs-lookup"><span data-stu-id="42d35-125">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>    

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="42d35-126">创建新的 Maven 项目</span><span class="sxs-lookup"><span data-stu-id="42d35-126">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="42d35-127">本指南使用 Maven 生成工具来生成和运行示例代码，但其他生成工具（例如 Gradle）也能配合用于 Java 的 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="42d35-127">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="42d35-128">在系统上的新目录中通过命令行创建一个 Maven 项目：</span><span class="sxs-lookup"><span data-stu-id="42d35-128">Create a Maven project from the command line in a new directory on your system:</span></span>

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<span data-ttu-id="42d35-129">这会在 `testAzureApp` 文件夹下创建一个基本的 Maven 项目。</span><span class="sxs-lookup"><span data-stu-id="42d35-129">This creates a basic Maven project under the `testAzureApp` folder.</span></span> <span data-ttu-id="42d35-130">将以下条目添加到项目 `pom.xml` 中，以导入本教程的示例代码中使用的库。</span><span class="sxs-lookup"><span data-stu-id="42d35-130">Add the following entries into the project `pom.xml` to import the libraries used in the sample code in this tutorial.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.2.1</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

<span data-ttu-id="42d35-131">在顶级 `project` 元素下添加 `build` 条目，以使用 [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) 来运行示例：</span><span class="sxs-lookup"><span data-stu-id="42d35-131">Add a `build` entry under the top-level `project` element to use the [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) to run the samples:</span></span>

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```
   
## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="42d35-132">创建 Linux 虚拟机</span><span class="sxs-lookup"><span data-stu-id="42d35-132">Create a Linux virtual machine</span></span>

<span data-ttu-id="42d35-133">在项目的 `src/main/java` 目录中创建名为 `AzureApp.java` 的新文件，并在其中粘贴以下代码块。</span><span class="sxs-lookup"><span data-stu-id="42d35-133">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="42d35-134">使用计算机的实际值更新 `userName` 和 `sshKey` 变量。</span><span class="sxs-lookup"><span data-stu-id="42d35-134">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="42d35-135">该代码会在美国东部 Azure 区域中运行的资源组 `sampleResourceGroup` 内创建名为 `testLinuxVM` 的新 Linux VM。</span><span class="sxs-lookup"><span data-stu-id="42d35-135">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

```java
package com.fabrikam.testAzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));    
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();
           
            // create a Ubuntu virtual machine in a new resource group 
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIpAddressDynamic()
                    .withoutPrimaryPublicIpAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();   

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

<span data-ttu-id="42d35-136">通过命令行运行示例：</span><span class="sxs-lookup"><span data-stu-id="42d35-136">Run the sample from the command line:</span></span>

```
mvn compile exec:java
```

<span data-ttu-id="42d35-137">当 SDK 向 Azure REST API 发出基础调用来配置虚拟机及其资源时，控制台中会显示一些 REST 请求和响应。</span><span class="sxs-lookup"><span data-stu-id="42d35-137">You'll see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="42d35-138">程序完成后，使用 Azure CLI 2.0 验证订阅中的虚拟机：</span><span class="sxs-lookup"><span data-stu-id="42d35-138">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="42d35-139">验证代码正常运行后，使用 CLI 删除 VM 及其资源。</span><span class="sxs-lookup"><span data-stu-id="42d35-139">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="42d35-140">从 GitHub 存储库部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="42d35-140">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="42d35-141">将 `AzureApp.java` 中的 main 方法替换为以下代码，并在运行代码之前将 `appName` 变量更新为唯一值。</span><span class="sxs-lookup"><span data-stu-id="42d35-141">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="42d35-142">此代码会将公共 GitHub 存储库的 `master` 分支中的某个 Web 应用程序部署到免费定价层中运行的新 [Azure 应用服务 Web 应用](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="42d35-142">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

<span data-ttu-id="42d35-143">如前所述使用 Maven 运行代码：</span><span class="sxs-lookup"><span data-stu-id="42d35-143">Run the code as before using Maven:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="42d35-144">使用 CLI 打开指向该应用程序的浏览器：</span><span class="sxs-lookup"><span data-stu-id="42d35-144">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="42d35-145">验证部署后，请从订阅中删除 Web 应用和计划。</span><span class="sxs-lookup"><span data-stu-id="42d35-145">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-a-sql-database"></a><span data-ttu-id="42d35-146">连接到 SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="42d35-146">Connect to a SQL database</span></span>

<span data-ttu-id="42d35-147">将 `AzureApp.java` 中的当前 main 方法替换为以下代码，并为 `dbPassword` 变量设置实际值。</span><span class="sxs-lookup"><span data-stu-id="42d35-147">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="42d35-148">此代码会创建新的 SQL 数据库（包含一条允许远程访问的防火墙规则），然后使用 SQL 数据库 JBDC 驱动程序连接到该数据库。</span><span class="sxs-lookup"><span data-stu-id="42d35-148">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

```java

    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```
<span data-ttu-id="42d35-149">通过命令行运行示例：</span><span class="sxs-lookup"><span data-stu-id="42d35-149">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="42d35-150">然后使用 CLI 清理资源：</span><span class="sxs-lookup"><span data-stu-id="42d35-150">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="42d35-151">将 Blob 写入新存储帐户</span><span class="sxs-lookup"><span data-stu-id="42d35-151">Write a blob into a new storage account</span></span>

<span data-ttu-id="42d35-152">将 `AzureApp.java` 中的当前 main 方法替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="42d35-152">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="42d35-153">此代码会创建一个 [Azure 存储帐户](https://docs.microsoft.com/azure/storage/storage-introduction)，然后使用用于 Java 的 Azure 存储库在云中创建新的文本文件。</span><span class="sxs-lookup"><span data-stu-id="42d35-153">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

```java
    public static void main(String[] args) {

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            // create a new storage account
            String storageAccountName = SdkContext.randomResourceName("st",8);
            StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                        .withRegion(Region.US_WEST2)
                        .withNewResourceGroup("sampleStorageResourceGroup")
                        .create();

            // create a storage container to hold the file
            List<StorageAccountKey> keys = storage.getKeys();
            final String storageConnection = "DefaultEndpointsProtocol=https;"
                   + "AccountName=" + storage.name()
                   + ";AccountKey=" + keys.get(0).value()
                    + ";EndpointSuffix=core.windows.net";

            CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
            CloudBlobClient serviceClient = account.createCloudBlobClient();

            // Container name must be lower case.
            CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
            container.createIfNotExists();

            // Make the container public
            BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // write a blob to the container
            CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
            blob.uploadText("hello Azure");

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

<span data-ttu-id="42d35-154">通过命令行运行示例：</span><span class="sxs-lookup"><span data-stu-id="42d35-154">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="42d35-155">可以通过 Azure 门户或使用 [Azure 存储资源管理器](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)浏览存储帐户中的 `helloazure.txt` 文件。</span><span class="sxs-lookup"><span data-stu-id="42d35-155">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="42d35-156">使用 CLI 清理存储帐户：</span><span class="sxs-lookup"><span data-stu-id="42d35-156">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="42d35-157">学习更多示例</span><span class="sxs-lookup"><span data-stu-id="42d35-157">Explore more samples</span></span>

<span data-ttu-id="42d35-158">若要详细了解如何使用用于 Java 的 Azure 管理库来管理资源和自动执行任务，请参阅针对[虚拟机](java-sdk-azure-virtual-machine-samples.md)、[Web 应用](java-sdk-azure-web-apps-samples.md)和 [SQL 数据库](java-sdk-azure-sql-database-samples.md)的示例代码。</span><span class="sxs-lookup"><span data-stu-id="42d35-158">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](java-sdk-azure-virtual-machine-samples.md), [web apps](java-sdk-azure-web-apps-samples.md) and [SQL database](java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="42d35-159">参考和发行说明</span><span class="sxs-lookup"><span data-stu-id="42d35-159">Reference and release notes</span></span>

<span data-ttu-id="42d35-160">我们为所有包提供了[参考](http://docs.microsoft.com/java/api)文档。</span><span class="sxs-lookup"><span data-stu-id="42d35-160">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="42d35-161">获取帮助和提供反馈</span><span class="sxs-lookup"><span data-stu-id="42d35-161">Get help and give feedback</span></span>

<span data-ttu-id="42d35-162">在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) 社区中提问。</span><span class="sxs-lookup"><span data-stu-id="42d35-162">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="42d35-163">在[项目 GitHub](https://github.com/Azure/azure-sdk-for-java) 中针对用于 Java 的 Azure 库报告 bug 和反映问题。</span><span class="sxs-lookup"><span data-stu-id="42d35-163">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>