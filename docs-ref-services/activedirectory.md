---
title: 用于 Java 的 Azure Active Directory 库
description: Azure Active Directory Java 客户端和管理库的参考文档
keywords: Azure, Java, SDK, API, SQL, 身份验证, AAD, Active Directory, Graph, OAuth 2.0
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 07/11/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: java
ms.openlocfilehash: 6147fc959727b53cd796534cefc927145b52d81d
ms.sourcegitcommit: 4d52e47073fb0b3ac40a2689daea186bad5b1ef5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49799833"
---
# <a name="azure-active-directory-libraries-for-java"></a>用于 Java 的 Azure Active Directory 库

## <a name="overview"></a>概述

使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 将用户登录并控制对应用程序和 API 的访问。

若要开始使用 Azure AD，请参阅[使用 Azure AD 进行 Java Web 应用登录和注销](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)。

## <a name="client-library"></a>客户端库

使用[用于 Java 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) 配置 OAuth2、OpenID Connect 或 Active Directory Graph 身份验证和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 单一登录。

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a>示例

使用 Azure Active Directory 的[图形 API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) 检索 Active Directory 租户中某个用户的 JSON Web 令牌 (JWT)。 然后，可以使用此令牌在应用程序或 API 中对用户进行身份验证。

```java
ExecutorService service = Executors.newFixedThreadPool(1);
AuthenticationContext context = new AuthenticationContext(AUTHORITY, false, service);
Future<AuthenticationResult> future = context.acquireToken(
    "https://graph.windows.net", YOUR_TENANT_ID, username, password,
    null);
AuthenticationResult result = future.get();
System.out.println("Access Token - " + result.getAccessToken());
System.out.println("Refresh Token - " + result.getRefreshToken());
System.out.println("ID Token - " + result.getIdToken());
```

## <a name="management-api"></a>管理 API

配置[基于角色的访问控制](/azure/active-directory/role-based-access-control-what-is)，并使用管理 API 将标识（例如用户和[服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)）分配到这些角色。 

向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a>示例 

创建新的服务主体并为其分配参与者角色。

```java
ServicePrincipal sp = Azure.servicePrincipals().define(spName)
    .withNewApplication("http://" + spName)
    .create();
RoleAssignment roleAssignment2 = authenticated.roleAssignments()
    .define("contribRoleAssignment")
    .forServicePrincipal(sp)
    .withBuiltInRole(BuiltInRole.CONTRIBUTOR)
    .withSubscriptionScope("862f67bc-d3ae-4243-bec7-3da6dca77717")
    .create();
```

> [!div class="nextstepaction"]
> [了解管理 API](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a>示例

[管理组、用户和角色](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)    
[在 Java Web 应用中登录和注销用户](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)    
[在 Azure AD 中使用命令行应用访问 API](https://github.com/Azure-Samples/active-directory-java-native-headless)   
[从 Java Web 应用调用 Active AD 图形 API](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  

详细了解可在应用中使用的 [Azure AD 示例 Java 代码](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)。
