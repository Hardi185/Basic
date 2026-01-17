# ASP.NET Core (Basic) - Quick Questions

## 📑 Table of Contents
1. [ConfigureServices() vs Configure()](#1-configureservices-vs-configure)
2. [Environment-Specific Settings](#2-how-do-you-handle-environment-specific-settings)
3. [What is ILogger?](#3-what-is-ilogger-and-why-is-it-used)
4. [What is Model Binding?](#4-what-is-model-binding)
5. [What is Model Validation?](#5-what-is-model-validation)

---

## 1. ConfigureServices() vs Configure()?

| Aspect | ConfigureServices() | Configure() |
|--------|-------------------|-------------|
| **Purpose** | Register services | Configure middleware |
| **When called** | Before Configure() | After ConfigureServices() |
| **What it does** | Sets up DI container | Sets up request pipeline |
| **Example** | `services.AddControllers()` | `app.UseRouting()` |

## 2. How do you handle environment-specific settings?

Using files like:
- `appsettings.Development.json`
- `appsettings.Production.json`
- `appsettings.Staging.json`

### File Structure:
```
📁 MyProject
  ├── appsettings.json (base settings)
  ├── appsettings.Development.json (dev overrides)
  └── appsettings.Production.json (prod overrides)
```

### How It Works:
```
appsettings.json (loaded first)
        ↓
appsettings.{Environment}.json (overrides base)
        ↓
Environment Variables (override both)
```

## 3. What is ILogger and why is it used?

`ILogger` is used to **log application information, warnings, and errors** for monitoring and debugging.

### Log Levels:

| Level | Usage |
|-------|-------|
| `LogTrace` | Detailed diagnostic info |
| `LogDebug` | Debugging information |
| `LogInformation` | General information |
| `LogWarning` | Warning messages |
| `LogError` | Error messages |
| `LogCritical` | Critical failures |

## 4. What is Model Binding?

**Model Binding** maps incoming request data (query, body, route) to action method parameters.

## 5. What is Model Validation?

**Model Validation** checks input data using attributes like `[Required]`, `[EmailAddress]`.
✅ Centralized validation logic

---
