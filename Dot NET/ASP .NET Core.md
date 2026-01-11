# Project Structure (Single project approach):
````yaml
MyProject/
│
├─ Controllers/        # MVC & API controllers
│   ├─ HomeController.cs
│   └─ ProductController.cs
│
├─ Models/             # Data models
│   └─ Product.cs
│
├─ Views/              # Razor views for MVC
│   └─ Home/
│       └─ Index.cshtml
│
├─ wwwroot/            # Static files (CSS, JS, images)
│
├─ appsettings.json    # Configuration
├─ Program.cs          # App entry point
├─ Startup.cs          # Configure services & middleware
└─ MyProject.csproj
````

### Starting Point:
````yaml
Program.cs (Main) --> builds Host --> Startup.cs (ConfigureServices & Configure) --> HTTP Pipeline starts
````

### Program.cs (.NET 5 style):
````yaml
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

namespace MyProject
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run(); // Entry point
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>(); // Connect Startup class
                });
    }
}
````````
✅ Explanation:

- Main() is the actual starting point of the app.
- CreateHostBuilder() sets up the web host.
- UseStartup<Startup>() tells ASP.NET Core to use Startup.cs for further configuration.

### Startup.cs (.NET 5 style):
````yaml
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;

namespace MyProject
{
    public class Startup
    {
        public IConfiguration Configuration { get; }

        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        // Add services (DI, MVC, DB, etc.)
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllersWithViews();

            // Register DbContext
            services.AddDbContext<AppDbContext>(options =>
                options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

            // Register your service and repository for DI
            services.AddScoped<IProductRepository, ProductRepository>();
            services.AddScoped<IProductService, ProductService>();
        }

        // Configure HTTP request pipeline
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();
            app.UseAuthentication();
            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");

                endpoints.MapControllers(); // API endpoints
            });
        }
    }
}
````

`services.AddControllersWithViews()` → MVC + API Contoller loading
-	AddControllersWithViews() does three things:
1.	Registers all MVC controllers (controllers returning views) in your project.
2.	Registers API controllers too (controllers with [ApiController] attribute that return JSON).This is why in a single project, you can have both API and Razor controllers.
3.	Adds model binding, validation, routing, filters, etc.
So yes: by calling this, you are essentially registering all your controllers (MVC + API) with the DI container.

⚠️ If you only want API controllers, you can use services.AddControllers() instead.
If you only want Razor Pages, use services.AddRazorPages().

### ✅ Standard .NET 6 Project (ONLY Program.cs)
📌 This is the ONLY file required in .NET 6.
````yaml
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// --------------------
// 1️⃣ Register Services (old ConfigureServices)
// --------------------
builder.Services.AddControllersWithViews(); // MVC + API
builder.Services.AddRazorPages();

// Example DI
// builder.Services.AddDbContext<AppDbContext>(options =>
//     options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// builder.Services.AddScoped<IProductRepository, ProductRepository>();
// builder.Services.AddScoped<IProductService, ProductService>();

var app = builder.Build();

// --------------------
// 2️⃣ Configure Middleware (old Configure)
// --------------------
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();

// --------------------
// 3️⃣ Map Endpoints
// --------------------
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.MapControllers();   // Web API
app.MapRazorPages();    // Razor Pages

// --------------------
app.Run();
````

### Controllers:
MVC Controller Example:
````yaml
public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewData["Message"] = "Hello from MVC!";
        return View();
    }
}
````

API Controller Example:
````yaml
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll()
    {
        var products = new List<string> { "Laptop", "Phone" };
        return Ok(products); // Returns JSON
    }
}
````
