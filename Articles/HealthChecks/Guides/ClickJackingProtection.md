# Health check: Click-Jacking Protection

_Checks if your site is allowed to be IFRAMEd by another site and thus would be susceptible to click-jacking._

## How to fix this health check
This health check can be fixed by adding a header before the response is started.

Preferable you use a security library like [NWebSec](https://docs.nwebsec.com/).

#### Adding Click-Jacking Protection using NWebSec
If you take a NuGet dependency on [NWebsec.AspNetCore.Middleware/](https://www.nuget.org/packages/NWebsec.AspNetCore.Middleware/), you can use third extension methods on `IApplicationBuilder`.
```cs
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseXfo(options => options.Deny());

        ...
    }
}
```


#### Adding Click-Jacking Protection using manual middleware
If you don't like to have a dependency on third party library. You can add the following custom middleware to the request pipeline.
```cs
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            context.Response.Headers.Add("X-Frame-Options", "DENY");
            await next();
        });

       ...
    }
}
```
