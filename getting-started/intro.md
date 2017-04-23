# Getting started

## Configuring a Web Host

1. Add the [App.Metrics.Extensions.Middleware](https://www.nuget.org/packages/App.Metrics.Extensions.Middleware/) and [App.Metrics.Formatters.Json](https://www.nuget.org/packages/App.Metrics.Formatters.Json/) nuget packages to your project.

2. App Metrics is configured using the usual pattern to provide services and configuration to an ASP.NET Core project by adding required services to the `ConfigureServices` method in the `Startup.cs`

Modify your `Startup.cs` with the following:

[!code-csharp[Main](../src/samples/AppMetrics.Startup.CodeSnippets/Startup.cs)]    

What does the above configuration do?

- `AddMetrics` registers App Metrics services with the `IServiceCollection`. This method also also provides options to change the default metrics configuration if you wish to do so.
- `AddHealthChecks` registers the Health Checking sytem, it will scan for classes within the project and any other project which has a reference to App Metrics and register any class which inherits `HealthCheck`. Alternatively, this method also provides access to the `IHealthCheckFactory` where you can register health checks inline.
- `AddMetricsMiddleware` registers middleware components providing a serveral metrics related endpoints, which are:
	1.  `/ping`: Used to determine if you can get a successful pont response, useful for load balancers
	2.  `/metrics`: Renders all metrics using the configured `IMetricsDataSerializer`, in our case in json format
	3.  `/metrics-text`: Renders a Humannized version of the metric data captured by the application
	4.  `/health`: Executes all health checks registered to determin the health status of the application

## Configuring a Console Application

1. Add the [App.Metrics](https://www.nuget.org/packages/App.Metrics/) and [App.Metrics.Extensions.Reporting.Console](https://www.nuget.org/packages/App.Metrics.Extensions.Reporting.Console/) nuget packages to your project.

2. App Metrics is configured using the usual pattern to provide services and configuration to an ASP.NET Core project by adding required services to the `ConfigureServices` method in the `Startup.cs` 

Modify your `Program.cs` with the following:

[!code-csharp[Main](../src/samples/AppMetrics.Startup.CodeSnippets/MetricsProgram.cs)]    	 

- `AddMetrics` registers App Metrics services with the `IServiceCollection`. This method also also provides options to change the default metrics configuration if you wish to do so.
- `AddHealthChecks` registers the Health Checking sytem, it will scan for classes within the project and any other project which has a reference to App Metrics and register any class which inherits `HealthCheck`. Alternatively, this method also provides access to the `IHealthCheckFactory` where you can register health checks inline.
- `AddReporting` provides access to the `IReportFactory` allowing multiple metric report providers to be configured that will execute on each report run at the specified interval

## Measuring Application Metrics

App Metrics provides access to an `IMetrics` interface which is registered as a Single Instance. This can be injected where required to start mesuring different types of metrics.

Each metric being measured should be described through one of the below, which provides details about the metric being measured, the only required property is the `Name` property.

- `GaugeOptions`: Allows the metric context, metric name, unit of measurement, and tags to be specified.
- `CounterOptions`: Allows the metric context, metric name, unit of measurement, and tags to be specified.
- `MeterOptions`: Allows the metric context, metric name, unit of measurement, rate unit and tags to be specified.
- `TimerOptions`: Allows the metric context, metric name, unit of measurement, rate unit, duration unit, sampling type and tags to be specified. Also allows a custom `IReservoir` to be used for sampling.
- `HistogramOptions`: Allows the metric context, metric name, unit of measurement, sampling type and tags to be specified. Also allows a custom `IReservoir` to be used for sampling.

> [!NOTE]
> See the [Api Documentation](../api/App.Metrics.Core.Options.html) for details on each of the above.

Below is an example of how organize an application's metrics

[!code-csharp[Main](../src/samples/AppMetrics.Metric.Code.Snippets/AppMetricsRegistry.cs)]    	     

And to record the above metrics through `IMetrics`

[!code-csharp[Main](../src/samples/AppMetrics.Metric.Code.Snippets/RecordMetrics.cs)]    	 

## Next Steps

- [Metrics Types](metric-types/overview.md)
- [Configuration Options](fundamentals/configuration.md)  