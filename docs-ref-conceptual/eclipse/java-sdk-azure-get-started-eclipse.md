---
title: "通过 Eclipse 开始使用用于 Java 的 Azure"
description: "结合自己的 Azure 订阅开始了解用于 Java 的 Azure 库的基本用法。"
keywords: "Azure, Java, SDK, API, 身份验证, 入门"
author: roygara
ms.author: v-rogara
manager: timlt
ms.date: 10/30/2017
ms.topic: get-started-article
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.service: multiple
ms.openlocfilehash: 1c1ef7b8646824c5c8bfcbbf5e0507c95ac1ee79
ms.sourcegitcommit: fcf1189ede712ae30f8c7626bde50c9b8bb561bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="get-started-with-the-azure-libraries-using-eclipse"></a><span data-ttu-id="d877b-104">通过 Eclipse 开始使用 Azure 库</span><span class="sxs-lookup"><span data-stu-id="d877b-104">Get started with the Azure libraries using Eclipse</span></span>

<span data-ttu-id="d877b-105">本指南逐步讲解如何设置开发环境和使用用于 Java 的 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="d877b-105">This guide walks you through setting up a development environment and using the Azure libraries for Java.</span></span> <span data-ttu-id="d877b-106">其中将会创建一个服务主体用于进行 Azure 身份验证，并运行一些示例代码在订阅中创建和使用 Azure 资源。</span><span class="sxs-lookup"><span data-stu-id="d877b-106">You'll create a service principal to authenticate with Azure and run some sample code that creates and uses Azure resources in your subscription.</span></span> <span data-ttu-id="d877b-107">可以选择性地使用 Eclipse 在 Azure 中进行 Java 开发。</span><span class="sxs-lookup"><span data-stu-id="d877b-107">Using Eclipse is optional for Java development with Azure.</span></span> <span data-ttu-id="d877b-108">包含 Maven 集成的任何 IDE 都可正常工作。</span><span class="sxs-lookup"><span data-stu-id="d877b-108">Any IDE that has Maven integration works.</span></span> <span data-ttu-id="d877b-109">或者，如果不想要使用任何 IDE，可以使用 Maven 从命令行运行代码。</span><span class="sxs-lookup"><span data-stu-id="d877b-109">Alternatively, you can run your code from the commandline using Maven if you prefer not to use any IDE.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d877b-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="d877b-110">Prerequisites</span></span>

- <span data-ttu-id="d877b-111">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="d877b-111">An Azure account.</span></span> <span data-ttu-id="d877b-112">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="d877b-112">If you don't have one, [get a free trial](https://azure.microsoft.com/free/)</span></span>
- <span data-ttu-id="d877b-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="d877b-113">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>
- <span data-ttu-id="d877b-114">[Eclipse](http://www.eclipse.org/downloads/) 的最新稳定版本</span><span class="sxs-lookup"><span data-stu-id="d877b-114">The latest stable version of [Eclipse](http://www.eclipse.org/downloads/)</span></span>

## <a name="set-up-authentication"></a><span data-ttu-id="d877b-115">设置身份验证</span><span class="sxs-lookup"><span data-stu-id="d877b-115">Set up authentication</span></span>

<span data-ttu-id="d877b-116">Java 应用程序需要 Azure 订阅中的读取和创建权限才能运行本教程中的示例代码。</span><span class="sxs-lookup"><span data-stu-id="d877b-116">Your Java application needs read and create permissions in your Azure subscription to run the sample code in this tutorial.</span></span> <span data-ttu-id="d877b-117">创建一个服务主体，并将应用程序配置为使用该服务主体的凭据运行。</span><span class="sxs-lookup"><span data-stu-id="d877b-117">Create a service principal and configure your application to run with its credentials.</span></span> <span data-ttu-id="d877b-118">通过服务主体可以创建一个与用户标识关联的非交互式帐户，该帐户仅拥有运行应用所需的特权。</span><span class="sxs-lookup"><span data-stu-id="d877b-118">Service principals provide a way to create a non-interactive account associated with your identity to which you grant only the privileges your app needs to run.</span></span>

<span data-ttu-id="d877b-119">[创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)以授予代码在订阅中创建和更新资源的权限，而无需直接使用帐户凭据。</span><span class="sxs-lookup"><span data-stu-id="d877b-119">[Create a service principal](/cli/azure/create-an-azure-service-principal-azure-cli) to grant your code permission to create and update resources in your subscription without using your account credentials directly.</span></span> <span data-ttu-id="d877b-120">请确保捕获输出。</span><span class="sxs-lookup"><span data-stu-id="d877b-120">Make sure to capture the output.</span></span> <span data-ttu-id="d877b-121">在密码参数而非 `MY_SECURE_PASSWORD` 中提供[安全密码](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy)。</span><span class="sxs-lookup"><span data-stu-id="d877b-121">Provide a [secure password](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy) in the password argument instead of `MY_SECURE_PASSWORD`.</span></span> <span data-ttu-id="d877b-122">密码必须为 8 到 16 个字符，并且至少符合以下 4 个条件中的 3 个：</span><span class="sxs-lookup"><span data-stu-id="d877b-122">Your password must be 8 to 16 characters and match at least 3 out of the 4 following criteria:</span></span>

* <span data-ttu-id="d877b-123">包含小写字符</span><span class="sxs-lookup"><span data-stu-id="d877b-123">Include lowercase characters</span></span>
* <span data-ttu-id="d877b-124">包含大写字符</span><span class="sxs-lookup"><span data-stu-id="d877b-124">Include uppercase characters</span></span>
* <span data-ttu-id="d877b-125">包含数字</span><span class="sxs-lookup"><span data-stu-id="d877b-125">Include numbers</span></span>
* <span data-ttu-id="d877b-126">包含以下符号之一：@ # $ % ^ & * - _ ！</span><span class="sxs-lookup"><span data-stu-id="d877b-126">Include one of the following symbols: @ # $ % ^ & * - _ !</span></span> <span data-ttu-id="d877b-127">+ = [ ] { } | \ : ‘ , .</span><span class="sxs-lookup"><span data-stu-id="d877b-127">+ = [ ] { } | \ : ‘ , .</span></span> <span data-ttu-id="d877b-128">?</span><span class="sxs-lookup"><span data-stu-id="d877b-128">?</span></span> <span data-ttu-id="d877b-129">/ \` ~ “ ( ) ;</span><span class="sxs-lookup"><span data-stu-id="d877b-129">/ \` ~ “ ( ) ;</span></span>


```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

<span data-ttu-id="d877b-130">这会提供采用以下格式的回复：</span><span class="sxs-lookup"><span data-stu-id="d877b-130">Which gives you a reply in the following format:</span></span>

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

<span data-ttu-id="d877b-131">接下来，将以下内容复制到系统上的某个文本文件中：</span><span class="sxs-lookup"><span data-stu-id="d877b-131">Next, copy the following into a text file on your system:</span></span>

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

<span data-ttu-id="d877b-132">将前四个值替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="d877b-132">Replace the top four values with the following:</span></span>

- <span data-ttu-id="d877b-133">subscription：使用在 Azure CLI 2.0 中运行 `az account show` 后返回的 *id* 值。</span><span class="sxs-lookup"><span data-stu-id="d877b-133">subscription: use the *id* value from `az account show` in the Azure CLI 2.0.</span></span>
- <span data-ttu-id="d877b-134">client：使用从服务主体输出中获取的输出中的 *appId* 值。</span><span class="sxs-lookup"><span data-stu-id="d877b-134">client: use the *appId* value from the output taken from a service principal output.</span></span>
- <span data-ttu-id="d877b-135">key：使用服务主体输出中的 *password* 值。</span><span class="sxs-lookup"><span data-stu-id="d877b-135">key: use the *password* value from the service principal output.</span></span>
- <span data-ttu-id="d877b-136">tenant：使用服务主体输出中的 *tenant* 值。</span><span class="sxs-lookup"><span data-stu-id="d877b-136">tenant: use the *tenant* value from the service principal output.</span></span>

<span data-ttu-id="d877b-137">将此文件保存在系统上可供代码读取的安全位置。</span><span class="sxs-lookup"><span data-stu-id="d877b-137">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="d877b-138">在将来的代码中可以使用此文件，因此，我们建议将它存储在本文所述应用程序外部的某个位置。</span><span class="sxs-lookup"><span data-stu-id="d877b-138">You may use this file for future code so it's recommended to store it somewhere external to the application in this article.</span></span>

<span data-ttu-id="d877b-139">在 shell 中使用该验证文件的完整路径来设置环境变量 `AZURE_AUTH_LOCATION`。</span><span class="sxs-lookup"><span data-stu-id="d877b-139">Set an environment variable `AZURE_AUTH_LOCATION` with the full path to the authentication file in your shell.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

<span data-ttu-id="d877b-140">如果在 Windows 环境中操作，请将变量添加到系统属性。</span><span class="sxs-lookup"><span data-stu-id="d877b-140">If you're working in a windows environment, add the variable to your system properties.</span></span> <span data-ttu-id="d877b-141">打开 PowerShell，在将第二个变量替换为文件的路径后，请输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="d877b-141">Open PowerShell and, after replacing the second variable with the path to your file, enter the following command:</span></span>

```powershell
[Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\<fullpath>\azureauth.properties", "Machine")
```

## <a name="create-a-new-maven-project"></a><span data-ttu-id="d877b-142">创建新的 Maven 项目</span><span class="sxs-lookup"><span data-stu-id="d877b-142">Create a new Maven project</span></span>

> [!NOTE]
> <span data-ttu-id="d877b-143">本指南使用 Maven 生成工具来生成和运行示例代码，但其他生成工具（例如 Gradle）也能配合用于 Java 的 Azure 库。</span><span class="sxs-lookup"><span data-stu-id="d877b-143">This guide uses Maven build tool to build and run the sample code, but other build tools such as Gradle also work with the Azure libraries for Java.</span></span> 

<span data-ttu-id="d877b-144">打开 Eclipse，选择“文件” -> “新建”。</span><span class="sxs-lookup"><span data-stu-id="d877b-144">Open Eclipse, select **File** -> **New**.</span></span> <span data-ttu-id="d877b-145">在显示的新窗口中，打开“Maven”文件夹，然后选择“Maven 项目”。</span><span class="sxs-lookup"><span data-stu-id="d877b-145">In the new window that appears open the Maven folder then select Maven Project.</span></span> 

<span data-ttu-id="d877b-146">在下一屏幕中保留默认选择，并选择“下一步”。</span><span class="sxs-lookup"><span data-stu-id="d877b-146">Leave the selections on the next screen defaults, and select **Next**.</span></span> <span data-ttu-id="d877b-147">在此屏幕中，针对原型相关的设置执行上述相同的操作。</span><span class="sxs-lookup"><span data-stu-id="d877b-147">Do the same for this screen regarding archetypes.</span></span>

<span data-ttu-id="d877b-148">出现提示输入 groupID、ArtifactID 等的屏幕时，请输入“com.fabrikam”作为 groupID，输入“AzureApp”作为 artifactID。</span><span class="sxs-lookup"><span data-stu-id="d877b-148">When you come to the screen asking for groupID, ArtifactID, etc. Enter "com.fabrikam" for the groupID and enter "AzureApp" for the artifactID.</span></span>

<span data-ttu-id="d877b-149">现在打开 pom.xml 文件。</span><span class="sxs-lookup"><span data-stu-id="d877b-149">Now, open the pom.xml file.</span></span> <span data-ttu-id="d877b-150">在 `dependencies` 标记中添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="d877b-150">Inside the `dependencies` tag add the following code:</span></span>

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

<span data-ttu-id="d877b-151">现在保存 pom.xml。</span><span class="sxs-lookup"><span data-stu-id="d877b-151">Now, save the pom.xml.</span></span> <span data-ttu-id="d877b-152">这会提示 Eclipse 下载所有指定的依赖项。</span><span class="sxs-lookup"><span data-stu-id="d877b-152">This prompts Eclipse to download all the specified dependencies.</span></span> <span data-ttu-id="d877b-153">此过程可能需要花费片刻时间。</span><span class="sxs-lookup"><span data-stu-id="d877b-153">This may take a moment.</span></span>
   
## <a name="install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="d877b-154">安装用于 Eclipse 的 Azure 工具包</span><span class="sxs-lookup"><span data-stu-id="d877b-154">Install the azure toolkit for Eclipse</span></span>

<span data-ttu-id="d877b-155">若要以编程方式部署 Web 应用或 API，则需要使用 [Azure 工具包](azure-toolkit-for-eclipse.md)，但是，该工具包目前不可用于其他任何类型的开发。</span><span class="sxs-lookup"><span data-stu-id="d877b-155">The [Azure toolkit](azure-toolkit-for-eclipse.md) is necessary if you're going to be deploying web apps or APIs programmatically but is not currently used for any other kinds of development.</span></span> <span data-ttu-id="d877b-156">下面是安装过程的摘要。</span><span class="sxs-lookup"><span data-stu-id="d877b-156">The following is a summary of the installation process.</span></span> <span data-ttu-id="d877b-157">有关详细步骤，请参阅[安装用于 Eclipse 的 Azure 工具包](azure-toolkit-for-eclipse.md)。</span><span class="sxs-lookup"><span data-stu-id="d877b-157">For detailed stpes, visit [Installing the Azure Toolkit for Eclipse](azure-toolkit-for-eclipse.md).</span></span>

<span data-ttu-id="d877b-158">选择“帮助”菜单，然后选择“安装新软件”。</span><span class="sxs-lookup"><span data-stu-id="d877b-158">Select the **Help** menu and then select **Install New software**.</span></span>

<span data-ttu-id="d877b-159">在“使用:”字段中，输入 `http://dl.microsoft.com/eclipse` 并按 Enter。</span><span class="sxs-lookup"><span data-stu-id="d877b-159">In the **Work with:** field enter `http://dl.microsoft.com/eclipse` and press enter.</span></span>

<span data-ttu-id="d877b-160">然后，选中“用于 Java 的 Azure 工具包”旁边的复选框，并取消选中“在安装过程中联系所有更新站点以查找所需的软件”对应的复选框。</span><span class="sxs-lookup"><span data-stu-id="d877b-160">Then, select the checkbox next to **Azure toolkit for Java** and uncheck the checkbox for **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="d877b-161">然后选择“下一步”。</span><span class="sxs-lookup"><span data-stu-id="d877b-161">Then select next.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="d877b-162">创建 Linux 虚拟机</span><span class="sxs-lookup"><span data-stu-id="d877b-162">Create a Linux virtual machine</span></span>

<span data-ttu-id="d877b-163">在项目的 `src/main/java` 目录中创建名为 `AzureApp.java` 的新文件，并在其中粘贴以下代码块。</span><span class="sxs-lookup"><span data-stu-id="d877b-163">Create a new file named `AzureApp.java` in the project's `src/main/java` directory and paste in the following block of code.</span></span> <span data-ttu-id="d877b-164">使用计算机的实际值更新 `userName` 和 `sshKey` 变量。</span><span class="sxs-lookup"><span data-stu-id="d877b-164">Update the `userName` and `sshKey` variables with real values for your machine.</span></span> <span data-ttu-id="d877b-165">该代码会在美国东部 Azure 区域中运行的资源组 `sampleResourceGroup` 内创建名为 `testLinuxVM` 的新 Linux VM。</span><span class="sxs-lookup"><span data-stu-id="d877b-165">The code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleResourceGroup` running in the US East Azure region.</span></span>

<span data-ttu-id="d877b-166">若要创建 `sshkey`，请打开 Azure Cloud Shell 并输入 `ssh-keygen -t rsa -b 2048`。</span><span class="sxs-lookup"><span data-stu-id="d877b-166">In order to create an `sshkey`, open the azure cloud shell and enter `ssh-keygen -t rsa -b 2048`.</span></span> <span data-ttu-id="d877b-167">为文件命名，访问 .public 文件以获取要在后续代码中使用的密钥，然后将整个密钥复制并粘贴到变量 `sshKey` 中。</span><span class="sxs-lookup"><span data-stu-id="d877b-167">Name the file and then access the .public file to get the key, which you use in the following code, copy and paste it all into your variable `sshKey`.</span></span>

```java
package com.fabrikam.AzureApp;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.PricingTier;
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
import java.util.List;

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


<span data-ttu-id="d877b-168">此时会在控制台中看到一些 REST 请求和响应，因为 SDK 会对 Azure REST API 进行基础调用，以便配置虚拟机及其资源。</span><span class="sxs-lookup"><span data-stu-id="d877b-168">You see some REST requests and responses in the console as the SDK makes the underlying calls to the Azure REST API to configure the virtual machine and its resources.</span></span> <span data-ttu-id="d877b-169">程序完成后，使用 Azure CLI 2.0 验证订阅中的虚拟机：</span><span class="sxs-lookup"><span data-stu-id="d877b-169">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="d877b-170">验证代码正常运行后，使用 CLI 删除 VM 及其资源。</span><span class="sxs-lookup"><span data-stu-id="d877b-170">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="d877b-171">从 GitHub 存储库部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="d877b-171">Deploy a web app from a GitHub repo</span></span>

<span data-ttu-id="d877b-172">将 `AzureApp.java` 中的 main 方法替换为以下代码，并在运行代码之前将 `appName` 变量更新为唯一值。</span><span class="sxs-lookup"><span data-stu-id="d877b-172">Replace the main method in `AzureApp.java` with the one below, updating the `appName` variable to a unique value before running the code.</span></span> <span data-ttu-id="d877b-173">此代码会将公共 GitHub 存储库的 `master` 分支中的某个 Web 应用程序部署到免费定价层中运行的新 [Azure 应用服务 Web 应用](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="d877b-173">This code deploys a web application from the `master` branch in a public GitHub repo into a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free pricing tier.</span></span>

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

<span data-ttu-id="d877b-174">如前所述使用 Maven 运行代码：</span><span class="sxs-lookup"><span data-stu-id="d877b-174">Run the code as before using Maven:</span></span>

<span data-ttu-id="d877b-175">使用 CLI 打开指向该应用程序的浏览器：</span><span class="sxs-lookup"><span data-stu-id="d877b-175">Open a browser pointed to the application using the CLI:</span></span>

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```
<span data-ttu-id="d877b-176">验证部署后，请从订阅中删除 Web 应用和计划。</span><span class="sxs-lookup"><span data-stu-id="d877b-176">Remove the web app and plan from your subscription once you've verified the deployment.</span></span>

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a><span data-ttu-id="d877b-177">连接到 Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="d877b-177">Connect to an Azure SQL database</span></span>

<span data-ttu-id="d877b-178">将 `AzureApp.java` 中的当前 main 方法替换为以下代码，并为 `dbPassword` 变量设置实际值。</span><span class="sxs-lookup"><span data-stu-id="d877b-178">Replace the current main method in `AzureApp.java` with the code below, setting a real value for the `dbPassword` variable.</span></span>
<span data-ttu-id="d877b-179">此代码会创建新的 SQL 数据库（包含一条允许远程访问的防火墙规则），然后使用 SQL 数据库 JBDC 驱动程序连接到该数据库。</span><span class="sxs-lookup"><span data-stu-id="d877b-179">This code creates a new SQL database with a firewall rule allowing remote access,  and then connects to it using the SQL Database JBDC driver.</span></span> 

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
<span data-ttu-id="d877b-180">通过命令行运行示例：</span><span class="sxs-lookup"><span data-stu-id="d877b-180">Run the sample from the command line:</span></span>

```
mvn clean compile exec:java
```

<span data-ttu-id="d877b-181">然后使用 CLI 清理资源：</span><span class="sxs-lookup"><span data-stu-id="d877b-181">Then clean up the resources using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="d877b-182">将 Blob 写入新存储帐户</span><span class="sxs-lookup"><span data-stu-id="d877b-182">Write a blob into a new storage account</span></span>

<span data-ttu-id="d877b-183">将 `AzureApp.java` 中的当前 main 方法替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="d877b-183">Replace the current main method in `AzureApp.java` with the code below.</span></span> <span data-ttu-id="d877b-184">此代码会创建一个 [Azure 存储帐户](https://docs.microsoft.com/azure/storage/storage-introduction)，然后使用用于 Java 的 Azure 存储库在云中创建新的文本文件。</span><span class="sxs-lookup"><span data-stu-id="d877b-184">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Java to create a new text file in the cloud.</span></span>

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

            // create a storage container to hold the files
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

<span data-ttu-id="d877b-185">通过命令行运行示例：</span><span class="sxs-lookup"><span data-stu-id="d877b-185">Run the sample from the command line:</span></span>

<span data-ttu-id="d877b-186">可以通过 Azure 门户或使用 [Azure 存储资源管理器](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)浏览存储帐户中的 `helloazure.txt` 文件。</span><span class="sxs-lookup"><span data-stu-id="d877b-186">You can browse for the `helloazure.txt` file in your storage account through the Azure portal or with [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs).</span></span>

<span data-ttu-id="d877b-187">使用 CLI 清理存储帐户：</span><span class="sxs-lookup"><span data-stu-id="d877b-187">Clean up the storage account using the CLI:</span></span>

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="d877b-188">学习更多示例</span><span class="sxs-lookup"><span data-stu-id="d877b-188">Explore more samples</span></span>

<span data-ttu-id="d877b-189">若要详细了解如何使用用于 Java 的 Azure 管理库来管理资源和自动执行任务，请参阅针对[虚拟机](../java-sdk-azure-virtual-machine-samples.md)、[Web 应用](../java-sdk-azure-web-apps-samples.md)和 [SQL 数据库](../java-sdk-azure-sql-database-samples.md)的示例代码。</span><span class="sxs-lookup"><span data-stu-id="d877b-189">To learn more about how to use the Azure management libraries for Java to manage resources and automate tasks, see our sample code for [virtual machines](../java-sdk-azure-virtual-machine-samples.md), [web apps](../java-sdk-azure-web-apps-samples.md) and [SQL database](../java-sdk-azure-sql-database-samples.md).</span></span>

## <a name="reference-and-release-notes"></a><span data-ttu-id="d877b-190">参考和发行说明</span><span class="sxs-lookup"><span data-stu-id="d877b-190">Reference and release notes</span></span>

<span data-ttu-id="d877b-191">我们为所有包提供了[参考](http://docs.microsoft.com/java/api)文档。</span><span class="sxs-lookup"><span data-stu-id="d877b-191">A [reference](http://docs.microsoft.com/java/api) is available for all packages.</span></span>

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="d877b-192">获取帮助和提供反馈</span><span class="sxs-lookup"><span data-stu-id="d877b-192">Get help and give feedback</span></span>

<span data-ttu-id="d877b-193">在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java) 社区中提问。</span><span class="sxs-lookup"><span data-stu-id="d877b-193">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure+java).</span></span> <span data-ttu-id="d877b-194">在[项目 GitHub](https://github.com/Azure/azure-sdk-for-java) 中针对用于 Java 的 Azure 库报告 bug 和反映问题。</span><span class="sxs-lookup"><span data-stu-id="d877b-194">Report bugs and open issues against the Azure libraries for Java on the [project GitHub](https://github.com/Azure/azure-sdk-for-java).</span></span>
