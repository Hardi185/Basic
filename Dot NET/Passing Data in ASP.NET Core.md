# Passing Data in ASP.NET Core

## 1️⃣ ViewData

### What it is
- A dictionary object (`ViewDataDictionary`)
- Used to pass data from **Controller → View**
- Request-scoped (data is lost after the request ends)

### Key points
- Key–value pair, Type: `Dictionary<string, object>`
- Needs type casting
- Slightly slower than ViewBag

### Example
```csharp
// Controller
ViewData["Message"] = "Hello World";

// View
<h2>@ViewData["Message"]</h2>
```

### When to use
- Small, simple data
- When you want explicit key-based access

## 2️⃣ ViewBag

### What it is
- A dynamic wrapper over ViewData
- Used to pass data from **Controller → View**

#### Key points
- No type casting needed
- Cleaner syntax
- Still request-scoped

#### Example
```csharp
// Controller
ViewBag.Message = "Hello World";

// View
<h2>@ViewBag.Message</h2>
```

#### When to use
- Small data
- Quick UI values (titles, flags, messages)

## 3️⃣ TempData

### What it is
- Used to pass data **between requests**
- Backed by Session

### Key points
- Survives one redirect
- Automatically removed after read
- Useful for redirect scenarios

### Example
```csharp
// Controller
TempData["Success"] = "Data saved successfully";
return RedirectToAction("Index");

// View
@if (TempData["Success"] != null)
{
    <div>@TempData["Success"]</div>
}
```

### When to use
- Success / error messages after redirect
- Wizard or multi-step flows


## 4️⃣ ViewModel

### What it is
- A strongly typed class
- Designed only for the View
- Can combine multiple models

#### Key points
- Compile-time safety
- Clean, maintainable
- Best practice for real applications

#### Example
```csharp
public class UserViewModel
{
    public string Name { get; set; }
    public string Email { get; set; }
    public List<Order> Orders { get; set; }
}

// Controller
return View(userViewModel);

// View
@model UserViewModel
<h2>@Model.Name</h2>
```

#### When to use
- Forms
- Complex UI screens
- Multiple entities in one view

## 5️⃣ DTO (Data Transfer Object)

### What it is
- A simple object used to transfer data
- Commonly used between **API ↔ Client**, **Service ↔ Controller**

### Key points
- No business logic
- Often used in Web APIs
- Prevents exposing DB entities directly

### Example
```csharp
public class UserDto
{
    public string Name { get; set; }
    public string Email { get; set; }
}

// API Controller
return Ok(userDto);
```

### When to use
- APIs
- Microservices
- External communication

## 🔥 Quick Comparison Table

| Feature | ViewData | ViewBag | TempData | ViewModel | DTO |
|---------|----------|---------|----------|-----------|-----|
| **Type Safety** | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Scope** | Current Request | Current Request | Next Request/Redirection | View | Transfer |
| **Backed By** | Dictionary | ViewData | Session | Class | Class |
| **Layer** | Controller → View | Controller → View | Request → Request | UI | Service/API |

---
