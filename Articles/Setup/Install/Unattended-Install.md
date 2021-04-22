---
v8-equivalent: "https://github.com/umbraco/UmbracoDocs/blob/main/Getting-Started/Setup/Install/Unattended-Install.md"
versionFrom: 9.0.0
verified-against: alpha-4
state: partial
updated-links: false
---

# Unattended Installs

In some cases, you might need to install one or multiple Umbraco instances automatically without having to run through the installation wizard to configure the instance.

You can use the **Unattended installs** feature to allow for quick installation and set up of Umbraco instances on something like Azure Web Apps.

This article will give you the details you need to install Umbraco unattended.

:::warning
    ????
:::
<!--- Would we have any warnings? below is what we had before
:::warning
When installing Umbraco using the unattended install feature, you will not be able to access the Umbraco backoffice once the installation has completed, as no password has been set for the default user.

Instead, you will need to configure an external login provider or set a password for the user in some other way.

Support for adding users through the unattended installation process will be added at a later point.
:::
-->

## Get the correct version of Umbraco

Since its beginning **Umbraco version 9** supports this feature.

So it will be enough to get a clean instance from either the [NuGet feed](https://www.nuget.org/packages/UmbracoCms/) or download a zip file directly from [Downloads](https://our.umbraco.com/download).

## Configure your database

As you will not be running through the installation wizard when using the unattended installs feature, you need to manually tell Umbraco which database to use.

* Set up and configure a new database - see [Requirements](../Requirements/#hosting) for details.
* Add the connection string using configuration.

Example:

```

```

## Define the correct Umbraco version

In order for Umbraco to be installed correctly, you will need to specify the exact version number before initializing the installation.

The version needs to be specified using ...

Example:

```


```

## Enable the unattended installs feature

The unattended installs feature is disabled by default and in order to enable it, you need to add the following JSON object to a JSON configuration source.

```json
{
    "Unattended": {
                    "InstallUnattended": true,
                    "UnattendedUserName": "FRIENDLY_NAME",
                    "UnattendedUserEmail": "EMAIL",
                    "UnattendedUserPassword": "PASSWORD"
                }
}

```

Remember to set the value of `InstallUnattended` to `true`.

## Initialize the unattended install

After completing the steps above you can now initialize the installation by booting up the Umbraco instance.

Once it has completed, you should see the following when visiting the frontend of the site.

![Frontend of Umbraco site installed using the unattended installs feature](images/Unattended/final-screen.png)

## Configuration options
Depending on your preferences, you can use any type of configuration to specify the connection string and login information, as well as enable unattended install. Due to the new configuration functionality, it is possible to read from all kinds of sources. One example can be using a JSON file or environment variables.  

**Program.cs** has a condition, which if met - *appsettings.Local.json* file will be added and configured as a configuration source. 

```c#
#if DEBUG
                .ConfigureAppConfiguration(config
                    => config.AddJsonFile(
                            "appsettings.Local.json",
                            optional: true,
                            reloadOnChange: true))
#endif
```

Having intellisense will help you to easily add your connection string and information needed for the unattended install.

```json
{
    "ConnectionStrings": {
        "umbracoDbDSN": "CONNECTION_STRING"
    },
    "Umbraco": {
        "CMS": {
            "Unattended": {
                "InstallUnattended": true,
                "UnattendedUserName": "FRIENDLY_NAME",
                "UnattendedUserEmail": "EMAIL",
                "UnattendedUserPassword": "PASSWORD"
            }
        }
    }
}
```

## More support
We have added support for unattended installs with Name, Email & Password & Connection String as CLI params & in Visual Studio. There you can filling your information as follows:

### CLI

```powershell
dotnet new umbraco -n MyNewProject --FriendlyName "Friendly User" --Email user@email.com --Password password1234 --ConnectionString "Server=(localdb)\\Umbraco;Database=MyDatabase;Integrated Security=true" --version 9.0.0
```

### Visual Studio
![Set up unattended install through Visual Studio](images/Unattended/VS-unattended-install.png)