---
title: 在 Eclipse 中显示 Azure Libraries Package for Java 的 Javadoc 内容
description: 如何在 Eclipse 中显示 Azure Libraries 的 Javadoc 内容。
services: ''
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: ''
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.author: robmcm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: bfed1a879bacf82436b71654c0ceca2826381eed
ms.sourcegitcommit: 115f4c8ad07a11f17d79e9d945d63917836b11c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61590599"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>在 Eclipse 中显示 Azure Libraries Package for Java 的 Javadoc 内容

通过将 Javadoc 内容关联到 Azure Libraries for Java，可以在 Eclipse 环境中查看 Azure Libraries for Java 的 Javadoc 内容。 以下步骤说明如何在 Eclipse 中使用此功能。

此过程假设已在生成路径中添加了 Azure Libraries for Java。

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>在 Eclipse 中显示 Azure Libraries for Java 的 Javadoc 内容

1. 在 Eclipse 的项目资源管理器内，在项目的“引用库”部分中，打开 Azure Library for Java JAR 的上下文菜单。 例如 **microsoft-windowsazure-api-0.1.0.jar**（根据安装的版本，版本号可能不同）。

1. 单击“属性”。

1. 在“属性”对话框中的左窗格内，单击“Javadoc 位置”。 此时会显示“Javadoc 位置”对话框。

1. 可以指定“Javadoc URL”或“存档中的 Javadoc”。

   * 如果选择指定 **Javadoc URL**，请使用 **http://dl.windowsazure.com/javadoc** 或 **http://dl.windowsazure.com/storage/javadoc** 等 URL。

   * 如果选择使用“存档中的 Javadoc”，可以指定外部文件或工作区文件。

   做出选择并根据需要浏览/验证。 以下示例会将 Azure Libraries for Java 关联到已本地下载到名为 **c:\MyAzureJARs** 的文件夹的相应 Javadoc JAR。

   ![显示 Javadoc 内容。][ic553487]

1. *可选步骤*：单击“验证”。 此处可能会显示 Javadoc JAR 的潜在问题。

1. 单击“确定”。

与库关联后，Javadoc 内容应会显示在 Eclipse IDE 中。 例如，如果 `blob` 在代码中定义为 `CloudBlockBlob` 类型，则在代码中键入 `blob.acquireLease` 时，会显示以下示例 Javadoc 内容：

![显示 Javadoc 内容。][ic553488]

## <a name="next-steps"></a>后续步骤

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
