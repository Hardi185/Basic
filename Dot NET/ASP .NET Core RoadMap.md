# ASP.NET Core Project Guide

## 📑 Table of Contents
1. [Project Architecture](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Archietecture%20-%20Structure.md)
2. [Starting Point](#starting-point)
   - [.NET 5 Style (Program.cs + Startup.cs)](#1-starting-point-for-net-5-style)
   - [.NET 6+ Style (Program.cs Only)](#2--standard-net-6-project-only-programcs)
3. [Controllers](#controllers)
   - [MVC Controller Example](#mvc-controller-example)
   - [API Controller Example](#api-controller-example)
4. [Routing](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Routing%20in%20ASP%20.NET%20Core.md#-mvc-view-folder-structure)
   - MVC Routing
   - Web API Routing
   - MVC View Folder Structure
5. [Passing Data in ASP.NET Core](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Passing%20Data%20in%20ASP.NET%20Core.md)
   - ViewData
   - ViewBag
   - TempData
   - ViewModel
   - DTO (Data Transfer Object)
6. [3 Dependency Injection (DI) lifetimes in ASP.NET Core](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/3%20Dependency%20Injection%20(DI)%20lifetimes%20in%20ASP.NET%20Core.md#%EF%B8%8F-important-rule-interview-favorite)
   - AddTransient
   - AddScoped
   - AddSingleton
   - Quick Comparison Table
   - Important DI Rules
   - IConfiguration in ASP.NET Core
        - Is IConfiguration Default?
        - Who Creates IConfiguration?
        - Configuration Sources
        - Where is IConfiguration Stored?
        - How to Use IConfiguration
        - Do We Create IConfiguration?
        - Interview Answer
        - Mental Model
7. [Transactions & ACID properties](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Transactions%20%26%20ACID%20properties.md#do-we-actually-use-await-transactioncommitasync-and-await-transactionrollbackasync-after-net-6)
   -  What is a Transaction?
   -  Commit & Rollback
   -  ACID Properties
   -  Transactions in ADO.NET
   -  TransactionScope
   -  Entity Framework Transactions
   -  Version-wise Summary
   -  CommitAsync & RollbackAsync in .NET 6+
8. [ADO.NET vs Entity Framework](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/ADO.NET%20vs%20Entity%20Framework.md#2%EF%B8%8F%E2%83%A3-what-is-entity-framework-ef--ef-core)
    - What is ADO.NET?
    - What is Entity Framework?
    - Core Difference
    - Architecture View
    - Feature Comparison
    - Performance Difference
    - When to Use ADO.NET
    - When to Use EF Core
    - Using Both Together
    - Interview Answer
    - Simple Analogy
9. [Request Pipeline & Middleware](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Request%20Pipeline%20%26%20Middleware.md#4%EF%B8%8F%E2%83%A3-common-middleware--why-we-need-them)
    - What is the Request Pipeline?
    - Why Do We Need Middleware?
    - High-Level Flow
    - Common Middleware & Their Purpose
    - Order Matters
    - Short-Circuiting Example
    - Interview Answer
    - Mental Model
10. [Linq](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Linq.md)
    - Introduction
    - Key Concepts
    - LINQ Query Syntax
         1. Query Syntax
         2. Method Syntax
    - Common LINQ Methods
    - Example: LINQ with a List
    - LINQ to SQL
    - LINQ to XML
    - LINQ to Entities
    - LINQ Joins in C#
         1. Inner Join
         2. Left Outer Join
         3. Right Outer Join
         4. Full Outer Join
         5. Cross Join
    - Summary of Joins in LINQ
    - Advantages of LINQ
11. [Entity framework(EF)](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Entity%20framework(EF).md)
    - Introduction
    - Key Features of Entity Framework
         1. Object-Relational Mapping (ORM)
         2. Database-First, Code-First, and Model-First Approaches
         3. LINQ Integration
         4. Change Tracking
         5. Migrations
         6. Database Provider Support
    - Benefits of Entity Framework
         1. Productivity
         2. Maintainability
         3. Cross-Database Compatibility
         4. Strongly Typed Queries
         5. Ease of Use
    - How Entity Framework Works
    - Approaches in Entity Framework
         1. Database-First
         2. Code-First
         3. Model-First
12. [Async Await](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/async_await.md)


---

## Starting Point

### 1. Starting Point for (.NET 5 style):
```yaml
Program.cs (Main) --> builds Host --> Startup.cs (ConfigureServices & Configure) --> HTTP Pipeline starts
```

### Program.cs (.NET 5 style):
```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

namespace MyProject
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run(); // Entry point
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>(); // Connect Startup class
                });
    }
}
```

✅ **Explanation:**

- `Main()` is the actual starting point of the app.
- `CreateHostBuilder()` sets up the web host.
- `UseStartup<Startup>()` tells ASP.NET Core to use Startup.cs for further configuration.

### Startup.cs (.NET 5 style):
```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;

namespace MyProject
{
    public class Startup
    {
        public IConfiguration Configuration { get; }

        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        // Add services (DI, MVC, DB, etc.)
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllersWithViews();

            // Register DbContext
            services.AddDbContext<AppDbContext>(options =>
                options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

            // Register your service and repository for DI
            services.AddScoped<IProductRepository, ProductRepository>();
            services.AddScoped<IProductService, ProductService>();
        }

        // Configure HTTP request pipeline
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();
            app.UseAuthentication();
            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");

                endpoints.MapControllers(); // API endpoints
            });
        }
    }
}
```

#### `services.AddControllersWithViews()` → MVC + API Controller loading

**AddControllersWithViews() does three things:**

1. Registers all MVC controllers (controllers returning views) in your project.
2. Registers API controllers too (controllers with `[ApiController]` attribute that return JSON). This is why in a single project, you can have both API and Razor controllers.
3. Adds model binding, validation, routing, filters, etc.

So yes: by calling this, you are essentially registering all your controllers (MVC + API) with the DI container.

⚠️ **Alternative options:**
- If you only want API controllers, use `services.AddControllers()` instead.
- If you only want Razor Pages, use `services.AddRazorPages()`.

---

### 2. ✅ Standard .NET 6 Project (ONLY Program.cs)

📌 This is the ONLY file required in .NET 6.
```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// --------------------
// 1️⃣ Register Services (old ConfigureServices)
// --------------------
builder.Services.AddControllersWithViews(); // MVC + API
builder.Services.AddRazorPages();

// Example DI
// builder.Services.AddDbContext<AppDbContext>(options =>
//     options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// builder.Services.AddScoped<IProductRepository, ProductRepository>();
// builder.Services.AddScoped<IProductService, ProductService>();

var app = builder.Build();

// --------------------
// 2️⃣ Configure Middleware (old Configure)
// --------------------
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();

// --------------------
// 3️⃣ Map Endpoints
// --------------------
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.MapControllers();   // Web API
app.MapRazorPages();    // Razor Pages

// --------------------
app.Run();
```

### 📝 NOTE:
```yaml
appsettings.json contains data like db connection string and so on.
When in .NET 5 Host.CreateDefaultBuilder() and in .NET 6 WebApplication.CreateBuilder(args) loads these configs includes:
- appsettings.json
- appsettings.Development.json
- Env variables
- CLI args
```

---

## Controllers

### MVC Controller Example:
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewData["Message"] = "Hello from MVC!";
        return View();
    }
}

[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet("list")]
    public IActionResult List() { }
}
```

### API Controller Example:
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll()
    {
        var products = new List<string> { "Laptop", "Phone" };
        return Ok(products); // Returns JSON
    }
}
```

---
