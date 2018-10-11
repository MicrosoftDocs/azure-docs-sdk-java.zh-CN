---
title: 使用 Java 配置 Web 应用部署 | Microsoft Docs
description: 通过用于 Java 的 Azure SDK 配置 Git 或 FTP Azure 应用服务部署的 Java 示例代码
author: rloutlaw
manager: douge
ms.assetid: 833e9c78-1e50-4c23-a611-f73a2f0c2983
ms.service: Azure
ms.devlang: java
ms.topic: article
ms.date: 03/30/2017
ms.author: routlaw;asirveda
ms.openlocfilehash: 910d1e43c9942d6402aeccb8757ba819b7453dab
ms.sourcegitcommit: b64017f119177f97da7a5930489874e67b09c0fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48893138"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a>从 Java 应用程序配置 Azure 应用服务部署源

[本示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)通过单个 [Azure 应用服务](https://docs.microsoft.com/azure/app-service/)计划将代码部署到四个应用程序，其中每个应用程序使用不同的部署源。

## <a name="run-the-sample"></a>运行示例

创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。 然后运行：

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

查看 [GitHub 上的完整示例代码](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java)。

## <a name="authenticate-with-azure"></a>使用 Azure 进行身份验证

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a>创建运行 Apache Tomcat 的应用服务应用

```java
// create a new Standard app service plan and create a single Java 8/Tomcat 8 app in it
WebApp app1 = azure.webApps().define(app1Name)
             .withNewResourceGroup(rgName)
             .withNewAppServicePlan(planName)
             .withRegion(Region.US_WEST)
             .withPricingTier(AppServicePricingTier.STANDARD_S1)
             .withJavaVersion(JavaVersion.JAVA_8_NEWEST)
             .withWebContainer(WebContainer.TOMCAT_8_0_NEWEST)
             .create();
```

`withJavaVersion()` 和 `withWebContainer()` 将应用服务配置为使用 Tomcat 8 为 HTTP 请求提供服务。

## <a name="deploy-a-java-application-using-ftp"></a>使用 FTP 部署 Java 应用程序
```java
// pass the PublishingProfile that contains FTP information to a helper method 
uploadFileToFtp(app1.getPublishingProfile(), "helloworld.war", 
      ManageWebAppSourceControl.class.getResourceAsStream("/helloworld.war"));

// Use the FTP classes in the Apache Commons library to connect to Azure using 
// the information from the PublishingProfile
private static void uploadFileToFtp(PublishingProfile profile, String fileName, InputStream file) throws Exception {
        FTPClient ftpClient = new FTPClient();
        String[] ftpUrlSegments = profile.ftpUrl().split("/", 2);
        String server = ftpUrlSegments[0];
        // Tomcat will deploy WAR files uploaded to this directory.
        String path = "./site/wwwroot/webapps"; 

        // FTP the build WAR to Azure
        ftpClient.connect(server);
        ftpClient.login(profile.ftpUsername(), profile.ftpPassword());
        ftpClient.setFileType(FTP.BINARY_FILE_TYPE);
        ftpClient.changeWorkingDirectory(path);
        ftpClient.storeFile(fileName, file);
        ftpClient.disconnect();
}
```

此代码将 WAR 文件上传到 `/site/wwwroot/webapps` 目录。 默认情况下，Tomcat 会在应用服务中部署此目录中的 WAR 文件。

## <a name="deploy-a-java-application-from-a-local-git-repo"></a>从本地 Git 存储库部署 Java 应用程序

```java
// get the publishing profile from the App Service webapp
PublishingProfile profile = app2.getPublishingProfile();

// create a new Git repo in the sample directory under src/main/resources 
Git git = Git
    .init()
    .setDirectory(new File(ManageWebAppSourceControl.class.getResource("/azure-samples-appservice-helloworld/").getPath()))
    .call();
git.add().addFilepattern(".").call();
// add the files in the sample app to an initial commit
git.commit().setMessage("Initial commit").call(); 

// push the commit using the Azure Git remote URL and credentials in the publishing profile
PushCommand command = git.push();
command.setRemote(profile.gitUrl()); 
command.setCredentialsProvider(new UsernamePasswordCredentialsProvider(profile.gitUsername(), profile.gitPassword()));
command.setRefSpecs(new RefSpec("master:master")); 
command.setForce(true);
command.call();
```      

此代码使用 [JGit](https://eclipse.org/jgit/) 库在 `src/main/resources/azure-samples-appservice-helloworld` 文件夹中创建新的 Git 存储库。 然后，本示例将该文件夹中的所有文件添加到初始提交内容，并使用 Web 应用的 `PublishingProfile` 中的 Git 部署信息将提交内容推送到 Azure。 

>[!NOTE]
> 存储库中的文件布局必须完全符合在 Azure 应用服务中的 `/site/wwwroot/` 目录下部署这些文件的方式。

## <a name="deploy-an-application-from-a-public-git-repo"></a>从公共 Git 存储库部署应用程序

```java
// deploy a .NET sample app from a public GitHub repo into a new webapp
WebApp app3 = azure.webApps().define(app3Name)
                .withNewResourceGroup(rgName)
                .withExistingAppServicePlan(plan)
                .defineSourceControl()
                .withPublicGitRepository(
                   "https://github.com/Azure-Samples/app-service-web-dotnet-get-started")
                .withBranch("master")
                .attach()
                .create();
 ```

 应用服务运行时使用存储库的 `master` 分支中的最新代码自动生成并部署 .NET 项目。

## <a name="continuous-deployment-from-a-github-repo"></a>从 GitHub 存储库持续部署

```java
// deploy the application whenever you push a new commit or merge a pull request into your master branch
WebApp app4 = azure.webApps()
                    .define(app4Name)
                    .withExistingResourceGroup(rgName)
                    .withExistingAppServicePlan(plan)
                    // Uncomment the following lines to turn on continuous deployment scenario
                    //.defineSourceControl()
                    //    .withContinuouslyIntegratedGitHubRepository("username", "reponame")
                    //    .withBranch("master")
                    //    .withGitHubAccessToken("YOUR GITHUB PERSONAL TOKEN")
                    //    .attach()
                    .create();
```  

`username` 和 `reponame` 值是在 GitHub 中使用的值。 [创建具有存储库读取权限的 GitHub 个人访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)并将其传递给 `withGitHubAccessToken`。 


## <a name="sample-explanation"></a>示例解释

本示例使用 Java 8 和 Tomcat 8 创建第一个在新建的[标准](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)应用服务计划中运行的应用程序。 然后，代码使用 `PublishingProfile` 对象中的信息通过 FTP 上传 WAR 文件，而 Tomcat 会部署该文件。

第二个应用程序使用的计划与第一个应用程序相同，也被配置为 Java 8/Tomcat 8 应用程序。 JGit 库在包含解包 Java Web 应用程序的文件夹（采用映射到应用服务的目录结构）中创建新的 Git 存储库。 提交新内容命令将该文件夹中的文件添加到新的 Git 存储库，Git 使用 Web 应用的 `PublishingProfile` 所提供的远程 URL 和用户名/密码将提交内容推送到 Azure。

第三个应用程序未针对 Java 和 Tomcat 进行配置， 而是直接从源部署了公共 GitHub 存储库中的某个 .NET 示例。

每次将更改推入或者将拉取请求合并到 GitHub 存储库的主分支时，第四个应用程序就会在主分支中部署代码。

| 示例中使用的类 | 说明
|-------|-------|
| [WebApp](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | 从 `azure.webApps().define()....create()` Fluent 链创建。 创建应用服务 Web 应用和该应用所需的所有资源。 大多数方法会在对象中查询配置详细信息，但 `restart()` 等谓词方法会更改 Web 应用的状态。
| [WebContainer](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | 包含静态公共字段的类，在定义运行 Java webcontainer 的 Web 应用时，这些字段用作 `withWebContainer()` 的参数。 为 Tomcat 和 Jetty 版本提供了相应的选项。
| [PublishingProfile](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | 使用 `getPublishingProfile()` 方法通过 WebApp 对象获取。 包含 FTP 和 Git 部署信息，其中包括部署用户名和密码（不同于 Azure 帐户或服务主体凭据）。
| [AppServicePlan](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | 由 `azure.appServices().appServicePlans().getByResourceGroup()` 返回。 提供了相应的方法用于检查计划中运行的 Web 应用的容量、层和数量。
| [AppServicePricingTier](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | 包含表示应用服务层的静态公共字段的类。 用于在创建应用期间通过 `withPricingTier()` 定义内联计划层，或者在通过 `azure.appServices().appServicePlans().define()` 定义计划时直接用于定义内联计划层
| [JavaVersion](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | 包含表示应用服务所支持 Java 版本的静态公共字段的类。 创建新的 Web 应用时，在 `define()...create()` 链接期间与 `withJavaVersion()` 结合使用。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [next-steps](includes/java-next-steps.md)]