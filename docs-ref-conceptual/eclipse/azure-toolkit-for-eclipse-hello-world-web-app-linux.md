---
title: 使用 Azure Toolkit for Eclipse 部署在云中的 Linux 容器内运行的 Hello World Web 应用
description: 在 Linux 容器中运行一个基本的 Hello World Web 应用，并使用 Azure Toolkit for Eclipse 将它部署到云中。
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: ''
ms.author: robmcm
ms.date: 12/20/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 799f21a282956f9a88aa35743157fc1292197569
ms.sourcegitcommit: 54e7f077d694a5b1dd7fa6c8870b7d476af9829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2019
ms.locfileid: "55649243"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a>使用 Azure Toolkit for Eclipse 将 Hello World Web 应用部署到云中的 Linux 容器

[Docker] 容器广泛用于部署 Web 应用程序。 开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。 Azure Toolkit for Eclipse 可以添加将容器部署到 Microsoft Azure 所需的功能，为 Java 开发人员简化了该过程。

本文演示通过 Azure Toolkit for Eclipse 创建基本的 Hello World Web 应用并将 Linux 容器中的 Web 应用发布到 Azure 所需的步骤。

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]
* [Docker] 客户端。

> [!NOTE]
>
> 若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。 可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。
>
> ![Docker 设置菜单][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>创建新 Web 应用项目

1. 按照 [Azure Toolkit for Eclipse 的登录说明](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions)一文中的步骤启动 Eclipse 并登录到 Azure 帐户。

1. 依次单击“文件”菜单、“新建”和“动态 Web 项目”。
   
   ![创建新项目][file-new-project]

1. 在“新建动态 Web 项目”对话框中指定项目名称和位置，然后单击“完成”。
   
   ![指定项目名称][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>创建 Azure 容器注册表，用作专用 Docker 注册表

以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。

> [!NOTE]
>
> 如果想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。
>

1. 浏览到 [Azure 门户]并登录。

   登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。

1. 依次单击“+ 创建资源”菜单图标、“容器”、“容器注册表”。
   
   ![创建新的 Azure 容器注册表][create-container-registry-01]

1. 当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。

   ![配置 Azure 容器注册表设置][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a>在 Docker 容器中部署 Web 应用

1. 右键单击项目资源管理器中的项目，选择“Azure”，然后单击“添加 Docker 支持”。

   将使用默认配置自动创建 Docker 文件。

   ![添加 Docker 支持][add-docker-support]

1. 添加 Docker 支持后，右键单击项目资源管理器中的项目，选择“Azure”，然后单击“发布到用于容器的 Web 应用”。

   ![发布到用于容器的 Web 应用][run-on-web-app-for-containers]

1. 显示“在用于容器的 Web 应用上运行”对话框时，填写必要信息：

   * **Docker 文件**：指定向项目添加 Docker 支持时创建的 Docker 文件的路径。 

   * **容器注册表**：从下拉菜单中选择在本文的上一部分创建的容器注册表。 “服务器 URL”、“用户名”和“密码”字段会自动填充。

   * **映像和标记**：指定容器映像名称；通常使用以下语法：“*registry*.azurecr.io/*appname*:latest”，其中： 
      * 注册表是上文所述的容器注册表 
      * appname 是 Web 应用的名称 

   * **用于容器的 Web 应用**：选择“使用现有”或“新建”，指定是将容器部署到现有 Web 应用还是创建新的 Web 应用。  指定的“应用名称”将创建 Web 应用的 URL，例如 *wingtiptoys.azurewebsites.net*。

   * **资源组**：指定是要使用现有资源组还是创建新的资源组。 

   * **应用服务计划**：指定是要使用现有应用服务计划还是创建新的应用服务计划。 

   ![在用于容器的 Web 应用上运行][run-on-web-app-linux]

1. 配置完上面列出的设置后，单击“确定”，将 Web 应用发布到 Azure。

1. 发布 Web 应用以后，即可浏览到此前为 Web 应用指定的 URL，例如 *wingtiptoys.azurewebsites.net*。

   ![浏览到 Web 应用][browsing-to-web-app]

## <a name="next-steps"></a>后续步骤

有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Azure 门户]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/azure-toolkit-for-eclipse-hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/file-new-project.png
[project-name]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/azure-toolkit-for-eclipse-hello-world-web-app-linux/run-on-web-app-linux.png
