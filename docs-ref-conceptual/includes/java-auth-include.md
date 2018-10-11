<span data-ttu-id="43bc8-101">创建一个[身份验证文件](../java-sdk-azure-authenticate.md#mgmt-file)，并在命令行中将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 导出到该文件。</span><span class="sxs-lookup"><span data-stu-id="43bc8-101">Create an [authentication file](../java-sdk-azure-authenticate.md#mgmt-file) and export an environment variable `AZURE_AUTH_LOCATION` on the command line with the full path to the file.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

<span data-ttu-id="43bc8-102">该身份验证文件用于配置管理库用来定义、创建和配置 Azure 资源的入口点 `Azure` 对象。</span><span class="sxs-lookup"><span data-stu-id="43bc8-102">The authentication file is used to configure the entry point `Azure` object used by the management libraries to define, create, and configure Azure resources.</span></span>

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

<span data-ttu-id="43bc8-103">[详细了解](../java-sdk-azure-authenticate.md#mgmt-auth)使用用于 Java 的 Azure 管理库时可用的身份验证选项。</span><span class="sxs-lookup"><span data-stu-id="43bc8-103">[Learn more](../java-sdk-azure-authenticate.md#mgmt-auth) about authentication options when using the Azure management libraries for Java.</span></span>