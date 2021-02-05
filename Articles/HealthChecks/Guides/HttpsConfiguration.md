# Health check: HTTPS Configuration

_Checks if your site is configured to work over HTTPS and if the Umbraco related configuration for that is correct._

## How to fix this health check
This health check actuall checks a couple of things.
First of all, ensure you website is running on HTTPS using a valid certificate. Furthermore, ensure to specific the  configuration on the following path: `Umbraco:CMS:Global:UseHttps`.

This configuration can be setup in a configuration source of your choice, but this guide only shows how to setup in one of the json file sources.

#### Updating the json configuration
The following json needs to be merged into one of you json sources. By default the following json sources are used: `appSettings.json`
and `appSettings.<environment>.json`, e.g. `appSettings.Development.json` or `appSettings.Production.json`.

```json
{
    "Umbraco": {
        "CMS": {
            "Global": {
                "UseHttps": <false,true>
            }
        }
    }
}
```

One example that can be used:
```json
{
    "Umbraco": {
        "CMS": {
            "Global": {
                "UseHttps": true
            }
        }
    }
}
```
