---
title: 将 Azure 的根证书添加到 Java CA 存储
description: 了解如何将证书颁发机构 (CA) 根证书添加到用于 Microsoft Azure 的 Java CA 证书 (cacerts) 存储。
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 05/23/2018
ms.author: robmcm
ms.openlocfilehash: 29b2b598968c9a3a896fffee3ce56f9b0cb4b1ee
ms.sourcegitcommit: 5282a51bf31771671df01af5814df1d2b8e4620c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090730"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a><span data-ttu-id="6530f-103">将根证书添加到 Java CA 证书存储</span><span class="sxs-lookup"><span data-stu-id="6530f-103">Adding a root certificate to the Java CA certificates store</span></span>

<span data-ttu-id="6530f-104">使用 Azure 服务（例如 Azure 服务总线）的应用程序需要信任 Baltimore CyberTrust 根证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-104">Applications that use Azure services (such as Azure Service Bus) need to trust the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="6530f-105">此证书可能已安装在系统中，但如果未安装，可遵循本教程中的步骤，使用 Oracle 的 **keytool** 将所需的证书颁发机构 (CA) 根证书添加到要用于 Azure 服务的 Java CA 证书 (cacerts) 存储。</span><span class="sxs-lookup"><span data-stu-id="6530f-105">This certificate may already be installed on your system, but if it is not, the steps in this tutorial will show you how to use Oracle's **keytool** to add the required certificate authority (CA) root certificate to the Java CA certificate (cacerts) store that you will use for Azure services.</span></span>

<span data-ttu-id="6530f-106">Oracle 的 keytool 实用工具是一个密钥和证书管理工具，可让开发人员管理用于 Java 的受信任证书列表。</span><span class="sxs-lookup"><span data-stu-id="6530f-106">Oracle's keytool utility is a _Key and Certificate Management Tool_, which allows developers to manage the list of trusted certificates for use with Java.</span></span> <span data-ttu-id="6530f-107">可以在压缩 JDK 并将其添加到 Azure 项目的 **approot** 文件夹之前使用 keytool 来添加 CA 证书，也可以运行使用 keytool 的启动任务来添加证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-107">You can use keytool to add the CA certificate before zipping your JDK and adding it to your Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool to add the certificate.</span></span>

<span data-ttu-id="6530f-108">Azure 已在 2013 年 4 月 15 日开始从 GTE CyberTrust 全局根证书迁移到 Baltimore CyberTrust 根证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-108">Beginning April 15, 2013, Azure began migrating from the GTE CyberTrust Global root certificate to the Baltimore CyberTrust root certificate.</span></span> <span data-ttu-id="6530f-109">以下步骤说明如何使用 keytool 将 Baltimore CyberTrust 根证书添加到 Java CA 证书 (cacerts) 存储。</span><span class="sxs-lookup"><span data-stu-id="6530f-109">The following steps show you how to use keytool to add the Baltimore CyberTrust root certificate to your Java CA certificate (cacerts) store.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="6530f-110">可以使用本文中的步骤配置 Java SDK，以信任来自其他受信任证书颁发机构的根证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-110">You can use the steps in this article to configure your Java SDK to trust the root certificates from other trusted certificate authorities.</span></span> <span data-ttu-id="6530f-111">例如，可以从 [GeoTrust 根证书](http://www.geotrust.com/resources/root-certificates/)上的证书列表中选择一个根证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-111">For example, you might choose a root certificate from the list of certificates at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span>
> 

## <a name="determining-which-root-certificates-are-installed"></a><span data-ttu-id="6530f-112">确定安装了哪些根证书</span><span class="sxs-lookup"><span data-stu-id="6530f-112">Determining which root certificates are installed</span></span>

<span data-ttu-id="6530f-113">Baltimore 证书可能已安装在你的 cacerts 存储中，因此你需要使用以下步骤来确定是否已安装。</span><span class="sxs-lookup"><span data-stu-id="6530f-113">The Baltimore certificate might already be installed in your cacerts store, so you need to use the following steps to determine if it has already been installed.</span></span>

1. <span data-ttu-id="6530f-114">在管理员命令提示符下，导航到 JDK 的 **jdk\jre\lib\security** 文件夹，然后运行以下命令列出系统上已安装的证书：</span><span class="sxs-lookup"><span data-stu-id="6530f-114">At an administrator command prompt, navigate to your JDK's **jdk\jre\lib\security** folder, and then run the following command to list the certificates that are installed on your system:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

1. <span data-ttu-id="6530f-115">如果系统提示输入存储密码，请输入默认密码 **changeit**。</span><span class="sxs-lookup"><span data-stu-id="6530f-115">If you are prompted for the store password, the default password is **changeit**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="6530f-116">如果想要更改存储密码，请参阅 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> 上的 keytool 文档。</span><span class="sxs-lookup"><span data-stu-id="6530f-116">If you want to change the store password, see the keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>
   > 

1. <span data-ttu-id="6530f-117">如果未看到包含指纹 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 的证书，请使用下一部分中的步骤下载并安装该证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-117">If you do not see the certificate with the thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, use the steps in the following section to download and install the certificate.</span></span>

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a><span data-ttu-id="6530f-118">将根证书添加到 cacerts 存储</span><span class="sxs-lookup"><span data-stu-id="6530f-118">To add a root certificate to the cacerts store</span></span>

1. <span data-ttu-id="6530f-119">从 <https://cacert.omniroot.com/bc2025.crt> 下载 Baltimore CyberTrust 根证书，并将其保存到 **jdk\jre\lib\security** 文件夹中扩展名为 **.cer** 的某个本地文件。</span><span class="sxs-lookup"><span data-stu-id="6530f-119">Download the Baltimore CyberTrust root certificate from <https://cacert.omniroot.com/bc2025.crt>, and save to a local file with extension **.cer** in your **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="6530f-120">本示例假设下载的 Baltimore CyberTrust 根证书文件为 **bc2025.cer**。</span><span class="sxs-lookup"><span data-stu-id="6530f-120">For this example, assume that you downloaded the Baltimore CyberTrust root certificate file as **bc2025.cer**.</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="6530f-121">该 Baltimore CyberTrust 根证书的序列号为 `02:00:00:b9`，SHA1 指纹为 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`。</span><span class="sxs-lookup"><span data-stu-id="6530f-121">The Baltimore CyberTrust root certificate has a serial number of `02:00:00:b9`, and a SHA1 thumbprint of `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`.</span></span>
   > 

2. <span data-ttu-id="6530f-122">使用以下命令将该证书导入 cacerts 存储：</span><span class="sxs-lookup"><span data-stu-id="6530f-122">Import the certificate to the cacerts store by using the following command:</span></span>

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   <span data-ttu-id="6530f-123">其中：</span><span class="sxs-lookup"><span data-stu-id="6530f-123">Where:</span></span>

   |  <span data-ttu-id="6530f-124">参数</span><span class="sxs-lookup"><span data-stu-id="6530f-124">Parameter</span></span>   |                              <span data-ttu-id="6530f-125">说明</span><span class="sxs-lookup"><span data-stu-id="6530f-125">Description</span></span>                               |
   |--------------|------------------------------------------------------------------------|
   |  `keystore`  |                    <span data-ttu-id="6530f-126">指定证书存储。</span><span class="sxs-lookup"><span data-stu-id="6530f-126">Specifies the certificate store.</span></span>                    |
   | `importcert` |            <span data-ttu-id="6530f-127">指定要导入证书。</span><span class="sxs-lookup"><span data-stu-id="6530f-127">Specifies that you are importing a certificate.</span></span>             |
   |   `alias`    |                <span data-ttu-id="6530f-128">指定证书的别名。</span><span class="sxs-lookup"><span data-stu-id="6530f-128">Specifies an alias for the certificate.</span></span>                 |
   |    `file`    | <span data-ttu-id="6530f-129">指定要导入的根证书的文件名。</span><span class="sxs-lookup"><span data-stu-id="6530f-129">Specifies the filename of the root certificate that you are importing.</span></span> |


3. <span data-ttu-id="6530f-130">如果系统提示是否信任该证书，请确认指纹是否为 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`，如果指纹正确，则键入 **y**。</span><span class="sxs-lookup"><span data-stu-id="6530f-130">If you are prompted to trust the certificate, verify the thumbprint as `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`, and type **y** if the thumbprint is correct.</span></span>

4. <span data-ttu-id="6530f-131">运行以下命令可确保已成功导入 CA 证书：</span><span class="sxs-lookup"><span data-stu-id="6530f-131">Run the following command to ensure the CA certificate has been successfully imported:</span></span>

   ```shell
   keytool -list -keystore cacerts
   ```

<span data-ttu-id="6530f-132">成功将根证书添加到 JDK 之后，可以压缩 JDK 的内容，并将其添加到 Azure 项目的 **approot** 文件夹。</span><span class="sxs-lookup"><span data-stu-id="6530f-132">After you have successfully added the root certificate to your JDK, you can zip the contents of JDK and add it to your Azure project's **approot** folder.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6530f-133">后续步骤</span><span class="sxs-lookup"><span data-stu-id="6530f-133">Next steps</span></span>

<span data-ttu-id="6530f-134">有关 keytool 实用工具的详细信息，请参阅 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。</span><span class="sxs-lookup"><span data-stu-id="6530f-134">For more information about the keytool utility, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

<span data-ttu-id="6530f-135">有关 Azure 使用的根证书的详细信息，请参阅 [Azure 根证书迁移](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6530f-135">For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span></span>

<span data-ttu-id="6530f-136">有关 Java 的详细信息，请参阅[面向 Java 开发人员的 Azure](/java/azure)。</span><span class="sxs-lookup"><span data-stu-id="6530f-136">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>
