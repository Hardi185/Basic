# .NET Framework and ASP.NET Overview

## What is .NET Framework?
.NET is a free, open- source developer platform created by Microsoft for building various types of applications. With .NET, developers can use multiple programming languages, editors, and libraries to create applications for web, mobile, desktop, games, and IoT. Initially released in the early 2000s, .NET has evolved into a versatile platform with a wide range of tools, frameworks, and languages.

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


## Summary
- **.NET Framework**: Legacy platform, Windows-only.
- **.NET Core**: Cross-platform, open-source, modular.
- **.NET (Unified Platform)**: Unified runtime for all platforms, starting with .NET 5.
- **ASP.NET**: Framework for building dynamic web applications and services, with a transition to cross-platform development through ASP.NET Core.

---

## Different Architacture that can be followed for ASP. NET web development:
- Use MVC pattern for server-rendered pages or APIs.
- Use it with Vue.js, Angular, or React for frontend decoupling.
  - Without microServices
  - With microServices

---

## MVC Architecture Using ASP.NET:

To build an MVC-based application using ASP.NET (for example, using ASP.NET Core MVC), the typical flow is structured in a way where the Model, View, and Controller each have distinct responsibilities. Here’s an explanation of how each component works and how they are interconnected, followed by a simple code example.
MVC Architecture Flow:

**1.	Model:**
- Represents the application's data and business logic.
- Responsible for data retrieval, validation, and updates.
- Often maps to a database entity (via Entity Framework or another ORM).

**2.	View:**
- Represents the UI (User Interface).
- Displays the data provided by the controller and receives user input.
- Typically written in Razor (for ASP.NET Core MVC), which is an HTML-based view engine.

**3.	Controller:**
- Acts as a mediator between the Model and the View.
- Handles user input, processes it, and returns the appropriate view or redirects to another action.
- Can manipulate data in the model and pass it to the view.

### Basic Example of an MVC Application (ASP.NET Core)

Here’s a basic example of how an MVC-based ASP.NET Core application works.

**Step 1: Create a Model**

The Model represents the data structure. For example, let’s say we are building an application to manage Books.

```csharp
// Models/Book.cs
namespace MyMvcApp.Models
{
    public class Book
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Author { get; set; }
        public decimal Price { get; set; }
    }
}
```

**Step 2: Create a Controller**

The Controller handles user requests and prepares data for the View.
```csharp
// Controllers/HomeController.cs
using Microsoft.AspNetCore.Mvc;
using MyMvcApp.Models;
using System.Collections.Generic;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Sample data (in a real app, this would come from a database)
        private static List<Book> books = new List<Book>
        {
            new Book { Id = 1, Title = "The Great Gatsby", Author = "F. Scott Fitzgerald", Price = 10.99m },
            new Book { Id = 2, Title = "1984", Author = "George Orwell", Price = 8.99m }
        };

        // Action to display all books
        public IActionResult Index()
        {
            return View(books); // Passing the list of books to the view
        }

        // Action to display a specific book by Id
        public IActionResult Details(int id)
        {
            var book = books.Find(b => b.Id == id);
            if (book == null)
                return NotFound();

            return View(book); // Passing the book to the view
        }
    }
}
```

**Step 3: Create Views**

The View renders the HTML for the user. In ASP.NET Core, we use Razor to generate dynamic HTML from the controller data.

**1. Index View:** Display a list of books
```html
<!-- Views/Home/Index.cshtml -->
@model IEnumerable<MyMvcApp.Models.Book>

<h2>Books List</h2>

<table>
    <tr>
        <th>Title</th>
        <th>Author</th>
        <th>Price</th>
        <th>Details</th>
    </tr>
    @foreach (var book in Model)
    {
        <tr>
            <td>@book.Title</td>
            <td>@book.Author</td>
            <td>@book.Price</td>
            <td><a href="@Url.Action("Details", "Home", new { id = book.Id })">View Details</a></td>
        </tr>
    }
</table>
```

**2. Details View:** Display details of a specific book
```html
<!-- Views/Home/Details.cshtml -->
@model MyMvcApp.Models.Book

<h2>@Model.Title</h2>
<p>Author: @Model.Author</p>
<p>Price: @Model.Price</p>
<a href="@Url.Action("Index", "Home")">Back to List</a>
```

**Step 4: Set Up Routes (Routing)**

In ASP.NET Core, routes are defined in the Startup.cs or Program.cs file (for newer versions). By default, the routing will map URLs to actions in the controller.
For example, by default, the route "/Home/Index" would map to the Index action in HomeController.

Here is an example of routing configuration in the Program.cs file:

```csharp
// Program.cs
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

namespace MyMvcApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

**Step 5: Running the Application**
When the application runs, here's what happens in sequence:
1.	The User navigates to /Home/Index.
2.	The HomeController processes the request, returning the Index view with the list of books.
3.	The Index.cshtml view renders the list of books, displaying the titles, authors, and prices.
4.	The user clicks on "View Details" to see more information about a book.
5.	The Details action in HomeController is called, retrieving the book by id and rendering the Details.cshtml view.


## Role of IIS in ASP.NET MVC:

IIS (Internet Information Services) is a web server developed by Microsoft. It is responsible for hosting and serving web applications, including ASP.NET applications. IIS handles HTTP requests, serves static files (like HTML, CSS, JS), and forwards dynamic requests (like those for ASP.NET pages) to the appropriate application. It is often used to host web applications on Windows Servers.

When you run an ASP.NET MVC application, IIS is responsible for:

- **Handling requests:** When a user makes a request (e.g., by navigating to /Home/Index), IIS intercepts the request and directs it to your application.
- **Routing:** IIS uses URL Routing (configured in your app) to forward requests to the correct Controller and Action.
- **Serving static files:** IIS serves static files like CSS, JavaScript, and image files.
- **Managing application lifecycle:** IIS starts and stops the application, handles requests, and ensures the web application runs smoothly.

**NOTE:**
- Action name(method name) and the view page regarding that should be of same name.
- if method id Index(), view name should be View.cshtml.

We can customize is as:
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        // Return a custom view named "CustomView.cshtml"
        return View("CustomView");
    }
}
```

- Most of the routes are handled through controller/action(method name)
 - Like `Home/Index`

Altho, we can have custom routes,

### 1. Using the Route Attribute (on Controller or Action)


You can define custom routes using the [Route] attribute. This can be applied at the controller or action level to map specific URLs to specific actions.

**Example:** Custom Route on Controller
```csharp
[Route("products")]
public class ProductsController : Controller
{
    // Matches the URL /products
    [Route("")]
    public IActionResult Index()
    {
        return View();
    }

    // Matches the URL /products/details/1
    [Route("details/{id}")]
    public IActionResult Details(int id)
    {
        return View(id);
    }
}
```

In this example:
- /products will map to Index action.
- /products/details/{id} will map to Details action with id as a parameter.

### 2. Using Custom Routes in Startup.cs (or Program.cs in newer versions)

You can also configure custom routes in the Configure method of Startup.cs (or Program.cs in newer versions):

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "productDetails",
            pattern: "products/details/{id}",
            defaults: new { controller = "Products", action = "Details" });

        // You can add more custom routes here
    });
}
```



