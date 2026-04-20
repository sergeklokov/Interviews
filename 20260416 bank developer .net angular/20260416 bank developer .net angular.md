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

Blazor has matured significantly and can handle large enterprise applications, similar to how Angular scaled from small SPAs to full enterprise solutions.

### 4. Data Access Layer Choices

**Question:** How do you decide between Entity Framework Core, Dapper, or heavy use of stored procedures during modernization?

**Answer:** I generally prefer **Entity Framework Core** for its clean LINQ code and high developer productivity. **Dapper** is an excellent lightweight option, especially for smaller applications or when maximum performance is needed.

Stored procedures are still valuable for complex business logic or performance-critical paths, because EF Core can sometimes be noticeably slower (the exact difference depends on the query and indexing). Stored procedures can be called from EF Core or via raw ADO.NET when required. In one project, moving heavy logic into a stored procedure resolved long timeouts and deadlocks.

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

**Middleware in ASP.NET Core** Components that sit in the HTTP request pipeline. Yes, I have implemented custom middleware for cross-cutting concerns.

**Validation Approaches** I use a combination: **Data Annotations** for client-side/UI validation + server-side validation (e.g., checking database state).

**API Versioning** I use URL path versioning most often (e.g., /api/v1/... and /api/v2/...) to keep APIs stable for consumers.

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