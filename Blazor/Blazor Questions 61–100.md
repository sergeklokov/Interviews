
# Blazor Interview Questions 61–100

## 61. How do you use EventCallback?
EventCallback lets child components notify parents.
```csharp
<!-- Child.razor -->
<button @onclick="() => OnClick.InvokeAsync()">Click</button>
@code {
    [Parameter] public EventCallback OnClick { get; set; }
}
```
---

## 62. How do child components send data to parents?
Use `EventCallback<T>`.
```csharp
<!-- Child.razor -->
<button @onclick="() => OnSend.InvokeAsync(42)">Send Value</button>
@code {
    [Parameter] public EventCallback<int> OnSend { get; set; }
}
```
---

## 63. How do you log errors in Blazor?
Use `ILogger`.
```csharp
@inject ILogger<Example> Log
@code {
    void DoWork() {
        try { throw new Exception(); }
        catch(Exception ex) { Log.LogError(ex, "Error occurred"); }
    }
}
```
---

## 64. How do you use ILogger?
Inject and call logging methods.
```csharp
Log.LogInformation("User clicked button");
```
---

## 65. What is a scoped service?
Scoped service lives for the duration of a user session.
```csharp
builder.Services.AddScoped<AppState>();
```
---

## 66. What is a transient service?
Created fresh each time it's injected.
```csharp
builder.Services.AddTransient<HelperService>();
```
---

## 67. How do you dispose components?
Implement `IDisposable`.
```csharp
public void Dispose() {
    timer?.Dispose();
}
```
---

## 68. How do you cancel async tasks?
Use `CancellationToken`.
```csharp
var token = cts.Token;
await Http.GetAsync("/api/data", token);
```
---

## 69. How do you detect navigation events?
Subscribe to `LocationChanged`.
```csharp
@inject NavigationManager Nav
@code {
    protected override void OnInitialized() {
        Nav.LocationChanged += (s,e) => Console.WriteLine(e.Location);
    }
}
```
---

## 70. What is NavigationLock?
Prevents accidental navigation.
```csharp
<NavigationLock OnBeforeInternalNavigation="Confirm" />
```
---

## 71. How do you optimize Blazor WASM performance?
Use linker trimming.
```json
"PublishTrimmed": true
```
---

## 72. How do you lazy load assemblies?
Mark assemblies with `LazyLoad`.
```json
"_framework": { "lazyAssembly": ["MyLib.dll"] }
```
---

## 73. How do you reduce WASM download size?
Enable compression.
```csharp
app.UseResponseCompression();
```
---

## 74. How do you use gRPC-Web?
Configure gRPC-Web service.
```csharp
app.UseGrpcWeb();
app.MapGrpcService<MyService>().EnableGrpcWeb();
```
---

## 75. How do you debug Blazor WASM?
Use browser dev tools.
```text
Press F12 → Sources → .NET assemblies
```
---

## 76. How do you debug Blazor Server?
Use Visual Studio debugger.
```text
Debug → Attach to Process → dotnet
```
---

## 77. How do you handle concurrency?
Use `lock` or state containers.
```csharp
lock(syncObj) { counter++; }
```
---

## 78. How do you secure API calls?
Use authorization token in HTTP headers.
```csharp
Http.DefaultRequestHeaders.Authorization = new("Bearer", token);
```
---

## 79. How do you store sensitive configuration?
Use Azure Key Vault.
```csharp
builder.Configuration.AddAzureKeyVault(...);
```
---

## 80. How do you build multi-tenant Blazor apps?
Inject tenant provider.
```csharp
@inject ITenantProvider Tenant
```
---

## 81. How do you build Blazor component libraries?
Use Razor Class Library.
```csharp
<MySharedUI.Button Text="Click" />
```
---

## 82. How do you use RenderTreeBuilder?
Manually construct UI.
```csharp
builder.AddContent(0, "Hello");
```
---

## 83. How do you implement advanced routing?
Use route constraints.
```csharp
@page "/order/{id:int}"
```
---

## 84. How do you structure large Blazor apps?
Use feature folders.
```text
/Features/Orders/OrderPage.razor
```
---

## 85. How do you modularize Blazor?
Create separate project modules.
```text
Modules/Product/ProductModule.cs
```
---

## 86. How do you integrate Azure services?
Inject Azure SDK clients.
```csharp
@inject BlobServiceClient Blob
```
---

## 87. How do you use Azure AD with Blazor?
Configure MSAL.
```csharp
builder.Services.AddMsalAuthentication(options => {});
```
---

## 88. How do you tune SignalR performance?
Configure hub options.
```csharp
builder.Services.AddSignalR(o => o.MaximumReceiveMessageSize = 102400);
```
---

## 89. How do you virtualize large tables?
Use `Virtualize`.
```csharp
<Virtualize Items="rows" ItemSize="40"></Virtualize>
```
---

## 90. How do you paginate data?
Request data in pages.
```csharp
var page = await Http.GetFromJsonAsync<Page>("/api/users?page=1");
```
---

## 91. How do you use third-party UI libraries?
Install package and use components.
```csharp
<MudButton Color="Color.Primary">Click</MudButton>
```
---

## 92. How do you publish Blazor WASM?
Use `dotnet publish`.
```bash
dotnet publish -c Release
```
---

## 93. How do you publish Blazor Server?
Deploy to Azure App Service.
```text
Publish → Azure → App Service
```
---

## 94. How do you optimize release builds?
Enable trimming.
```json
"PublishTrimmed": true
```
---

## 95. How do you handle large JSON payloads?
Stream responses.
```csharp
await JsonSerializer.DeserializeAsync(stream);
```
---

## 96. How do you manage global state?
Use a state management service.
```csharp
public class GlobalState { public string Theme { get; set; } }
```
---

## 97. How do you prevent UI freezing?
Use async calls.
```csharp
await Task.Yield();
```
---

## 98. How do you trigger UI updates from external events?
Call `InvokeAsync(StateHasChanged)`.
```csharp
InvokeAsync(StateHasChanged);
```
---

## 99. How do you handle multiple API calls?
Use `Task.WhenAll`.
```csharp
var results = await Task.WhenAll(call1, call2);
```
---

## 100. How do you organize reusable UI?
Create shared UI library.
```text
/SharedComponents/Button.razor
```
---
