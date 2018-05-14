创建一个[身份验证文件](../java-sdk-azure-authenticate.md#mgmt-file)，并在命令行中将包含完整路径的环境变量 `AZURE_AUTH_LOCATION` 导出到该文件。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

该身份验证文件用于配置管理库用来定义、创建和配置 Azure 资源的入口点 `Azure` 对象。

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[详细了解](../java-sdk-azure-authenticate.md#mgmt-auth)使用用于 Java 的 Azure 管理库时可用的身份验证选项。