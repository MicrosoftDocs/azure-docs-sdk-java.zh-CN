---
title: "用于 Java 的 Azure 库入门"
description: "了解如何创建 Azure 云资源，以及如何在 Java 应用程序中连接和使用这些资源。"
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
ms.openlocfilehash: f069183c96cdc42d590d2e58a5a6a500be5ab69a
ms.sourcegitcommit: 720c2eaf66532d277015610ec375c71e934d9ee6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2018
---
# <a name="get-started-with-cloud-development-using-the-azure-libraries-for-java"></a>使用用于 Java 的 Azure 库开始云开发

本指南逐步讲解如何为 Java 中的 Azure 开发设置开发环境。 然后，创建并连接一些 Azure 资源，以执行一些基本任务，例如，上传文件或部署 Web 应用程序。 完成本指南后，便可以在自己的 Java 应用程序中开始使用 Azure 服务。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。
- [Java 8](https://www.azul.com/downloads/zulu/)（Azure Cloud Shell 中已随附）
- [Maven 3](http://maven.apache.org/download.cgi)（Azure Cloud Shell 中已随附）

## <a name="set-up-authentication"></a>设置身份验证

Java 应用程序需要 Azure 订阅中的读取和创建权限才能运行本教程中的示例代码。 创建一个服务主体，并将应用程序配置为使用该服务主体的凭据运行。 通过服务主体可以创建一个与用户标识关联的非交互式帐户，该帐户仅拥有运行应用所需的特权。

[使用 Azure CLI 2.0 创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)并捕获输出。 在密码参数而非 `MY_SECURE_PASSWORD` 中提供[安全密码](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)。 密码必须为 8 到 16 个字符，并且至少符合以下 4 个条件中的 3 个：

* 包含小写字符
* 包含大写字符
* 包含数字
* 包含以下符号之一：@ # $ % ^ & * - _ ！ + = [ ] { } | \ : ‘ , . ? / ` ~ “ ( ) ;


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

这会提供采用以下格式的回复：

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

接下来，将以下内容复制到系统上的某个文本文件中：

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

将前四个值替换为以下内容：

- subscription：使用在 Azure CLI 2.0 中运行 `az account show` 后返回的 *id* 值。
- client：使用从服务主体输出中获取的输出中的 *appId* 值。
- key：使用服务主体输出中的 *password* 值。
- tenant：使用服务主体输出中的 *tenant* 值。

将此文件保存在系统上可供代码读取的安全位置。 在将来的代码中可以使用此文件，因此，我们建议将它存储在本文所述应用程序外部的某个位置。

在 shell 中使用该验证文件的完整路径来设置环境变量 `AZURE_AUTH_LOCATION`。   

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

如果在 Windows 环境中操作，请将变量添加到系统属性。 使用管理员权限打开 PowerShell 窗口，在将第二个变量替换为文件的路径后，输入以下命令：

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="create-a-new-maven-project"></a>创建新的 Maven 项目

> [!NOTE]
> 本指南使用 Maven 生成工具来生成和运行示例代码，但其他生成工具（例如 Gradle）也能配合用于 Java 的 Azure 库。 

在系统上的新目录中通过命令行创建一个 Maven 项目：

```
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp  \ 
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

这会在 `testAzureApp` 文件夹下创建一个基本的 Maven 项目。 将以下条目添加到项目 `pom.xml` 中，以导入本教程的示例代码中使用的库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
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

在顶级 `project` 元素下添加 `build` 条目，以使用 [maven-exec-plugin](http://www.mojohaus.org/exec-maven-plugin/) 来运行示例：

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```
   
## <a name="create-a-linux-virtual-machine"></a>创建 Linux 虚拟机

在项目的 `src/main/java/com/fabirkam` 目录中创建名为 `AzureApp.java` 的新文件，并在其中粘贴以下代码块。 使用计算机的实际值更新 `userName` 和 `sshKey` 变量。 该代码会在美国东部 Azure 区域中运行的资源组 `sampleResourceGroup` 内创建名为 `testLinuxVM` 的新 Linux VM。

```java
package com.fabrikam;

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
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
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

通过命令行运行示例：

```
mvn compile exec:java
```

当 SDK 向 Azure REST API 发出基础调用来配置虚拟机及其资源时，控制台中会显示一些 REST 请求和响应。 程序完成后，使用 Azure CLI 2.0 验证订阅中的虚拟机：

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

验证代码正常运行后，使用 CLI 删除 VM 及其资源。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>从 GitHub 存储库部署 Web 应用

将 `AzureApp.java` 中的 main 方法替换为以下代码，并在运行代码之前将 `appName` 变量更新为唯一值。 此代码会将公共 GitHub 存储库的 `master` 分支中的某个 Web 应用程序部署到免费定价层中运行的新 [Azure 应用服务 Web 应用](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。

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

如前所述使用 Maven 运行代码：

```
mvn clean compile exec:java
```

使用 CLI 打开指向该应用程序的浏览器：

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

验证部署后，请从订阅中删除 Web 应用和计划。

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>连接到 Azure SQL 数据库

将 `AzureApp.java` 中的当前 main 方法替换为以下代码，并为 `dbPassword` 变量设置实际值。
此代码会创建新的 SQL 数据库（包含一条允许远程访问的防火墙规则），然后使用 SQL 数据库 JBDC 驱动程序连接到该数据库。 

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
通过命令行运行示例：

```
mvn clean compile exec:java
```

然后使用 CLI 清理资源：

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>将 Blob 写入新存储帐户

将 `AzureApp.java` 中的当前 main 方法替换为以下代码。 此代码会创建一个 [Azure 存储帐户](https://docs.microsoft.com/azure/storage/storage-introduction)，然后使用用于 Java 的 Azure 存储库在云中创建新的文本文件。

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

通过命令行运行示例：

```
mvn clean compile exec:java
```

可以通过 Azure 门户或使用 [Azure 存储资源管理器](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)浏览存储帐户中的 `helloazure.txt` 文件。

使用 CLI 清理存储帐户：

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>学习更多示例

若要详细了解如何使用用于 Java 的 Azure 管理库来管理资源和自动执行任务，请参阅针对[虚拟机](java-sdk-azure-virtual-machine-samples.md)、[Web 应用](java-sdk-azure-web-apps-samples.md)和 [SQL 数据库](java-sdk-azure-sql-database-samples.md)的示例代码。

## <a name="reference-and-release-notes"></a>参考和发行说明

我们为所有包提供了[参考](http://docs.microsoft.com/java/api)文档。

## <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) 社区中提问。 在[项目 GitHub](https://github.com/Azure/azure-sdk-for-java) 中针对用于 Java 的 Azure 库报告 bug 和反映问题。
