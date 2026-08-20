
# ILogger + Azure Logging Questions and Answers

## 1. How do you configure ILogger to use Azure Application Insights?
Azure Application Insights is a cloud telemetry platform. ILogger integrates with it through built-in providers.
```csharp
builder.Services.AddApplicationInsightsTelemetry();

builder.Logging.AddApplicationInsights(
    configureTelemetryConfiguration: (config) => {},
    configureApplicationInsightsLoggerOptions: (options) => {}
);

_logger.LogInformation("Application Insights logging enabled.");
```
---

## 2. How do you log custom telemetry events to Azure Application Insights?
TelemetryClient allows sending custom events alongside ILogger output.
```csharp
@inject TelemetryClient Telemetry

<button @onclick="TrackAction">Track Event</button>

@code {
    void TrackAction()
    {
        Telemetry.TrackEvent("ButtonClicked", new Dictionary<string,string>
        {
            { "User", "John" },
            { "Action", "ClickedButton" }
        });
    }
}
```
---

## 3. How do you send exceptions to Application Insights using ILogger?
If the AI provider is configured, ILogger sends exceptions automatically.
```csharp
try
{
    throw new InvalidOperationException("Something failed!");
}
catch(Exception ex)
{
    _logger.LogError(ex, "Operation failed at {Time}", DateTime.UtcNow);
}
```
---

## 4. How do you log to Azure Blob Storage using ILogger?
Use Serilog's Azure Blob storage sink.
```csharp
Log.Logger = new LoggerConfiguration()
    .WriteTo.AzureBlobStorage(
        connectionString: "<blob-connection>",
        containerName: "logs",
        blobName: "app.log")
    .CreateLogger();

builder.Host.UseSerilog();
```
---

## 5. How do you configure ILogger for Azure Web Apps logging?
Azure Web Apps exposes diagnostics providers that ILogger can use.
```csharp
builder.Logging.AddAzureWebAppDiagnostics();

_logger.LogWarning("Running inside Azure Web App.");
```
---

## 6. How do you use ILogger inside an Azure Function?
Azure Functions inject ILogger through FunctionContext.
```csharp
[Function("HelloFunc")]
public static HttpResponseData Run(
    [HttpTrigger(AuthorizationLevel.Function, "get")] HttpRequestData req,
    FunctionContext ctx)
{
    var logger = ctx.GetLogger("HelloFunc");
    logger.LogInformation("Function executed.");

    var response = req.CreateResponse(HttpStatusCode.OK);
    response.WriteString("Ok");
    return response;
}
```
---

## 7. How do you send structured logs to Azure Monitor using ILogger?
Azure Monitor supports structured logging.
```csharp
_logger.LogInformation(
    "Request {Id} completed in {Time}ms by {User}",
    request.Id, duration, user.Name);
```
---

## 8. How do you use Azure Log Analytics workspace with ILogger?
Logs can be sent via the Logs Ingestion API.
```csharp
var client = new LogsIngestionClient(
    new Uri(workspaceEndpoint),
    dataCollectionRuleId,
    credential);

await client.UploadLogsAsync(
    streamName: "AppLogs",
    new[] {
        new {
            Timestamp = DateTime.UtcNow,
            Level = "Info",
            Message = "Test log"
        }
    });
```
---

## 9. How do you add cloud-only logging filters for Azure?
Use AddFilter to restrict logs when running in Azure.
```csharp
builder.Logging.AddFilter("Microsoft", LogLevel.Warning);
builder.Logging.AddFilter("System", LogLevel.Error);
builder.Logging.AddFilter("MyApp", LogLevel.Debug);
```
---

## 10. How do you audit user actions in Azure Application Insights using ILogger?
Use structured logging to collect audit information.
```csharp
_logger.LogInformation(
    "Audit: User {UserId} performed {Action} at {Time}",
    user.Id,
    "DeleteItem",
    DateTime.UtcNow);
```
---

