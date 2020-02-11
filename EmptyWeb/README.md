# Code Exercise

1. Look around the files available in this project and tell us what you think
each of these files does.

2. Attempt to run this application and see the output

3. Attempt to run this application from the command line. The .NET CLI is
already installed and available on your `PATH`. It can be invoked using the
`dotnet` command.

4. The current project only returns `Hello World` to all HTTP requests. We're
going to modify the code to now return a JSON response.

    Replace the following code in `Startup.cs`
    
    ```csharp
                    endpoints.MapGet("/", async context =>
                    {
                        await context.Response.WriteAsync("Hello World!");
                    });
    ```
    
    with this new snippet
    
    ```csharp
                    endpoints.MapGet("/", async context =>
                    {
                        var weather = new { MinTemperature = 61.2, MaxTemperature = 89.1, Humidity = 81.0, City = "Miami" };
                        await JsonSerializer.SerializeAsync(context.Response.Body, weather);
                    });
    ```

5. We're now going to modify the project to add log our weather before writing
directly to the HTTP response body.

    ```csharp
                    endpoints.MapGet("/", async context =>
                    {
                        var weather = new { MinTemperature = 61.2, MaxTemperature = 89.1, Humidity = 81.0, City = "Miami" };
                        var logger = context.RequestServices.GetService<ILogger<Startup>>();
                        logger.LogInformation("Max temperature in {City} is {MaxTemperature}", weather.City, weather.MaxTemperature);
                        await JsonSerializer.SerializeAsync(context.Response.Body, weather);
                    });
    ```
