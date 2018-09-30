---
title: 使用 Azure DevOps 对 MicroProfile 应用程序进行 CI/CD
description: 了解如何使用 Azure DevOps 设置一个 CI/CD 发布周期，以便将 MicroProfile 应用程序部署到用于容器的 Azure Web 应用实例
services: Azure DevOps
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/14/2018
ms.devlang: Java
ms.service: Azure DevOps
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: c2b6bf3370982d26d8d23fede370e0105a70b734
ms.sourcegitcommit: fd67d4088be2cad01c642b9ecf3f9475d9cb4f3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46506328"
---
# <a name="cicd-for-microprofile-applications-using-azure-devops"></a>使用 Azure DevOps 对 MicroProfile 应用程序进行 CI/CD

本教程介绍 Java EE 开发人员如何使用 Azure DevOps（正式名称为 VSTS）轻松地设置 CI/CD 发布周期，以便将 [MicroProfile](http://microprofile.io) 应用程序部署到用于容器的 Azure Web 应用。  在此示例中，我们将使用一个 MicroProfile 应用程序，该应用程序使用 [Payara Micro](https://www.payara.fish/payara_micro) 作为基础映像。   

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
我们将开始 Azure DevOps 容器化过程：先生成一个 Docker 映像，然后将该容器映像推送到 Azure 容器注册表。  然后，通过 Azure DevOps 发布管道将容器映像部署到 Web 应用。

## <a name="prerequisites"></a>先决条件
- 复制并保存来自 [Github](https://github.com/Azure-Samples/microprofile-hello-azure) 的 Git URL
- 注册或登录 [Azure DevOps](https://dev.azure.com) 帐户
- 创建新的 [Azure DevOps 项目](https://docs.microsoft.com/en-us/vsts/organizations/projects/create-project?view=vsts&tabs=new-nav)，使用上述 Git URL **导入一个存储库**
- 创建 [Azure 容器注册表](https://azure.microsoft.com/en-us/services/container-registry) (ACR)
- 创建用于容器的 Azure Web 应用
> [!NOTE]
>
> 预配 Web 应用实例时，在“容器设置”中选择“快速入门”


## <a name="create-a-build-definition"></a>创建生成定义

每次在 Java EE 应用程序源应用程序中进行提交时，Azure DevOps 中的生成定义就会自动执行生成中的所有任务。  在此示例中，Azure DevOps 将使用 Maven 来生成 Java MicroProfile 项目。

1. 在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。  然后，选择“生成”链接 

<img src="media/VSTS/Buid-and-Release1.png">

2. 单击“新建管道”按钮，然后单击“继续”，开始定义生成任务
3. 从模板列表中选择“Maven”，然后单击“应用”按钮以生成 Java 项目
4. 使用代理池字段对应的下拉菜单选择“托管的 Linux 预览版”选项。
> [!NOTE]
>
> 这会告知 Azure DevOps 要使用的生成服务器。  可以使用专用自定义生成服务器

5. 若要为持续集成配置生成，请选择“触发器”选项卡，然后勾选“启用持续集成”复选框。  

<img src="media/VSTS/Build-Triggers2.png"> 
 
6. 选择“任务”选项卡，返回到生成管道主页
7. 使用“保存和排队”下拉菜单选择“保存”选项
 

## <a name="create-a-docker-build-image"></a>创建 Docker 生成映像

在此任务中，Azure DevOps 将 Dockerfile 与 Payara Micro 提供的基础映像配合使用，以便创建 Docker 映像。  

1. 选择“任务”选项卡，返回到生成管道主页
2. 单击“+”图标，向生成定义添加新任务
 
<img src="media/VSTS/Tasks-add4.png">
 
3. 从模板列表中选择“Docker”，然后单击“添加”按钮
4. 为“显示名称”字段输入说明性名称
5. 在“容器注册表类型”对应的下拉菜单中验证“Azure 容器注册表”是否处于选中状态
> [!NOTE]
>
>  如果使用 Docker 中心或其他注册表，请改选“容器注册表”。  然后单击“+ 新建”按钮，以便为其提供凭据和连接信息。 然后跳到“命令”部分，以便继续操作。

6. 使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。  然后单击“授权”按钮
7. 在“Azure 容器注册表”下拉菜单中，选择在 Azure 中创建的注册表名称。
8. 在“命令”下拉菜单中，验证“生成”选项是否处于选中状态。
9. 对于 **Dockerfile**，请单击文本框旁边的路径导航图标，以便从 github 项目选择 Dockerfile。  然后单击“确定”按钮。

<img src="media/VSTS/Dockerfile5.png">

10. 在“映像名称”下勾选“包括最新标记”复选框。 
11. 使用“保存和排队”下拉菜单选择“保存”选项。

## <a name="push-docker-image-to-acr"></a>将 Docker 映像推送到 ACR

在此任务中，Azure DevOps 会将 Docker 映像推送到 Azure 容器注册表。  可以通过它将 MicroProfile API 应用程序作为容器化 Java Web 应用运行。

1. 由于我们在 Azure DevOps 中使用 Docker，因此请重复上面的步骤 1 - 7 以创建新的 Docker 模板，如“创建 Docker 生成映像”部分所述。
2. 在“命令”下拉菜单中，选择“推送”。
3. 单击“保存和排队”选项卡，然后选择“保存和排队”选项。
4. 对于弹出窗口中的代理池，验证“托管的 Linux 预览版”是否处于选中状态。  然后单击“保存和排队”按钮。
5. 单击生成号，验证 Java 项目的生成管道是否已成功完成。

<img src="media/VSTS/Build-Number6.png">
 

## <a name="create-a-release-definition-for-a-java-app"></a>创建 Java 应用的发布定义

一旦生成过程成功完成，Azure DevOps 中的发布管道就会自动触发部署过程，将生成项目部署到目标环境（例如 Azure）。   可以针对开发、测试、过渡或生产环境创建发布管道。

1. 在 Azure DevOps 项目页顶部单击“生成和发布”选项卡。  然后，选择“发布”链接。

<img src="media/VSTS/Release-new-pipeline7.png">
 
2. 单击“新建管道”按钮**
3. 在模板列表中选择“将 Java 应用部署到 Azure 应用服务”，然后单击“应用”按钮。

<img src="media/VSTS/deploy-java-template8.png">
 
4. 设置“阶段名称”（例如“开发”、“测试”、“过渡”或“生产”）。  然后单击“X”按钮，关闭弹出窗口
5. 单击“项目”部分的“+ 添加”按钮。  这样会将项目从生成定义链接到此发布定义。  
6. 使用“源(生成管道)”对应的下拉菜单选择生成定义。 然后单击“添加”按钮继续。

<img src="media/VSTS/add-artifact9.png">
 
7. 单击管道上的“任务”选项卡。  然后，选择阶段名称。
 
<img src="media/VSTS/release-stage10.png">

8. 使用“Azure 订阅”下拉菜单选择 Azure 订阅 ID。
9. 从“应用类型”下拉菜单中选择“Linux 应用”
10. 在“应用服务名称”下拉菜单中选择此前创建的用于容器的 Web 应用实例的名称
11. 在“注册表或命名空间”字段中输入 Azure 容器注册表的名称。  例如 **myregistry.azure.io**
12. 在“存储库”字段中输入注册表名称
13. 单击“部署 Azure 应用服务”。  在“标记”文本框中输入容器映像的标记 
14. 单击“在代理上运行”。  在代理池下拉菜单中选择“托管的 Linux 预览版” 

## <a name="setup-environment-variables"></a>设置环境变量

1. 单击“变量”选项卡。然后单击“+ 添加”按钮，定义环境变量
2. 为容器注册表 URL、用户名和密码添加变量名称和值。   为安全起见，请单击锁状图标，让密码值处于隐藏状态。

例如：
- registry.password
- registry.url
- registry.username

<img src="media/VSTS/environment-variables12.png">

3. 单击“任务”选项卡，返回到发布管道定义主页
4. 单击“部署 Azure 应用服务”。 
5. 展开“应用程序和配置设置”部分，然后单击“应用设置”字段的导航路径，以便添加在部署期间连接到容器注册表所需的环境变量。
6. 单击“+ 添加”按钮，定义以下应用设置并分配环境变量****
- DOCKER_REGISTRY_SERVER_PASSWORD = $(registry.password)
- DOCKER_REGISTRY_SERVER_URL = $(registry.url)
- DOCKER_REGISTRY_SERVER_USERNAME = $(registry.username)

<img src="media/VSTS/environment-variables14.png">
 
7. 单击“确定”按钮继续

## <a name="setup-continious-deployment--deploy-java-application"></a>设置持续部署和部署 Java 应用程序

1. 若要启用持续部署，请单击“管道”选项卡
2. 在“项目”部分，单击闪电图标。  然后将“持续部署触发器”设置为“启用”。

<img src="media/VSTS/release-enable-CD.png">
 
3. 单击“保存”按钮，然后单击“确定”按钮 
4. 单击“+ 发布”下拉菜单，然后选择“创建发布”链接
5. 使用“触发器从自动改为手动的阶段”下拉菜单，选择阶段名称的复选框
6. 单击“创建”按钮继续
7. 单击发行版号。  然后将鼠标悬停在阶段名称上方，单击“部署”按钮
8. 然后在弹出窗口中单击“部署”按钮，启动部署到 Azure 的部署过程


## <a name="test-the-java-web-application"></a>测试 Java Web 应用程序
1. 在 Web 浏览器中运行 Web 应用 URL：  
https://{your-app-service-name}.azurewebsites.net/api/hello

 
<img src="media/VSTS/web-app16.png">

2. 网页会显示 **Hello Azure!**
 
<img src="media/VSTS/web-api17.png">
