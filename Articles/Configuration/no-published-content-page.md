# No Published Content Page

When Umbraco is first installed, if no starter kit is chosen, there will be no content created in the backoffice, and no published content to display.

It's also possible to create content, but have it unpublished.

If browsing the front-end of the website, when this situation of no published content is found, a custom page is displayed.

This page is served from `~/umbraco/UmbracoWebsite/NoNodes.cshtml`.

## Customising The Page

Whilst the contents of this page can be edited directly, this isn't recommended, as care would need to be taken to avoid the changes getting overwritten in upgrades.

Instead there are two options.

To return the contents of a different Razor file, create this file at your chosen location and configure it's use within the `Global` section of the `appsettings.json` file:

```
"Umbraco": {
    "CMS": {
      ...
      "Global": {
        ...
        "NoNodesViewPath": "~/Views/CustomNoNodes.cshtml",
        ...
      },
	  ...
```

For finer control, you can write code to handle the route directly, allowing the implementation of a custom controller, view model and view:

```
    public class CustomNoNodesComposer: ComponentComposer<CustomNoNodesComponent>, IUserComposer
    {
    }

    public class CustomNoNodesComponent : IComponent
    {
        public void Initialize()
        {
            if (RouteTable.Routes[Constants.Web.NoContentRouteName] is Route route)
            {
                route.Defaults = new RouteValueDictionary()
                {
                    ["controller"] = "CustomNoNodes",
                    ["action"] = "Index"
                };
            }
        }

        public void Terminate() { }
    }
```
