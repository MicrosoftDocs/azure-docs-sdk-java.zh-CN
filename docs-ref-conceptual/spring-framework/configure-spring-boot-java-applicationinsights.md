---
title: 将 Spring Boot Initializer 应用配置为使用 Azure Application Insights SpringBoot Starter
description: 将使用 Spring Initializer 创建的 Spring Boot 应用程序配置为使用 Appliaction Insights SpringBoot Starter。
services: Application-Insights
documentationcenter: java
author: dhaval24
manager: alexklim
editor: ''
ms.assetid: ''
ms.author: dhdoshi
ms.date: 11/21/2018
ms.devlang: java
ms.service: Azure Monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.workload: na
ms.openlocfilehash: eef5afa1bcd8ceb92eca1584df8816b73ac78948
ms.sourcegitcommit: 8d0c59ae7c91adbb9be3c3e6d4a3429ffe51519d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52338731"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a><span data-ttu-id="c71ff-103">将 Spring Boot Initializer 应用配置为使用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="c71ff-103">Configure a Spring Boot Initializer app to use Application Insights</span></span>

<span data-ttu-id="c71ff-104">本文引导你使用 **[Spring Initializr]** 创建一个 Spring Boot 应用程序，该应用程序使用 Azure Application Insights Spring Boot Starter 对云中的 Java 应用程序进行端到端监视。</span><span class="sxs-lookup"><span data-stu-id="c71ff-104">This article walks you through creating a Spring Boot application using **[Spring Initializr]**, that uses Azure Application Insights Spring Boot Starter for end-to-end monitoring of Java applications on cloud.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="c71ff-105">\*此启动器目前以 \**BETA 版（公共预览版）*<em>提供。</em></span><span class="sxs-lookup"><span data-stu-id="c71ff-105">\*This starter is currently in \**BETA (public preview)*<em>.</em></span></span>

## <a name="prerequisites"></a><span data-ttu-id="c71ff-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="c71ff-106">Prerequisites</span></span>

<span data-ttu-id="c71ff-107">为完成本文介绍的步骤，需要满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="c71ff-107">The following prerequisites are required in order to complete the steps in this article:</span></span>

* <span data-ttu-id="c71ff-108">Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。</span><span class="sxs-lookup"><span data-stu-id="c71ff-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c71ff-109">一个受支持的 Java 开发工具包 (JDK)。</span><span class="sxs-lookup"><span data-stu-id="c71ff-109">A supported Java Development Kit (JDK).</span></span> <span data-ttu-id="c71ff-110">有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。</span><span class="sxs-lookup"><span data-stu-id="c71ff-110">For more information about the JDKs available for use when developing on Azure, see <https://aka.ms/azure-jdks>.</span></span>
* <span data-ttu-id="c71ff-111">[Apache Maven](http://maven.apache.org/) 3.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="c71ff-111">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="c71ff-112">使用 Spring Initializr 创建自定义应用程序</span><span class="sxs-lookup"><span data-stu-id="c71ff-112">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="c71ff-113">浏览到 [https://start.spring.io/](https://start.spring.io/)。</span><span class="sxs-lookup"><span data-stu-id="c71ff-113">Browse to [https://start.spring.io/](https://start.spring.io/).</span></span>

1. <span data-ttu-id="c71ff-114">指定你想要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后在依赖项部分选择 Web 依赖项。</span><span class="sxs-lookup"><span data-stu-id="c71ff-114">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then select web dependency in the dependenies section.</span></span>

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="c71ff-116">Spring Initializr 将使用“组”名称和“项目”名称创建包名称，例如：*com.example.demo*。</span><span class="sxs-lookup"><span data-stu-id="c71ff-116">The Spring Initializr will use the **Group** and **Artifact** names to create the package name; for example: *com.example.demo*.</span></span>
   >

1. <span data-ttu-id="c71ff-117">单击“生成项目”对应的按钮。</span><span class="sxs-lookup"><span data-stu-id="c71ff-117">Click the button to **Generate Project**.</span></span>

1. <span data-ttu-id="c71ff-118">出现提示时，将项目下载到本地计算机中的路径。</span><span class="sxs-lookup"><span data-stu-id="c71ff-118">When prompted, download the project to a path on your local computer.</span></span>

1. <span data-ttu-id="c71ff-119">提取本地系统上的文件之后，自定义 Spring Boot 应用程序便可进行编辑。</span><span class="sxs-lookup"><span data-stu-id="c71ff-119">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![自定义 Spring Boot 项目文件][SI02]

## <a name="create-an-application-insights-resource-on-azure"></a><span data-ttu-id="c71ff-121">在 Azure 中创建 Application Insights 资源</span><span class="sxs-lookup"><span data-stu-id="c71ff-121">Create an Application Insights Resource on Azure</span></span>

1. <span data-ttu-id="c71ff-122">浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“+ 新建”。</span><span class="sxs-lookup"><span data-stu-id="c71ff-122">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure 门户][AZ01]

1. <span data-ttu-id="c71ff-124">依次单击“管理工具”、“Application Insights”。</span><span class="sxs-lookup"><span data-stu-id="c71ff-124">Click **Management Tools**, and then click **Application Insights**.</span></span>

   ![Azure 门户][AZ02]

1. <span data-ttu-id="c71ff-126">在“新建 Application Insights 资源”页上，指定以下信息：</span><span class="sxs-lookup"><span data-stu-id="c71ff-126">On the **New Application Insights Resource** page, specify the following information:</span></span>

   * <span data-ttu-id="c71ff-127">输入 Application Insights 资源的**名称**。</span><span class="sxs-lookup"><span data-stu-id="c71ff-127">Enter the **Name** for your Application Insights resource.</span></span>
   * <span data-ttu-id="c71ff-128">在“应用程序类型”中选择“Java Web 应用程序”。</span><span class="sxs-lookup"><span data-stu-id="c71ff-128">Choose the **Application Type** to Java Web Application.</span></span>
   * <span data-ttu-id="c71ff-129">指定“订阅”、“资源组”和“位置”。</span><span class="sxs-lookup"><span data-stu-id="c71ff-129">Specify your **Subscription**, **Resource group** and **Location**.</span></span>
   * <span data-ttu-id="c71ff-130">如果想要在 Azure 门户上固定资源，请选择“固定到仪表板”选项。</span><span class="sxs-lookup"><span data-stu-id="c71ff-130">Select Pin to dashboard option, if you would like to pin the resource on your Azure portal.</span></span>

   <span data-ttu-id="c71ff-131">指定这些选项后，单击“创建”以创建 Application Insights 资源。</span><span class="sxs-lookup"><span data-stu-id="c71ff-131">When you have specified these options, click **Create** to create your Application Insights resource.</span></span>

   ![Azure 门户][AZ03]

1. <span data-ttu-id="c71ff-133">创建资源后，Azure **仪表板**上以及“所有资源”页下会列出该资源。</span><span class="sxs-lookup"><span data-stu-id="c71ff-133">Once your resource has been created, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources** pages.</span></span> <span data-ttu-id="c71ff-134">可在上述任何位置单击该资源，打开 Application Insights 资源概述页。</span><span class="sxs-lookup"><span data-stu-id="c71ff-134">You can click on your resource on any of those locations to open the overview page of the Application Insights resource.</span></span> <span data-ttu-id="c71ff-135">请复制此概述页中的**检测密钥**。</span><span class="sxs-lookup"><span data-stu-id="c71ff-135">From this overview page please copy the **instrumentation key**.</span></span>

   ![Azure 门户][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a><span data-ttu-id="c71ff-137">将下载的 Spring Boot 应用程序配置为使用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="c71ff-137">Configure your downloaded Spring Boot Application to use Application Insights</span></span>

1. <span data-ttu-id="c71ff-138">在应用的根目录中找到 *POM.xml* 文件，并在其 dependencies 节中添加以下依赖项。</span><span class="sxs-lookup"><span data-stu-id="c71ff-138">Locate the *POM.xml* file in the root directory of your app, and add the following dependency in its dependencies section.</span></span> 

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.0.1-BETA</version>
</dependency>
```

1. <span data-ttu-id="c71ff-139">在应用的“资源”目录中找到 application.properties 文件，或创建此文件（若此文件不存在）。</span><span class="sxs-lookup"><span data-stu-id="c71ff-139">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![找到 application.properties 文件][RE01]

1. <span data-ttu-id="c71ff-141">在文本编辑器中打开 *application.properties* 文件，将以下行添加到该文件，然后将示例值替换为包含相应凭据的相应属性：</span><span class="sxs-lookup"><span data-stu-id="c71ff-141">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties with appropriate credentials:</span></span>

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   <span data-ttu-id="c71ff-142">有关微调 Application Insights 的其他方法，请参阅 [Application Insights Springboot Starter 自述文件](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。</span><span class="sxs-lookup"><span data-stu-id="c71ff-142">For more ways to fine tune Application Insights please refer to [Application Insights Springboot starter readme](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

   > [!NOTE]
   > 
   > <span data-ttu-id="c71ff-143">可将不同的 Application Insights 检测密钥（即</span><span class="sxs-lookup"><span data-stu-id="c71ff-143">You can use different Application Insights instrumentation keys (i.e</span></span> <span data-ttu-id="c71ff-144">不同的资源）用于不同的配置文件（例如 PROD、DEV 等）。有关更多信息，请参阅 [Spring Boot 配置文件的特定属性]。</span><span class="sxs-lookup"><span data-stu-id="c71ff-144">different resources) for different profiles like PROD, DEV etc. Please refer to [Spring Boot Profile Specific Properties] for additional information.</span></span> 

1. <span data-ttu-id="c71ff-145">保存并关闭 application.properties 文件。</span><span class="sxs-lookup"><span data-stu-id="c71ff-145">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="c71ff-146">在包的源文件夹中创建名为“控制器”的文件夹，例如：</span><span class="sxs-lookup"><span data-stu-id="c71ff-146">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   <span data-ttu-id="c71ff-147">-或-</span><span class="sxs-lookup"><span data-stu-id="c71ff-147">-or-</span></span>

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. <span data-ttu-id="c71ff-148">在 *controller* 文件夹中创建名为 *TestController.java* 的新文件。</span><span class="sxs-lookup"><span data-stu-id="c71ff-148">Create a new file named *TestController.java* in the *controller* folder.</span></span> <span data-ttu-id="c71ff-149">在文本编辑器中打开该文件，然后向其添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="c71ff-149">Open the file in a text editor and add the following code to it:</span></span>

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   <span data-ttu-id="c71ff-150">需要将 `com.example.demo` 替换为项目的包名称的地方。</span><span class="sxs-lookup"><span data-stu-id="c71ff-150">Where you will need to replace `com.example.demo` with the package name for your project.</span></span>

1. <span data-ttu-id="c71ff-151">保存并关闭 *TestController.java* 文件。</span><span class="sxs-lookup"><span data-stu-id="c71ff-151">Save and close the *TestController.java* file.</span></span>

1. <span data-ttu-id="c71ff-152">使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：</span><span class="sxs-lookup"><span data-stu-id="c71ff-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="c71ff-153">使用 Web 浏览器浏览到 http://localhost:8080/sample/hello 以测试 Web 应用；如果有可用的 Curl，也可使用以下示例所示的语法：</span><span class="sxs-lookup"><span data-stu-id="c71ff-153">Test the web app by browsing to http://localhost:8080/sample/hello using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   <span data-ttu-id="c71ff-154">示例控制器中应会显示</span><span class="sxs-lookup"><span data-stu-id="c71ff-154">You should see the "hello!"</span></span> <span data-ttu-id="c71ff-155">“hello!”消息。</span><span class="sxs-lookup"><span data-stu-id="c71ff-155">message from your sample controller displayed.</span></span> <span data-ttu-id="c71ff-156">Application Insights 会自动收集此请求，并连同控制器逻辑中指定的关联自定义事件、自定义指标、自定义依赖项和自定义跟踪，将其作为遥测项发送。</span><span class="sxs-lookup"><span data-stu-id="c71ff-156">Application Insights will automatically collect this request and send it as a telemetry item with it's associated custom event, custom metric, custom dependency and custom trace as specified in the controller logic.</span></span> 

   <span data-ttu-id="c71ff-157">几秒钟后，Azure 门户中应会显示数据。</span><span class="sxs-lookup"><span data-stu-id="c71ff-157">After a few seconds you should see the data on Azure portal.</span></span> 

   ![Azure 门户][AZ05]

   <span data-ttu-id="c71ff-159">可以单击“应用程序映射”磁贴查看高级组件及其彼此之间的交互。</span><span class="sxs-lookup"><span data-stu-id="c71ff-159">You can click on Application Map tile to view high level components and their interaction with each other.</span></span> <span data-ttu-id="c71ff-160">建议在此位置获取整个应用程序的简要概述。</span><span class="sxs-lookup"><span data-stu-id="c71ff-160">This is a recommended place to get a high level overview of entire application.</span></span> <span data-ttu-id="c71ff-161">可根据 Spring 应用程序名称识别每个 Spring Boot 微服务。</span><span class="sxs-lookup"><span data-stu-id="c71ff-161">Each Spring Boot Microservice is recognized by the spring application name.</span></span> <span data-ttu-id="c71ff-162">请记得设置应用程序名称。</span><span class="sxs-lookup"><span data-stu-id="c71ff-162">Please remember to set it.</span></span>

   ![Azure 门户][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a><span data-ttu-id="c71ff-164">将 Springboot 应用程序配置为向 Application Insights 发送 log4j 日志</span><span class="sxs-lookup"><span data-stu-id="c71ff-164">Configure Springboot Application to send log4j logs to Application Insights</span></span>

1. <span data-ttu-id="c71ff-165">修改项目的 POM.xml 文件，并按如下所示添加/修改 dependencies 节。</span><span class="sxs-lookup"><span data-stu-id="c71ff-165">Modify the POM.xml file of the project and add/modify the dependencies section with following.</span></span> 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.0.1-BETA</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. <span data-ttu-id="c71ff-166">保存并关闭 *POM.xml* 文件。</span><span class="sxs-lookup"><span data-stu-id="c71ff-166">Save and close the *POM.xml* file.</span></span>

3. <span data-ttu-id="c71ff-167">在 \src\main\resources 文件夹中创建新文件 *log4j2.xml* 并对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="c71ff-167">In \src\main\resources folder, create a new file *log4j2.xml* and configure it.</span></span> <span data-ttu-id="c71ff-168">例如：</span><span class="sxs-lookup"><span data-stu-id="c71ff-168">For example:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```
4. <span data-ttu-id="c71ff-169">再次按前面所示生成并运行 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="c71ff-169">Build and run the Spring Boot application again as shown above.</span></span> 

<span data-ttu-id="c71ff-170">几秒钟内，Azure 门户中应会显示所有 Spring 日志。</span><span class="sxs-lookup"><span data-stu-id="c71ff-170">Within few seconds, you should see all the spring logs being available on Azure Portal.</span></span> 

![Azure 门户][AZ06]

<span data-ttu-id="c71ff-172">甚至可以在 Analytics 门户中查看详细的日志消息和执行分析。</span><span class="sxs-lookup"><span data-stu-id="c71ff-172">You can even look at the detailed log messages and do analysis on Analytics Portal.</span></span> 

![Azure 门户][AZ07]

## <a name="next-steps"></a><span data-ttu-id="c71ff-174">后续步骤</span><span class="sxs-lookup"><span data-stu-id="c71ff-174">Next steps</span></span>

<span data-ttu-id="c71ff-175">有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="c71ff-175">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="c71ff-176">将 Spring Boot 应用程序部署到 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="c71ff-176">Deploy a Spring Boot Application to the Azure App Service</span></span>](deploy-spring-boot-java-web-app-on-azure.md)

* [<span data-ttu-id="c71ff-177">在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序</span><span class="sxs-lookup"><span data-stu-id="c71ff-177">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](deploy-spring-boot-java-app-on-kubernetes.md)

<span data-ttu-id="c71ff-178">Application Insights 支持收集外部依赖项及其与传入请求之间的关联。</span><span class="sxs-lookup"><span data-stu-id="c71ff-178">Application Insights supports automatic collection of external dependencies and its correlation with incoming requests.</span></span> <span data-ttu-id="c71ff-179">目前支持自动收集 Oracle、MsSQL、MySQL 和 Redis 数据。</span><span class="sxs-lookup"><span data-stu-id="c71ff-179">Currently we support autocollection of Oracle, MsSQL, MySQL and Redis.</span></span> <span data-ttu-id="c71ff-180">有关启用自动收集的更多详细信息，请参阅[如何使用 Application Insights Java 代理](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent)一文。</span><span class="sxs-lookup"><span data-stu-id="c71ff-180">For more details on enabling autocollection please follow the article [how to use Application Insights Java agent](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent).</span></span>

<span data-ttu-id="c71ff-181">有关 Azure Application Insights 的详细信息及其监视功能，请参阅 **[Application Insights]** 主页。</span><span class="sxs-lookup"><span data-stu-id="c71ff-181">For more information about Azure Application Insights, and it's monitoring capabilities, see the **[Application Insights]** home page.</span></span>

<span data-ttu-id="c71ff-182">有关 Application Insights Spring Boot Starter 的其他配置详细信息，请参阅[此链接](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。</span><span class="sxs-lookup"><span data-stu-id="c71ff-182">For more information about additional configuration details of Application Insights Spring Boot Starter, please refer to this [link](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).</span></span>

<span data-ttu-id="c71ff-183">如需请求新功能和报告潜在的 bug，请在 [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) 存储库中提出问题。</span><span class="sxs-lookup"><span data-stu-id="c71ff-183">For feature requests and potential bugs, please open issues on our [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) repository.</span></span>

<span data-ttu-id="c71ff-184">有关将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[用于 Visual Studio Team Services 的 Java 工具]。</span><span class="sxs-lookup"><span data-stu-id="c71ff-184">For more information about using Azure with Java, see the [Azure for Java Developers] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="c71ff-185">[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。</span><span class="sxs-lookup"><span data-stu-id="c71ff-185">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="c71ff-186">基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。</span><span class="sxs-lookup"><span data-stu-id="c71ff-186">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="c71ff-187">为帮助开发人员开始使用 Spring Boot，[https://github.com/spring-guides/](https://github.com/spring-guides/) 上提供了几个 Spring Boot 示例。</span><span class="sxs-lookup"><span data-stu-id="c71ff-187">To help developers get started with Spring Boot, several sample Spring Boot packages are available at [https://github.com/spring-guides/](https://github.com/spring-guides/).</span></span> <span data-ttu-id="c71ff-188">除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。</span><span class="sxs-lookup"><span data-stu-id="c71ff-188">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<!-- URL List -->

[面向 Java 开发人员的 Azure]: https://docs.microsoft.com/java/azure/
[Azure for Java Developers]: https://docs.microsoft.com/java/azure/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[用于 Visual Studio Team Services 的 Java 工具]: https://java.visualstudio.com/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 配置文件的特定属性]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Boot Profile Specific Properties]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: https://docs.microsoft.com/azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
