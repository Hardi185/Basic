# Table of Content:

   - [MVC Routing](#3️⃣-routing-in-mvc-view-based)
   - [Web API Routing](#4️⃣-routing-in-web-api-data-based)
   - [MVC View Folder Structure](#-mvc-view-folder-structure)

----
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
