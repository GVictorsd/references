# ASP.NET
## Docs: 
https://learn.microsoft.com/en-gb/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-7.0&tabs=visual-studio

## Creating new project
```
dotnet new mvc -o MvcMovie
code -r MvcMovie

# running
dotnet dev-certs https --trust
# select trust
# build the project
dotnet build
# to generate the solution file(.sln)
dotnet new sln
# run the project
dotnet run
# run in watch mode
dotnet watch run
```

## Adding a controller
- add a new file to `Controllers` folder with file name `HelloWorldController.cs`

```
using Microsoft.AspNetCore.Mvc;
using System.Text.Encodings.Web;

namespace MvcMovie.Controllers;

public class HelloWorldController : Controller
{
    // 
    // GET: /HelloWorld/
    public string Index()
    {
        return "This is my default action...";
    }
    // 
    // GET: /HelloWorld/Welcome/ 
    public string Welcome()
    {
        return "This is the Welcome action method...";
    }
}
```
- this will allow routing to `localhost:{PORT}/HelloWorld` will execute index method of the above class

### Default controller route
```
## Program.cs
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```
From above,
- controller is the name of the controller class to execute
- action is the method from that class
- id is passed as `ID`, a parameter to the method 

```
url: https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4

// GET: /HelloWorld/Welcome/ 
// Requires using System.Text.Encodings.Web;
public string Welcome(string name, int numTimes = 1)
{
    return HtmlEncoder.Default.Encode($"Hello {name}, NumTimes is: {numTimes}");
}
```

### using id
```
url : https://localhost:{PORT}/HelloWorld/Welcome/3?name=Rick
public string Welcome(string name, int ID = 1)
{
    return HtmlEncoder.Default.Encode($"Hello {name}, ID: {ID}");
}
```

## Creating View
- View templates are created using Razor
- have .cshtml extension
- html created using c#

```
# replace index return
return View()
```
- crete new file Views/HelloWorld/Index.cshtml
```
@{
    ViewData["Title"] = "Index";
}
<h2>Index</h2>
<p>Hello from our View Template!</p>
```

### NOTES:
- `Views/Shared/_Layout.cshtml` is holds the html view which is common for all the controller views.
- _Layout.cshtml file can be configured in `_ViewStart.cshtml` file
- RenderBody:
@RenderBody() in _Layout.cshtml file is where the html from the views is inserted.

## ViewData Dictionary
### Accessing parameters in the views from the _Layout.cshtml
```
# the following reads and sets the title of the view in the _Layout.cshtml file

 <title>@ViewData["Title"] - Movie App</title>
```

## Adding Model
Model classes are used with entity Framework Core(EF Core) to work with database which is an object relational mapping(ORM) framework that simplifies data access code. The model classes created are known as POCO classes(Plain Old CLR Objects)
```
# Movie.cs to Models folder
using System.ComponentModel.DataAnnotations;
namespace MvcMovie.Models;

public class Movie
{
    public int Id { get; set; }
    public string? Title { get; set; }
    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }
    public string? Genre { get; set; }
    public decimal Price { get; set; }
}
```
- Id is required by the DataBase for primary key
- The datatype attribute on ReleaesDate specifies type of data(Date)(here it requires only date and not time)
- question mark after string indicated nullable.

### Add NuGet packages
```
dotnet tool uninstall --global dotnet-aspnet-codegenerator
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool uninstall --global dotnet-ef
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```
Above cmds add
- cli for EF Core
- aspnet-codegenerator scaffolding tool
- Design time tools for EF Core
- EF Core SQLite provider, which installs EF Core package as dependency
- Packages for scaffolding(Microsoft.VisualStudio.Web.CodeGeneration.Design and Microsoft.EntityFrameworkCore.SqlServer)

### Scaffold movie pages
Use scaffolding tool to produce CRUD pages for the movie model.
```
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovie.Data.MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries --databaseProvider sqlite

# for help
dotnet aspnet-codegenerator controller -h
```
-m	The name of the model.
-dc	The data context.
--relativeFolderPath	The relative output folder path to create the files.
--useDefaultLayout|-udl	The default layout should be used for the views.
--referenceScriptLibraries	Adds _ValidationScriptsPartial to Edit and Create pages.
--databaseProvider sqlite	Specify DbContext should use SQLite instead of SQL Server.

### Changes made by above cmd(scaffolding updates)
Creates
- A movies controller: Controllers/MoviesController.cs
- Razor view files for Create, Delete, Details, Edit, and Index pages: Views/Movies/*.cshtml
- A database context class: Data/MvcMovieContext.cs
Scaffolding updates the following:
- Registers the database context in the Program.cs file
- Adds a database connection string to the appsettings.json file.

## Initial Migration
- use EF Core Migrations feature to create the database. Migrations are tools to create and update database to match the data model.
```
dotnet ef migrations add InitialCreate
# creates Migrations/{timestamp}_InitialCreate.cs migrations file where InitialCreate is the arg given to the cmd. can be anything but first migration is usually named InitialCreate and is used to generate schema as specified in the Data/MvcMovieContext.cs file

dotnet ef database update
# updates the db to latest migration which is created by the prev command. this runs the Up method in Migrations/{timestamp}_InitialCreate.cs file which creates the database.
```

```
opending /Movies should open the db Index.cshtml view
```