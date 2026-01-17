# ASP.NET Core MVC - Quick Questions

## 📑 Table of Contents
1. [What is MVC?](#1-what-is-mvc)
2. [What is Razor View?](#2-what-is-razor-view)
3. [Layout & _ViewStart.cshtml](#3-layout--_viewstartcshtml)
4. [What is Partial View?](#4-what-is-partial-view)
5. [What is Tag Helper?](#5-what-is-tag-helper)
6. [How to Pass Data from Controller to View](#6-how-do-you-pass-data-from-controller-to-view)

---

## 1. What is MVC?

    -  Model → Business logic & data
    -  View → UI (HTML)
    -  Controller → Handles request, coordinates Model & View

## 2. What is Razor View?

    -	Layout → Common UI (header, footer)
    -	_ViewStart.cshtml → Sets default layout for views
    
### File Structure:
```
📁 Views
  ├── Shared
  │   └── _Layout.cshtml (common template)
  ├── _ViewStart.cshtml (sets default layout)
  └── Home
      └── Index.cshtml (uses layout automatically)
```

### How It Works:
```
Request → _ViewStart.cshtml (sets layout)
              ↓
         Index.cshtml (content)
              ↓
         _Layout.cshtml (wraps content)
              ↓
         Complete HTML Response
```

## 4. What is Partial View?

A **Partial View** is a **reusable piece of UI** used inside multiple views.

### Key Features:
- Reusable components
- Doesn't have its own layout
- File naming convention: `_PartialName.cshtml` (underscore prefix)

## 5. What is Tag Helper?

**Tag Helpers** are server-side helpers that generate HTML using C# syntax.

### Key Features:
- HTML-like syntax
- IntelliSense support
- Easier to read than HTML Helpers
- Built into ASP.NET Core

### Example:

**Traditional HTML Helper:**
```csharp
@Html.ActionLink("Home", "Index", "Home", new { @class = "nav-link" })
```

**Tag Helper (Cleaner):**
```html
<a asp-controller="Home" asp-action="Index" class="nav-link">Home</a>
```

### Common Tag Helpers:

#### **Link Tag Helper:**
```html
<a asp-controller="Product" asp-action="Details" asp-route-id="5">View Details</a>

<!-- Generates: -->
<a href="/Product/Details/5">View Details</a>
```

#### **Form Tag Helper:**
```html
<form asp-controller="Account" asp-action="Login" method="post">
    <input asp-for="Email" />
    <input asp-for="Password" type="password" />
    <button type="submit">Login</button>
</form>
```

#### **Image Tag Helper:**
```html
<img src="~/images/logo.png" asp-append-version="true" />

<!-- Adds cache-busting query string -->
```

#### **Validation Tag Helpers:**
```html
<input asp-for="Email" class="form-control" />
<span asp-validation-for="Email" class="text-danger"></span>
```

### Built-in Tag Helpers:

| Tag Helper | Purpose |
|------------|---------|
| `asp-controller` | Specify controller |
| `asp-action` | Specify action |
| `asp-route-{param}` | Add route parameters |
| `asp-for` | Bind to model property |
| `asp-validation-for` | Show validation errors |
| `asp-append-version` | Cache busting |
| `asp-area` | Specify area |

### Enable Tag Helpers:

**_ViewImports.cshtml:**
```csharp
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

## 6. How do you pass data from Controller to View?

### For Detailed Explanation:

📚 **See complete guide:**  
[Passing Data in ASP.NET Core](https://github.com/Hardi185/Basic/blob/main/Dot%20NET/Passing%20Data%20in%20ASP.NET%20Core.md)
---
