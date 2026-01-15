# ASP.NET Core Project Guide

## 📑 Table of Contents
1. [Project Architecture](#project-architecture)
2. [Starting Point](#starting-point)
   - [.NET 5 Style (Program.cs + Startup.cs)](#1-starting-point-for-net-5-style)
   - [.NET 6+ Style (Program.cs Only)](#2--standard-net-6-project-only-programcs)
3. [Controllers](#controllers)
   - [MVC Controller Example](#mvc-controller-example)
   - [API Controller Example](#api-controller-example)
4. [Routing](#routing)

---

## Project Architecture

### Project Structure (Single project approach):
```yaml
MyProject/
│
├─ Controllers/        # MVC & API controllers
│   ├─ HomeController.cs
│   └─ ProductController.cs
│
├─ Models/             # Data models
│   └─ Product.cs
│
├─ Views/              # Razor views for MVC
│   └─ Home/
│       └─ Index.cshtml
│
├─ wwwroot/            # Static files (CSS, JS, images)
│
├─ appsettings.json    # Configuration
├─ Program.cs          # App entry point
├─ Startup.cs          # Configure services & middleware
└─ MyProject.csproj
```

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

## Routing

📌 **How routing works in .NET 5 and above**

### 1️⃣ What is Routing in ASP.NET Core?

**Routing = Mapping a URL to code**

It decides which controller & action (or endpoint) should handle an incoming HTTP request.

**Example:**
```
/products/details/5
```
➡️ Controller: `ProductsController`  
➡️ Action: `Details`  
➡️ Parameter: `id = 5`

### 2️⃣ Types of Routing in ASP.NET Core

ASP.NET Core supports two routing styles (for BOTH MVC & Web API):

1. **Conventional Routing**
2. **Attribute Routing** ✅ (most common for Web API)

### 3️⃣ Routing in MVC (View-based)

MVC returns Views (HTML pages).

#### A) Conventional Routing (Most common in MVC)

Configured in `Program.cs`:
```csharp
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

**How it works:**

| URL | Maps to |
|-----|---------|
| `/` | `HomeController` → `Index` |
| `/Home/About` | `HomeController` → `About` |
| `/Product/Details/10` | `ProductController` → `Details(10)` |

**MVC Controller Example:**
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }

    public IActionResult About()
    {
        return View();
    }
}
```

#### B) Attribute Routing in MVC

You can also use attributes:
```csharp
[Route("products")]
public class ProductController : Controller
{
    [Route("list")]
    public IActionResult List()
    {
        return View();
    }
}
```

➡️ URL: `/products/list`

### 4️⃣ Routing in Web API (Data-based)

Web API returns JSON / XML, not views.

👉 **Attribute Routing is preferred**

#### A) Attribute Routing in Web API (MOST USED)

**Controller:**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll()
    {
        return Ok("All products");
    }

    [HttpGet("{id}")]
    public IActionResult GetById(int id)
    {
        return Ok($"Product {id}");
    }
}
```

**URLs:**

| HTTP Method | URL | Action |
|-------------|-----|--------|
| GET | `/api/products` | `GetAll` |
| GET | `/api/products/5` | `GetById(5)` |

#### B) HTTP Verb-based Routing (Web API Feature)
```csharp
[HttpPost]
public IActionResult Create(Product model) { }

[HttpPut("{id}")]
public IActionResult Update(int id, Product model) { }

[HttpDelete("{id}")]
public IActionResult Delete(int id) { }
```

👉 **Routing depends on:**
- URL
- HTTP Method (GET, POST, PUT, DELETE)


### 5️⃣ Conventional Routing in Web API (Rare)

You can use it, but **not recommended**.
```csharp
app.MapControllerRoute(
    name: "api",
    pattern: "api/{controller}/{action}/{id?}");
```

⚠️ This breaks RESTful standards.

### 6️⃣ Endpoint Routing (ASP.NET Core)

Used internally by both MVC & Web API:
```csharp
app.MapControllers();
```

❌ Without this, routing won't work!


### 7️⃣ Route Parameters & Constraints
```csharp
[HttpGet("{id:int}")]
public IActionResult Get(int id)
```

**Only matches:**
- ✅ `/api/products/5`
- ❌ `/api/products/abc`

### 📁 MVC View Folder Structure

**Question:** If I want to hit `/Home/Index`, do we need to store the page like this?
```
Views/
 └── Home/
     └── Index.cshtml
```

**Short answer: YES (by default)** 👍

#### Default MVC Convention (Important)

If you hit this URL:
```
/Home/Index
```

ASP.NET Core MVC looks for the view by convention:
```
Views/
 └── Home/
     └── Index.cshtml
```

✅ **This is REQUIRED if:**
- You return `return View();`
- You don't explicitly specify a view name
- You follow default MVC conventions

#### Why MVC needs this structure?

When you write:
```csharp
public IActionResult Index()
{
    return View();
}
```

MVC internally searches in this order:

1. `Views/Home/Index.cshtml` ✅
2. `Views/Shared/Index.cshtml`
3. ❌ Not found → runtime error

So the folder name must match the controller name (without "Controller").

#### ❌ Wrong structure (won't work)
```
Views/
 └── Index.cshtml
```

Unless you explicitly tell MVC where the view is.

### 🔄 Exceptions to Convention

#### Exception 1️⃣: Using Shared Folder

You can place views in:
```
Views/
 └── Shared/
     └── Index.cshtml
```

This works for all controllers, but not recommended for page-specific views.

#### Exception 2️⃣: Explicit View Name

You can break the convention like this:
```csharp
return View("~/Views/SomeOtherFolder/MyPage.cshtml");
```

or
```csharp
return View("MyCustomView");
```

**Folder:**
```
Views/
 └── Home/
     └── MyCustomView.cshtml
```

#### Exception 3️⃣: Razor Pages (Different concept)

If you were using Razor Pages, routing is folder-based, not controller-based:
```
Pages/
 └── Home/
     └── Index.cshtml
```

**URL:**
```
/Home/Index
```

⚠️ This has NO controller at all

### 📊 MVC vs Razor Pages (Quick Difference)

| Feature | MVC | Razor Pages |
|---------|-----|-------------|
| Uses Controller | ✅ Yes | ❌ No |
| Folder-based routing | ❌ No | ✅ Yes |
| Views folder | `Views` | `Pages` |

---
