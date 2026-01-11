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

## NOTE:
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

---

## Decoupled Architecture using ASP.NET and Modern Frontend Frameworks

In a decoupled architecture, the backend serves as an API layer that provides data and business logic. The frontend consumes these APIs to render the user interface dynamically. This separation allows independent development and flexibility in choosing technologies.

### Key Benefits:
- **Separation of Concerns:** Frontend and backend teams can work independently.
- **Flexibility:** Switch frontend frameworks without affecting backend functionality.
- **Scalability:** Backend APIs can be reused across multiple frontend applications.
- **Support for SPAs:** Enables Single Page Applications (SPA) that provide a dynamic and responsive user experience.

### Example Implementation

### 1. Backend (ASP.NET Core - HomeController)
The backend provides a REST API endpoint to serve data as JSON. Here's an example controller:

```csharp
using Microsoft.AspNetCore.Mvc;

public class HomeController : Controller
{
    [HttpGet("/api/custom-view")]
    public IActionResult GetCustomViewData()
    {
        var data = new { message = "Hello from the backend!" };
        return Ok(data);  // Return data as JSON
    }
}
```

### 2. Frontend Examples
The frontend can be implemented using popular frameworks like Vue.js, Angular, or React. These frameworks fetch data from the backend API and update the UI dynamically.

#### a. Vue.js Example

```html
<template>
  <div>
    <h1>{{ message }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: ""
    };
  },
  created() {
    fetch("/api/custom-view")
      .then((response) => response.json())
      .then((data) => {
        this.message = data.message;
      });
  }
};
</script>
```

#### b. Angular Example

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-home',
  template: `<h1>{{ message }}</h1>`,
})
export class HomeComponent implements OnInit {
  message: string;

  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    this.http.get<any>('/api/custom-view').subscribe(data => {
      this.message = data.message;
    });
  }
}
```

#### c. React Example

```javascript
import React, { useState, useEffect } from "react";

function Home() {
  const [message, setMessage] = useState("");

  useEffect(() => {
    fetch("/api/custom-view")
      .then((response) => response.json())
      .then((data) => setMessage(data.message));
  }, []);

  return <h1>{message}</h1>;
}

export default Home;
```

## Architecture Flow

### Flow Without Microservices
1. **Frontend:** Sends HTTP requests to the backend API to fetch or send data.
2. **Backend:** Processes requests, performs business logic, and interacts with the database.
3. **API:** Acts as a mediator, exposing endpoints for frontend communication.

### Flow With Microservices
*Details will be updated soon.*
