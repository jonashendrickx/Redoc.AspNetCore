This project was started because `Swashbuckle` is no longer being maintained, and will be dropped from .NET templates with the .NET 9 release. This library will guarantee a smooth transition from .NET 8 to .NET 9.

# Example

By default the UI will be available on the following path:

`BASEURL/api-docs/index.html`

# Configuration

ReDoc ships with its own set of configuration parameters, all described here https://github.com/Rebilly/ReDoc/blob/master/README.md#redoc-options-object. In `Redoc.AspNetCore`, most of these are surfaced through the ReDoc middleware options:

```csharp
app.UseReDoc(c =>
{
    c.SpecUrl("/v1/swagger.json");
    c.EnableUntrustedSpec();
    c.ScrollYOffset(10);
    c.HideHostname();
    c.HideDownloadButton();
    c.ExpandResponses("200,201");
    c.RequiredPropsFirst();
    c.NoAutoAuth();
    c.PathInMiddlePanel();
    c.HideLoading();
    c.NativeScrollbars();
    c.DisableSearch();
    c.OnlyRequiredInSamples();
    c.SortPropsAlphabetically();
});
```

_Using `c.SpecUrl("/v1/swagger.json")` multiple times within the same `UseReDoc(...)` will not add multiple urls._

<h3 id="redoc-inject-custom-css">Inject Custom CSS</h3>

To tweak the look and feel, you can inject additional CSS stylesheets by adding them to your `wwwroot` folder and specifying the relative paths in the middleware options:

```csharp
app.UseReDoc(c =>
{
    ...
    c.InjectStylesheet("/redoc/custom.css");
}
```

It is also possible to modify the theme by using the `AdditionalItems` property, see https://github.com/Rebilly/ReDoc/blob/master/README.md#redoc-options-object for more information.

```csharp
app.UseReDoc(c =>
{
    ...
    c.ConfigObject.AdditionalItems = ...
}
```

<h3 id="redoc-customize-indexhtml">Customize index.html</h3>

To customize the UI beyond the basic options listed above, you can provide your own version of the ReDoc index.html page:

```csharp
app.UseReDoc(c =>
{
    c.IndexStream = () => GetType().Assembly
        .GetManifestResourceStream("YourNamespace.index.html"); // requires file to be added as an embedded resource
});
```

_To get started, you should base your custom index.html on the [default version](src/Redoc.AspNetCore/index.html)_
