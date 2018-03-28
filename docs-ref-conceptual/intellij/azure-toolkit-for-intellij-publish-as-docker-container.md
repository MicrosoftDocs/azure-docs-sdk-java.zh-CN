---
title: 使用用于 IntelliJ 的 Azure 工具包发布 Docker 容器
description: 了解如何使用用于 IntelliJ 的 Azure 工具包将 Web 应用作为 Docker 容器发布到 Microsoft Azure。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 64cefc1ace5d0377dea25fdbdc83d8dada31ddf7
ms.sourcegitcommit: ed130145f9e5c2d803791d96bb118023175e644a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="a61a7-103">使用用于 IntelliJ 的 Azure 工具包将 Web 应用发布为 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="a61a7-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="a61a7-104">Docker 容器广泛用于部署 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a61a7-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="a61a7-105">开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。</span><span class="sxs-lookup"><span data-stu-id="a61a7-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="a61a7-106">用于 IntelliJ 的 Azure 工具包可以添加用于部署到 Microsoft Azure 的“发布为 Docker 容器”功能，为 Java 开发人员简化了部署过程。</span><span class="sxs-lookup"><span data-stu-id="a61a7-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="a61a7-107">本文逐步引导你完成将应用程序作为 Docker 容器发布到 Azure 的过程。</span><span class="sxs-lookup"><span data-stu-id="a61a7-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a61a7-108">[Docker 网站]上提供了有关 Docker 的详细信息。</span><span class="sxs-lookup"><span data-stu-id="a61a7-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="a61a7-109">使用 Docker 容器将 Web 应用发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="a61a7-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="a61a7-110">若要发布 Web 应用，必须创建一个随时可用于部署的项目。</span><span class="sxs-lookup"><span data-stu-id="a61a7-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="a61a7-111">有关详细信息，请参阅[有关创建项目的其他信息](#artifacts)部分。</span><span class="sxs-lookup"><span data-stu-id="a61a7-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="a61a7-112">在完成部署向导至少一次后，再次运行向导时，在本演练中指定的大部分设置将用作默认值。</span><span class="sxs-lookup"><span data-stu-id="a61a7-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="a61a7-113">在 IntelliJ 中打开你的 Web 应用项目。</span><span class="sxs-lookup"><span data-stu-id="a61a7-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="a61a7-114">若要启动“发布为 Docker 容器”向导，请执行以下操作之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="a61a7-115">在“项目”工具窗口中右键单击你的项目，然后依次单击“Azure”、“发布为 Docker 容器”：</span><span class="sxs-lookup"><span data-stu-id="a61a7-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![“发布为 Docker 容器”命令][PUB01]

   * <span data-ttu-id="a61a7-117">在 IntelliJ 工具栏中单击“发布组”按钮，然后单击“发布为 Docker 容器”：</span><span class="sxs-lookup"><span data-stu-id="a61a7-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="a61a7-118">![“发布为 Docker 容器”命令][PUB02]</span><span class="sxs-lookup"><span data-stu-id="a61a7-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="a61a7-119">此时将打开“在 Azure 中部署 Docker 容器”向导。</span><span class="sxs-lookup"><span data-stu-id="a61a7-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![“在 Azure 中部署 Docker 容器”向导][PUB03]

3. <span data-ttu-id="a61a7-121">在“键入映像名称，选择项目的路径，并检查要使用的 Docker 主机”窗口中执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="a61a7-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="a61a7-122">a.</span><span class="sxs-lookup"><span data-stu-id="a61a7-122">a.</span></span> <span data-ttu-id="a61a7-123">在“Docker 映像名称”框中输入 Docker 主机的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="a61a7-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="a61a7-124">（向导会自动创建名称，但你可以修改该名称。）</span><span class="sxs-lookup"><span data-stu-id="a61a7-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="a61a7-125">b.</span><span class="sxs-lookup"><span data-stu-id="a61a7-125">b.</span></span> <span data-ttu-id="a61a7-126">“主机”区域将显示已创建的所有 Docker 主机。</span><span class="sxs-lookup"><span data-stu-id="a61a7-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="a61a7-127">执行下列操作之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-127">Do either of the following:</span></span> 
      * <span data-ttu-id="a61a7-128">如果有现有的 Docker 主机，可以在其中部署 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="a61a7-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="a61a7-129">若要创建 Docker 主机，请单击绿色加号 (**+**)。</span><span class="sxs-lookup"><span data-stu-id="a61a7-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="a61a7-130">此时将打开“创建 Docker 主机”对话框。</span><span class="sxs-lookup"><span data-stu-id="a61a7-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![“在 Azure 中部署 Docker 容器”向导][PUB04a]

4. <span data-ttu-id="a61a7-132">在“配置新虚拟机”窗口中提供有关 Docker 主机的以下信息。</span><span class="sxs-lookup"><span data-stu-id="a61a7-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="a61a7-133">（向导会自动生成大多数信息，但你可以修改其中的任何信息。）</span><span class="sxs-lookup"><span data-stu-id="a61a7-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="a61a7-134">a.</span><span class="sxs-lookup"><span data-stu-id="a61a7-134">a.</span></span> <span data-ttu-id="a61a7-135">在“名称”框中输入 Docker 主机的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="a61a7-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="a61a7-136">（这与前面指定的 Docker 映像名称不同。）</span><span class="sxs-lookup"><span data-stu-id="a61a7-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="a61a7-137">b.</span><span class="sxs-lookup"><span data-stu-id="a61a7-137">b.</span></span> <span data-ttu-id="a61a7-138">在“订阅”框中，输入主机要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="a61a7-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="a61a7-139">c.</span><span class="sxs-lookup"><span data-stu-id="a61a7-139">c.</span></span> <span data-ttu-id="a61a7-140">在“区域”框中，输入主机所在的地理区域。</span><span class="sxs-lookup"><span data-stu-id="a61a7-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="a61a7-141">d.单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="a61a7-141">d.</span></span> <span data-ttu-id="a61a7-142">在“OS 和大小”选项卡上执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="a61a7-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="a61a7-143">**主机 OS**：输入包含主机的虚拟机的操作系统。</span><span class="sxs-lookup"><span data-stu-id="a61a7-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="a61a7-144">**大小**：输入主机的虚拟机大小。</span><span class="sxs-lookup"><span data-stu-id="a61a7-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="a61a7-145">e.</span><span class="sxs-lookup"><span data-stu-id="a61a7-145">e.</span></span> <span data-ttu-id="a61a7-146">在“资源组”选项卡上选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="a61a7-147">**新建资源组**：为主机创建资源组。</span><span class="sxs-lookup"><span data-stu-id="a61a7-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="a61a7-148">**现有资源组**：指定 Azure 帐户中的现有资源组。</span><span class="sxs-lookup"><span data-stu-id="a61a7-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="a61a7-149">f.</span><span class="sxs-lookup"><span data-stu-id="a61a7-149">f.</span></span> <span data-ttu-id="a61a7-150">在“网络”选项卡上选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="a61a7-151">**新建虚拟网络**：为主机创建虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="a61a7-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="a61a7-152">**现有虚拟网络**：指定 Azure 帐户中的现有虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="a61a7-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="a61a7-153">g.</span><span class="sxs-lookup"><span data-stu-id="a61a7-153">g.</span></span> <span data-ttu-id="a61a7-154">在“存储”选项卡上选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="a61a7-155">**新建存储帐户**：为主机创建存储帐户。</span><span class="sxs-lookup"><span data-stu-id="a61a7-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="a61a7-156">**现有存储帐户**：指定 Azure 帐户中的现有存储帐户。</span><span class="sxs-lookup"><span data-stu-id="a61a7-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="a61a7-157">单击“资源组名称” 的 Azure 数据工厂。</span><span class="sxs-lookup"><span data-stu-id="a61a7-157">Click **Next**.</span></span>  
     <span data-ttu-id="a61a7-158">此时将打开“配置登录凭据和端口设置”窗口。</span><span class="sxs-lookup"><span data-stu-id="a61a7-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![“配置登录凭据和端口设置”窗口][PUB05]

6. <span data-ttu-id="a61a7-160">选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-160">Select one of the following options:</span></span>

      * <span data-ttu-id="a61a7-161">**从 Azure Key Vault 导入凭据**：指定以前存储在 Azure 订阅中的凭据集。</span><span class="sxs-lookup"><span data-stu-id="a61a7-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="a61a7-162">共享订阅的另一帐户或服务主体不会自动访问使用特定帐户或服务主体创建的 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="a61a7-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="a61a7-163">若要允许另一帐户或服务主体使用 Key Vault，必须使用 Azure 门户添加该帐户或服务主体。</span><span class="sxs-lookup"><span data-stu-id="a61a7-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="a61a7-164">**新建登录凭据**：创建一组新的登录凭据。</span><span class="sxs-lookup"><span data-stu-id="a61a7-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="a61a7-165">如果选择此选项，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="a61a7-165">If you select this option, do the following:</span></span>

    <span data-ttu-id="a61a7-166">a.</span><span class="sxs-lookup"><span data-stu-id="a61a7-166">a.</span></span> <span data-ttu-id="a61a7-167">在“VM 凭据”选项卡上，为 Docker 主机的虚拟机登录凭据提供以下信息：</span><span class="sxs-lookup"><span data-stu-id="a61a7-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host:</span></span>

    * <span data-ttu-id="a61a7-168">**用户名**：输入虚拟机登录凭据的用户名。</span><span class="sxs-lookup"><span data-stu-id="a61a7-168">**Username**: Enter the username for your virtual-machine login credentials.</span></span>

    * <span data-ttu-id="a61a7-169">**密码**和**确认**：输入虚拟机登录凭据的密码。</span><span class="sxs-lookup"><span data-stu-id="a61a7-169">**Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>

    * <span data-ttu-id="a61a7-170">**SSH**：输入 Docker 主机的安全外壳 (SSH) 设置。</span><span class="sxs-lookup"><span data-stu-id="a61a7-170">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="a61a7-171">可选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-171">You can select one of the following options:</span></span>

        * <span data-ttu-id="a61a7-172">**无**：指定虚拟机不允许 SSH 连接。</span><span class="sxs-lookup"><span data-stu-id="a61a7-172">**None**: Specifies that your virtual machine does not allow SSH connections.</span></span>

        * <span data-ttu-id="a61a7-173">**自动生成**：自动创建用于通过 SSH 建立连接的必需设置。</span><span class="sxs-lookup"><span data-stu-id="a61a7-173">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>

        * <span data-ttu-id="a61a7-174">**从目录导入**：指定包含以前已保存的一组 SSH 设置的目录。</span><span class="sxs-lookup"><span data-stu-id="a61a7-174">**Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="a61a7-175">该目录必须包含以下两个文件：</span><span class="sxs-lookup"><span data-stu-id="a61a7-175">The directory must contain the following two files:</span></span>

            * <span data-ttu-id="a61a7-176">*id_rsa*：包含用户的 RSA 标识。</span><span class="sxs-lookup"><span data-stu-id="a61a7-176">*id_rsa*: Contains the RSA identification for a user.</span></span>

            * <span data-ttu-id="a61a7-177">*id_rsa.pub*：包含用于身份验证的 RSA 公钥。</span><span class="sxs-lookup"><span data-stu-id="a61a7-177">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span>

    <span data-ttu-id="a61a7-178">b.</span><span class="sxs-lookup"><span data-stu-id="a61a7-178">b.</span></span> <span data-ttu-id="a61a7-179">在“Docker 守护程序访问”选项卡上提供以下信息：</span><span class="sxs-lookup"><span data-stu-id="a61a7-179">On the **Docker Daemon Access** tab, provide the following information:</span></span>

    ![创建 Docker 主机][PUB06]
    
    * <span data-ttu-id="a61a7-181">**Docker 守护程序端口**：输入 Docker 主机的唯一 TCP 端口。</span><span class="sxs-lookup"><span data-stu-id="a61a7-181">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span>
    
    * <span data-ttu-id="a61a7-182">**TLS 安全性**：输入 Docker 主机的传输层安全性设置。</span><span class="sxs-lookup"><span data-stu-id="a61a7-182">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="a61a7-183">可从以下选项中选择：</span><span class="sxs-lookup"><span data-stu-id="a61a7-183">You can choose from the following options:</span></span>
    
        * <span data-ttu-id="a61a7-184">无：指定虚拟机不允许 TLS 连接。</span><span class="sxs-lookup"><span data-stu-id="a61a7-184">**None**: Specifies that your virtual machine does not allow TLS connections.</span></span>
        
        * <span data-ttu-id="a61a7-185">**自动生成**：自动创建用于通过 TLS 建立连接的必需设置。</span><span class="sxs-lookup"><span data-stu-id="a61a7-185">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span>
        
        * <span data-ttu-id="a61a7-186">**从目录导入**：指定包含以前已保存的一组 TLS 设置的目录。</span><span class="sxs-lookup"><span data-stu-id="a61a7-186">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="a61a7-187">该目录必须包含以下六个文件：</span><span class="sxs-lookup"><span data-stu-id="a61a7-187">The directory must contain the following six files:</span></span>
        
            * <span data-ttu-id="a61a7-188">*ca.pem* 和 *ca key.pem*：包含 TLS 证书颁发机构的证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="a61a7-188">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span>
            
            * <span data-ttu-id="a61a7-189">cert.pem 和 key.pem：包含用于 TLS 身份验证的客户端证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="a61a7-189">*cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.</span></span>
            
            * <span data-ttu-id="a61a7-190">server.pem 和 server-key.pem：包含用于 TLS 身份验证的客户端证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="a61a7-190">*server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span>

7. <span data-ttu-id="a61a7-191">输入所需的信息后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="a61a7-191">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="a61a7-192">此时将再次显示“在 Azure 中部署 Docker 容器”向导。</span><span class="sxs-lookup"><span data-stu-id="a61a7-192">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![“在 Azure 中部署 Docker 容器”向导][PUB07]

8. <span data-ttu-id="a61a7-194">单击“资源组名称” 的 Azure 数据工厂。</span><span class="sxs-lookup"><span data-stu-id="a61a7-194">Click **Next**.</span></span>  
    <span data-ttu-id="a61a7-195">此时将打开“配置要创建的 Docker 容器”窗口。</span><span class="sxs-lookup"><span data-stu-id="a61a7-195">The **Configure the Docker container to be created** window opens.</span></span>

   ![“配置要创建的 Docker 容器”窗口][PUB08]

9. <span data-ttu-id="a61a7-197">在“配置要创建的 Docker 容器”窗口中提供以下信息：</span><span class="sxs-lookup"><span data-stu-id="a61a7-197">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="a61a7-198">a.</span><span class="sxs-lookup"><span data-stu-id="a61a7-198">a.</span></span> <span data-ttu-id="a61a7-199">在“Docker 容器名称”框中，输入 Docker 容器的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="a61a7-199">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="a61a7-200">b.</span><span class="sxs-lookup"><span data-stu-id="a61a7-200">b.</span></span> <span data-ttu-id="a61a7-201">选择以下 Docker 映像之一：</span><span class="sxs-lookup"><span data-stu-id="a61a7-201">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="a61a7-202">**预定义的 Docker 映像**：指定 Azure 中预先存在的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="a61a7-202">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="a61a7-203">此框中的 Docker 映像列表包括 Azure 工具包已配置为要修补的多个映像，以便能够自动部署项目。</span><span class="sxs-lookup"><span data-stu-id="a61a7-203">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="a61a7-204">**自定义 Dockerfile**：指定本地计算机中以前保存的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="a61a7-204">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="a61a7-205">这是一项比较高级的功能，面向想要部署自己的 Dockerfile 的开发人员。</span><span class="sxs-lookup"><span data-stu-id="a61a7-205">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="a61a7-206">但是，使用此选项的开发人员需负责确保正确生成其 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="a61a7-206">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="a61a7-207">由于 Azure 工具包不会验证自定义 Dockerfile 中的内容，因此，如果 Dockerfile 有问题，部署可能会失败。</span><span class="sxs-lookup"><span data-stu-id="a61a7-207">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="a61a7-208">此外，由于 Azure 工具包预期自定义 Dockerfile 中包含 Web 应用项目，因此会尝试打开 HTTP 连接。</span><span class="sxs-lookup"><span data-stu-id="a61a7-208">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="a61a7-209">如果开发人员发布不同类型的项目，在部署后可能会收到无实质影响的错误。</span><span class="sxs-lookup"><span data-stu-id="a61a7-209">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="a61a7-210">c.</span><span class="sxs-lookup"><span data-stu-id="a61a7-210">c.</span></span> <span data-ttu-id="a61a7-211">在“端口设置”框中，输入 Docker 容器的唯一 TCP 端口绑定。</span><span class="sxs-lookup"><span data-stu-id="a61a7-211">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="a61a7-212">完成前面的步骤后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="a61a7-212">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="a61a7-213">Azure 工具包随即开始在 Docker 容器中将你的 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="a61a7-213">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="a61a7-214">除非已将 IntelliJ 配置为在后台部署，否则会出现“正在部署到 Azure”进度条。</span><span class="sxs-lookup"><span data-stu-id="a61a7-214">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![部署进度条][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="a61a7-216">有关创建项目的其他信息</span><span class="sxs-lookup"><span data-stu-id="a61a7-216">Additional information about creating artifacts</span></span>

<span data-ttu-id="a61a7-217">若要创建随时可用于部署的项目，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="a61a7-217">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="a61a7-218">在 IntelliJ 中打开你的 Web 应用项目。</span><span class="sxs-lookup"><span data-stu-id="a61a7-218">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="a61a7-219">依次单击“File”（文件）、“Project Structure”（项目结构）。</span><span class="sxs-lookup"><span data-stu-id="a61a7-219">Click **File**, and then click **Project Structure**.</span></span>

   ![“项目结构”命令][ART01]

3. <span data-ttu-id="a61a7-221">若要添加项目，请单击绿色加号 (**+**)，然后单击“Web 应用程序: 存档”。</span><span class="sxs-lookup"><span data-stu-id="a61a7-221">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![“Web 应用程序: 存档”命令][ART02]

4. <span data-ttu-id="a61a7-223">在“名称”框中输入项目的名称（请不要添加 *.war* 扩展名），然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="a61a7-223">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![项目名称框][ART03]

<span data-ttu-id="a61a7-225">有关在 IntelliJ 中创建项目的详细信息，请参阅 JetBrains 网站上的 [Configuring artifacts]（配置项目）。</span><span class="sxs-lookup"><span data-stu-id="a61a7-225">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a61a7-226">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a61a7-226">Next steps</span></span>

<span data-ttu-id="a61a7-227">有关 Docker 的其他资源，请参阅官方 [Docker 网站]。</span><span class="sxs-lookup"><span data-stu-id="a61a7-227">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Docker 网站]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
