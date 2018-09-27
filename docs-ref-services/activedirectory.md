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
ms.openlocfilehash: 36758977a64600d5ac11a0c5ac59bed981e8fa33
ms.sourcegitcommit: 7c6a15f574fb85ee22f6ccdb7864627b73a6c1f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2018
ms.locfileid: "47398159"
---
# <a name="azure-active-directory-libraries-for-java"></a><span data-ttu-id="cfb1c-104">用于 Java 的 Azure Active Directory 库</span><span class="sxs-lookup"><span data-stu-id="cfb1c-104">Azure Active Directory libraries for Java</span></span>

## <a name="overview"></a><span data-ttu-id="cfb1c-105">概述</span><span class="sxs-lookup"><span data-stu-id="cfb1c-105">Overview</span></span>

<span data-ttu-id="cfb1c-106">使用 [Azure Active Directory](/azure/active-directory/active-directory-whatis) 将用户登录并控制对应用程序和 API 的访问。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

<span data-ttu-id="cfb1c-107">若要开始使用 Azure AD，请参阅[使用 Azure AD 进行 Java Web 应用登录和注销](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java)。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-107">To get started with Azure AD, see [Java web app sign-in and sign-out with Azure AD](/azure/active-directory/develop/active-directory-devquickstarts-webapp-java).</span></span>

## <a name="client-library"></a><span data-ttu-id="cfb1c-108">客户端库</span><span class="sxs-lookup"><span data-stu-id="cfb1c-108">Client library</span></span>

<span data-ttu-id="cfb1c-109">使用[用于 Java 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-java) 配置 OAuth2、OpenID Connect 或 Active Directory Graph 身份验证和 [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) 单一登录。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-109">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Java](https://github.com/AzureAD/azure-activedirectory-library-for-java).</span></span>

<span data-ttu-id="cfb1c-110">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用客户端库。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-110">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the client library in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.2.0</version>
</dependency>
```   

### <a name="example"></a><span data-ttu-id="cfb1c-111">示例</span><span class="sxs-lookup"><span data-stu-id="cfb1c-111">Example</span></span>

<span data-ttu-id="cfb1c-112">使用 Azure Active Directory 的[图形 API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) 检索 Active Directory 租户中某个用户的 JSON Web 令牌 (JWT)。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-112">Retrieve a JSON Web Token (JWT) for a user in your an Active Directory tenant using Azure Active Directory's [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api).</span></span> <span data-ttu-id="cfb1c-113">然后，可以使用此令牌在应用程序或 API 中对用户进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-113">This token can then be used to authenticate the user with an application or API.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="cfb1c-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="cfb1c-114">Management API</span></span>

<span data-ttu-id="cfb1c-115">配置[基于角色的访问控制](/azure/active-directory/role-based-access-control-what-is)，并使用管理 API 将标识（例如用户和[服务主体](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)）分配到这些角色。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-115">Configure [role based access control](/azure/active-directory/role-based-access-control-what-is) and assign identities (such as users and [service principals](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects)) to those roles with the management API.</span></span> 

<span data-ttu-id="cfb1c-116">向 Maven `pom.xml` 文件中[添加依赖项](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies)，以便在项目中使用管理 API。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-116">[Add a dependency](https://maven.apache.org/guides/getting-started/index.html#How_do_I_use_external_dependencies) to your Maven `pom.xml` file to use the management API in your project.</span></span>

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-mgmt-graph-rbac</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="example"></a><span data-ttu-id="cfb1c-117">示例</span><span class="sxs-lookup"><span data-stu-id="cfb1c-117">Example</span></span> 

<span data-ttu-id="cfb1c-118">创建新的服务主体并为其分配参与者角色。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-118">Create a new service principal and assign it the Contributor role.</span></span>

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
> [<span data-ttu-id="cfb1c-119">了解管理 API</span><span class="sxs-lookup"><span data-stu-id="cfb1c-119">Explore the Management APIs</span></span>](/java/api/overview/azure/activedirectory/management)


## <a name="samples"></a><span data-ttu-id="cfb1c-120">示例</span><span class="sxs-lookup"><span data-stu-id="cfb1c-120">Samples</span></span>

<span data-ttu-id="cfb1c-121">[管理组、用户和角色](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span><span class="sxs-lookup"><span data-stu-id="cfb1c-121">[Manage groups, users, and roles](https://github.com/Azure-Samples/aad-java-manage-users-groups-and-roles)  </span></span>  
<span data-ttu-id="cfb1c-122">[在 Java Web 应用中登录和注销用户](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span><span class="sxs-lookup"><span data-stu-id="cfb1c-122">[Sign-in and sign-out users in a Java web app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  </span></span>  
<span data-ttu-id="cfb1c-123">[在 Azure AD 中使用命令行应用访问 API](https://github.com/Azure-Samples/active-directory-java-native-headless) </span><span class="sxs-lookup"><span data-stu-id="cfb1c-123">[Access an API with Azure AD using a command line app](https://github.com/Azure-Samples/active-directory-java-native-headless) </span></span>  
[<span data-ttu-id="cfb1c-124">从 Java Web 应用调用 Active AD 图形 API</span><span class="sxs-lookup"><span data-stu-id="cfb1c-124">Call the Active AD Graph API from your Java web app</span></span>](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect)  

<span data-ttu-id="cfb1c-125">详细了解可在应用中使用的 [Azure AD 示例 Java 代码](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java)。</span><span class="sxs-lookup"><span data-stu-id="cfb1c-125">Explore more [sample Java code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=java) you can use in your apps.</span></span>
