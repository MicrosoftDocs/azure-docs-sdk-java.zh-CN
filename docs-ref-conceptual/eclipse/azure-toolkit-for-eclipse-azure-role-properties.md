---
title: "Azure 角色属性"
description: "了解如何使用 Azure Toolkit for Eclipse 来配置 Azure 角色设置。"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5c0ec412-5702-465a-8f47-87a8ce99a267
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: cd734c64ba6d1394cb261bace92dee9dd579dd08
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-properties"></a>Azure 角色属性
可以在 Azure Toolkit for Eclipse 内设置你的 Azure 角色的各种配置设置。

## <a name="configuring-azure-role-properties"></a>配置 Azure 角色属性
配置你的 Azure 角色属性被通过辅助角色的属性对话框。 在 Eclipse 的项目资源管理器窗格中打开角色的上下文菜单并选择**Azure**子菜单。 （如果看不到 Eclipse 项目资源管理器中的角色，展开 Azure 项目在项目资源管理器。）

![][ic789599]

可以设置从的各种属性**属性**对话框本主题中所述。 请注意，许多属性自动填充创建新的 Azure 部署项目时。

下面的属性页才可用于 Azure 角色。

* [虚拟机属性](#virtual_machine_properties)
* [Caching 属性](#caching_properties)
* [证书属性](#certificates_properties)
* [组件属性](#components_properties)
<!-- * [Debugging properties](#debugging_properties) -->
* [终结点属性](#endpoints_properties)
* [环境变量属性](#environment_variables_properties)
* [负载平衡 / 会话相关性 （即"粘滞会话"） 属性](#session_affinity_properties)
* [本地存储属性](#local_storage_properties)
* [服务器配置属性](#server_configuration_properties)
* [SSL 卸载属性](#ssl_offloading_properties)

<a name="virtual_machine_properties"></a>

### <a name="virtual-machine-properties"></a>虚拟机属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**属性**，并且你将能够更改虚拟机大小，并还更改实例数，如下图中所示。

![][ic719499]

> [!NOTE]
> 仅限 Windows： 时将实例数设置为一个值大于 1，并且还配置应用程序服务器，该工具包将只允许 1 角色实例以在仿真程序中，而不考虑此设置中运行。 这是为了避免不同服务器实例之间的端口绑定冲突 （例如，尝试将绑定到端口 8080 全部） 在同一台计算机上运行时。 你所需的实例计数设置已保留，但仅当你部署到云进入效果。
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Caching 属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**Caching**。 在此对话框中，你可以启用命名并置的 memcache 兼容缓存，从而可以帮助加快 web 应用程序。

![][ic719483]

在**Caching**属性页中，你可以指定以下全局设置：

* 是否启用共存缓存。
* 缓存大小的内存百分比。
* 如果你不想要保存缓存状态，你的应用程序作为云服务，或无运行时保存缓存状态的存储帐户名称。 （存储帐户名称是时不使用在计算模拟器中运行你的应用程序。）如果存储帐户名称设置为**（自动）** （这是默认值），则缓存配置将自动使用相同的存储帐户中选择一个**发布到 Azure**对话框。

> [!NOTE]
> **（自动）**设置才能产生所需的效果仅当你发布你的部署使用 Eclipse 工具包发布向导。 如果你改为发布.cspkg 文件手动使用外部机制，如[Azure 管理门户][Azure Management Portal]，部署将无法正常工作。
> 
> 

以下对话框显示缓存的属性。

![][ic719501]

* **名称：**共存缓存的名称。
* **端口号：**要供缓存使用的端口号。
* **过期策略：**指定缓存中的键的到期时的以下值之一。
  * **绝对：**密钥过期时指定的时间**生存分钟数**为止。
  * **NeverExpires:**密钥没有过期时间。
  * **SlidingWindow:**的密钥将过期，如果指定的时间内未访问，就会对它进行**生存分钟数**; 每次访问时，重置到期时钟。
* **生存分钟数：** memcache 密钥生存，受过期策略约束的分钟的最大数量。
* **具有不同角色实例上的复制备份的高可用性：**如果启用，有助于提供高可用性利用复制不同角色实例上的备份。 请注意，在至少两个角色实例必须实际上为你要运行此功能的部署。

若要添加新的缓存，请单击**添加**按钮**Caching**属性页中，和一个**配置命名缓存**对话框将打开。 为上面所述的属性提供值。

若要修改命名的缓存，请选择该缓存，并单击**编辑**按钮**Caching**属性页。 对话框将打开以允许你修改缓存属性。 按**确定**以保存缓存值。

若要删除某个缓存，请选择该缓存，然后单击**删除**按钮**Caching**属性页，然后单击**是**以确认删除。

有关如何使用缓存的详细信息，请参阅[如何使用并置缓存][How to Use Co-located Caching]。

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>证书属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**证书**。

![][ic710964]

在此对话框中，你可以添加或删除由 Eclipse 项目引用的证书。 请注意，此处列出的证书不会自动存储在任何 Java 密钥库内，因此不会自动可供从 Java 应用程序中的任何使用。 它们仅注册到 Azure，以便它们可以预先加载到 Windows 证书存储上运行你的部署的虚拟机，随后供其他 Windows 软件。 目前，唯一的功能使用的证书的工具包引用在这种方式**证书**对话框是[SSL 卸载][SSL Offloading]，因为其需依赖于 Internet 信息服务 (IIS) 和应用程序请求路由 (ARR)，需要正确的证书，以使其可在这种方式。

将项目部署到 Azure 中使用发布向导时，系统将提示你为指向对应于这些证书，并提供其密码，以便自动将其上载到 Azure 的服务，但仅当它们尚未上载存在以前的个人信息交换 (PFX) 文件。

<a name="components_properties"></a> 

### <a name="components-properties"></a>组件属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**组件**。 在此对话框中，你可以添加、 修改或删除您的角色，这些组件，以及更改它们的处理顺序。

![][ic719502]

组件功能，可将依赖项添加到你的 Azure 部署项目，如 Java 应用程序项目、 特殊文件和你的部署所需的可执行的命令行语句。

对于每个组件，您可以指定：

* 在生成时，将将组件导入你的 Azure 部署项目时要执行的步骤。
* 在部署 Azure 云中的该组件时要执行的步骤。

> [!NOTE]
> 当指定组件文件或命令行，请记住，你的部署将发布到 Windows 虚拟机时，因此你自定义步骤必须对于基于 Windows 的操作系统有效。 
> 
> 

组件具有以下属性：

* **导入：**指示如何将组件导入项目生成项目时的方法。 这可以是下列值之一：
  * **复制：**从指定的本地路径复制组件**从**到角色的属性**approot**目录。
  * **EAR:**组件是 Java 企业存档文件 (EAR) 从在指定的本地路径企业应用程序项目导入**从**属性。 （这是自动检测到通过工具包根据该位置处的项目的性质）。
  * **JAR:**组件是 Java 存档 (JAR)，从指定的本地路径上的 Java 项目导入**从**属性。 （这是自动检测到通过工具包根据该位置处的项目的性质）。
  * **none:**不执行任何操作以导入组件。 这是适用假定组件已必须存在于角色的**approot**目录中，或组件时只是一个可执行的命令行语句，规定**作为**属性时**部署**方法是**exec**。
  * **WAR:** Java web 应用程序存档 (WAR) 并从指定的本地路径上的动态 Web 项目中导入了组件**从**属性。 （这是自动检测到通过工具包根据该位置处的项目的性质）。
  * **zip:**的组件是一个 zip 文件，并且通过压缩大小能目录或由指定的文件导入**从**属性。
* **从：**上你本地计算机添加到的文件夹或文件，它表示要在导入到你的部署的项的源路径。 此属性中，可以使用 Windows 环境变量。 所有可导入组件将导入到角色的**approot**生成项目时的目录。
  
    请注意，必须部署到云 （而非计算模拟器） 时部署下载的组件的功能。 请参阅下面有关添加组件的相关的信息。    
* **为：**文件名在其下组件将导入到角色的**approot**目录并最终部署在 Azure 云中。 将此属性留空以保留的名称相同因为它是本地计算机上。 (对于可执行文件的组件，即，那些其**部署**方法设置为**exec**，这可以是任意的 Windows 命令行语句。)
  
  > [!IMPORTANT]
  > 如果此值使用空格字符，它们将处理方式不同根据部署方法。 如果部署方法**exec**，空格将解释为命令行参数分隔符而不是文件名称的一部分。 对于所有其他部署方法，空格将解释为文件名称的一部分。
  > 
  > 
* **部署：**指示在启动部署时应用到该组件的操作的方法。 这可以是下列值之一：
  
  * **复制：**组件将复制到指定的目标路径**到**属性。
  * **exec:**组件是一个可执行指定的路径上下文中执行的 Windows 命令行语句**到**属性，在部署开始的时间。
  * **none:**启动部署时，任何操作应用于该组件。
  * **zip:**组件将解压缩到指定的目标路径**到**属性。 此方法是时才可用**导入**属性是**zip**。
* **目标机构：**组件都部署在虚拟机上的目标路径。 在此属性，可以使用 Windows 环境变量和文件路径是相对于**approot**。

若要添加新组件，请单击**添加**按钮**组件**属性页中，和**Azure 角色组件**对话框将打开。 为上面所述的属性提供值。 

下面显示一个示例，用于添加新的 WAR 组件。

![][ic719503]

当你想要部署一次下载，从组件的情况下部署到云 （而不计算模拟器），确保**时在云中，而不是包中包括部署从**已选中。 如果你想要从你的 Azure 存储帐户下载，选择从存储帐户**存储帐户**下拉列表 (你可以单击**帐户**链接可以修改列表中)，这将部分填充**URL**字段，然后再填写 URL 的剩余部分。 如果不希望使用 Azure 存储空间，选择**（无）**从**存储帐户**下拉列表，并将 URL 输入到你的组件在**URL**字段。 指定以下方法之一：

* **复制：**下载组件将复制到指定的目标路径**To Directory**路径。
* **相同：**用于的相同方法**通过下载部署**同样适用于**从程序包部署**。
* **zip:**下载组件将解压缩到指定的目标路径**To Directory**路径。

若要修改某个组件，选择的组件，然后单击**编辑**按钮**组件**属性页。 允许你修改组件属性，将打开一个对话框。 按**确定**以保存组件值。

若要删除某一组件，请选择该组件，并单击**删除**按钮**组件**属性页，然后单击**是**以确认删除。

按列出的顺序处理组件。 使用**移**和**下移**按钮可排列顺序。

> [!NOTE]
> 服务器配置功能依赖于以及组件。 无法删除或编辑而不删除相应的服务器配置这些组件。 您将有关该尝试对此类组件进行更改时提示。
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open the context menu for the role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have the ability to enable or disable remote debugging, as well as create debug configurations, as shown in the following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>终结点属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**终结点**。 在此对话框中，你可以创建终结点，以及编辑或删除终结点，如下图中所示。

![][ic719505]

若要添加一个终结点，请单击**添加**按钮**终结点**属性页中，和**添加终结点**对话框将打开。

![][ic710897]

输入终结点的名称，选择的类型 (任一**输入**，**内部**，或**InstanceInput**)，并指定公用和专用端口。 按**确定**以保存新的终结点值。

具体取决于终结点的类型，你可以使用端口范围，如下所示：

* 对于实例输入终结点，公用端口可以是端口范围 (例如**2000年-2010年**) 和专用端口是固定的值。
* 内部终结点，未使用的公共端口，和专用端口可以是端口范围，或留的空，或者设置为星号以指示它由 Azure 自动设置。
* 对于输入终结点，公用端口只能是固定的值，和专用端口可以是固定的值或留的空或设置为星号以指示由 Azure 自动设置。

如果你想要使用单个端口号，而不是一个范围，将文本框末尾的范围为空。

对于端口设置为自动进行的如果你需要确定在运行时实际使用哪个端口，你的应用程序可以使用 Azure 服务运行时 API 的说明进行操作，这记录在[com.microsoft.windowsazure.serviceruntime 包摘要][com.microsoft.windowsazure.serviceruntime package summary]。

<!-- To see how instance input endpoints can be used to help with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

若要修改某个终结点，选择终结点，然后单击**编辑**按钮**终结点**属性页。 允许你修改终结点名称、 类型和公用和专用端口，将打开一个对话框。 按**确定**以保存修改的终结点值。

若要删除终结点，选择终结点，然后单击**删除**按钮**终结点**属性页，然后单击**是**以确认删除。

为了正确配置某些功能 （如缓存、 会话相关性或 SSL 卸载） 由用户对角色启用，该工具包可能会自动配置将与用户定义的终结点一起列出的特殊终结点。 该工具包就会禁止用户编辑或删除此类自动生成的终结点，只要启用了关联的功能。

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>环境变量属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**环境变量**。 在此对话框中，你可以创建环境变量，以及修改或删除环境变量，如下图中所示。

![][ic719506]

在角色启动时，环境变量都可用于启动脚本。

> [!NOTE]
> 指定环境变量，请记住，你的部署将发布到 Windows 虚拟机，因此你的环境变量必须是有效的对于基于 Windows 的操作系统。
> 
> 

作为环境变量在角色启动时处于可用的示例，通过单击创建新的环境变量**添加**按钮。 下面的示例演示名为的环境变量**MyRoleVersion**正在创建和分配的值**1.0**。

![][ic659268]

在 jsp 代码中，无法显示值使用`System.getenv`方法：

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

你的应用程序运行时生成此输出：

![][ic552233]

若要修改环境变量，请选择该环境变量，然后单击**编辑**按钮**环境变量**属性页。 对话框将打开以允许你修改环境变量属性。 按**确定**保存环境变量值。

若要删除的环境变量，请选择该环境变量，然后单击**删除**按钮**环境变量**属性页，然后单击**是**以确认删除。

为了正确配置的一些功能 （如服务器配置、 远程调试或本地存储） 启用的用户对角色，该工具包可能会自动配置将与用户定义的环境变量一起列出的特殊环境变量。 该工具包就会禁止用户编辑或删除此类自动生成的环境变量，只要启用了关联的功能。

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>负载平衡 / 会话相关性 （即"粘滞会话"） 属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**负载平衡**。 在此对话框中，你可以启用或禁用会话相关性，如下图中所示。

![][ic719492]

有关相关信息，请参阅[会话相关性][Session Affinity]。 另请注意此功能的行为中的 SSL 卸载，上下文中所述[SSL 卸载][SSL Offloading]。

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>本地存储属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**本地存储**。 在此对话框中，你可以创建、 修改或删除虚拟机运行你的应用程序的临时本地存储区。 可以为的本地存储的大小以及角色回收时下, 图中所示是否保留内容设置特定值。

![][ic719508]

你还可以选择指定对应于本地存储的环境变量。

默认情况下，你将部署到 Azure 的所有内容都放置 （并且解压缩） 在**approot**的角色实例的文件夹。 虽然大多数简单部署在解压缩后都适合那里，为分配空间**approot**目录都是有限，未明确定义 （小于 1 GB 是合理的经验法则）。 因此，为了确保 Azure 分配足够的磁盘空间可能不适合的较大部署**approot**文件夹，则应该将设置本地存储资源使用**本地存储**对话框。 轻松执行此操作，请参阅[部署大型部署][Deploying Large Deployments]。

您可以轻松地引用从启动脚本的存储资源 (例如，你**startup.cmd**) 中所示使用环境变量通过 Eclipse 工具包自动关联资源，**本地存储**对话框。 该环境变量将包含每次执行启动脚本时已配置的本地资源的完整路径。 

若要修改本地存储资源，请选择该本地存储资源，然后单击**编辑**按钮**本地存储**属性页。 对话框将打开以允许你修改本地存储资源属性。 按**确定**以保存本地存储资源值。

若要删除本地存储资源，请选择该本地存储资源，然后单击**删除**按钮**本地存储**属性页，然后单击**是**以确认删除。

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>服务器配置属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**服务器配置**。 在此对话框中，你可以添加、 删除和修改你的部署，使用的 JDK 和 Java 应用程序服务器，以及添加或删除你的部署使用的应用程序 （例如 WAR、 JAR 或 EAR 文件）。

### <a name="jdk-configuration"></a>JDK 配置
此对话框允许你指定要用于部署的 JDK 包。 如果在 Windows 上使用 Eclipse，可以指定要使用本地 Azure 模拟器中运行时的 JDK 包，并可以选择将该本地安装部署到 Azure。 在非 Windows 操作系统，模拟器 JDK 设置不适用，无法部署本地安装的 JDK，因为它不与 Windows 兼容。 但是，无论你使用的操作系统，你可以始终选择第三方 JDK 包部署到 Azure，或从备用下载位置指向你自己的 Windows 兼容 JDK 包。

下面是如何在 Windows 上指定 JDK 的一个示例：

![][ic780647]

如果在 Windows 上使用 Eclipse，则可以指定要使用计算仿真程序; 使用 JDK为此，请确保**使用此文件路径中的 JDK 进行本地测试**签入**模拟器部署**部分。 然后，指定 JDK; 的本地路径你可以根据需要浏览到不同 JDK 未自动选择你想要使用的一个。 你还可以选择将 JDK 部署到 Azure 云服务;为此，请选择**部署我的本地 JDK （自动上载到云存储）**选项**云部署**部分。

注意： 在非 Windows 操作系统，**模拟器部署**设置和**部署我的本地 JDK**选项将不可用。 下面的示例阐释了在 Mac 或其他受支持的非 Windows 操作系统上指定 JDK:

![][ic789643]

无论所使用的操作系统，你有以下两个**云部署**选项的源和 JDK 包类型：

* **部署第三方 JDK 包在 Azure 上可用** 
* **通过自定义下载部署** 

如果你使用**部署第三方 JDK 包可从 Azure**选项：

1. 检查名为的复选框**部署第三方 JDK 包可从 Azure**。
2. 从下拉列表中，选择可在 Azure 的第三方 JDK 包。
3. 你**JDK**外观将类似于 Windows 上的以下选项卡：![][ic780648]和它类似于以下 Mac OS 或其他支持的非 Windows 操作系统：![][ic789643]
4. 单击 **“确定”** 以保存你的更改。
5. 当系统提示您接受许可协议，从第三方 JDK 包提供程序，查看许可条款。 假设你接受这些条款，单击**是**关闭**接受许可协议**对话框。
    请注意，下拉列表中显示哪些项的基本逻辑**部署第三方 JDK 包可从 Azure**可以自定义选项。 自定义项， **JDK**对话框中，单击**自定义**链接。 这将关闭**JDK**属性页，然后打开**componentsets.xml**在 Eclipse 中，你可以然后根据需要修改其中的文件。 文档**componentsets.xml**纳入**componentsets.xml**文件本身。

如果你使用**部署从自定义下载 JDK**选项：

1. 创建 JDK 安装目录的 ZIP，从而确保目录节点本身是 ZIP 结构中，而不是其内容的子级。 记下目录的名称，因为你将需要它更高版本，并请记住此 JDK 安装将部署到 Windows 虚拟机。
2. 作为 blob 上载到 Azure 存储帐户 ZIP。 你可以执行此使用可从外部使用的工具用于将 blob 上载到 Azure 存储空间。 建议使用专用 blob。 记下 ZIP 内容的 blob URL。
3. 检查名为的复选框**部署从自定义下载 JDK**。
    如果你想要从你的 Azure 存储帐户下载，选择从存储帐户**存储帐户**下拉列表 (你可以单击**帐户**链接可以修改列表中)，这将部分填充**URL**字段，然后再填写 URL 的剩余部分。 如果不希望使用 Azure 存储空间，选择**（无）**从**存储帐户**下拉列表，并将 URL 输入到 JDK 下载在**URL**字段。 如果使用 Azure 存储空间，在 URL 中的 blob 名称必须为小写。
4. 确保**JAVA_HOME**文本框引用正确的目录名称。 默认情况下，它将引用相同的 JDK 目录名称为供本地使用选择的值。 但如果 ZIP 中包含的目录具有不同的名称 （例如，由于使用不同版本），更新中的目录名称**JAVA_HOME**文本框中相应地，因为此设置将在云 （而不是在计算模拟器中） 中使用。
5. 单击 **“确定”** 以保存你的更改。

就是这样。 现在，当你针对云进行构建，你将注意到包大小将小得多，生成过程通常需要更少的时间和部署本身在发布到云时还应充分较少的时间。 请注意，**部署我的本地 JDK （自动上载到云存储）**或**部署从自定义下载 JDK**选项为实际上仅当你的应用程序部署在云中。 它们具有不会影响你的计算模拟器体验;将部署到计算仿真程序时，将仍可使用本地版本的组件。 

### <a name="server-configuration"></a>服务器配置
下面是你可以如何指定应用程序服务器的一个示例。

![][ic796926]

验证**部署此类型的服务器**复选框为选中，，然后选择你想要使用的应用程序服务器的类型。

用于指定要用于云部署的服务器，你可以利用以下选项：

1. **部署 Azure 上提供的第三方服务器**-这是在其中部署效率和简单性是优先考虑，服务器不需要自定义配置的开发/测试方案中特别适用。 当你想要使用这些服务器之一作为起点，但你包括相应的服务器自定义步骤中部署的启动逻辑或者。
2. **通过自定义下载部署**-在具有你想要在云中使用特别准备和配置服务器时，此选项特别适用于生产方案。
3. **部署我的本地服务器安装**-此选项特别适用中如果本地服务器安装已供你使用自定义配置。 如果选择此选项，你还必须指定在你的本地服务器的路径**本地服务器路径**下面文本框。

如果你使用**部署在 Azure 上可用的第三方服务器**选项：

1. 检查名为的复选框**部署在 Azure 上可用的第三方服务器**。
2. 从下拉菜单中，选择要用于在云中部署所需的服务器软件。 请注意，如果你已指定要使用更早版本的服务器类型，您将限制为选择仅是作为该服务器类型相同的系列中的云服务器。 但是，如果你未选择服务器类型，则可以选择任何在 Azure 当前可用的服务器与服务器类型将自动为你选择。
3. 单击 **“确定”** 以保存你的更改。

如果使用**通过自定义下载部署**选项：

1. 请确保已选择服务器类型按前面的步骤。 这将告知插件如何部署服务器从自定义下载，因为它必须是同一系列作为所选的服务器类型。
2. 检查名为的复选框**通过自定义下载部署**。
    如果你想要从你的 Azure 存储帐户下载，选择从存储帐户**存储帐户**下拉列表 (你可以单击**帐户**链接可以修改列表中)，这将部分填充**URL**字段，然后填写你的服务器的 URL 的剩余部分下载 ZIP （当使用 Azure 存储空间，在 URL 中的 blob 名称必须小写）。 如果不希望使用 Azure 存储空间，选择**（无）**从**存储帐户**下拉列表，并将 URL 输入到你的服务器下载 ZIP 中的**URL**字段。 该 ZIP 将包含表示你的应用程序服务器安装目录的子文件夹。 例如，如果你对 Apache Tomcat 7.0.35 使用 zip，则 zip 内将表示安装目录，如子文件夹**apache tomcat 7.0.35**。 
3. 指定主目录环境变量的值。 它将默认为用于你的本地应用程序服务器，如果有的话，值，但如果你的云应用程序服务器不同于你的本地应用程序服务器，可以指定不同的值。 但是，你需要确保你的云应用程序服务器与服务器同一系列的先前所选类型。
    如果在将来更新你的云应用程序服务器 zip，你可以手动更改主目录设置中，或者，若要让它再次与本地设置 （如果你更改了本地应用程序服务器） 匹配。
4. 单击 **“确定”** 以保存你的更改。

哪些项显示中的基本逻辑**服务器**选项卡**服务器配置**可以自定义属性页。 这是一项高级的功能，如果你的需求超出默认值，或者如果你想要添加其他服务器，则可能需要。 自定义逻辑，**服务器**对话框中，单击**自定义**链接。 这将关闭**服务器配置**属性页，然后打开**componentsets.xml**在 Eclipse 中，然后可以修改所需扩展配置模板的服务器的文件。 文档**componentsets.xml**纳入**componentsets.xml**文件本身。

如果你使用**部署我的本地服务器 （自动上载到云存储）**选项：

1. 检查名为的复选框**部署我的本地服务器 （自动上载到云存储）**。
2. 使用**存储帐户**下拉列表中，选择**（自动）**。 如果指定**（自动）**此处 Eclipse 工具包将使用相同的存储帐户为你的服务器作为选择为你的部署中的一个**发布到 Azure**对话框。
3. 单击 **“确定”** 以保存你的更改。

选择在计算机上的服务器安装路径**本地服务器路径**文本框中，如果存在任何以下条件：

* 你想要在模拟器 （仅适用于 Windows） 中测试你的部署。
* 你想要部署到云中的本地安装的服务器。
* 你想要使用你自己在云中，在其中用例的自定义服务器下载，还应确保**部署我的本地服务器 （自动上载到云存储）**上面选择选项。

如果任何前面的选项适用于你的情况，本地服务器设置是可选的。

### <a name="applications-configuration"></a>应用程序配置
下面是你可以如何指定应用程序的示例。

![][ic719512]

单击**添加**以添加另一个应用程序，或**删除**删除应用程序。 出于效率考虑，如果你想要用于下载的源的应用程序部署到云中，使用时[组件属性](#components_properties)来指定 URL，存储帐户等。 

从 2014 年 4 月发行版开始，你的应用程序将自动上载到同一存储帐户 (在**eclipsedeploy**容器) 与你为部署选择。 你的部署的启动逻辑包含首先从该存储帐户中下载这些应用程序的步骤。 这意味着您可以将你的应用程序而无需重新生成并手动上载较新版本的应用程序直接置于该存储帐户 （例如使用 Azure 门户） 来重新部署整个包，升级你的部署中，替换的 WAR 文件最初上载到那里工具包。 然后，只需启动使用 Azure 管理门户，同样，或通过命令行实用程序的所有这些角色实例的回收。 （触发角色回收直接从 Eclipse 工具包中是不目前支持。）

### <a name="notes-about-server-configuration"></a>有关服务器配置的说明
通过所做的更改**服务器配置**属性页中将反映`<component>`package.xml 文件的元素。

当你使用**自动上载...**或**通过下载部署...**选项 JDK 或应用程序服务器，并且你要生成的云 （而不计算模拟器），并连接到网络，你可能注意到生成消息如下所示在控制台输出中，如 Ant 生成器验证下载的可用性：

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

如果你选择**通过下载部署...**选项，可能会显示以下警告，但将继续生成：

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

此警告是尚未验证下载的可用性的唯一指示。 因此如果部署在云中失败出于某种原因，检查以查看是否收到此警告。

如果你想要禁用下载验证 （例如，如果你认为它不必要地降低生成），请将设置`verifydownloads`属性设为`false`中`<windowsazurepackage>`package.xml 元素： 

`<windowsazurepackage verifydownloads="false" ...>` 

如果你选择**自动上载...**选项，然后在控制台窗口，你将看到生成报告的上载每隔 5 秒，每当需要上载时进度消息。

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>SSL 卸载属性
打开在 Eclipse 的项目资源管理器窗格中，角色的上下文菜单单击**Azure**，然后单击**SSL 卸载**。 

![][ic719481]

在此对话框中，你可以启用 SSL 卸载，从而可以轻松地启用安全超文本传输协议 (HTTPS) 支持在 Azure 上的 Java 部署中，而无需在 Java 应用程序服务器中配置 SSL。 有关详细信息，请参阅[SSL 卸载][ SSL Offloading]和[如何使用 SSL 卸载][How to Use SSL Offloading]。

## <a name="see-also"></a>另请参阅
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[安装 Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]

[在 Eclipse 中的 azure 创建 Hello World 应用程序][Creating a Hello World Application for Azure in Eclipse]

[Azure 项目属性][Azure Project Properties]

[Azure 存储帐户列表][Azure Storage Account List]

有关通过 Java 使用 Azure 的详细信息，请参阅[Azure Java 开发人员中心][Azure Java Developer Center]。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Project Properties]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure Storage Account List]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.microsoft.windowsazure.serviceruntime package summary]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugging a specific role instance in a multi-instance deployment]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debugging Azure Applications in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Deploying Large Deployments]: http://go.microsoft.com/fwlink/?LinkID=699536
[How to Use Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How to Use SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->
