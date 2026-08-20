
# Blazor Interview Questions 1–30

## 1. What is Blazor?
Blazor is a framework that enables developers to build client-side web applications using C#, Razor, and .NET instead of JavaScript.
```csharp
<h3>@message</h3>
@code {
    string message = "Hello from Blazor!";
}
```
---

## 2. What is a Razor component?
A Razor component is a `.razor` file containing C#, HTML markup, and event-handling logic.
```csharp
<!-- Counter.razor -->
<h3>Counter</h3>
<p>Value: @count</p>
<button @onclick="Increment">+</button>
@code {
    int count = 0;
    void Increment() => count++;
}
```
---

## 3. What is Blazor Server?
Blazor Server executes UI logic on the server and uses SignalR to update the browser.
```csharp
// Program.cs
builder.Services.AddServerSideBlazor();

// _Host.cshtml
<component type="typeof(App)" render-mode="ServerPrerendered" />
```
---

## 4. What is Blazor WebAssembly?
Blazor WebAssembly runs client-side using WebAssembly.
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
await builder.Build().RunAsync();
```
---

## 5. How do you bind data in Blazor?
Blazor supports one-way, event, and two-way data binding.
```csharp
<input @bind="username" />
<p>Hello, @username!</p>
@code {
    string username = "";
}
```
---

## 6. How do you handle button click events?
Use `@onclick` to bind methods.
```csharp
<button @onclick="ShowMessage">Click me</button>
@code {
    string message = "";
    void ShowMessage() { message = "Button clicked!"; }
}
```
---

## 7. How do you navigate between pages?
Use the `@page` directive and `NavigationManager`.
```csharp
@page "/home"
@inject NavigationManager Nav
<button @onclick="Go">Go to About</button>
@code {
    void Go() => Nav.NavigateTo("/about");
}
```
---

## 8. How do you inject services?
Use `@inject`.
```csharp
@inject TimeService TimeSvc
<p>The time is: @TimeSvc.Now</p>
```
---

## 9. How do you call JavaScript from Blazor?
Use `IJSRuntime`.
```javascript
function showAlert(message) { alert(message); }
```
```csharp
@inject IJSRuntime JS
<button @onclick="Show">Show Alert</button>
@code {
    async Task Show() {
        await JS.InvokeVoidAsync("showAlert", "Hello from JS");
    }
}
```
---

## 10. How do you call C# from JavaScript?
Use `[JSInvokable]`.
```javascript
function trigger(dotnetObj) {
    dotnetObj.invokeMethodAsync("CalledFromJs");
}
```
```csharp
@inject IJSRuntime JS
<button @onclick="Call">Call from JS</button>
@code {
    async Task Call() {
        var obj = DotNetObjectReference.Create(this);
        await JS.InvokeVoidAsync("trigger", obj);
    }

    [JSInvokable]
    public void CalledFromJs() {
        Console.WriteLine("JavaScript called .NET");
    }
}
```
---

## 11. How do you validate forms?
Use `EditForm`, model binding, and validation attributes.
```csharp
<EditForm Model="@person" OnValidSubmit="Save">
    <DataAnnotationsValidator />
    <ValidationSummary />
    <input @bind="person.Name" />
    <button type="submit">Save</button>
</EditForm>
@code {
    Person person = new();
    void Save() { Console.WriteLine(person.Name); }
}
public class Person { [Required] public string Name { get; set; } }
```
---

## 12. What is EditForm?
A Blazor component that handles forms and validation.
```csharp
<EditForm Model="@login" OnValidSubmit="Login">
    <InputText @bind-Value="login.Username" />
    <InputPassword @bind-Value="login.Password" />
    <button>Login</button>
</EditForm>
@code {
    Login login = new();
    void Login() { }
}
```
---

## 13. What are Cascading Parameters?
Shared data automatically passed down the component tree.
```csharp
<CascadingValue Value="Theme"><Child /></CascadingValue>
@code { string Theme = "Light"; }
```
```csharp
@code { [CascadingParameter] public string Theme { get; set; } }
```
---

## 14. How do you pass parameters?
Use `[Parameter]`.
```csharp
@code { [Parameter] public string Name { get; set; } }
```
```csharp
<Display Name="John" />
```
---

## 15. What is the component lifecycle?
Lifecycle includes init, parameter set, and render phases.
```csharp
@code {
    protected override void OnInitialized() {
        Console.WriteLine("Initialized");
    }
}
```
---

## 16. What is OnInitializedAsync?
An async lifecycle entry point.
```csharp
@inject HttpClient Http
@code {
    Weather[] forecasts;
    protected override async Task OnInitializedAsync() {
        forecasts = await Http.GetFromJsonAsync<Weather[]>("/api/weather");
    }
}
```
---

## 17. What is OnParametersSetAsync?
Runs when parameters change.
```csharp
@code {
    [Parameter] public int Id { get; set; }
    protected override Task OnParametersSetAsync() {
        Console.WriteLine($"Reloading {Id}");
        return Task.CompletedTask;
    }
}
```
---

## 18. What is StateHasChanged()?
Manually triggers a UI rerender.
```csharp
@code {
    int count;
    void ExternalUpdate() {
        count++;
        StateHasChanged();
    }
}
```
---

## 19. What is RenderFragment?
Represents a chunk of UI.
```csharp
@code {
    RenderFragment Content => b => b.AddContent(0, "Dynamic UI");
}
```
---

## 20. What are templated components?
Components that take `RenderFragment<T>`.
```csharp
@typeparam T
@foreach (var item in Items) { @Template(item) }
```
---

## 21. What is CSS isolation?
Styles scoped per component.
```css
/* Counter.razor.css */
button { color: red; }
```
---

## 22. What is JavaScript isolation?
Per-component JS via `.razor.js`.
```javascript
export function log(msg) { console.log(msg); }
```
```csharp
var module = await JS.InvokeAsync<IJSObjectReference>("import", "./Counter.razor.js");
await module.InvokeVoidAsync("log", "Hello");
```
---

## 23. How do you use HttpClient in Blazor WASM?
HttpClient uses browser fetch API.
```csharp
@inject HttpClient Http
@code {
    var data = await Http.GetFromJsonAsync<string[]>("/weather");
}
```
---

## 24. How do you use HttpClient in Blazor Server?
Register it manually.
```csharp
builder.Services.AddHttpClient();
```
---

## 25. What is SignalR?
A real-time connection library Blazor Server relies on.
```csharp
builder.Services.AddSignalR();
```
---

## 26. How does Blazor Server update UI?
Via UI diffs sent over SignalR.
```csharp
@code { int count; void Inc() => count++; }
```
---

## 27. What is _Imports.razor?
Shared namespaces for components.
```csharp
@using System.Net.Http
@using BlazorApp.Models
```
---

## 28. What are layout components?
Shared UI wrappers.
```csharp
@layout MainLayout
```
---

## 29. What is a partial class?
Splits logic from UI.
```csharp
public partial class Counter { int count; }
```
---

## 30. How do you share state?
Use a DI container.
```csharp
@inject AppState State
<p>Value: @State.Value</p>
<button @onclick="() => State.Value++">+</button>
```
---
