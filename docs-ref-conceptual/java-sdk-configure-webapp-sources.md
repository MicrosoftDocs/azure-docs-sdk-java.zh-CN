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
ms.openlocfilehash: c416cb871ac8264cd6da2045d1ffdd50607fb092
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61592622"
---
# <a name="configure-azure-app-service-deployment-sources-from-your-java-applications"></a><span data-ttu-id="a6b93-103">从 Java 应用程序配置 Azure 应用服务部署源</span><span class="sxs-lookup"><span data-stu-id="a6b93-103">Configure Azure App Service deployment sources from your Java applications</span></span>

<span data-ttu-id="a6b93-104">[本示例](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel)通过单个 [Azure 应用服务](https://docs.microsoft.com/azure/app-service/)计划将代码部署到四个应用程序，其中每个应用程序使用不同的部署源。</span><span class="sxs-lookup"><span data-stu-id="a6b93-104">[This sample](https://github.com/Azure-Samples/compute-java-create-virtual-machines-across-regions-in-parallel) deploys code to four applications in a single [Azure App Service](https://docs.microsoft.com/azure/app-service/) plan, each using a different deployment source.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="a6b93-105">运行示例</span><span class="sxs-lookup"><span data-stu-id="a6b93-105">Run the sample</span></span>

<span data-ttu-id="a6b93-106">创建一个[身份验证文件](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)，并将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 设置为计算机上的文件。</span><span class="sxs-lookup"><span data-stu-id="a6b93-106">Create an [authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md) and set an environment variable `AZURE_AUTH_LOCATION` with the full path to the file on your computer.</span></span> <span data-ttu-id="a6b93-107">然后运行：</span><span class="sxs-lookup"><span data-stu-id="a6b93-107">Then run:</span></span>

```
git clone https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps.git
cd app-service-java-configure-deployment-sources-for-web-apps
mvn clean compile exec:java
```

<span data-ttu-id="a6b93-108">查看 [GitHub 上的完整示例代码](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java)。</span><span class="sxs-lookup"><span data-stu-id="a6b93-108">View the [complete sample code on GitHub](https://github.com/Azure-Samples/app-service-java-configure-deployment-sources-for-web-apps/blob/master/src/main/java/com/microsoft/azure/management/appservice/samples/ManageWebAppSourceControl.java).</span></span>

## <a name="authenticate-with-azure"></a><span data-ttu-id="a6b93-109">使用 Azure 进行身份验证</span><span class="sxs-lookup"><span data-stu-id="a6b93-109">Authenticate with Azure</span></span>

[!INCLUDE [auth-include](includes/java-auth-include.md)]

## <a name="create-a-app-service-app-running-apache-tomcat"></a><span data-ttu-id="a6b93-110">创建运行 Apache Tomcat 的应用服务应用</span><span class="sxs-lookup"><span data-stu-id="a6b93-110">Create a App Service app running Apache Tomcat</span></span>

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

<span data-ttu-id="a6b93-111">`withJavaVersion()` 和 `withWebContainer()` 将应用服务配置为使用 Tomcat 8 为 HTTP 请求提供服务。</span><span class="sxs-lookup"><span data-stu-id="a6b93-111">`withJavaVersion()` and `withWebContainer()` configure App Service to serve HTTP requests using Tomcat 8.</span></span>

## <a name="deploy-a-java-application-using-ftp"></a><span data-ttu-id="a6b93-112">使用 FTP 部署 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="a6b93-112">Deploy a Java application using FTP</span></span>
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

<span data-ttu-id="a6b93-113">此代码将 WAR 文件上传到 `/site/wwwroot/webapps` 目录。</span><span class="sxs-lookup"><span data-stu-id="a6b93-113">This code uploads a WAR file to the `/site/wwwroot/webapps` directory.</span></span> <span data-ttu-id="a6b93-114">默认情况下，Tomcat 会在应用服务中部署此目录中的 WAR 文件。</span><span class="sxs-lookup"><span data-stu-id="a6b93-114">Tomcat deploys WAR files placed in this directory by default in App Service.</span></span>

## <a name="deploy-a-java-application-from-a-local-git-repo"></a><span data-ttu-id="a6b93-115">从本地 Git 存储库部署 Java 应用程序</span><span class="sxs-lookup"><span data-stu-id="a6b93-115">Deploy a Java application from a local Git repo</span></span>

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

<span data-ttu-id="a6b93-116">此代码使用 [JGit](https://eclipse.org/jgit/) 库在 `src/main/resources/azure-samples-appservice-helloworld` 文件夹中创建新的 Git 存储库。</span><span class="sxs-lookup"><span data-stu-id="a6b93-116">This code uses the [JGit](https://eclipse.org/jgit/) libraries to create a new Git repo in the `src/main/resources/azure-samples-appservice-helloworld` folder.</span></span> <span data-ttu-id="a6b93-117">然后，本示例将该文件夹中的所有文件添加到初始提交内容，并使用 Web 应用的 `PublishingProfile` 中的 Git 部署信息将提交内容推送到 Azure。</span><span class="sxs-lookup"><span data-stu-id="a6b93-117">The sample then adds all files in the folder to an initial commit and pushes the commit to Azure using Git deployment information from the webapp's `PublishingProfile`.</span></span> 

>[!NOTE]
> <span data-ttu-id="a6b93-118">存储库中的文件布局必须完全符合在 Azure 应用服务中的 `/site/wwwroot/` 目录下部署这些文件的方式。</span><span class="sxs-lookup"><span data-stu-id="a6b93-118">The layout of the files in the repo must match exactly how you want the files deployed under the `/site/wwwroot/` directory in Azure App Service.</span></span>

## <a name="deploy-an-application-from-a-public-git-repo"></a><span data-ttu-id="a6b93-119">从公共 Git 存储库部署应用程序</span><span class="sxs-lookup"><span data-stu-id="a6b93-119">Deploy an application from a public Git repo</span></span>

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

 <span data-ttu-id="a6b93-120">应用服务运行时使用存储库的 `master` 分支中的最新代码自动生成并部署 .NET 项目。</span><span class="sxs-lookup"><span data-stu-id="a6b93-120">The App Service runtime automatically builds and deploys the .NET project using the latest code on the `master` branch of the repo.</span></span>

## <a name="continuous-deployment-from-a-github-repo"></a><span data-ttu-id="a6b93-121">从 GitHub 存储库持续部署</span><span class="sxs-lookup"><span data-stu-id="a6b93-121">Continuous deployment from a GitHub repo</span></span>

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

<span data-ttu-id="a6b93-122">`username` 和 `reponame` 值是在 GitHub 中使用的值。</span><span class="sxs-lookup"><span data-stu-id="a6b93-122">The `username` and `reponame` values are the ones used in GitHub.</span></span> <span data-ttu-id="a6b93-123">[创建具有存储库读取权限的 GitHub 个人访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)并将其传递给 `withGitHubAccessToken`。</span><span class="sxs-lookup"><span data-stu-id="a6b93-123">[Create a GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) with repo read permissions and pass it to `withGitHubAccessToken`.</span></span> 


## <a name="sample-explanation"></a><span data-ttu-id="a6b93-124">示例解释</span><span class="sxs-lookup"><span data-stu-id="a6b93-124">Sample explanation</span></span>

<span data-ttu-id="a6b93-125">本示例使用 Java 8 和 Tomcat 8 创建第一个在新建的[标准](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)应用服务计划中运行的应用程序。</span><span class="sxs-lookup"><span data-stu-id="a6b93-125">The sample creates the first application using Java 8 and Tomcat 8 running in a newly created [Standard](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) App Service plan.</span></span> <span data-ttu-id="a6b93-126">然后，代码使用 `PublishingProfile` 对象中的信息通过 FTP 上传 WAR 文件，而 Tomcat 会部署该文件。</span><span class="sxs-lookup"><span data-stu-id="a6b93-126">The code then FTPs a WAR file using the information in the `PublishingProfile` object and Tomcat deploys it.</span></span>

<span data-ttu-id="a6b93-127">第二个应用程序使用的计划与第一个应用程序相同，也被配置为 Java 8/Tomcat 8 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a6b93-127">The second application uses in the same plan as the first and is also configured as a Java 8/Tomcat 8 application.</span></span> <span data-ttu-id="a6b93-128">JGit 库在包含解包 Java Web 应用程序的文件夹（采用映射到应用服务的目录结构）中创建新的 Git 存储库。</span><span class="sxs-lookup"><span data-stu-id="a6b93-128">The JGit libraries create a new Git repository in a folder that contains an unpacked Java web application in a directory structure that maps to App Service.</span></span> <span data-ttu-id="a6b93-129">提交新内容命令将该文件夹中的文件添加到新的 Git 存储库，Git 使用 Web 应用的 `PublishingProfile` 所提供的远程 URL 和用户名/密码将提交内容推送到 Azure。</span><span class="sxs-lookup"><span data-stu-id="a6b93-129">A new commit adds the files in the folder to the new Git repo, and Git pushes the commit to Azure with a remote URL and username/password provided by the webapp's `PublishingProfile`.</span></span>

<span data-ttu-id="a6b93-130">第三个应用程序未针对 Java 和 Tomcat 进行配置，</span><span class="sxs-lookup"><span data-stu-id="a6b93-130">The third application is not configured for Java and Tomcat.</span></span> <span data-ttu-id="a6b93-131">而是直接从源部署了公共 GitHub 存储库中的某个 .NET 示例。</span><span class="sxs-lookup"><span data-stu-id="a6b93-131">Instead, a .NET sample in a public GitHub repo is deployed directly from source.</span></span>

<span data-ttu-id="a6b93-132">每次将更改推入或者将拉取请求合并到 GitHub 存储库的主分支时，第四个应用程序就会在主分支中部署代码。</span><span class="sxs-lookup"><span data-stu-id="a6b93-132">The fourth application deploys the code in your master branch every time you push changes or merge a pull request into the GitHub repo's master branch.</span></span>

| <span data-ttu-id="a6b93-133">示例中使用的类</span><span class="sxs-lookup"><span data-stu-id="a6b93-133">Class used in sample</span></span> | <span data-ttu-id="a6b93-134">说明</span><span class="sxs-lookup"><span data-stu-id="a6b93-134">Notes</span></span>
|-------|-------|
| [<span data-ttu-id="a6b93-135">WebApp</span><span class="sxs-lookup"><span data-stu-id="a6b93-135">WebApp</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_app) | <span data-ttu-id="a6b93-136">从 `azure.webApps().define()....create()` Fluent 链创建。</span><span class="sxs-lookup"><span data-stu-id="a6b93-136">Created from the `azure.webApps().define()....create()` fluent chain.</span></span> <span data-ttu-id="a6b93-137">创建应用服务 Web 应用和该应用所需的所有资源。</span><span class="sxs-lookup"><span data-stu-id="a6b93-137">Creates a App Service web app and any resources needed for the app.</span></span> <span data-ttu-id="a6b93-138">大多数方法会在对象中查询配置详细信息，但 `restart()` 等谓词方法会更改 Web 应用的状态。</span><span class="sxs-lookup"><span data-stu-id="a6b93-138">Most methods query the object for configuration details, but verb methods like `restart()` change the state of the webapp.</span></span>
| [<span data-ttu-id="a6b93-139">WebContainer</span><span class="sxs-lookup"><span data-stu-id="a6b93-139">WebContainer</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._web_container) | <span data-ttu-id="a6b93-140">包含静态公共字段的类，在定义运行 Java webcontainer 的 Web 应用时，这些字段用作 `withWebContainer()` 的参数。</span><span class="sxs-lookup"><span data-stu-id="a6b93-140">Class with static public fields used as paramters to `withWebContainer()` when defining a WebApp running a Java webcontainer.</span></span> <span data-ttu-id="a6b93-141">为 Tomcat 和 Jetty 版本提供了相应的选项。</span><span class="sxs-lookup"><span data-stu-id="a6b93-141">Has choices for both Jetty and Tomcat versions.</span></span>
| [<span data-ttu-id="a6b93-142">PublishingProfile</span><span class="sxs-lookup"><span data-stu-id="a6b93-142">PublishingProfile</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._publishing_profile) | <span data-ttu-id="a6b93-143">使用 `getPublishingProfile()` 方法通过 WebApp 对象获取。</span><span class="sxs-lookup"><span data-stu-id="a6b93-143">Obtained through a WebApp object using the `getPublishingProfile()` method.</span></span> <span data-ttu-id="a6b93-144">包含 FTP 和 Git 部署信息，其中包括部署用户名和密码（不同于 Azure 帐户或服务主体凭据）。</span><span class="sxs-lookup"><span data-stu-id="a6b93-144">Contains FTP and Git deployment information, including deployment username and password (which is separate from Azure account or service principal credentials).</span></span>
| [<span data-ttu-id="a6b93-145">AppServicePlan</span><span class="sxs-lookup"><span data-stu-id="a6b93-145">AppServicePlan</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_plan) | <span data-ttu-id="a6b93-146">由 `azure.appServices().appServicePlans().getByResourceGroup()` 返回。</span><span class="sxs-lookup"><span data-stu-id="a6b93-146">Returned by `azure.appServices().appServicePlans().getByResourceGroup()`.</span></span> <span data-ttu-id="a6b93-147">提供了相应的方法用于检查计划中运行的 Web 应用的容量、层和数量。</span><span class="sxs-lookup"><span data-stu-id="a6b93-147">Methods are availble to check the capacity, tier, and number of web apps running in the plan.</span></span>
| [<span data-ttu-id="a6b93-148">AppServicePricingTier</span><span class="sxs-lookup"><span data-stu-id="a6b93-148">AppServicePricingTier</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._app_service_pricing_tier) | <span data-ttu-id="a6b93-149">包含表示应用服务层的静态公共字段的类。</span><span class="sxs-lookup"><span data-stu-id="a6b93-149">Class with static public fields representing App Service tiers.</span></span> <span data-ttu-id="a6b93-150">用于在创建应用期间通过 `withPricingTier()` 定义内联计划层，或者在通过 `azure.appServices().appServicePlans().define()` 定义计划时直接用于定义内联计划层</span><span class="sxs-lookup"><span data-stu-id="a6b93-150">Used to define a plan tier in-line during app creation with `withPricingTier()` or directly when defining a plan via `azure.appServices().appServicePlans().define()`</span></span>
| [<span data-ttu-id="a6b93-151">JavaVersion</span><span class="sxs-lookup"><span data-stu-id="a6b93-151">JavaVersion</span></span>](https://docs.microsoft.com/java/api/com.microsoft.azure.management.appservice._java_version) | <span data-ttu-id="a6b93-152">包含表示应用服务所支持 Java 版本的静态公共字段的类。</span><span class="sxs-lookup"><span data-stu-id="a6b93-152">Class with static public fields representing Java versions supported by App Service.</span></span> <span data-ttu-id="a6b93-153">创建新的 Web 应用时，在 `define()...create()` 链接期间与 `withJavaVersion()` 结合使用。</span><span class="sxs-lookup"><span data-stu-id="a6b93-153">Used with `withJavaVersion()` during the `define()...create()` chain when creating a new webapp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6b93-154">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a6b93-154">Next steps</span></span>

[!INCLUDE [next-steps](includes/java-next-steps.md)]