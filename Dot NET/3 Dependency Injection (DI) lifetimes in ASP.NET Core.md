
# Dependency Injection Lifetimes in ASP.NET Core

## 📑 Table of Contents - Dependency Injection Lifetimes

1. [AddTransient](#1️⃣-addtransient)
2. [AddScoped](#2️⃣-addscoped)
3. [AddSingleton](#3️⃣-addsingleton)
4. [Quick Comparison Table](#-quick-interview-comparison-table)
5. [Important DI Rules](#️-important-rule-interview-favorite)
6. [IConfiguration in ASP.NET Core](#IConfiguration-in-ASP-NET-Core)
  - [Is IConfiguration Default?](#1️⃣-is-iconfiguration-default)
  - [Who Creates IConfiguration?](#2️⃣-who-creates-iconfiguration)
  - [Configuration Sources](#3️⃣-where-does-iconfiguration-read-from)
  - [Where is IConfiguration Stored?](#4️⃣-where-is-iconfiguration-stored)
  - [How to Use IConfiguration](#5️⃣-how-do-you-use-iconfiguration)
  - [Do We Create IConfiguration?](#6️⃣-do-we-ever-create-iconfiguration-ourselves)
  - [Interview Answer](#7️⃣-interview-ready-answer)
  - [Mental Model](#8️⃣-simple-mental-model)

## 1️⃣ AddTransient

👉 **New instance every time it is requested**

- A new object is created each time it's injected.
- Used for lightweight, stateless services.

### Example use cases
- Utility services
- Formatters
- Email/SMS senders

### Example
```csharp
services.AddTransient<IEmailService, EmailService>();
```

### Key point
- Multiple injections = multiple objects


## 2️⃣ AddScoped

👉 **One instance per HTTP request**

- Same instance is shared within a single request.
- New instance is created for each new request.

### Example use cases
- Business services
- Repository classes
- Database context (`DbContext`)

### Example
```csharp
services.AddScoped<IUserService, UserService>();
```

### Key point
- Same request → same object
- Different request → different object



## 3️⃣ AddSingleton

👉 **One instance for the entire application lifetime**

- Created once and reused for all requests.
- Lives until the application shuts down.

### Example use cases
- Configuration
- Caching
- Logging
- In-memory data

### Example
```csharp
services.AddSingleton<ICacheService, CacheService>();
```

### Key point
- Shared across all users and requests
- Must be thread-safe

## 🔥 Quick Interview Comparison Table

| Lifetime | Instance Creation | Scope |
|----------|------------------|-------|
| **Transient** | Every injection | Short |
| **Scoped** | Per HTTP request | Medium |
| **Singleton** | Once per app | Long |


## ⚠️ Important Rule (Interview Favorite)

❌ **Never inject Scoped service into Singleton**

✅ **Allowed:** Transient → Scoped → Singleton

---

## IConfiguration in ASP.NET Core

### 1️⃣ Is `IConfiguration` default?

✅ **YES**

`IConfiguration` is automatically created by ASP.NET Core when the app starts.

**You never manually `new` it up.**

---

### 2️⃣ Who creates `IConfiguration`?

#### In .NET 5
```csharp
Host.CreateDefaultBuilder(args)
```

This method:
- Builds configuration
- Registers `IConfiguration` in DI container

---

#### In .NET 6+
```csharp
var builder = WebApplication.CreateBuilder(args);
```

This internally does:
- Loads config sources
- Builds `IConfiguration`
- Registers it in DI

---

### 3️⃣ Where does IConfiguration read from?

By default, it reads from:

1. `appsettings.json`
2. `appsettings.{Environment}.json`
3. Environment variables
4. Command-line arguments
5. User secrets (Development)

👉 **Order matters (later overrides earlier)**

---

### 4️⃣ Where is IConfiguration stored?

Internally:
```csharp
builder.Configuration   // .NET 6+
```

Or injected anywhere using DI:
```csharp
IConfiguration
```

---

### 5️⃣ How do YOU use IConfiguration?

#### Option 1: Constructor Injection (MOST COMMON)
```csharp
public class ProductService
{
    private readonly IConfiguration _config;

    public ProductService(IConfiguration config)
    {
        _config = config;
    }
}
```

---

#### Option 2: Read in Program.cs (.NET 6+)
```csharp
var connectionString = 
    builder.Configuration.GetConnectionString("DefaultConnection");
```

---

#### Option 3: Strongly typed config (BEST PRACTICE)
```json
// appsettings.json
"MySettings": {
  "MaxItems": 10
}
```
```csharp
builder.Services.Configure<MySettings>(
    builder.Configuration.GetSection("MySettings"));

public class MyService
{
    public MyService(IOptions<MySettings> options)
    {
        int max = options.Value.MaxItems;
    }
}
```

---

### 6️⃣ Do we ever create IConfiguration ourselves?

🚫 **NO (almost never)**

**Only rare cases:**
- Console apps without host
- Custom test setups

**Even then:**
```csharp
new ConfigurationBuilder()
```

---

### 7️⃣ Interview-Ready Answer

`IConfiguration` is **provided by ASP.NET Core by default**. It is automatically created during application startup and registered in the dependency injection container. Developers only **consume it via dependency injection** and do not manually create it.

---

### 8️⃣ Simple Mental Model
```
ASP.NET Core startup
      ↓
Builds IConfiguration
      ↓
Registers in DI
      ↓
Available everywhere
```

---
