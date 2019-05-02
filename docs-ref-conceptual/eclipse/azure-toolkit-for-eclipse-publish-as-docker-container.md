---
title: 使用用于 Eclipse 的 Azure 工具包发布 Docker 容器
description: 了解如何使用用于 Eclipse 的 Azure 工具包将 Web 应用作为 Docker 容器发布到 Microsoft Azure。
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
ms.openlocfilehash: 4eb159bef52b384de32ada18937b0b9a47a93afc
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61590720"
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="2a4ad-103">使用用于 Eclipse 的 Azure 工具包将 Web 应用发布为 Docker 容器</span><span class="sxs-lookup"><span data-stu-id="2a4ad-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2a4ad-104">Docker 容器广泛用于部署 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="2a4ad-105">开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="2a4ad-106">用于 Eclipse 的 Azure 工具包可以添加用于部署到 Microsoft Azure 的“发布为 Docker 容器”功能，为 Java 开发人员简化了部署过程。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="2a4ad-107">本文逐步引导完成将应用程序作为 Docker 容器发布到 Azure 的过程。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="2a4ad-108">[Docker 网站]上提供了有关 Docker 的详细信息。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="2a4ad-109">使用 Docker 容器将 Web 应用发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="2a4ad-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="2a4ad-110">在 Eclipse 中打开 Web 应用项目。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="2a4ad-111">若要启动“发布为 Docker 容器”向导，请执行以下操作之一：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="2a4ad-112">在“导航器”视图中右键单击项目，并依次单击“Azure”、“发布为 Docker 容器”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![“导航器”视图中的“发布为 Docker 容器”命令][PUB01]

   * <span data-ttu-id="2a4ad-114">在 Eclipse 工具栏中单击“发布”按钮，并单击“发布为 Docker 容器”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Eclipse 工具栏中的“发布为 Docker 容器”命令][PUB02]
      
   <span data-ttu-id="2a4ad-116">此时会打开“在 Azure 中部署 Docker 容器”向导。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![“在 Azure 中部署 Docker 容器”向导][PUB03]

3. <span data-ttu-id="2a4ad-118">在“键入映像名称，选择项目的路径，并检查要使用的 Docker 主机”窗口中执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

   <span data-ttu-id="2a4ad-119">a.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-119">a.</span></span> <span data-ttu-id="2a4ad-120">在“Docker 映像名称”框中输入 Docker 主机的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="2a4ad-121">（向导会自动创建名称，但你可以修改该名称。）</span><span class="sxs-lookup"><span data-stu-id="2a4ad-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

   <span data-ttu-id="2a4ad-122">b.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-122">b.</span></span> <span data-ttu-id="2a4ad-123">“主机”区域将显示已创建的所有 Docker 主机。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="2a4ad-124">执行下列操作之一：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-124">Do either of the following:</span></span>

   * <span data-ttu-id="2a4ad-125">如果有现有的 Docker 主机，可以在其中部署 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
   * <span data-ttu-id="2a4ad-126">若要创建新的 Docker 主机，请单击“添加”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-126">To create a new Docker host, click **Add**.</span></span>  
      
      <span data-ttu-id="2a4ad-127">此时会打开“创建 Docker 主机”对话框。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-127">The **Create Docker Host** dialog box opens.</span></span>

   ![“在 Azure 中部署 Docker 容器”向导][PUB04a]

4. <span data-ttu-id="2a4ad-129">在“配置新虚拟机”窗口中指定 Docker 主机的以下选项。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="2a4ad-130">（向导会自动生成大多数选项，但可以修改其中的任何选项。）</span><span class="sxs-lookup"><span data-stu-id="2a4ad-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="2a4ad-131">a.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-131">a.</span></span> <span data-ttu-id="2a4ad-132">**名称**：输入 Docker 主机的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="2a4ad-133">（这与前面指定的 Docker 映像名称不同。）</span><span class="sxs-lookup"><span data-stu-id="2a4ad-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="2a4ad-134">b.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-134">b.</span></span> <span data-ttu-id="2a4ad-135">**订阅**：输入主机要使用的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="2a4ad-136">c.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-136">c.</span></span> <span data-ttu-id="2a4ad-137">**区域**：输入主机所在的地理区域。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="2a4ad-138">d.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-138">d.</span></span> <span data-ttu-id="2a4ad-139">在“主机 OS 和大小”选项卡上：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-139">On the **Host OS and Size** tab:</span></span> 
   * <span data-ttu-id="2a4ad-140">**主机 OS**：输入包含主机的虚拟机的操作系统。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
   * <span data-ttu-id="2a4ad-141">**大小**：输入主机的虚拟机大小。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="2a4ad-142">e.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-142">e.</span></span> <span data-ttu-id="2a4ad-143">在“资源组”选项卡上：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-143">On the **Resource Group** tab:</span></span> 
   * <span data-ttu-id="2a4ad-144">**新建资源组**：为主机新建资源组。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-144">**New resource group**: Create a new resource group for your host.</span></span>
   * <span data-ttu-id="2a4ad-145">**现有资源组**：输入 Azure 帐户中的现有资源组。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="2a4ad-146">f.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-146">f.</span></span> <span data-ttu-id="2a4ad-147">在“网络”选项卡上：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-147">On the **Network** tab:</span></span> 
   * <span data-ttu-id="2a4ad-148">**新建虚拟网络**：为主机创建新的虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-148">**New virtual network**: Create a new virtual network for your host.</span></span>
   * <span data-ttu-id="2a4ad-149">**现有虚拟网络**：输入 Azure 帐户中的现有虚拟网络。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="2a4ad-150">g.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-150">g.</span></span> <span data-ttu-id="2a4ad-151">在“存储”选项卡上：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-151">On the **Storage** tab:</span></span> 
   * <span data-ttu-id="2a4ad-152">**新建存储帐户**：为主机创建新的存储帐户。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-152">**New storage account**: Create a new storage account for your host.</span></span>
   * <span data-ttu-id="2a4ad-153">**现有存储帐户**：输入 Azure 帐户中的现有存储帐户。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="2a4ad-154">单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-154">Click **Next**.</span></span>

6. <span data-ttu-id="2a4ad-155">在“配置登录凭据和端口设置”窗口中，选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

   * <span data-ttu-id="2a4ad-156">**从 Azure Key Vault 导入凭据**：指定 Azure 订阅中存储的一组以前保存的凭据。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span> 

   >[!NOTE]
   ><span data-ttu-id="2a4ad-157">共享订阅的另一帐户或服务主体不会自动访问使用特定帐户或服务主体创建的 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="2a4ad-158">若要允许另一帐户或服务主体使用 Key Vault，必须使用 Azure 门户添加该帐户或服务主体。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>
   >

   * <span data-ttu-id="2a4ad-159">**新建登录凭据**：创建一组新的登录凭据。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="2a4ad-160">如果选择此选项，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-160">If you select this option, do the following:</span></span> 
    
     * <span data-ttu-id="2a4ad-161">在“VM 凭据”选项卡上，为 Docker 主机的虚拟机登录凭据选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span> 

       * <span data-ttu-id="2a4ad-162">**用户名**：输入虚拟机登录凭据的用户名。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-162">**Username**: Enter the username for your virtual machine login credentials.</span></span> 
       * <span data-ttu-id="2a4ad-163">**密码**和**确认**：输入虚拟机登录凭据的密码。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span> 
       * <span data-ttu-id="2a4ad-164">**SSH**：输入 Docker 主机的安全外壳 (SSH) 设置。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="2a4ad-165">可从以下选项中选择：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-165">You can choose from the following options:</span></span> 
          * <span data-ttu-id="2a4ad-166">**无**：指定你的虚拟机将不允许 SSH 连接。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span> 
          * <span data-ttu-id="2a4ad-167">**自动生成**：自动创建用于通过 SSH 建立连接的必需设置。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span> 
          * <span data-ttu-id="2a4ad-168">**从目录导入**：指定包含以前已保存的一组 SSH 设置的目录。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="2a4ad-169">该目录必须包含以下两个文件：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-169">The directory must contain the following two files:</span></span> 
             * <span data-ttu-id="2a4ad-170">*id_rsa*：包含用户的 RSA 标识。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-170">*id_rsa*: Contains the RSA identification for a user.</span></span> 
             * <span data-ttu-id="2a4ad-171">*id_rsa.pub*：包含用于身份验证的 RSA 公钥。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span> 
        
         ![创建 Docker 主机][PUB05]

     * <span data-ttu-id="2a4ad-173">在“Docker 守护程序凭据”选项卡上指定以下选项：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span> 

       * <span data-ttu-id="2a4ad-174">**Docker 守护程序端口**：输入 Docker 主机的唯一 TCP 端口。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span> 
       * <span data-ttu-id="2a4ad-175">**TLS 安全性**：输入 Docker 主机的传输层安全性设置。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="2a4ad-176">可从以下选项中选择：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-176">You can choose from the following options:</span></span> 
          * <span data-ttu-id="2a4ad-177">**无**：指定你的虚拟机将不允许 TLS 连接。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span> 
          * <span data-ttu-id="2a4ad-178">**自动生成**：自动创建用于通过 TLS 建立连接的必需设置。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span> 
          * <span data-ttu-id="2a4ad-179">**从目录导入**：指定包含以前已保存的一组 TLS 设置的目录。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="2a4ad-180">更具体地说，该目录必须包含以下六个文件：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-180">More specifically, the directory must contain the following six files:</span></span> 
             * <span data-ttu-id="2a4ad-181">*ca.pem* 和 *ca-key.pem*：包含 TLS 证书颁发机构的证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span> 
             * <span data-ttu-id="2a4ad-182">*cert.pem* 和 *key.pem*：包含用于 TLS 身份验证的客户端证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span> 
             * <span data-ttu-id="2a4ad-183">*server.pem* 和 *server-key.pem*：包含主机的服务器证书和公钥。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span> 

         ![创建 Docker 主机][PUB06]

7. <span data-ttu-id="2a4ad-185">输入前面的所有信息后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-185">After you have entered all of the preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="2a4ad-186">在“在 Azure 中部署 Docker 容器”向导中，单击“下一步”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![“在 Azure 中部署 Docker 容器”向导][PUB07]

9. <span data-ttu-id="2a4ad-188">在“配置要创建的 Docker 容器”窗口中执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="2a4ad-189">a.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-189">a.</span></span> <span data-ttu-id="2a4ad-190">在“Docker 容器名称”框中，输入 Docker 容器的唯一名称。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="2a4ad-191">b.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-191">b.</span></span> <span data-ttu-id="2a4ad-192">选择以下 Docker 映像之一：</span><span class="sxs-lookup"><span data-stu-id="2a4ad-192">Choose one of the following Docker images:</span></span> 

   * <span data-ttu-id="2a4ad-193">**预定义的 Docker 映像**：指定 Azure 中预先存在的 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span> 

     >[!NOTE]
     ><span data-ttu-id="2a4ad-194">此框中的 Docker 映像列表包括 Azure 工具包已配置为要修补的多个映像，以便能够自动部署项目。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>
     >

   * <span data-ttu-id="2a4ad-195">**自定义 Dockerfile**：指定本地计算机中以前保存的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

     >[!NOTE]
     ><span data-ttu-id="2a4ad-196">这是一项比较高级的功能，面向想要部署自己的 Dockerfile 的开发人员。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="2a4ad-197">但是，使用此选项的开发人员需负责确保正确生成其 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="2a4ad-198">Azure 工具包不会验证自定义 Dockerfile 中的内容，因此，如果 Dockerfile 有问题，部署可能会失败。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="2a4ad-199">此外，Azure 工具包预期自定义 Dockerfile 中包含 Web 应用项目，因此会尝试打开 HTTP 连接。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="2a4ad-200">如果开发人员发布不同类型的项目，在部署后可能会收到无实质影响的错误。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>
     >

   <span data-ttu-id="2a4ad-201">c.</span><span class="sxs-lookup"><span data-stu-id="2a4ad-201">c.</span></span> <span data-ttu-id="2a4ad-202">**端口设置**：输入 Docker 容器的唯一 TCP 端口绑定。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

      ![“配置要创建的 Docker 容器”窗口][PUB08]

10. <span data-ttu-id="2a4ad-204">完成前面的所有步骤后，单击“完成”。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="2a4ad-205">Azure 工具包随即开始在 Docker 容器中将你的 Web 应用部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2a4ad-206">后续步骤</span><span class="sxs-lookup"><span data-stu-id="2a4ad-206">Next steps</span></span>

<span data-ttu-id="2a4ad-207">有关 Docker 的其他资源，请参阅官方 [Docker 网站]。</span><span class="sxs-lookup"><span data-stu-id="2a4ad-207">For additional resources for Docker, see the official [Docker website].</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Docker 网站]: https://www.docker.com/
[Docker website]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png