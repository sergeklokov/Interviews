
# ILogger Questions, Answers, and Code Examples

## 1. What is ILogger in .NET?
ILogger is the standard .NET interface used for structured logging across all .NET applications.
```csharp
public class Demo
{
    private readonly ILogger<Demo> _logger;

    public Demo(ILogger<Demo> logger)
    {
        _logger = logger;
        _logger.LogInformation("Demo class initialized.");
    }
}
```
---

## 2. Is ILogger specific to Blazor?
No. ILogger is a generic logging API used in ASP.NET Core, MAUI, Blazor, Worker Services, and more.
```csharp
public partial class MainPage
{
    public MainPage(ILogger<MainPage> logger)
    {
        logger.LogInformation("Loaded MainPage.");
    }
}
```
---

## 3. How does ILogger work in Blazor Server?
It logs messages to server-side providers like Console, Seq, or Serilog.
```csharp
@inject ILogger<Index> Logger
<button @onclick="() => Logger.LogInformation("Clicked")">Click</button>
```
---

## 4. How does ILogger work in Blazor WebAssembly?
Blazor WASM logs directly to browser DevTools console.
```csharp
builder.Logging.SetMinimumLevel(LogLevel.Debug);
```
---

## 5. Can ILogger write to files?
Yes, using logging providers such as Serilog.
```csharp
Log.Logger = new LoggerConfiguration()
    .WriteTo.File("logs/app.log")
    .CreateLogger();
```
---

## 6. Can ILogger write to SQLite?
Yes. Implement a custom ILoggerProvider that writes log entries into SQLite.
```csharp
public void Log<TState>(LogLevel level, EventId id,
    TState state, Exception ex, Func<TState, Exception, string> formatter)
{
    using var conn = new SqliteConnection("Data Source=app.db");
    conn.Open();
    var cmd = conn.CreateCommand();
    cmd.CommandText = "INSERT INTO Logs(Level, Message) VALUES($lvl, $msg)";
    cmd.Parameters.AddWithValue("$lvl", level.ToString());
    cmd.Parameters.AddWithValue("$msg", formatter(state, ex));
    cmd.ExecuteNonQuery();
}
```
---

## 7. What is an ILoggerProvider?
A provider determines where logs are written.
```csharp
public class MyProvider : ILoggerProvider
{
    public ILogger CreateLogger(string categoryName) => new MyLogger();
    public void Dispose() {}
}
```
---

## 8. Can ILogger log to multiple destinations?
Yes. Add multiple providers.
```csharp
builder.Logging.AddConsole();
builder.Logging.AddProvider(new SQLiteLoggerProvider());
```
---

## 9. How do you inject ILogger in MAUI?
Like all DI services.
```csharp
public MainPage(ILogger<MainPage> logger)
{
    logger.LogWarning("Page created");
}
```
---

## 10. What is structured logging?
Structured logging adds fields to logs.
```csharp
_logger.LogInformation("User {Id} logged in", user.Id);
```
---

## 11. How do you log exceptions?
Using LogError.
```csharp
try { throw new Exception("Error!"); }
catch (Exception ex) { _logger.LogError(ex, "An exception occurred."); }
```
---

## 12. Can ILogger filter logs?
Yes.
```csharp
builder.Logging.AddFilter("Microsoft", LogLevel.Warning);
```
---

## 13. Does ILogger work in background tasks?
Yes.
```csharp
public class Worker : BackgroundService
{
    private readonly ILogger<Worker> _logger;
    public Worker(ILogger<Worker> logger) => _logger = logger;
    protected override async Task ExecuteAsync(CancellationToken t)
    {
        _logger.LogInformation("Worker running");
    }
}
```
---

## 14. Why use ILogger instead of custom logging?
It supports structured logging, DI, multiple sinks, and high performance.
```csharp
_logger.LogDebug("Processed item {ItemId}", id);
```
---

## 15. How do you implement SQLite logging?
Create custom provider + logger.
```csharp
public class SQLiteLogger : ILogger
{
    public void Log<TState>(LogLevel level, EventId id, TState st, Exception ex, Func<TState, Exception, string> fmt)
    {
        // Write to SQLite
    }
}
```
---

## 16. How do you log to SQL Server?
Use Serilog MSSqlServer.
```csharp
.WriteTo.MSSqlServer(conn, "Logs", autoCreateSqlTable: true)
```
---

## 17. How do you view Blazor WASM logs?
Open browser DevTools.
```csharp
Console.WriteLine("Hello from WASM");
```
---

## 18. Can ILogger log to the cloud?
Yes.
```csharp
builder.Logging.AddApplicationInsights("YOUR_KEY");
```
---

## 19. How does MAUI Android handle ILogger?
Logs go to Logcat by default.
```csharp
builder.Logging.AddDebug();
```
---

## 20. What is LogLevel?
Severity of logs.
```csharp
_logger.LogCritical("System failure!");
```
---

## 21. Can ILogger include timestamps?
Provider adds timestamps automatically.
```csharp
_logger.LogInformation("Running at {Time}", DateTime.Now);
```
---

## 22. Can ILogger output JSON logs?
Yes via Serilog.
```csharp
.WriteTo.File(new JsonFormatter(), "logs.json")
```
---

## 23. What is ILoggerFactory?
Creates ILogger instances.
```csharp
var logger = factory.CreateLogger("MyCat");
```
---

## 24. How do you disable console logging?
```csharp
builder.Logging.ClearProviders();
```
---

## 25. Can ILogger send logs via HTTP?
Yes using custom providers.
```csharp
await http.PostAsJsonAsync("/log", new { message });
```
---

## 26. What is BeginScope?
Adds context to logs.
```csharp
using (_logger.BeginScope("Transaction {Id}", txId))
{
    _logger.LogInformation("Started");
}
```
---

## 27. Can ILogger track user activity?
Yes.
```csharp
_logger.LogInformation("User {Id} viewed {Page}", user.Id, page);
```
---

## 28. Can ILogger be used in static classes?
Yes, if passed in.
```csharp
public static void LogSomething(ILogger logger)
{
    logger.LogInformation("Static log");
}
```
---

## 29. What happens if no providers are added?
No logs are written.
```csharp
builder.Logging.ClearProviders();
```
---

## 30. How do you do async logging?
ILogger is sync, but providers can queue work.
```csharp
Task.Run(() => _sqlite.WriteLog(msg));
```
---

## 31. How do you use Serilog with ILogger?
```csharp
builder.Host.UseSerilog();
```
---

## 32. Can ILogger log formatted messages?
Yes.
```csharp
_logger.LogInformation("Order {OrderId} processed", id);
```
---

## 33. Can ILogger log to remote APIs?
Yes.
```csharp
await http.PostAsJsonAsync("/audit", logEntry);
```
---

## 34. Does EF Core use ILogger?
Yes.
```csharp
options.UseLoggerFactory(factory);
```
---

## 35. How do you capture performance metrics?
```csharp
var sw = Stopwatch.StartNew();
// work
_logger.LogInformation("Elapsed: {Ms}ms", sw.ElapsedMilliseconds);
```
---
