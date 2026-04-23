**Interview Q&A Summary – Sergiy** **Modernizing Legacy .NET Applications** **Date:** April 2026

### 1. Legacy .NET Framework Experience

**Question:** Tell me about your experience with legacy .NET Framework, especially since we still support and modernize legacy applications.

**Answer:** I have extensive hands-on experience with the full .NET Framework — from the earliest versions through Visual Studio 2022. I heavily used .NET 4.7.2 and strongly recommend upgrading to **4.8.1** immediately if still on it. For the past several years I have worked with modern .NET versions up to .NET 8, and I have also experimented with .NET 9 in personal projects. Many organizations still need these upgrades because of Windows 11 requirements and older framework end-of-support dates.

### 2. ASP.NET Web Forms Experience

**Question:** Have you worked with ASP.NET Web Forms (ASMX/ASPX) in monolithic applications? We have some 25-year-old apps built this way.

**Answer:** Yes, I worked extensively with Web Forms and .aspx pages. They are suitable for simple form-based applications, but they tend to become very difficult to maintain when developers add many UpdatePanels and complex postback logic. I much prefer **MVC**, **Razor Pages**, or **Blazor** for more complex applications. I am also very comfortable with JavaScript and jQuery, which are commonly used in these legacy systems, as well as modern Angular.

### 3. Modernization & Migration Strategies

**Question:** When migrating legacy Web Forms applications (there is no direct Microsoft upgrade wizard), what strategies and architectural decisions have you used?

**Answer:** The typical approach is a **manual rewrite** since reliable automatic upgrade tools do not exist for Web Forms.

Key decisions usually include:

- Keep the existing database (with possible normalization improvements or splitting large tables).
- Replace legacy APIs (such as WCF) with modern REST APIs.
- Use the old application as a functional prototype or technical specification for the new UI.
- Front-end technology is a team decision. Common choices are Angular (very popular for upgrades), Blazor (capable of large-scale apps with proper lazy loading), or updated MVC/Razor Pages.

In more details:  
When migrating legacy Web Forms applications, I typically use an incremental approach. I start by extracting business logic from code‑behind into services or APIs, so both the legacy and new platforms can share the same backend. Then I migrate the UI feature by feature, usually to MVC, Razor Pages, or ASP.NET Core, while reducing ViewState and session dependencies. This allows parallel running, minimizes risk, and avoids a big‑bang rewrite.
For the database, I usually keep it initially and avoid big schema changes early. I add a data access layer or repositories and expose data through services/APIs, so both the legacy Web Forms app and the new platform use the same database safely. Schema refactoring, performance tuning, or splitting databases is done incrementally, once the new application is stable and usage patterns are well understood.

Blazor has matured significantly and can handle large enterprise applications, similar to how Angular scaled from small SPAs to full enterprise solutions.

[Strangler Fig pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig)

#### Modern Technologies for Rewriting Legacy ASP.NET Web Forms Applications

Below are **several concrete examples of modern technologies** commonly used to **rewrite or modernize legacy ASP.NET Web Forms (ASPX/ASMX) monolithic applications**, especially when applying an incremental approach such as the **Strangler Fig pattern**.

---

#### Backend / API Layer

- **ASP.NET Core (.NET 8 / LTS)**  
  Modern, high‑performance, cross‑platform framework for building REST APIs and backend services. A natural replacement for ASMX services and Web Forms code‑behind logic.

- **Minimal APIs**  
  A lightweight ASP.NET Core approach for quickly exposing endpoints with minimal boilerplate—ideal for carving out small pieces of legacy functionality.

- **gRPC (for internal services)**  
  High‑performance, strongly typed service‑to‑service communication, useful after extracting parts of the monolith into independent services.

---

#### Frontend / UI

- **Angular**  
  Common enterprise choice:
  - Strong typing with TypeScript  
  - Mature tooling and long‑term support  
  - Works well side‑by‑side with Web Forms during transition

- **React**  
  Often used for incremental UI replacement (widget‑by‑widget) embedded into existing ASPX pages.

- **Blazor (Server or WebAssembly)**  
  Good fit for teams wanting to stay mostly in C#:
  - Blazor Server for faster migrations  
  - Blazor WebAssembly for rich client‑side experiences

---

#### Integration & Legacy Coexistence

- **ASP.NET Core API Gateway**  
  Acts as a façade in front of legacy ASMX services while routing new functionality to modern services—classic Strangler Fig entry point.

- **YARP (Yet Another Reverse Proxy) or Azure API Management**  
  Transparently routes traffic between legacy Web Forms endpoints and new services.

---

#### Data & Persistence

- **Entity Framework Core**  
  Modern ORM replacing older ADO.NET or NHibernate usage.

- **Dapper**  
  Lightweight micro‑ORM for performance‑critical paths or when working with complex legacy schemas.

- **Database‑per‑service (gradual)**  
  Over time, extracted services own their own schemas instead of sharing a monolithic database.

---

#### Authentication & Security

- **Azure AD / Entra ID**  
  Enterprise identity platform using modern OpenID Connect and OAuth 2.0 standards.

- **JWT & OAuth 2.0**  
  Standard approach for securing APIs and SPAs such as Angular applications.

---

#### Infrastructure & DevOps

- **Docker**  
  Containerizes newly extracted services without requiring a full rewrite.

- **Kubernetes or Azure App Service**  
  Enables independent deployment and scaling of modern services.

- **CI/CD (Azure DevOps or GitHub Actions)**  
  Supports frequent, low‑risk releases—often a key benefit over legacy Web Forms deployments.

---

#### Typical Target Architecture

### 4. Data Access Layer Choices

**Question:** How do you decide between Entity Framework Core, Dapper, or heavy use of stored procedures during modernization?

**Answer:** I generally prefer **Entity Framework Core** for its clean LINQ code and high developer productivity. **Dapper** is an excellent lightweight option, especially for smaller applications or when maximum performance is needed.

Stored procedures are still valuable for complex business logic or performance-critical paths, because EF Core can sometimes be noticeably slower (the exact difference depends on the query and indexing). Stored procedures can be called from EF Core or via raw ADO.NET when required. In one project, moving heavy logic into a stored procedure resolved long timeouts and deadlocks.

#### How I decide between EF Core, Dapper, and stored procedures

In modernization projects, the goal isn’t to pick a single data‑access strategy—it’s to **use each where it fits best**.

- **Entity Framework Core**  
  Best for **new or refactored business logic** where maintainability matters. It works well for write‑heavy workflows, domain modeling, and services extracted using the **Strangler Fig pattern**. EF Core keeps business rules in C#, simplifies schema evolution, and improves long‑term clarity.

- **Dapper**  
  Ideal for **performance‑critical or complex read queries**, reporting, and working with legacy schemas. It provides full SQL control with minimal overhead and predictable performance.

- **Stored Procedures**  
  Still valuable where they already provide real benefits—shared database contracts, highly tuned queries, or regulated environments. Treat them as **integration boundaries**, not the default for new logic.

**Typical successful mix:**

#### Calling a SQL Server stored procedure from EF Core (single example)

```csharp
// DTO matching the stored procedure result
public class OrderSummary
{
    public int OrderId { get; set; }
    public decimal Total { get; set; }
}

// Call the stored procedure
var orders = await context.Set<OrderSummary>()
    .FromSqlRaw(
        "EXEC dbo.GetOrderSummaries @CustomerId",
        new SqlParameter("@CustomerId", customerId)
    )
    .AsNoTracking()
    .ToListAsync();
```

### 5. Database Performance Troubleshooting

**Question:** How do you diagnose and resolve database performance issues?

**Answer:** I start in **SQL Server Management Studio (SSMS)** by:

- Reviewing slow queries for type conversions in JOINs, missing indexes, or table scans (using execution plans).
- Adding or rebuilding indexes as needed.
- Breaking down very large SQL statements for easier debugging.

Common legacy problems include overly shared tables causing deadlocks, insufficient indexing as data volume grows, and read queries that lock tables unnecessarily. Using NOLOCK hints (with caution) or adjusting transaction isolation levels can help in read-heavy scenarios.

**Clustered vs Non-clustered Index** A **clustered index** defines the physical order of rows in the table (only one allowed per table, usually on the primary key). A **non-clustered index** is a separate structure with pointers to the data and can have multiple per table.

**Table without clustered index** It is called a **heap**. Heaps can cause space reclamation issues after large deletes in some SQL Server versions.

### 6. Rapid-Fire Technical Questions

**Async / Await in C#** Used to write non-blocking asynchronous code so the thread is not blocked while waiting for I/O operations (very common in UI and backend services).  

```csharp
public async Task<string> GetDataAsync()
{
    // Simulate async work (e.g., I/O or HTTP call)
    await Task.Delay(1000);
    return "Hello from async method";
}

public async Task ExampleAsync()
{
    string result = await GetDataAsync();
    Console.WriteLine(result);
}
```

**Middleware in ASP.NET Core** Components that sit in the HTTP request pipeline. Yes, I have implemented custom middleware for cross-cutting concerns.  
`Mental model: ` Middleware = filters for HTTP requests, executed in order, forming a pipeline.

```csharp
app.Use(async (context, next) =>
{
    Console.WriteLine("Before request");

    await next(); // call next middleware

    Console.WriteLine("After request");
});
```

Built‑in middleware examples
```csharp
app.UseAuthentication();
app.UseAuthorization();
app.UseRouting();
```

**Validation Approaches** I use a combination: **Data Annotations** for client-side/UI validation + server-side validation (e.g., checking database state).

```csharp
[HttpPost]
public IActionResult CreateUser(UserRequest request)
{
    if (request.Age < 18 || request.EducationLevel != "HighSchool")
    {
        ModelState.AddModelError(
            string.Empty,
            "Age or education level is invalid."
        );
    }

    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok();
}
```

#### Validation in Angular

#### Angular Validation Examples (Reactive Forms)

Below are **both common Angular validation examples** so you can copy them together:
1. **Field‑level validation**
2. **Cross‑field (combo) validation**

---

#### 1️⃣ Field‑Level Validation (single field rules)

Component (TypeScript)
```ts
import { FormBuilder, Validators } from '@angular/forms';

constructor(private fb: FormBuilder) {}

userForm = this.fb.group({
  email: ['', [Validators.required, Validators.email]],
  age: [null, [Validators.required, Validators.min(18)]]
});
```

Template(HTML)
```html
<input formControlName="email" placeholder="Email">
<div *ngIf="userForm.controls.email.invalid && userForm.controls.email.touched">
  Invalid email
</div>

<input type="number" formControlName="age" placeholder="Age">
<div *ngIf="userForm.controls.age.invalid && userForm.controls.age.touched">
  Age must be 18 or older
</div>

<button [disabled]="userForm.invalid">Submit</button>
```

**API Versioning** I use URL path versioning most often (e.g., /api/v1/... and /api/v2/...) to keep APIs stable for consumers.

dotnet add package Microsoft.AspNetCore.Mvc.Versioning

```csharp
builder.Services.AddApiVersioning(options =>
{
    options.DefaultApiVersion = new ApiVersion(1, 0);
    options.AssumeDefaultVersionWhenUnspecified = true;
    options.ReportApiVersions = true;
});

[ApiController]
[ApiVersion("1.0")]
[Route("api/v{version:apiVersion}/users")]
public class UsersV1Controller : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok(new { Name = "John", Version = "v1" });
    }
}
```


**Calling Stored Procedures in EF Core** Yes, supported via FromSqlInterpolated or ExecuteSqlInterpolated.

**Explicit Transactions in EF Core** Yes, using context.Database.BeginTransaction() inside a using block, followed by Commit().

**Secure Web API** Primarily **JWT (JSON Web Tokens)**. I have also used Windows-based authentication in corporate environments.

**Server-side Pagination** Handled in SQL with OFFSET / FETCH or by passing page parameters to stored procedures. Front-end frameworks provide UI components.

**Two-way Data Binding in Angular** Data flows automatically between the UI and the component model using [(ngModel)] (banana-in-a-box syntax).

**Microservices vs Monolithic** Microservices run as small, independent services (usually in containers), offering better scalability and lower resource usage when idle compared to a single monolithic application.

**CI/CD & DevOps** I have used **GitHub + Azure DevOps** pipelines with feature branches, pull requests, automated builds, tests, and quality gates (SonarQube, security scans, etc.).

**Debugging Multiple API Calls in Angular** Reproduce in browser developer tools with source maps enabled, then trace service calls and component lifecycle.

**Typical Production Bug** Null reference exceptions are very common. I reproduce the issue, add proper null checks, provide user-friendly messages, and add logging.

**Authentication & Authorization** JWT tokens for APIs + role-based authorization using [Authorize(Roles = "...")] attributes on the backend and route guards in Angular.

**Read works but Write fails after deployment** Common after .NET upgrades: connection string issues, read-only database user permissions, or driver/connection pool problems. Debug the write path specifically and test locally.
