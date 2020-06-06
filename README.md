# ASP.NET Core Razor Page with Blazor WebAssembly

ASP.NET Core Razor Page app with Blazor WebAssembly based component.

Steps to create:

1. `dotnet new sln -n RazorPagesWithBlazorWebAssembly`
1. `dotnet new webapp -o WebApplication1`
1. `dotnet sln add WebApplication1`
1. `dotnet new blazorwasm -o BlazorApp1`
1. `dotnet sln add BlazorApp`
1. `dotnet add WebApplication1 reference BlazorApp1`
1. `dotnet add BlazorApp1 package Microsoft.AspNetCore.Components.WebAssembly.Server`
1. Update the `Configure` method *WebApplication1/Startup.cs*:

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
            app.UseWebAssemblyDebugging();
        }
        else
        {
            app.UseExceptionHandler("/Error");
            // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
            app.UseHsts();
        }

        app.UseHttpsRedirection();
        app.UseBlazorFrameworkFiles();
        app.UseStaticFiles();

        app.UseRouting();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapRazorPages();
        });
    }
    ```

1. Update *WebApplication1/Pages/Index.cshtml* to a tag for the `Counter` component and to add the Blazor WebAssembly script:

```cshtml
<my-counter>Loading</my-counter>

@section Scripts {
    <script src="_framework/blazor.webassembly.js"></script>
}
```

1. Update *BlazorApp1/Program.cs* to remove `App` as a root component and add the `Counter` component as a root component instead. You'll need to add a `using` directive for `BlazorApp1.Pages` as well:

    ```csharp
    builder.RootComponents.Add<Counter>("my-counter");
    ```

1. Delete *BlazorApp/wwwroot/index.html* so it doesn't conflict with the web app.
1. `cd WebApplication1`
1. `dotnet run`
1. Browse to https://localhost:5001

    ![image](https://user-images.githubusercontent.com/1874516/83955966-776e9b00-a80d-11ea-9b8b-221d65375c6d.png)
