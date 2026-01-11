# .NET Framework and ASP.NET Overview

## What is .NET Framework?
.NET is a free, open- source developer platform created by Microsoft for building various types of applications. With .NET, developers can use multiple programming languages, editors, and libraries to create applications for web, mobile, desktop, games, and IoT. Initially released in the early 2000s, .NET has evolved into a versatile platform with a wide range of tools, frameworks, and languages.

## .NET Frameworks and Their Uses

| Framework | Purpose | Used For |
|-----------|---------|----------|
| **ASP.NET** | Web Development | Websites, Web APIs, Web Apps |
| **Blazor** | Web UI Framework | Interactive Web Apps with C# instead of JavaScript |
| **WinForms** | Desktop UI | Windows Desktop Applications |
| **WPF (Windows Presentation Foundation)** | Desktop UI | Modern Windows Applications with XAML |
| **MAUI (.NET Multi-platform App UI)** | Cross-Platform UI | Mobile and Desktop Apps (Windows, macOS, Android, iOS) |
| **.NET MAUI Blazor** | Hybrid Apps | Web + Native Apps in C# |
| **Unity (using .NET)** | Game Development | 2D/3D Games with C# |
| **ML.NET** | Machine Learning | AI and ML-based Applications |
| **Xamarin (Now MAUI)** | Mobile Apps | Android & iOS Apps using C# |
| **Entity Framework Core** | ORM (Object-Relational Mapping) | Database Access for Applications |


### Evolution of the Runtime Platform
-  **.NET Framework** → **.NET Core** → **.NET (Unified Platform)**
-  The platform has undergone significant transformations, with new versions and features added over time.


## .NET Core

### Description:
-  A modern, cross-platform version of .NET.
- Introduced in 2016 as an open-source and modular alternative to the .NET Framework.
- Includes ASP.NET Core for web development.

### Key Features:
- Cross-platform (Windows, Linux, macOS).
- Modular and high-performance.

### Versions:
- **.NET Core 1.0** (2016)
- **.NET Core 2.0**, **2.1** (2017-2018)
- **.NET Core 3.0**, **3.1** (2019, LTS)


## .NET (Unified Platform)

### Description:
- The evolution and unification of .NET Framework and .NET Core into a single platform.
- Renamed simply to .NET starting from version 5 in 2020.
- Includes ASP.NET Core for web development.

### Key Features:
- Unified libraries and runtime for all platforms.
- Cross-platform and high-performance.

### Versions:
- **.NET 5** (2020)
- **.NET 6** (2021, LTS)
- **.NET 7** (2022)
- **.NET 8** (2023, LTS)

---

## What is ASP.NET?
ASP.NET is a web application framework developed by Microsoft to allow programmers to build dynamic websites, applications, and services. As part of the .NET Framework, ASP.NET enables developers to use programming languages such as C#, VB.NET, or F# to build web applications easily.

### Key Features:
- Tools and libraries specifically for web development.
- Integration with the .NET platform.


## ASP.NET Versions and Corresponding .NET Versions

| **ASP.NET Version**          | **Release Year** | **.NET Version**              |
|------------------------------|------------------|--------------------------------|
| **ASP.NET 1.0**              | 2002             | .NET Framework 1.0            |
| **ASP.NET 2.0**              | 2005             | .NET Framework 2.0            |
| **ASP.NET 3.5**              | 2007             | .NET Framework 3.5            |
| **ASP.NET 4.0**              | 2010             | .NET Framework 4.0            |
| **ASP.NET MVC 1.0**          | 2009             | .NET Framework 3.5 or later   |
| **ASP.NET MVC 3.0**          | 2011             | .NET Framework 4.0            |
| **ASP.NET MVC 4.0**          | 2012             | .NET Framework 4.5            |
| **ASP.NET 5 (Core 1.0)**     | 2016             | .NET Core 1.0                 |
| **ASP.NET Core 2.0**         | 2017             | .NET Core 2.0                 |
| **ASP.NET Core 3.0**         | 2019             | .NET Core 3.0                 |
| **ASP.NET Core 5.0**         | 2020             | .NET 5.0 (Unified Platform)   |
| **ASP.NET Core 6.0**         | 2021             | .NET 6.0 (LTS)                |
| **ASP.NET Core 7.0**         | 2022             | .NET 7.0                      |
| **ASP.NET Core 8.0**         | 2023             | .NET 8.0 (LTS)                |

### NOTE:
- .NET Framework:	ASP.NET MVC & ASP.NET Web API = separate frameworks
- .NET Core:	ASP.NET Core = rebuilt single framework
- .NET 5+:	Same ASP.NET Core, unified


## Summary
- **.NET Framework**: Legacy platform, Windows-only.
- **.NET Core**: Cross-platform, open-source, modular.
- **.NET (Unified Platform)**: Unified runtime for all platforms, starting with .NET 5.
- **ASP.NET**: Framework for building dynamic web applications and services, with a transition to cross-platform development through ASP.NET Core.

---



## Role of IIS in ASP.NET MVC:

IIS (Internet Information Services) is a web server developed by Microsoft. It is responsible for hosting and serving web applications, including ASP.NET applications. IIS handles HTTP requests, serves static files (like HTML, CSS, JS), and forwards dynamic requests (like those for ASP.NET pages) to the appropriate application. It is often used to host web applications on Windows Servers.

When you run an ASP.NET MVC application, IIS is responsible for:

- **Handling requests:** When a user makes a request (e.g., by navigating to /Home/Index), IIS intercepts the request and directs it to your application.
- **Routing:** IIS uses URL Routing (configured in your app) to forward requests to the correct Controller and Action.
- **Serving static files:** IIS serves static files like CSS, JavaScript, and image files.
- **Managing application lifecycle:** IIS starts and stops the application, handles requests, and ensures the web application runs smoothly.

---

## Extension methods in C#

Please refer this:
[Extension methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)

---

## EF as ORM for .NET application:

Please refer this:
[Entity framework](https://github.com/Hardi185/Basic/blob/main/Entity%20framework.md)

