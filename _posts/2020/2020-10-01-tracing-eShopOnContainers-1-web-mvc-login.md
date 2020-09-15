---
layout: post
title: 'Tracing eShopOnContainers (1): WebMVC Login'
subtitle: ''
categories: [Programming]
tags: [Programming, eShopOnContainers, Microservices]
date: 2020-10-01 01:00:00 PM UTC
---

<!-- started September 10, 2020 12:00 PM Philippine Time -->

The purpose of this post is to trace the integration between the `WebMVC` and the `Identity.API` projects of [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers).

`WebMVC` is an ASP.NET MVC project and `Identity.API` is an IdentityServer4 project.

<!--more-->


### Step 1: Run eShopOnContainers

Open the eShopOnContainers solution in Visual Studio 2019, then wait until the browser opens to `http://localhost:5100/`.

You will be shown the home page of eShopOnContainers:

![eshop-webmvc-app-screenshot.png](https://raw.githubusercontent.com/dotnet-architecture/eShopOnContainers/0cb8424fdca35f99e859cfdd72f6311469545fe4/img/eshop-webmvc-app-screenshot.png)

(If you are shown an error instead of the home page, it is because the other microservices are not yet ready. Just wait for a few more seconds then refresh the page.)

**Now**, open your browser to `http://host.docker.internal:5100/`. **Do not use** the default `http://localhost:5100/` because it will result to [an `unauthorized_client` error when you navigate to the login page](/2020/08/18/eShopOnContainers-error-unauthorized_client).


### Step 2: Pre-Login

Click on the Login button.

This is the code for the Login button, taken from `\src\Web\WebMVC\Views\Shared\_LoginPartial.cshtml`:

``` html
<a asp-area="" asp-controller="Account" asp-action="SignIn" class="esh-identity-name esh-identity-name--upper">
    Login
</a>
```

Clicking on the Login button will invoke the `SignIn` action method in the `AccountController` class of the `WebMVC` project:

``` csharp
public class AccountController : Controller
{
    ...
    [Authorize(AuthenticationSchemes = OpenIdConnectDefaults.AuthenticationScheme)]
    public async Task<IActionResult> SignIn(string returnUrl)
    {
        ...
    }
    ...
}
```

The `[Authorize]` attribute on that action method [triggers the authentication handshake](http://docs.identityserver.io/en/aspnetcore2/quickstarts/3_interactive_login.html). Before it enters the `SignIn` method, it first checks if the user is already logged-in. If the user is not yet logged-in, it will return a response with a `302 Found redirect` status code and the `Location` to redirect to:

<!-- 
```
HTTP/1.1 302 Found
Date: Sun, 13 Sep 2020 14:20:41 GMT
Server: Kestrel
Content-Length: 0
Location: http://host.docker.internal:5105/connect/authorize?client_id=mvc&redirect_uri=...
``` 
-->

```
HTTP/1.1 302 Found
Date: Sun, 13 Sep 2020 14:20:41 GMT
Server: Kestrel
Content-Length: 0
Location: http://host.docker.internal:5105/connect/authorize?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637356036421601745.MmU1ZTBlZjMtNWMwYS00ODBhLWE4Y2EtN2QyNDcwN2EzNzViODRkYjhlNGYtNzFkNy00ZDkyLWJiYjktNzViODQ1ZTkyOWU1&state=CfDJ8BrGg2aS-ORHvUrWZLrxrPU0hJlarrhXIcvLS-TGsQefnMUxM-DMAYQvnCmhCc2NiVrTWi7z3vifu0W1a5D5Oe9iZk1gIku_g7E8qa9rdRsk20rCaioXby6UgIjYBPE_6P9P7AnQKy32DkXxjuzhhXS1J1lacoaC9Qp1IhSje3wKnKB5Oqz3Q0_9_oG0GaUHW-I4_5P37ESmnXpxRsk9dq3XRHIMrdfBox7OCIc5T9lyZBOb_1J5hn5S5-saRwgmr4zkRAEweu8hSVhfD_oGnZ9xUF0RP19jgHPgY7NkYqf5NSWaiboOLsFnTyhUB--DA_AYVPVfu1uidzsiD-GDYck&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0
Set-Cookie: .AspNetCore.OpenIdConnect.Nonce.CfDJ8BrGg2aS-ORHvUrWZLrxrPW-BLHqHtuOa5roHvl3vq37w5FhJ0DRbJ45Bw-735liSqI7y8NIPZuoOXxOJK4tzKG4hrT6PnGVGATjQim7pRb2hKCDOyhnNJ0XZdQEo0-mnod9kGk0F3hNm3IH0JR9AUxHgCjIz9f_xKLz2kI_JwSKS64NkJLF56rSJTdrVIowE30jnJFyi2d1ufRzd3qELbbajre3RLV6tCZ6wSD-K52vavqid-GTRDL8PI7zfW3Cdov59tZ_YNeuKbVUvib_x04=N; expires=Sun, 13 Sep 2020 14:35:42 GMT; path=/signin-oidc; samesite=none; httponly
Set-Cookie: .AspNetCore.Correlation.OpenIdConnect.e1A7qhbZI5hv5LErSQh8Jcxi5-f3g8W-x3KxYbBKI_A=N; expires=Sun, 13 Sep 2020 14:35:42 GMT; path=/signin-oidc; samesite=none; httponly
```

You might be asking how the Web MVC project knows the `Location` to redirect to. It is through this code you can see in `Startup.cs` of the `WebMVC` project:

``` csharp
services.AddAuthentication(...)
.AddOpenIdConnect(options =>
{
    options.SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;    
    options.Authority = identityUrl.ToString(); // http://localhost:5105 in appsettings.json
    options.SignedOutRedirectUri = callBackUrl.ToString(); // http://localhost:5100/ in appsettings.json
    options.ClientId = useLoadTest ? "mvctest" : "mvc";
    options.ClientSecret = "secret";
    ...
});
```

The `Authority` URL in that setup points to the `Identity.API` microservice -- `http://localhost:5105`. So your `WebMVC` project now knows the location of your `Identity.API`.

Then `Identity.API` provides a what they call the _discovery document_, which in our case is located in `https://localhost:5105/.well-known/openid-configuration`.

The [IdentityServer4 documentation](https://identityserver4.readthedocs.io/en/latest/quickstarts/1_client_credentials.html) says that

> The discovery document is a **standard endpoint** in identity servers. The discovery document will be used by your clients and APIs to download the necessary configuration data.

The discovery document contains this:

``` json
{
    "issuer": "null",
    "jwks_uri": "http://localhost:5105/.well-known/openid-configuration/jwks",
    "authorization_endpoint": "http://localhost:5105/connect/authorize",
    ...
}
```

Notice that the `authorization_endpoint` is the same as the `Location` response header you encountered when logging in above. That is how the Web MVC project knows the `Location` to redirect to.

If you ask how you will know the `ClientId` and `ClientSecret` to use for the authentication setup, you will need to communicate with the maintainers of the IdentityServer project. But in eShopOnContainers we have access to the IdentityServer code base, the `Identity.API` project. Those information can be found in `\src\Services\Identity\Identity.API\Configuration\Config.cs`:

``` csharp
new Client
{
    ClientId = "mvc",
    ClientName = "MVC Client",
    ClientSecrets = new List<Secret>
    {
        new Secret("secret".Sha256())
    },
    ClientUri = $"{clientsUrl["Mvc"]}", // public uri of the client
    ...
    RedirectUris = new List<string> 
    {
        $"{clientsUrl["Mvc"]}/signin-oidc"
    },
    PostLogoutRedirectUris = new List<string> 
    {
        $"{clientsUrl["Mvc"]}/signout-callback-oidc"
    },
    ...
},
```


Now, after the `WebMVC` redirects to `http://host.docker.internal:5105/connect/authorize`, the `Identity.API` responds with a `302 Found redirect`, with `Location` pointing to `http://host.docker.internal:5105/Account/Login`, which points to the `Login` action method of the `AccountController` inside the `Identity.API` project

```
HTTP/1.1 302 Found
Date: Sun, 13 Sep 2020 14:20:41 GMT
Server: Kestrel
Content-Length: 0
Location: http://host.docker.internal:5105/Account/Login?ReturnUrl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637356036421601745.MmU1ZTBlZjMtNWMwYS00ODBhLWE4Y2EtN2QyNDcwN2EzNzViODRkYjhlNGYtNzFkNy00ZDkyLWJiYjktNzViODQ1ZTkyOWU1%26state%3DCfDJ8BrGg2aS-ORHvUrWZLrxrPU0hJlarrhXIcvLS-TGsQefnMUxM-DMAYQvnCmhCc2NiVrTWi7z3vifu0W1a5D5Oe9iZk1gIku_g7E8qa9rdRsk20rCaioXby6UgIjYBPE_6P9P7AnQKy32DkXxjuzhhXS1J1lacoaC9Qp1IhSje3wKnKB5Oqz3Q0_9_oG0GaUHW-I4_5P37ESmnXpxRsk9dq3XRHIMrdfBox7OCIc5T9lyZBOb_1J5hn5S5-saRwgmr4zkRAEweu8hSVhfD_oGnZ9xUF0RP19jgHPgY7NkYqf5NSWaiboOLsFnTyhUB--DA_AYVPVfu1uidzsiD-GDYck%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0
Content-Security-Policy: script-src 'unsafe-inline'
```

Now you might be asking how `Identity.API` knows that it should redirect to `http://.../Account/Login`. If you search the codebase, you will not find the answer to your question. I found the answer in the [doucumentation](https://docs.identityserver.io/en/dev/topics/signin.html#login-workflow):

> When IdentityServer receives a request at the authorization endpoint and the user is not authenticated, the user will be redirected to the configured login page. You must inform IdentityServer of the path to your login page via the `UserInteraction` settings on the options (the default is `/account/login`)...

It is supposed to be written in the `Startup.cs` of the `Identity.API` project, like this:

``` csharp
services.AddIdentityServer(x =>
{
    x.UserInteraction = "/Account/Login"
    ...
})
```

But because `/Account/Login` is the default, there is no need to add it in the code.


### Step 3: Login

You are now presented with the login page from the `Identity.API` microservice.

![login page](https://user-images.githubusercontent.com/1712635/32029607-95d58f4e-b9aa-11e7-90a4-fea616c1a865.png)


**I will end this here.** You can trace the rest of the code by yourself.

But one last thing. You will see this `SignInAsync` code in the `AccountController` of the `Identity.API` project, which is executed during login:

``` csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginViewModel model)
{    
    ...
    await _loginService.SignInAsync(user, props);
    ...
}
```

The [ASP.NET docs](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/cookie?view=aspnetcore-3.1) says that

> `SignInAsync` creates an encrypted cookie and adds it to the current response.

**After you login**, you can view that chunked encrypted cookie using your browser's dev tools, like in this image:

![chuncked encrypted cookie created by HttpContext.Authentication.SignInAsync()](https://web.archive.org/web/20170809191300im_/https://i.stack.imgur.com/p2Gy8.png)

That encrypted cookie is where code such as 

``` csharp
var token = await HttpContext.GetTokenAsync("access_token");
``` 

and

``` csharp
async Task<string> GetToken()
{
    const string ACCESS_TOKEN = "access_token";

    return await _httpContextAccesor.HttpContext
        .GetTokenAsync(ACCESS_TOKEN);
}
``` 

in your `WebMVC` app gets the access token from.


-----

<div class="small" markdown="1">

#### References:

- [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)
- [Which OAuth 2.0 Flow Should I Use?](https://auth0.com/docs/authorization/which-oauth-2-0-flow-should-i-use) from Auth0 docs
- [An Illustrated Guide to OAuth and OpenID Connect](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc) by David Neal from Okta
- [ASP.NET Core, C#, IdentityServer4 - Authentication - Tricking Library Ep28](https://www.youtube.com/watch?v=Ql0ZB67J0TQ&list=PLOeFnOV9YBa4LslgNo31ukBrwpJTz7BzM&index=31&t=0s&ab_channel=RawCoding) by Anton Wieslander of Raw Coding
- [IdentityServer4 documentation](https://identityserver4.readthedocs.io/en/latest/quickstarts/1_client_credentials.html)
- [User Authentication and Identity with Angular, Asp.Net Core and IdentityServer](https://fullstackmark.com/post/21/user-authentication-and-identity-with-angular-aspnet-core-and-identityserver) by Mark Macneil
- [Login Workflow](https://docs.identityserver.io/en/dev/topics/signin.html#login-workflow) from the IdentityServer4 docs
- [Accessing the OIDC tokens in ASP.NET Core 2.0](https://www.jerriepelser.com/blog/accessing-tokens-aspnet-core-2/)
- [How to manually decrypt an ASP.NET Core Authentication cookie?](https://web.archive.org/web/20170809191253/https://stackoverflow.com/questions/42842511/how-to-manually-decrypt-an-asp-net-core-authentication-cookie)
- [ASP.NET Core 3 - IdentityServer4 - Ep.12 Cookies, id_token, access_token, Claims](https://www.youtube.com/watch?v=bcfThLdEcOM&ab_channel=RawCoding) by Anton Wieslander of Raw Coding

</div>
