---
title: "使用用于 IntelliJ 的 Azure 工具包运行 Hello World Web 应用"
description: "了解如何在 Linux 容器中创建基本的 Hello World Web 应用，并使用用于 IntelliJ 的 Azure 工具包将其发布到 Azure。"
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 11/01/2017
ms.author: robmcm
ms.openlocfilehash: 421241b12d8bd9027d4bef8564e1c1ab5a01993a
ms.sourcegitcommit: fc48e038721e6910cb8b1f8951df765d517e504d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2017
---
# <a name="run-a-hello-world-web-app-in-a-linux-container-by-using-the-azure-toolkit-for-intellij"></a>使用用于 IntelliJ 的 Azure 工具包运行 Hello World Web 应用

[Docker] 容器广泛用于部署 Web 应用程序。 开发人员可在其中将其所有项目文件和依赖项整合成单个包，以便部署到服务器。 用于 IntelliJ 的 Azure 工具包可以添加用于将容器部署到 Microsoft Azure 的功能，为 Java 开发人员简化了部署过程。

本文演示如何创建基本的 Hello World Web 应用和使用用于 IntelliJ 的 Azure 工具包将 Linux 容器中的 Web 应用发布到 Azure。

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]
* [Docker] 客户端。

> [!NOTE]
>
> 若要完成本教程中的步骤，需要将 [Docker] 配置为在不具备 TLS 的情况下在端口 2375 上公开守护程序。 可在安装 Docker 时配置此设置，或通过 Docker 设置菜单进行配置。
>
> ![Docker 设置菜单][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>创建新 Web 应用项目

1. 启动 IntelliJ，并按照 [用于 IntelliJ 的 Azure 工具包的登录说明] 一文中的步骤登录 Azure 帐户。

1. 依次单击“文件”菜单、“新建”、“项目”。
   
   ![创建新项目][file-new-project]

1. 在“新建项目”对话框中，选择 Maven，然后选择 maven-archetype-webapp，然后单击“下一步”。
   
   ![选择 Maven archetype webapp][maven-archetype-webapp]
   
1. 为 Web 应用指定 GroupId 和 ArtifactId，然后单击“下一步”。
   
   ![指定 GroupId 和 ArtifactId][groupid-and-artifactid]

1. 自定义任何 Maven 设置或接受默认设置，然后单击“下一步”。
   
   ![指定 Maven 设置][maven-options]

1. 指定项目名称和位置，并单击“完成”。
   
   ![指定项目名称][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>创建 Azure 容器注册表，用作专用 Docker 注册表

以下步骤将引导你完成使用 Azure 门户来创建 Azure 容器注册表的步骤。

> [!NOTE]
>
> 如果想要使用 Azure CLI（而不是 Azure 门户），请按照[使用 Azure CLI 2.0 创建专用 Docker 容器注册表][Create Docker Registry using Azure CLI]中的步骤操作。
>

1. 浏览到 [Azure 门户]并登录。

   登录到你在 Azure 门户的帐户后，可以按照[使用 Azure 门户创建专用 Docker 容器注册表]一文中的步骤操作，为方便起见，在以下步骤中进行了解释。

1. 单击“+ 新建”菜单图标，然后依次单击“容器”、“Azure 容器注册表”。
   
   ![创建新的 Azure 容器注册表][AR01]

1. 显示 Azure 容器注册表模板的信息页面时，请单击“创建”。 

   ![创建新的 Azure 容器注册表][AR02]

1. 当显示“创建容器注册表”页时，输入你的“注册表名称”和“资源组”，为“管理员用户”选择“启用”，然后单击“创建”。

   ![配置 Azure 容器注册表设置][AR03]

1. 创建容器注册表后，在 Azure 门户中导航到你的容器注册表，然后单击“访问密钥”。 记下用户名和密码，以供后续步骤中使用。

   ![Azure 容器注册表访问密钥][AR04]

## <a name="deploy-your-web-app-in-a-docker-container"></a>在 Docker 容器中部署 Web 应用

1. 右键单击项目资源管理器中的项目，选择“Azure”，然后单击“添加 Docker 支持”。

   将使用默认配置自动创建 Docker 文件。

   ![添加 Docker 支持][add-docker-support]

1. 添加 Docker 支持后，右键单击项目资源管理器中的项目，选择“Azure”，然后单击“在 Web 应用 (Linux) 上运行”。

   ![在 Web 应用 (Linux) 上运行][run-on-web-app-linux]

1. 出现“在 Web 应用 (Linux) 上运行”对话框时，填写必要信息：

   * 名称：用于指定显示在 Azure Toolkit 中的友好名称。 

   * 服务器 URL：用于指定上文所述的容器注册表 URL，通常使用以下语法：“registry.azurecr.io”。 

   * 用户名和密码：用于指定上文所述的容器注册表 URL 的访问密钥。 

   * 映像和标记：用于指定容器映像名称；通常使用以下语法：“registry.azurecr.io/appname:latest”，其中： 
      * 注册表是上文所述的容器注册表 
      * appname 是 Web 应用的名称 

   * “使用现有的 Web 应用”或“创建新的 Web 应用”：用于指定是将容器部署到现有 Web 应用还是创建新的 Web 应用。 

   * 资源组：指定是要使用现有资源组还是创建新的资源组。 

   * 应用服务计划：指定是要使用现有应用服务计划还是创建新的应用服务计划。 

1. 配置完上面列出的设置后，单击“运行”。

   ![创建 Web 应用][create-web-app]

1. 发布 Web 应用后，所做设置会保存为默认设置，可单击工具栏上的绿色箭头图标在 Azure 上运行应用程序。 可通过单击 Web 应用的下拉菜单来修改这些设置，然后单击“编辑配置”。

   ![“编辑配置”菜单][edit-configuration-menu]

1. 出现“运行/调试配置”对话框后，可修改任意默认设置，然后单击“确定”。

   ![“编辑配置”对话框][edit-configuration-dialog]

## <a name="next-steps"></a>后续步骤

有关 Docker 的其他资源，请参阅官方 [Docker 网站][Docker]。

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

[Azure 门户]: https://portal.azure.com/
[使用 Azure 门户创建专用 Docker 容器注册表]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[AR01]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR01.png
[AR02]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR02.png
[AR03]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR03.png
[AR04]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/AR04.png

[docker-settings-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/file-new-project.png
[maven-archetype-webapp]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-archetype-webapp.png
[groupid-and-artifactid]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/groupid-and-artifactid.png
[maven-options]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/maven-options.png
[project-name]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/project-name.png
[add-docker-support]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/add-docker-support.png
[run-on-web-app-linux]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/run-on-web-app-linux.png
[create-web-app]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/create-web-app.png
[edit-configuration-menu]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-menu.png
[edit-configuration-dialog]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/edit-configuration-dialog.png
[successfully-deployed]: media/azure-toolkit-for-intellij-hello-world-web-app-linux/successfully-deployed.png
