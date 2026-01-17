# ASP.NET Project Types – Architecture Overview

## 📑 Table of Contents
1. [ASP.NET Core MVC (.NET Core / .NET 6+)](#apnet-core-mvc-net-core--net-6)
2. [ASP.NET Core Web API (.NET Core / .NET 6+)](#2-aspnet-core-web-api-net-core--net-6)
3. [ASP.NET MVC (.NET Framework 4.x)](#3-aspnet-mvc-net-framework-4x)
4. [ASP.NET Web API (.NET Framework)](#4-aspnet-web-api-net-framework)
5. [Quick Comparison](#-quick-comparison)
6. [Recommendation](#-recommendation)

---

## 1️⃣ ASP.NET Core MVC (.NET Core / .NET 6+)

👉 Used for **server-rendered UI** using Razor Views and Controllers  
👉 Can return JSON, but **primary focus = UI**

### 📁 Project Structure
```
MyProject/
│
├─ Controllers/
│ ├─ HomeController.cs
│ └─ ProductController.cs
│
├─ Models/
│ └─ Product.cs
│
├─ Views/
│ ├─ Home/
│ │ └─ Index.cshtml
│ └─ Shared/
│ └─ _Layout.cshtml
│
├─ wwwroot/
│ ├─ css/
│ ├─ js/
│ └─ images/
│
├─ appsettings.json
├─ Program.cs ✅ Entry point
├─ Startup.cs ❌ (Merged into Program.cs in .NET 6+)
└─ MyProject.csproj
```

### 🔑 Key Points
- Uses **Controllers + Views**
- Razor pages (`.cshtml`)
- `return View()` is common
- Suitable for **monolithic web applications**
- Cross-platform

## 2️⃣ ASP.NET Core Web API (.NET Core / .NET 6+)

👉 Used for **REST APIs only**  
👉 No UI, no Views

### 📁 Project Structure
```
MyProject.Api/
│
├─ Controllers/
│ └─ ProductController.cs
│
├─ Models/
│ └─ Product.cs
│
├─ DTOs/
│ └─ ProductDto.cs
│
├─ Services/
│ └─ ProductService.cs
│
├─ Repositories/
│ └─ ProductRepository.cs
│
├─ appsettings.json
├─ Program.cs ✅ Only startup file
└─ MyProject.Api.csproj
```

### 🔑 Key Points
- No Views, no `wwwroot`
- Controllers inherit from `ControllerBase`
- Uses `[ApiController]`
- Returns **JSON**
- Ideal for **microservices** and **frontend-backend separation**
- Cross-platform

## 3️⃣ ASP.NET MVC (.NET Framework 4.x)

👉 Legacy UI-based web application  
👉 Still found in old enterprise systems

### 📁 Project Structure
```
MyProject/
│
├─ Controllers/
│ └─ HomeController.cs
│
├─ Models/
│ └─ Product.cs
│
├─ Views/
│ ├─ Home/
│ │ └─ Index.cshtml
│ └─ Shared/
│
├─ Content/ ❌ (CSS)
├─ Scripts/ ❌ (JS)
│
├─ App_Start/
│ ├─ RouteConfig.cs
│ ├─ FilterConfig.cs
│ └─ BundleConfig.cs
│
├─ Global.asax ✅ Application entry
├─ Web.config
└─ MyProject.csproj
```

### 🔑 Key Points
- Entry point = `Global.asax`
- Heavy `Web.config`
- Separate `Scripts` & `Content` folders
- **Windows-only**
- Slower and tightly coupled
- Mostly used for **legacy maintenance**

## 4️⃣ ASP.NET Web API (.NET Framework)

👉 Legacy **API-only** project before ASP.NET Core

### 📁 Project Structure
```
MyProject.Api/
│
├─ Controllers/
│ └─ ProductController.cs
│
├─ Models/
│ └─ Product.cs
│
├─ App_Start/
│ └─ WebApiConfig.cs
│
├─ Global.asax
├─ Web.config
└─ MyProject.Api.csproj
```

### 🔑 Key Points
- No Views
- Controllers inherit from `ApiController`
- Routing configured in `WebApiConfig`
- Uses `UseExceptionHandler` (legacy pattern)
- Not cross-platform
- **Replaced by ASP.NET Core Web API**
