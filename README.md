# programming_error_solutions
Įvairių konfigūracijos ir kitų problemų sprendimai atimantis daug laiko :)

### PROBLEMA NR. 1, Asp .NET Core C# projekto OAUTH2/OIDC išorinis provaideris (angl. External Provider)

Jeigu turime callback kelią pvz. ```options.CallbackPath = new PathString("/signin-oidc");``` jokiais būdais šis kelias negali sutapti su programoje egzistuojančiu Controller keliu (angl. route), pvz. 
```C# 
[HttpGet("/signin-oidc")]
public async Task<IActionResult> SignInOidcCallback(string returnUrl = "/"){...} 
```

### OPENID konfigūracijos fragmentas su išorinio provaiderio Callback keliu.
```C#
.AddOpenIdConnect("oidc", options =>
{
    options.SignInScheme = IdentityConstants.ExternalScheme;
    options.SignOutScheme = IdentityConstants.ExternalScheme;

    options.Authority = "https://login-test.XXXXXXXXXXXX.org";
    options.ClientId = "XXXXWebsite";
    options.ClientSecret = "XXXXXXXXXXXXXXXXfJFT3jlkrN";
    options.ResponseType = "code id_token";

    options.Scope.Add("openid");
    options.Scope.Add(JwtClaimTypes.Email);
    options.Scope.Add(JwtClaimTypes.Profile);
    
    options.SaveTokens = true;
    
    options.CallbackPath = new PathString("/signin-oidc");

    options.RequireHttpsMetadata = false;

    options.TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name", RoleClaimType = "role"
    };
});
```
