---
title: 使用 ILogger 在 Azure Application Insights 中浏览 .NET 跟踪日志
description: 将 Azure Application Insights ILogger 实现与 ASP.NET Core 及控制台应用程序配合使用的示例。
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 4c385d2af0d9e4e213cd690b3c30d8719588220d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2019
ms.locfileid: "58399499"
---
# <a name="ilogger"></a>ILogger

ASP.NET Core 支持适用于各种内置和第三方日志记录提供程序的日志记录 API。 本文介绍如何在控制台和 ASP.NET Core 应用程序中使用 Application Insights ILogger 实现处理日志记录。 若要详细了解基于 ILogger 的日志记录，请参阅[这篇文章](https://docs.microsoft.com/aspnet/core/fundamentals/logging)。

## <a name="console-application"></a>控制台应用程序

下文展示了配置为向 Application Insights 发送 ILogger 跟踪的示例控制台应用程序。

已安装的包：

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(loggingBuilder =>
        {
        // Optional: Apply filters to configure LogLevel Trace or above is sent to ApplicationInsights for all
        // categories.
        loggingBuilder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");                
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional. Epecially in case of AspNetCore request info is already
        // present in scope.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

## <a name="aspnet-core-application"></a>ASP.NET Core 应用程序

下文展示了配置为向 Application Insights 发送 ILogger 跟踪的示例 ASP.NET Core 应用程序。 可按照此示例从 Program.cs、Startup.cs 或任何其他控制器/应用程序逻辑发送 ILogger 跟踪。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        logger.LogInformation("From Program. Running the host now.."); // This will be picked up by AI
        host.Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(logging =>
        {
            logging.AddApplicationInsights("ikeyhere");

            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // ApplicationInsights for all categories.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);

            // Additional filtering For category starting in "Microsoft",
            // only Warning or above will be sent to Application Insights.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Warning);
        })
        .Build();
}
```

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

        // The following will be picked up by Application Insights.
        _logger.LogInformation("From ConfigureServices. Services.AddMVC invoked"); 
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
        // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
            _logger.LogInformation("An example of a Information trace..");
            _logger.LogWarning("An example of a Warning trace..");
            _logger.LogTrace("An example of a Trace level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

在以上两个示例中，使用了独立包 Microsoft.Extensions.Logging.ApplicationInsights。 默认情况下，此配置使用最低要求的 `TelemetryConfiguration` 将数据发送到 Application Insights。 最低要求指使用的通道将为 `InMemoryChannel`，没有采样，也没有标准 TelemetryInitializer。 可为控制台应用程序替代此行为，如以下示例所示。

安装此附加包：

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

以下部分介绍如何替代默认的 `TelemetryConfiguration`。 此示例配置 `ServerTelemetryChannel`、采样和自定义 `ITelemetryInitializer`。

```csharp
    // Create DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding ApplicationInsights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly call Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if application terminates, telemetry is sent to the back-end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

尽管可以在 ASP.NET Core 应用程序使用上述方法，但更常用的方法是结合常规应用程序监视 （请求、 依赖项等） ILogger 捕获，如下所示。

安装此附加包：

```xml
<PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
```

向 `ConfigureServices` 方法添加以下代码。 此代码将使用默认配置启用常规应用程序监视（ServerTelemetryChannel、LiveMetrics、Request/Dependencies、Correlation 等）。

```csharp
services.AddApplicationInsightsTelemetry("ikeyhere");
```

在此示例中，`ApplicationInsightsLoggerProvider` 使用的配置与常规应用程序监视使用的配置相同。 因此，`ILogger` 跟踪和其他遥测（Requests、Dependencies 等）将运行同一组 `TelemetryInitializers`、`TelemetryProcessors` 和 `TelemetryChannel`。 它们将以相同方式关联和采样/不采样。

但是，此行为有一个例外。 记录来自 `Program.cs` 或 `Startup.cs` 自身的内容时，`TelemetryConfiguration` 未完全设置，因此，这些日志不会拥有默认配置。 但是，其他各个日志（例如，来自 Controllers、Models 等的日志）会共享该配置。

## <a name="control-logging-level"></a>控制日志记录级别

除了筛选日志，如上面的示例中所示的代码，也可能是控制捕获的 Application Insights 日志记录级别，通过修改`appsettings.json`。 [日志记录基础文档 ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering)演示如何实现此目的。 专门为 Application Insights 提供程序别名的名称是`ApplicationInsights`中所示，下面的示例将配置`ApplicationInsights`捕获日志的`Warning`及更高版本所有类别。

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="frequently-asked-questions"></a>常见问题

*我看到一些 ILogger 日志显示在 Application Insights 中两次？*

* 如果您有旧版的 （现在已过时） 做到这一点`ApplicationInsightsLoggerProvider`启用通过调用`AddApplicationInsights`上`ILoggerFactory`。 检查是否在`Configure`方法具有以下内容，并将其删除。

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* 如果从 Visual Studio 调试时遇到双日志记录，然后修改使用 ApplicationInsights，如下所示，通过设置来启用的代码`EnableDebugLogger`为 false。 调试应用程序时，这是仅相关。

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```



## <a name="next-steps"></a>后续步骤

了解有关以下方面的详细信息：

- [基于 ILogger 的日志记录](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
- [.NET 跟踪日志](../../azure-monitor/app/asp-net-trace-logs.md)
