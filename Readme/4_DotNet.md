# .NET Fundamentals

## 🔍 What is .NET

.Net (pronounced "dot net") is a free and open source application platform. The support by Microsoft and the regular updates and new features are what makes this tool so useful. It can be used for a variety of different things such as:

- Mobile Apps
- Desktop Apps
- Microservices
- Game Development
- Machine Learning
- Web Development

In this article I want to focus on Web Development and the most important things you will need when building your first Web App. To be completely honest I will only be covering the API side of things, knowing that [Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) can also provide an integrated frontend for your website. (Side note, I normally go with Angular for my frontends - let me know if you want me to write about it as well)

## 🆚 Minimal APIs vs Controller

Traditional Controllers follow the MVC ([Model-View-Controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)) pattern to separate concerns as you should. They follow the typical conventions and are very well structured. Since they were the only way for most of our time, it is clear that most apps use this way to write APIs.

```c#
public class HelloController : ControllerBase
{
    [HttpGet("hello")]
    public IActionResult GetHello()
    {
        return Ok("Hello World!");
    }

    [HttpPost("echo")]
    public IActionResult Echo([FromBody] string message)
    {
        return Ok(message);
    }
}
```

Minimal APIs were introduced in .NET 6 to simplify API creation within .NET. The syntax is more concise, there is less boilerplate and you can still do most things that you could do with controllers as well (in .NET 8). Since Microsoft is heavily working on the feature parity of minimal APIs, I assume that in the future, this will be the preferred way to build APIs from scratch - but this might just be me.

```c#
...
app.MapGet("/hello", () => "Hello World!");
app.MapPost("/echo", (string message) => Results.Ok(message));
...
```

I think the examples are already very telling and I do not need to mention, that one of them seems more _minimal_ than the other, do I?

## 🖥️ .NET Cli

To start out a new App you need to install the [.NET SDK](https://dotnet.microsoft.com/en-us/download) (Software Development Kit) and install the [.NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/). To create a new App run `dotnet new web -o [Project Name]` and open it with the editor / IDE of choice. I recommend JetBrains Rider but if you want to go with free software you can also use Visual Studio or Visual Studio Code with some extensions. You want to look out for the `Program.cs` which is the entry point of your application.

Check the content and it should look somewhat like this:

```c#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();
app.MapGet("/hello", () => "Hello World!");
app.Run();
```

Creating a `builder` that could also handle things like Dependency Injection, Database Connections, Authentication and Authorization as well as lots of other things that I will talk about later. As you see, there is an Endpoint defined for `/hello`. If we run the app via our IDE (or .NET CLI `dotnet run`) the API is waiting for a call. Try calling `http://localhost:5292/hello` (check `/Properties/launchSettings.json` of your project to see what Port you are running on) in your browser, and you should get a "Hello World" message on your screen.

You can check all options of the dotnet CLI by using `dotnet --help` in your terminal. Most of the commands will be handled by your IDE, but you can also execute them from the terminal if you are fancy.

## 📋 Validation

As you have seen in the examples before those APIs really are _minimal_. Let's have a closer look at an endpoint that also validates the request for the GET Route /colorSelector/

```c#
string ColorName(string color) => $"Color specified: {color}!";

app.MapGet("/colorSelector/{color}", ColorName)
    .AddEndpointFilter(async (invocationContext, next) =>
    {
        var color = invocationContext.GetArgument<string>(0);

        if (color == "Red")
        {
            return Results.Problem("Red not allowed!");
        }
        return await next(invocationContext);
    });
```

As you see, we defined the route as the first parameter of `app.MapGet()` and also map the specified color to our function by putting curly braces around it and matched the name. The validation is built with an endpoint filter that checks the invocation of the endpoint for the argument, then does checks (in this case the color is not allowed to be red, because red is evil) and calls `next` so other filters can also run. A filter is not always a validator, but sometimes it just logs requests, or adds an entry for statistics or does some other effect that should happen on every endpoint call.

## 🧭 Routing

In many cases you want to cluster multiple endpoints. Reasons could be that you want to use the same filter on all of them, or use the same authorization for them, or just don't want to write the path multiple times. In this case we can group multiple routes like this

```c#
var user = app.MapGroup("/user");
var admin = user.MapGroup("/admin");

admin.AddEndpointFilter((context, next) =>
{
    app.Logger.LogInformation("/admin group filter");
    return next(context);
});

user.MapGet("/", () => "Hello!");
```

This helps with organizing different paths and endpoints, as well as reducing duplication. Also this allows you to cluster your endpoints in folders or files outside the Program.cs without losing track of a file.

## 💉 Dependency Injection (e.g. Database) and Configuration

You can inject services into your API endpoints to reduce the complexity of the endpoint. Mostly you want to extract some logic into services so you can reuse them. In this example I setup a database in my project and inject the DbContext in my endpoint

Program.cs

```c#
using Microsoft.EntityFrameworkCore;
using backend.ShopDbContext;

var builder = WebApplication.CreateBuilder(args);
string? connectionString = builder.Configuration.GetConnectionString("DefaultConnection");
builder.Services.AddDbContext<ShopDbContext>(opt => opt.UseNpgsql(connectionString));
var app = builder.Build();
using (var Scope = app.Services.CreateScope())
{
    var context = Scope.ServiceProvider.GetRequiredService<ShopDbContext>();
    context.Database.Migrate();
}
app.MapGet("/", async (ShopDbContext ctx) =>
{
    var prod = await ctx.Products.FirstOrDefaultAsync();
    return prod;
});

app.Run();
```

Appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost; Port=5432; Database=postgres; Username=postgres; Password=MYPASSWORD;"
  }
}
```

To use Npgsql I installed a [Nuget Package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL) that works with EF-Core and supports [PostreSQL Databases](https://www.postgresql.org/). The code extracts the [connection string](https://www.connectionstrings.com/) provided in the configuration (Appsettings.json) in the object `ConnectionStrings`. To setup the Database to match the schema defined in the code, you will have to apply the schema by calling `Database.Migrate()` so I do it at the start of the application.

The Endpoint now injects the ShopDbContext magically (you could specify it by using the `[FromServices]` attribute) and gets the first element, which is a rather useless example. Most likely you want to return all, or specify the ID of the entry you want to get and look for it in the database.

## 🙏🏽 Thanks

Thank you so much if you read this article all the way! Leave a comment if you have any questions, I'll be more than happy to answer right away. If you are shy you can also message me directly on [GitHub](https://github.com/Suneeh), [Instagram](https://www.instagram.com/_suneeh/) or [TikTok](https://www.tiktok.com/@_suneeh).
