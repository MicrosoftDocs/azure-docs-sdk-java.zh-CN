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
ms.date: 07/02/2018
ms.author: robmcm
ms.openlocfilehash: 3f2de63f7eb1422ff1dd6db45d68e02f4af188b8
ms.sourcegitcommit: 0ed7c5af0152125322ff1d265c179f35028f3c15
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2018
ms.locfileid: "37864037"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a>将根证书添加到 Java CA 证书存储

使用 Azure 服务（例如 Azure 服务总线）的应用程序需要信任 Baltimore CyberTrust 根证书。 此证书可能已安装在系统中，但如果未安装，可遵循本教程中的步骤，使用 Oracle 的 **keytool** 将所需的证书颁发机构 (CA) 根证书添加到要用于 Azure 服务的 Java CA 证书 (cacerts) 存储。

Oracle 的 keytool 实用工具是一个密钥和证书管理工具，可让开发人员管理用于 Java 的受信任证书列表。 可以在压缩 JDK 并将其添加到 Azure 项目的 **approot** 文件夹之前使用 keytool 来添加 CA 证书，也可以运行使用 keytool 的启动任务来添加证书。

Azure 已在 2013 年 4 月 15 日开始从 GTE CyberTrust 全局根证书迁移到 Baltimore CyberTrust 根证书。 以下步骤说明如何使用 keytool 将 Baltimore CyberTrust 根证书添加到 Java CA 证书 (cacerts) 存储。

> [!NOTE]
> 
> 可以使用本文中的步骤配置 Java SDK，以信任来自其他受信任证书颁发机构的根证书。 例如，可以从 [GeoTrust 根证书](http://www.geotrust.com/resources/root-certificates/)上的证书列表中选择一个根证书。
> 

## <a name="determining-which-root-certificates-are-installed"></a>确定安装了哪些根证书

Baltimore 证书可能已安装在你的 cacerts 存储中，因此你需要使用以下步骤来确定是否已安装。

1. 在管理员命令提示符下，导航到 JDK 的 **jdk\jre\lib\security** 文件夹，然后运行以下命令列出系统上已安装的证书：

   ```shell
   keytool -list -keystore cacerts
   ```

1. 如果系统提示输入存储密码，请输入默认密码 **changeit**。

   > [!NOTE]
   > 
   > 如果想要更改存储密码，请参阅 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> 上的 keytool 文档。
   > 

1. 如果未看到包含指纹 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` 的证书，请使用下一部分中的步骤下载并安装该证书。

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a>将根证书添加到 cacerts 存储

1. 从 <https://cacert.omniroot.com/bc2025.crt> 下载 Baltimore CyberTrust 根证书，并将其保存到 **jdk\jre\lib\security** 文件夹中扩展名为 **.cer** 的某个本地文件。 本示例假设下载的 Baltimore CyberTrust 根证书文件为 **bc2025.cer**。

   > [!NOTE]
   > 
   > 该 Baltimore CyberTrust 根证书的序列号为 `02:00:00:b9`，SHA1 指纹为 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`。
   > 

2. 使用以下命令将该证书导入 cacerts 存储：

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```
   其中：

   |  参数   |                              说明                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | 指定证书存储。                                       |
   | `importcert` | 指定要导入证书。                        |
   | `alias`      | 指定证书的别名。                                |
   | `file`       | 指定要导入的根证书的文件名。 |


3. 如果系统提示是否信任该证书，请确认指纹是否为 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`，如果指纹正确，则键入 **y**。

4. 运行以下命令可确保已成功导入 CA 证书：

   ```shell
   keytool -list -keystore cacerts
   ```

成功将根证书添加到 JDK 之后，可以压缩 JDK 的内容，并将其添加到 Azure 项目的 **approot** 文件夹。

## <a name="next-steps"></a>后续步骤

有关 keytool 实用工具的详细信息，请参阅 <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。

有关 Java 的详细信息，请参阅[面向 Java 开发人员的 Azure](/java/azure)。

<!-- For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx). -->
