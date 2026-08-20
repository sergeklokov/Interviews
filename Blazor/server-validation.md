
# Server-Side Validation in Blazor — Questions, Answers, and Code Examples

---

## 1. How do you perform server-side validation in Blazor when validation requires database checks?
**Answer:** Server-side validation is used when Blazor cannot validate input values on the client due to complexity or dependency on data stored in a database. The Blazor client sends the user input to an API endpoint that validates the data on the server. The API then returns validation errors or success. Blazor displays server validation messages manually.

### Code Example — Validate Email Exists in Database

**Server-side (API):**
```csharp
[HttpPost("register")]
public async Task<IActionResult> Register(UserModel model, MyDbContext db)
{
    if (await db.Users.AnyAsync(u => u.Email == model.Email))
        ModelState.AddModelError("Email", "Email already exists");

    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    db.Users.Add(new User { Email = model.Email });
    await db.SaveChangesAsync();

    return Ok(new { Success = true });
}

public class UserModel
{
    [Required]
    public string Email { get; set; }
}
```

**Blazor component:**
```csharp
<EditForm Model="@user" OnValidSubmit="Submit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText @bind-Value="user.Email" />
    <button>Register</button>

    @if (!string.IsNullOrEmpty(ServerError))
    {
        <p style="color:red">@ServerError</p>
    }
</EditForm>

@code {
    UserModel user = new();
    string ServerError;

    async Task Submit()
    {
        var response = await Http.PostAsJsonAsync("register", user);

        if (!response.IsSuccessStatusCode)
        {
            ServerError = "Email already exists.";
        }
    }
}
```

---

## 2. How do you validate fields requiring complex calculations that cannot be done on the client?
**Answer:** Complex business logic often cannot be performed on the client (e.g., financial formulas, risk scoring, tax engines). Blazor sends the input values to the backend API. The server runs the required business rules and returns validation status or calculated results.

### Code Example — Validating Tax Calculation on Server

**Server-side:**
```csharp
[HttpPost("validate-tax")]
public IActionResult ValidateTax(CalcModel model)
{
    if (model.Amount <= 0)
        ModelState.AddModelError("Amount", "Amount must be greater than zero.");

    var tax = ComplexTaxEngine.Compute(model.Amount);

    if (tax < 0)
        ModelState.AddModelError("Tax", "Invalid tax calculation.");

    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    return Ok(new { Tax = tax });
}

public class CalcModel
{
    public decimal Amount { get; set; }
}
```

**Blazor component:**
```csharp
<EditForm Model="@calc" OnValidSubmit="Calculate">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputNumber @bind-Value="calc.Amount" />
    <button>Calculate Tax</button>

    @if (!string.IsNullOrEmpty(ServerError))
    {
        <p style="color:red">@ServerError</p>
    }

    @if (Tax > 0)
    {
        <p>Tax: @Tax</p>
    }
</EditForm>

@code {
    CalcModel calc = new();
    decimal Tax;
    string ServerError;

    async Task Calculate()
    {
        var res = await Http.PostAsJsonAsync("validate-tax", calc);

        if (!res.IsSuccessStatusCode)
        {
            ServerError = "Server validation failed.";
            return;
        }

        var body = await res.Content.ReadFromJsonAsync<dynamic>();
        Tax = (decimal)body.Tax;
    }
}
```

---

## 3. How do you validate input using external APIs or business rule engines?
**Answer:** When validation requires external systems (credit score API, ERP rules, inventory check), the server performs the validation by calling the external API, applying business rules, and returning validation messages to Blazor.

### Code Example — External Credit Score Validation

**Server-side:**
```csharp
[HttpPost("validate-credit")]
public async Task<IActionResult> ValidateCredit(CreditCheckModel model, ICreditApi creditApi)
{
    var score = await creditApi.GetCreditScore(model.SSN);

    if (score < 600)
        ModelState.AddModelError("CreditScore", "Credit score is too low.");

    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    return Ok(new { Score = score });
}

public class CreditCheckModel
{
    public string SSN { get; set; }
}
```

**Blazor component:**
```csharp
<EditForm Model="@credit" OnValidSubmit="Check">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText @bind-Value="credit.SSN" />
    <button>Check</button>

    @if (!string.IsNullOrEmpty(ServerError))
    {
        <p style="color:red">@ServerError</p>
    }

    @if (Score > 0)
    {
        <p>Your score: @Score</p>
    }
</EditForm>

@code {
    CreditCheckModel credit = new();
    string ServerError = "";
    int Score = 0;

    async Task Check()
    {
        var res = await Http.PostAsJsonAsync("validate-credit", credit);
        if (!res.IsSuccessStatusCode)
        {
            ServerError = "Credit score too low for approval.";
            return;
        }

        var data = await res.Content.ReadFromJsonAsync<dynamic>();
        Score = (int)data.Score;
    }
}
```

---

## 4. How do you return detailed server-side validation errors to Blazor?
**Answer:** ASP.NET Core returns model validation errors as `ValidationProblemDetails`. Blazor reads the error dictionary from the response and displays the errors.

### Code Example — Returning Structured Errors

**Server-side:**
```csharp
[HttpPost("validate")]
public IActionResult Validate(UserInput input)
{
    if (input.Age < 18)
        ModelState.AddModelError("Age", "User must be at least 18.");

    if (!ModelState.IsValid)
        return ValidationProblem(ModelState);

    return Ok();
}
```

**Blazor component:**
```csharp
@code {
    Dictionary<string, string[]> Errors;
    UserInput user = new();

    async Task Submit()
    {
        var response = await Http.PostAsJsonAsync("validate", user);

        if (response.StatusCode == HttpStatusCode.BadRequest)
        {
            Errors = await response.Content.ReadFromJsonAsync<Dictionary<string, string[]>>();
        }
    }
}

@if (Errors != null)
{
    foreach (var err in Errors)
    {
        <p style="color:red">@err.Key: @string.Join(", ", err.Value)</p>
    }
}
```

---

## 5. How do you validate values based on database business rules?
**Answer:** Complex business rules stored in the database (like discount limits, inventory thresholds) must be validated server-side. Blazor submits the data to an API which loads the rules from the DB and returns validation results.

### Code Example — Discount Validation Based on Business Rules

**Server-side:**
```csharp
[HttpPost("validate-discount")]
public async Task<IActionResult> ValidateDiscount(DiscountModel model, RulesDbContext db)
{
    var rule = await db.DiscountRules.FindAsync(model.ProductId);

    if (model.Amount > rule.MaxDiscount)
        ModelState.AddModelError("Amount", $"Discount cannot exceed {rule.MaxDiscount}%");

    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    return Ok(new { Approved = true });
}

public class DiscountModel {
    public int ProductId { get; set; }
    public int Amount { get; set; }
}
```

**Blazor component:**
```csharp
<EditForm Model="@discount" OnValidSubmit="Validate">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputNumber @bind-Value="discount.Amount" />
    <button>Apply Discount</button>

    @if (!string.IsNullOrEmpty(Error))
    {
        <p style="color:red">@Error</p>
    }
</EditForm>

@code {
    DiscountModel discount = new();
    string Error;

    async Task Validate()
    {
        var res = await Http.PostAsJsonAsync("validate-discount", discount);
        if (!res.IsSuccessStatusCode)
        {
            Error = "Discount exceeds allowed limit.";
        }
    }
}
```

---

