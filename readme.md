# GreenPass
## Demo [GreenPass API](https://greenpassapi.azurewebsites.net)
To use the demo, call any of the methods available under the Validation controller passing GreenPass raw string as input (e.g. HC1:...)
The trust list and business rules are updated every 24h.
This demo adheres to Italian official rules for date expiration calculation but is provided without any legal guarantee, no SLA and no uptime guarantee.

## By [Luca Pisano](https://lucapisano.it)

This package enables Green Pass QR code reading and reading
NuGet available at [NuGet.Org/packages/GreenPass](https://www.nuget.org/packages/GreenPass)
## Features

- .NET Standard 2.0 runs on any platform. Docker, Windows, Linux, MacOS, Android, iOS, Smartwatches, Raspberry, Embedded Controllers, ecc
- Decode QR Code string into readable JSON
- Verify GreenPass using official Trust List servers
- Validation certificates caching for configurable TimeSpan to improve performance (default is 24h)
- Support for custom Trust List servers is provided using appsettings.json
- Very Green Pass expiry date according to Italian rules

Current implementation does not check for certificate revocation lists, if any jurisdiction uses them.

```csharp
//In ASP.NET Core projects, call .AddGreenPassValidator() in ConfigureServices() method
public void ConfigureServices(IServiceCollection services)
        {
            services.AddGreenPassValidator(Configuration)
            ;            
        }
```
```csharp
var res = await _sp.GetRequiredService<ValidationService>().Validate(scanResult);
if(res.IsInvalid)
/*certificate is invalid, either expired or not yet valid*/
```
This is an example of how you can configure it
```json
{
  "ValidatorOptions": {
    "CertificateListProviderUrl": "https://dgcg.covidbevis.se/tp/trust-list"    
    }
}
```
Actual rules verification is implemented for Italian DGC server format available at https://get.dgc.gov.it/v1/dgc/settings
Special thanks to the authors of DGCValidator https://github.com/ehn-dcc-development/DGCValidator

Any artifact present in this repository, code included, is made under pure personal terms and is not related to my job.
