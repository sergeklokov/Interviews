
# Blazor Interview Questions 31–60

## 31. What are layout components?
Layout components define the shared structure (header, footer, navigation) used by pages.
```csharp
<!-- MainLayout.razor -->
<div class="layout">
    <NavMenu />
    <div class="content">
        @Body
    </div>
</div>
```
---

## 32. What is a code-behind file?
Code-behind files separate C# logic from Razor markup using `.razor.cs` partial class files.
```csharp
// Page.razor.cs
public partial class Page {
    int counter = 0;
    void Increment() => counter++;
}
```
---

## 33. What is a partial class in Blazor?
Partial classes allow splitting component logic across multiple files.
```csharp
// MyComponent.razor.cs
public partial class MyComponent {
    int value = 10;
}
```
---

## 34. How do you share state between components?
Use a shared service registered as scoped.
```csharp
// AppState.cs
public class AppState { public int Count { get; set; } }
```
```csharp
@inject AppState State
<p>Count: @State.Count</p>
<button @onclick="() => State.Count++">+</button>
```
---

## 35. What is a state container?
A state container holds shared UI state and triggers updates.
```csharp
public class CounterState {
    public int Value { get; private set; }
    public event Action OnChange;
    public void Increment() { Value++; OnChange?.Invoke(); }
}
```
---

## 36. What is Virtualize in Blazor?
Virtualize renders only visible items in large lists for performance.
```csharp
<Virtualize Items="items" ItemSize="50">
    <ItemContent>
        @(context => <p>@context</p>)
    </ItemContent>
</Virtualize>
```
---

## 37. How do you optimize render performance?
Avoid unnecessary re-renders by using `ShouldRender`.
```csharp
@code {
    protected override bool ShouldRender() => shouldRenderFlag;
}
```
---

## 38. What is prerendering?
Prerendering renders components on the server before sending HTML to the client.
```csharp
<component type="typeof(App)" render-mode="ServerPrerendered" />
```
---

## 39. How do you use ErrorBoundary?
Wrap UI in `ErrorBoundary` to catch errors.
```csharp
<ErrorBoundary>
    <ChildComponent />
</ErrorBoundary>
```
---

## 40. How do you create a custom component?
Define a Razor file and expose parameters.
```csharp
<!-- CustomCard.razor -->
<div class="card">
    <h3>@Title</h3>
    @ChildContent
</div>
@code {
    [Parameter] public string Title { get; set; }
    [Parameter] public RenderFragment ChildContent { get; set; }
}
```
---

## 41. How do you create a reusable component library?
Create a Razor Class Library project and place components inside it.
```csharp
// In RCL project
<MySharedButton Text="Click" />
```
---

## 42. How do you upload files in Blazor?
Use `InputFile` component.
```csharp
<InputFile OnChange="Upload" />
@code {
    async Task Upload(InputFileChangeEventArgs e) {
        var file = e.File;
        using var stream = file.OpenReadStream();
    }
}
```
---

## 43. How do you download files in Blazor?
Use JavaScript to trigger browser download.
```javascript
// download.js
export function downloadFile(fileName, content) {
    const blob = new Blob([content]);
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = fileName;
    a.click();
}
```
```csharp
var module = await JS.InvokeAsync<IJSObjectReference>("import", "./download.js");
await module.InvokeVoidAsync("downloadFile", "test.txt", "Hello world");
```
---

## 44. How do you integrate JavaScript libraries?
Import modules using JS interop.
```csharp
var module = await JS.InvokeAsync<IJSObjectReference>("import", "./charts.js");
await module.InvokeVoidAsync("renderChart", data);
```
---

## 45. How do you manipulate DOM in Blazor?
Use JS interop to access DOM APIs.
```javascript
export function setFocus(id) {
    document.getElementById(id).focus();
}
```
```csharp
await JS.InvokeVoidAsync("setFocus", "inputBox");
```
---

## 46. How do you use localStorage?
Use JS interop to store values.
```javascript
localStorage.setItem('theme', 'dark');
```
```csharp
await JS.InvokeVoidAsync("localStorage.setItem", "theme", "dark");
```
---

## 47. How do you use sessionStorage?
Use JS interop.
```csharp
await JS.InvokeVoidAsync("sessionStorage.setItem", "name", "John");
```
---

## 48. How do you authenticate in Blazor WASM?
Use `AuthenticationStateProvider` with tokens.
```csharp
builder.Services.AddAuthorizationCore();
```
---

## 49. How do you authorize users?
Use `AuthorizeView`.
```csharp
<AuthorizeView>
    <p>Authorized content</p>
</AuthorizeView>
```
---

## 50. What is AuthenticationStateProvider?
Provides the current user's authentication state.
```csharp
public override Task<AuthenticationState> GetAuthenticationStateAsync() { }
```
---

## 51. How do you extend AuthenticationStateProvider?
Override authentication logic.
```csharp
public class CustomAuth : AuthenticationStateProvider {
    public override Task<AuthenticationState> GetAuthenticationStateAsync() { }
}
```
---

## 52. How do you refresh tokens?
Call API to renew access tokens.
```csharp
var newToken = await Http.PostAsync("/refresh", null);
```
---

## 53. What is a Blazor PWA?
Blazor WASM app with offline capabilities.
```json
{
  "short_name": "BlazorApp",
  "display": "standalone"
}
```
---

## 54. How do you generate a service worker?
Use PWA template.
```javascript
self.addEventListener('fetch', event => {});
```
---

## 55. How do you use caching in PWAs?
Use Cache API.
```javascript
caches.open('v1').then(cache => cache.put('/', response));
```
---

## 56. How do you call REST APIs?
Use HttpClient.
```csharp
var data = await Http.GetFromJsonAsync<string[]>("/api/data");
```
---

## 57. How do you deserialize JSON?
Use `GetFromJsonAsync`.
```csharp
var item = await Http.GetFromJsonAsync<MyItem>("api/item");
```
---

## 58. How do you use EF Core in Blazor Server?
Inject DbContext.
```csharp
@inject AppDbContext Db
@code {
    var users = Db.Users.ToList();
}
```
---

## 59. How do you use SQL with Blazor?
Use a backend API or EF Core.
```csharp
var rows = await Db.Table.ToListAsync();
```
---

## 60. How do you build custom input components?
Inherit from `InputBase<T>`."
```csharp
public class MyInput : InputBase<string> {
    protected override bool TryParseValueFromString(string value, out string result, out string error) {
        result = value;
        error = null;
        return true;
    }
}
```
---
