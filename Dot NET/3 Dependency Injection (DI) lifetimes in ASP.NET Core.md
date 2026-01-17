
# Dependency Injection Lifetimes in ASP.NET Core

## 📑 Table of Contents - Dependency Injection Lifetimes

1. [AddTransient](#1️⃣-addtransient)
2. [AddScoped](#2️⃣-addscoped)
3. [AddSingleton](#3️⃣-addsingleton)
4. [Quick Comparison Table](#-quick-interview-comparison-table)
5. [Important DI Rules](#️-important-rule-interview-favorite)

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
