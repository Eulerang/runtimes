---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Aplicar HTTPS en todas las páginas de la aplicación
{: #enforce_https}

Para aplicar HTTPS en lugar de HTTP en todas las páginas de la aplicación cuando se ejecuta en {{site.data.keyword.Bluemix}}, es necesario realizar los cambios siguientes en el archivo ".htaccess".

Añada las siguientes sentencias `using` a la clase `Startup`:

```
  using Microsoft.AspNetCore.Mvc;
  using Microsoft.AspNetCore.Rewrite;
```

Añada lo siguiente al método `ConfigureServices` en la clase `Startup`:

```
  // if app is running in {{site.data.keyword.Bluemix_notm}}
  if (!string.IsNullOrEmpty(Environment.GetEnvironmentVariable("BLUEMIX_REGION")))
  {
    services.Configure<MvcOptions>(options =>
    {
      options.Filters.Add(new RequireHttpsAttribute());
    });
  }
```

Añada lo siguiente al método `Configure` en la clase `Startup`:

```
  // if app is running in {{site.data.keyword.Bluemix_notm}}
  if (!string.IsNullOrEmpty(Environment.GetEnvironmentVariable("BLUEMIX_REGION")))
  {
    app.Use(async (context, next) =>
      {
        if (string.Equals(context.Request.Headers["X-Forwarded-Proto"], "https", StringComparison.OrdinalIgnoreCase))
        {
          context.Request.Scheme = "https";
        }

        await next();
      });
    var options = new RewriteOptions()
                .AddRedirectToHttps();

    app.UseRewriter(options);
  }
```
