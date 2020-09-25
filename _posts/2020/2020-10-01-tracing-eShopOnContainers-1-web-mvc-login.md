---
layout: post
title: 'Tracing eShopOnContainers (1): WebMVC Login'
subtitle: ''
categories: [Programming]
tags: [Programming, eShopOnContainers, Microservices]
date: 2020-10-01 01:00:00 AM UTC
---

<!-- started September 10, 2020 12:00 PM Philippine Time -->

The purpose of this post is to trace the integration between the `WebMVC` and the `Identity.API` projects of [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers).

`WebMVC` is an ASP.NET MVC project and `Identity.API` is an IdentityServer4 project. 

This post assumes that you already have knowledge on how ASP.NET MVC works.

It also assumes that you don't know much about OpenID Connect, like I don't know much about it :innocent:

<!--more-->


### Step 1: Run eShopOnContainers

Open the eShopOnContainers solution in Visual Studio 2019, then wait until the browser opens to `http://localhost:5100/` --- that is the URL for the `WebMVC` site.

You will be shown the home page of eShopOnContainers:

![eshop-webmvc-app-screenshot.png](https://raw.githubusercontent.com/dotnet-architecture/eShopOnContainers/0cb8424fdca35f99e859cfdd72f6311469545fe4/img/eshop-webmvc-app-screenshot.png)

(If you are shown an error instead of the home page, it is because the other microservices are not yet ready. Just wait for a few more seconds then refresh the page.)

**Now**, open your browser to `http://host.docker.internal:5100/`. **Do not use** the default `http://localhost:5100/` because it will result to [an `unauthorized_client` error when you navigate to the login page](/2020/08/18/eShopOnContainers-error-unauthorized_client).


### Step 2: Pre-Login

You see that Login button on the top-right of the home page?...

This is the code for that Login button, taken from `\src\Web\WebMVC\Views\Shared\_LoginPartial.cshtml`:

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

The `[Authorize]` attribute on that action method [triggers the authentication handshake](http://docs.identityserver.io/en/aspnetcore2/quickstarts/3_interactive_login.html). Before the execution enters the `SignIn` method, it first checks if the user is already logged-in. If the user is not yet logged-in, it will return a response with a `302 Found redirect` status code and the `Location` to redirect to:

{: .small}
**request:**

{: .small}
```
GET /Account/SignIn HTTP/1.1
Host: host.docker.internal:5100
Connection: keep-alive
Upgrade-Insecure-Requests: 1
DNT: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://host.docker.internal:5100/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,ja;q=0.8
Cookie: .AspNetCore.Antiforgery.9TtSrW0hzOs=CfDJ8NrRmhUhbrhOjfxyAV4PYe32fcMBx_wcUkZFYw-AiZQm1go4ZVekpyQCLWJAMvMWTXCPyemSu-2-AwtEkRWckw22CFJL6Smt3eJqX1qXOkZKYN-HKTk1JDb0M5DiLt3T1gj_3kVEw5CB53H95xjX_x4
```

{: .small}
**response:**

{: .small}
```
HTTP/1.1 302 Found
Date: Thu, 17 Sep 2020 05:56:37 GMT
Server: Kestrel
Content-Length: 0
Location: http://host.docker.internal:5105/connect/authorize?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm&state=CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0
Set-Cookie: .AspNetCore.OpenIdConnect.Nonce.CfDJ8NrRmhUhbrhOjfxyAV4PYe3JHLK25Norm5LsgSd6bivHRQ4csCggp_pow1_S0L5DJiACuPLvHK8bf9LZIRQHSKtjjAj2iksGFSPNdsP_qhGpj7_QiwIXIm4ranX4koDqoPmM55Krmww5UKYhB4o-6Up3RI-LIMEgxRnzLo7pHxuoFnKg1h8uBsvlra2RtDIxq1QhNTdav5p2fQyN8Ni4RyHe9AyBHrt1UjJZRaGDQlyVaantnF5zriVPHYuEjIDW4CFzGrJvN705gZJuddCoRGs=N; expires=Thu, 17 Sep 2020 06:11:37 GMT; path=/signin-oidc; samesite=none; httponly
Set-Cookie: .AspNetCore.Correlation.OpenIdConnect.8pgRX_wtoYfy9hnfHHS2mDvmKJOotASo4lYK42nA830=N; expires=Thu, 17 Sep 2020 06:11:37 GMT; path=/signin-oidc; samesite=none; httponly
```

You might be asking how the Web MVC project knows the URL of the `Location` to redirect to --- `http://host.docker.internal:5105/connect/authorize`. 

It is, partly, through this setup code you can see in `Startup.cs` of the `WebMVC` project:

``` csharp
services.AddAuthentication(...)
.AddOpenIdConnect(options =>
{
    options.SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;

    // identityUrl is http://localhost:5105 in appsettings.json 
    options.Authority = identityUrl.ToString(); 

    // callBackUrl is http://localhost:5100/ in appsettings.json
    options.SignedOutRedirectUri = callBackUrl.ToString();

    options.ClientId = useLoadTest ? "mvctest" : "mvc";
    options.ClientSecret = "secret";
    ...
});
```

The `options.Authority` in that setup points to the URL of our Authentication Server, whose code base is the `Identity.API` project.


<div class='message' markdown="1">

According to the [IdentityServer4 documentation](https://identityserver4.readthedocs.io/en/latest/intro/terminology.html), an Authentication Server is also known as 

> security token service, identity provider, OpenID Connect provider, IP-STS

and its function is to

> issue security tokens to clients

Just think of security tokens as like passwords or keys for now: it determines whether you have access to something or not.

In the case of the eShopOnContainers, the `Identity.API` issues security tokens to the clients which are the `WebMVC`, the `WebSPA`, and the mobile apps projects.

</div>

Also, `Identity.API` provides a what they call the _discovery document_, which in our case is located in `https://localhost:5105/.well-known/openid-configuration`.

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

Notice that the `authorization_endpoint` is the same as the `Location` response header you encountered above when logging in. That is how the `WebMVC` project knows the `Location` to redirect to: it looks at the _standard_ discovery document endpoint and retrieves then `authorization_endpoint` URL from the JSON response.

You will also notice `options.ClientId` and `options.ClientSecret` in `WebMVC`'s `Startup` class. The values for those were taken from `Identity.API`'s `Config.cs`:

``` csharp
new Client
{
    ClientId = "mvc",
    ClientName = "MVC Client",
    ClientSecrets = new List<Secret> { new Secret("secret".Sha256()) },
    ClientUri = $"{clientsUrl["Mvc"]}", // public uri of the client
    AllowedGrantTypes = GrantTypes.Hybrid,
    ...
    RedirectUris = new List<string> { $"{clientsUrl["Mvc"]}/signin-oidc" },
    PostLogoutRedirectUris = new List<string> { $"{clientsUrl["Mvc"]}/signout-callback-oidc" },
    ...
},
```


Now, the call t `http://host.docker.internal:5105/connect/authorize` gets a `302 Found redirect` response, with the `Location` header pointing to `http://host.docker.internal:5105/Account/Login`, 


<!-- 
which points to the `Login` action method of the `AccountController` inside the `Identity.API` project
 -->

{: .small}
**request:**

{: .small}
```
GET /connect/authorize?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm&state=CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0 HTTP/1.1
Host: host.docker.internal:5105
Connection: keep-alive
Upgrade-Insecure-Requests: 1
DNT: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://host.docker.internal:5100/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,ja;q=0.8
Cookie: .AspNetCore.Antiforgery.9TtSrW0hzOs=CfDJ8NrRmhUhbrhOjfxyAV4PYe32fcMBx_wcUkZFYw-AiZQm1go4ZVekpyQCLWJAMvMWTXCPyemSu-2-AwtEkRWckw22CFJL6Smt3eJqX1qXOkZKYN-HKTk1JDb0M5DiLt3T1gj_3kVEw5CB53H95xjX_x4
```

{: .small}
**response:**

{: .small}
```
HTTP/1.1 302 Found
Date: Thu, 17 Sep 2020 05:56:37 GMT
Server: Kestrel
Content-Length: 0
Location: http://host.docker.internal:5105/Account/Login?ReturnUrl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm%26state%3DCfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0
Content-Security-Policy: script-src 'unsafe-inline'
```

If you search the `Identity.API` codebase you will not find any code which says _"redirect to `http://.../Account/Login`"_. You might be asking how `Identity.API` knows where to redirect to. I found the answer to that question in the [IdentityServer4 doucumentation](https://docs.identityserver.io/en/dev/topics/signin.html#login-workflow):

> When IdentityServer receives a request at the authorization endpoint and the user is not authenticated, the user will be redirected to the configured login page. You must inform IdentityServer of the path to your login page via the `UserInteraction` settings on the options (the default is `/account/login`)...

It is supposed to be written in the `Startup.cs` of `Identity.API`, like this:

``` csharp
services.AddIdentityServer(x =>
{
    x.UserInteraction = "/Account/Login"
    ...
})
```

But because `/Account/Login` is the default, there is no need to put it in the code.

Yehey!!


### Step 3: Login

You are now presented with the login page from the `Identity.API` microservice.

![login page](https://user-images.githubusercontent.com/1712635/32029607-95d58f4e-b9aa-11e7-90a4-fea616c1a865.png)


**I will end the tracing here.** You can trace the rest of the Login code by yourself.

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

in your `WebMVC` project gets the access token from.

And, by the way, here is the Login Flow from the eShopOnContainers' [wiki](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Code-Flow), in case you need it when tracing the code for Login:

![Login Flow from the eShopOnContainers' wiki](https://raw.githubusercontent.com/wiki/dotnet-architecture/eShopOnContainers/images/Code-Flow/user-login.png)


-----

**Another thing to think about until a hundered years from now:** The authentication handshake between `WebMVC` and `Identity.API` uses this what-they-call ["Hybrid Flow"](https://connect2id.com/learn/openid-connect), which involves the exchange of an authorisation code. I cannot figure out yet in which call `Identity.API` passes the authorization code to `WebMVC`.

These are the **first three** HTTP calls when logging in:

**1st call:**

{: .small}
**request:**

{: .small}
```
POST /Account/Login?returnurl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm%26state%3DCfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0 HTTP/1.1
Host: host.docker.internal:5105
Connection: keep-alive
Content-Length: 1124
Cache-Control: max-age=0
Origin: http://host.docker.internal:5105
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://host.docker.internal:5105/Account/Login?ReturnUrl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm%26state%3DCfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,ja;q=0.8
Cookie: .AspNetCore.Antiforgery.9TtSrW0hzOs=CfDJ8K9Ehnko85pHtZ-5L4xRwA9P8XrBJ_95B7lEZ9kX21Ezh87o5vGSmCqEf83ishLyA0EMoQSD3LOpvci9Bn8jmxddGH47scc78qbjGfYKAyip3oVfCRLkZabrXcQoNpXmj27RnFMxDbcTuU_LIYTHDYw
```


{: .small}
**response:**

{: .small}
```
HTTP/1.1 302 Found
Date: Thu, 17 Sep 2020 05:56:44 GMT
Server: Kestrel
Content-Length: 0
Cache-Control: no-cache
Pragma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Location: /connect/authorize/callback?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm&state=CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0
Set-Cookie: idsrv.session=1kWSRVmsC8FWnqTprEEyLQ; path=/; samesite=lax
Set-Cookie: .AspNetCore.Identity.Application=CfDJ8K9Ehnko85pHtZ-5L4xRwA8hd7VOBYxBPfXslW7ZOC0J8RuHlDxXMWD8ymRu9sKbufCQrvSmkvUbnhXkQ61kr_vnoU-dFo2ANB08C6etWFbGi7L4DUWvV_w820lCNZGna6t73ky1S2w-O3GGfIkFDA7dUbzfYxXz5sLlEtvMfT2syxhYFHS6UecMAqIA-YXNxx0W_wZjp7NewBmITsYfVyiG3jdSAETQpsjw9XJMQDuHQ0YsI6vSTqtTlwzwEAxIR6kLW-xPFpf_cY-rluhJw6RLa05NVRHT_gkJN6KtIt5HTUQBujfzH_i5IKC7CQe0yJakp3_LDtTaATXWTIx38b4wdLqNlWAZpL6mcqY4_mKsJhvrHzYChYcrxQmWbVphXO_x8cj77wFpLiIYxv6LFskb51jZQN9Te_rv0jNsx_fqZhR6vgodWmte-_gi0PXQwmlxery7151DI5DFQVg1flH_ApP0czNqm8_-9QFBIBtD_JljRvV8A7iNxeDmx3nXitmK-iChX0G9FKaGtaQvpd7MWvhv2GhoWtQ1G6LNXzFeEc8N2fuJ0mn7EcQOcSe3kEj6YTVi1d9dJCDCPJNiolTVCj-8rhj2rQ2Y0uWH6uqLvzlmmZA2kjT6ssBINRZbRo7d6DcSTS0YnDK6eJTwfqvvu-yimxHPutyZCC69DhwKpsDmGKGF8ztuGSIv5jTisblofOdsM-wP0-IMn1rexyf7FW1kmAfvpBxhmolmYPcd6V6JK7qk4WQIy74EPTv_0xuWIidmF5rqGM5xb0DOFE44_iKdRqlQJt3ZkdZsbpYY2e8voNg0D3TOqW8h1dMYAcz9SELVqIGYVsNUwK3t0ItWZvGnXnSyDBpO0izAq_vEr-_TgcYku_iaQlZbsMCpPwcLzbJzp9k5D-yff2AX6hmgD6nmtrvY5k6P75oE948bVRovaSEKgPP0KvMWJD6nP7dmRbOmCNbENy7E1fIYbpjqsoMNwSCrlkewV19FOslM0DDUi969sHLlkU1TnfaHdyoP52Zy5sqhE7CM_RvLK62IZ4pTJm5lalt95KQLcbnGskIrdwhNll1Ad52vS7Ss5NfwzX2lBINJyPnMNvw-0gv8PGgop2iBiBlxXhQz0zCznamlbivnBL9lOx30yu_SeXCcQx8gLV1IVTT5g3LY7SrMIMCMOZnnqE2aCdx0VkNCpHyhiTjWa9hCoh3BaAAFvEff7Ps8C4kg2tUwJYg2ykcqkPJ2FzJO9StZsyIWF40mDdi-7g-_wDLXLkritzRl-UFC_JLxez6x8UMkPob395HoFVTK1P3QP61ngtY-5q5mDAP7YYp-J4HH272VrFkiEPqm9soyrRMyXeJ_225CKsM-kFuVv4v3K4v03YP4k4sxIbe7vn9o9h3liRRDjSSoGagUk4DfMZh8tjBpWikGug0w2gxkKL-AtoWmHS3L714WMrAWcTTeslPzGBiuGuK-qA77W5wzW9Qkan3NSA1os5tnbseZCUstth7IIk4gCH239zUUghOUhWyxFNJfiRa8laF3wBy6aYQewUGykI_4RbWPcrYBSmqp-_J0oSSs1eMse5WtUgU_cln1Bq07hoWt7uv7J-bwE5BkXX6ice8lOwrnN3rX2Y_gRGXdVdB5wC5RgpYtjkiOpmR-hx2u4VZ8EjCkMadt1nvUvTEbTESq-z6oKPziuxDiG8lQCKRPvmPfp-v5LQb88OysEXt3qS4seWW21k9twbdVdBYusz0KTu4-HHOe5v_67GoPjy_b_w1Pf1N7MtBySBKuUW7QqwtV_4bOR5iqImDMN5b05Etnt7NJbw3h_j-8lGu7w_QFvLU_y80hYSHneXvhh7EnztCDeQ6eHB-QPbZquPZVE77_TyDYxn2ECUu9D-c33TQS5Az-sOid6teglezIhz-LQTaH0d9lEYue73uZ63rnqiyW2deVjMUlDHwGyXpFg7lU1oYX-TivstkNzDuGHkDyQCD5nETQ49g3DF9BXbYF7Ow5Q2ohoI7PAwfV85Kl54aSGWaisUecZNzC6oilZAJqISeOJoQ0Xeza816iW2Xt1i8YhSPByd68ROwjApEhBWdjWdXduZVfXKzW8aldpWqV2rnQg0LWJ-H1WSW-NlCs67GzvTpib8K7nkeY7_LFqgCfhVLrcBAetGw_QDljrVDTdvGvfQ; path=/; samesite=lax; httponly
Content-Security-Policy: script-src 'unsafe-inline'
```


The guide ["OpenID Connect explained" from Connect2id](https://connect2id.com/learn/openid-connect) seems to suggest that this step is where the authorization code (`code=SplxlOBeZQQYbYS6WxSbIA` in the sample below) should be passed from the Authorization Server to the Client:

> ```
> HTTP/1.1 302 Found
> Location: https://client.example.org/cb?
>           code=SplxlOBeZQQYbYS6WxSbIA
>           &state=af0ifjsldkj
> ```

But I cannot find a `code` in the query string of the `Location` in the response of the **1st call**. But I can see a `state` --- `state=CfDJ8NrRmhUhbrhOjfxyAV4P...`

```
HTTP/1.1 302 Found
...
Location: /connect/authorize/callback?client_id=mvc&...
          state=CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk
...
```


Hmmm...


**2nd call:**

{: .small}
**request:**

{: .small}
```
GET /connect/authorize/callback?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm&state=CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0 HTTP/1.1
Host: host.docker.internal:5105
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
DNT: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://host.docker.internal:5105/Account/Login?ReturnUrl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637359189979543914.NzdjMmVjYmEtMDU4Zi00OGE0LTgyMDktOGYwZjQyNjdlMjdkOWM5YzMyYjQtYTU1Yy00ZTRkLTg4YTEtMDA0MjU3YmQ4YjZm%26state%3DCfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,ja;q=0.8
Cookie: .AspNetCore.Antiforgery.9TtSrW0hzOs=CfDJ8K9Ehnko85pHtZ-5L4xRwA9P8XrBJ_95B7lEZ9kX21Ezh87o5vGSmCqEf83ishLyA0EMoQSD3LOpvci9Bn8jmxddGH47scc78qbjGfYKAyip3oVfCRLkZabrXcQoNpXmj27RnFMxDbcTuU_LIYTHDYw; idsrv.session=1kWSRVmsC8FWnqTprEEyLQ; .AspNetCore.Identity.Application=CfDJ8K9Ehnko85pHtZ-5L4xRwA8hd7VOBYxBPfXslW7ZOC0J8RuHlDxXMWD8ymRu9sKbufCQrvSmkvUbnhXkQ61kr_vnoU-dFo2ANB08C6etWFbGi7L4DUWvV_w820lCNZGna6t73ky1S2w-O3GGfIkFDA7dUbzfYxXz5sLlEtvMfT2syxhYFHS6UecMAqIA-YXNxx0W_wZjp7NewBmITsYfVyiG3jdSAETQpsjw9XJMQDuHQ0YsI6vSTqtTlwzwEAxIR6kLW-xPFpf_cY-rluhJw6RLa05NVRHT_gkJN6KtIt5HTUQBujfzH_i5IKC7CQe0yJakp3_LDtTaATXWTIx38b4wdLqNlWAZpL6mcqY4_mKsJhvrHzYChYcrxQmWbVphXO_x8cj77wFpLiIYxv6LFskb51jZQN9Te_rv0jNsx_fqZhR6vgodWmte-_gi0PXQwmlxery7151DI5DFQVg1flH_ApP0czNqm8_-9QFBIBtD_JljRvV8A7iNxeDmx3nXitmK-iChX0G9FKaGtaQvpd7MWvhv2GhoWtQ1G6LNXzFeEc8N2fuJ0mn7EcQOcSe3kEj6YTVi1d9dJCDCPJNiolTVCj-8rhj2rQ2Y0uWH6uqLvzlmmZA2kjT6ssBINRZbRo7d6DcSTS0YnDK6eJTwfqvvu-yimxHPutyZCC69DhwKpsDmGKGF8ztuGSIv5jTisblofOdsM-wP0-IMn1rexyf7FW1kmAfvpBxhmolmYPcd6V6JK7qk4WQIy74EPTv_0xuWIidmF5rqGM5xb0DOFE44_iKdRqlQJt3ZkdZsbpYY2e8voNg0D3TOqW8h1dMYAcz9SELVqIGYVsNUwK3t0ItWZvGnXnSyDBpO0izAq_vEr-_TgcYku_iaQlZbsMCpPwcLzbJzp9k5D-yff2AX6hmgD6nmtrvY5k6P75oE948bVRovaSEKgPP0KvMWJD6nP7dmRbOmCNbENy7E1fIYbpjqsoMNwSCrlkewV19FOslM0DDUi969sHLlkU1TnfaHdyoP52Zy5sqhE7CM_RvLK62IZ4pTJm5lalt95KQLcbnGskIrdwhNll1Ad52vS7Ss5NfwzX2lBINJyPnMNvw-0gv8PGgop2iBiBlxXhQz0zCznamlbivnBL9lOx30yu_SeXCcQx8gLV1IVTT5g3LY7SrMIMCMOZnnqE2aCdx0VkNCpHyhiTjWa9hCoh3BaAAFvEff7Ps8C4kg2tUwJYg2ykcqkPJ2FzJO9StZsyIWF40mDdi-7g-_wDLXLkritzRl-UFC_JLxez6x8UMkPob395HoFVTK1P3QP61ngtY-5q5mDAP7YYp-J4HH272VrFkiEPqm9soyrRMyXeJ_225CKsM-kFuVv4v3K4v03YP4k4sxIbe7vn9o9h3liRRDjSSoGagUk4DfMZh8tjBpWikGug0w2gxkKL-AtoWmHS3L714WMrAWcTTeslPzGBiuGuK-qA77W5wzW9Qkan3NSA1os5tnbseZCUstth7IIk4gCH239zUUghOUhWyxFNJfiRa8laF3wBy6aYQewUGykI_4RbWPcrYBSmqp-_J0oSSs1eMse5WtUgU_cln1Bq07hoWt7uv7J-bwE5BkXX6ice8lOwrnN3rX2Y_gRGXdVdB5wC5RgpYtjkiOpmR-hx2u4VZ8EjCkMadt1nvUvTEbTESq-z6oKPziuxDiG8lQCKRPvmPfp-v5LQb88OysEXt3qS4seWW21k9twbdVdBYusz0KTu4-HHOe5v_67GoPjy_b_w1Pf1N7MtBySBKuUW7QqwtV_4bOR5iqImDMN5b05Etnt7NJbw3h_j-8lGu7w_QFvLU_y80hYSHneXvhh7EnztCDeQ6eHB-QPbZquPZVE77_TyDYxn2ECUu9D-c33TQS5Az-sOid6teglezIhz-LQTaH0d9lEYue73uZ63rnqiyW2deVjMUlDHwGyXpFg7lU1oYX-TivstkNzDuGHkDyQCD5nETQ49g3DF9BXbYF7Ow5Q2ohoI7PAwfV85Kl54aSGWaisUecZNzC6oilZAJqISeOJoQ0Xeza816iW2Xt1i8YhSPByd68ROwjApEhBWdjWdXduZVfXKzW8aldpWqV2rnQg0LWJ-H1WSW-NlCs67GzvTpib8K7nkeY7_LFqgCfhVLrcBAetGw_QDljrVDTdvGvfQ
```


{: .small}
**response:**

{: .small}
```
HTTP/1.1 200 OK
Date: Thu, 17 Sep 2020 05:56:44 GMT
Content-Type: text/html; charset=UTF-8
Server: Kestrel
Cache-Control: no-store, no-cache, max-age=0
Pragma: no-cache
Transfer-Encoding: chunked
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: .AspNetCore.Identity.Application=CfDJ8K9Ehnko85pHtZ-5L4xRwA-A5Ob5-mj6wsQ3W0xNU5_QRHFMI9SOWdK8rsePEe6FuL9ZvMaed8ptjBsjajaL2EQUwbYllhStmm8pFmlTWkZuWdMY9pFsywefnXZ8QaGJv8B3G6Uvn9aJH86_ZK_NbvCYfB5REGJx1NSaA5-EGlUtvt-4B594lNOSwAxjIEKE223JO5XXfxDzA647iOvvu1850duz7xeCOtH-Ze_GgO2OKXT7nlILwuA016c68okuPE9QzBGMA9zGp7MWUZBpzR8TQUXvP-krp5q6zaBX_TxBb8nl7CJNNiCiAbh6bR4uEPLNNFKiN9kD28-WMbwzQAdz7fbaZ3yfE51KTi1d_dueUAlJhQPGVfZypQlb-IGEx1HHPo7HG97c6b3Nh88CoyCpPJ3IE_aev7aiKRscm2vmkOVYk-_F1hxnVIrs78ba0_iAfH5GktQTsTQvxxictr5982NVFQn0Avnf6S0trdV9bpYrt_KUd90VQ-T_1SuYm8LkNxfUkyUmdRuUCZDG6K-6H_W7IINx_0fwromhH5gkXgyN3csQPDu9kph0kOKpGBbccreEUEyfbMwqEbuKoyxXVoGhiz0L-NrAuga5kXFG2pVGTUxI53o17nbRTHvaX1fPUzD4TZkF1CUKilkhviTC8CT2uvc-fCJPbnJ5WZ1gYsQr_91BsbM57AqJDUDWh4qkG9SbLMLCxflRPTS64wKR1EP5ojo0hkkN3FCRkEe4jrkS2wsP_Dvbc9gX3oU8-dd9fAerR3_dF73b9QzQMWv1N1QU1JnIY3qIM2wYlUbuZvTnrO-p5CdSEkBqyRlbMyV6837FFw3OQN90krPw8BxhxN1cZRRssWIvMpGri2xq_rR-jTHXUE06SGTe2iujpbzIIxKg_8ind65Scas1dfgj1JWY6r1j7e52XJQ-CUlnCJQXyCdZWKSoiTPJ1tVvaz87zMxKDbWuFbr-4bEMhb3MpA8VLwjduv-8Ebdcem1OiF5F0eMHR2GruZN9iG6eXE6InM04vZcrbSmyu0q_ORMkIBxSrBy70ox10aEtx7MN6F0v-cxwPzu_nFkRzLQY7CUzwi6wbdR5YXXKDVsRRRmmTkRk1OjA6VLFFz5j_CiYB7tl9dqccgf44Rclu-k0wFRRJbbInctfQQ3G9YMKIqABaHtqgO3MtU5VqVhKYpxhZqoD4ZkGmnqw8N8wuyQl9mOXEV_SFWbw7RGqMdX0xyQJ0-EOuxGcXmCG4wWmAnL281iV_5LI0PwqX28GbtkVM8PmXvvDX7JBuA6aF5ByACXqYuAxjql3lhR_dsBKe_iCm3KfjXAHMxIZwzlJ6SpkF0bjekqmfEsdKc-Bm6c3s4l-OIUXQeO6vovBYjCN-HaOJDnubowi403ge4Pkn8numOxjbod3ewT_NWVRT7Xyhz7S46nQqfpuMS0RxQT5R5lCj2x0f6FGXwbJOWp2oAJS0rbkPDPrBAxFbWvq0uaHcvCoeRC53JBjhokMo8euzVjpxA-GZG322U3bkK-J4fY1vc7hxmZps3ob2R4Qq_KOqNPkKL6kKSNprkXA2sBiODaLKNVbf-SJ9J6_BTa4_MPKZq4DwQYAYNAWqhpd5Jzh9VwnCfkXMDGQrEg3h0Jfa_kTdADpP9ApMvjNLTKlaMR-NynVMKiWhAIjpqJkP7hHffA7tZ_sLgmtbelfZle_uxi2x_5SIO3RVNm8i7W44FUUNA8tG8UypXZt1_PGCyYMWHfW6X_quT7n7b6mD-CrS8_uI1WvUzcEXCmWhQ6WUfIKm4U59zta2wcT-ouxbzp38ARxaGHBpVRkA3EPnqVwV-rd-mOEC_aDyVzVclQNsLTmfMlBTsAOhYqHZAkp1o6ZsXML7_vJvxhO2rDMkuxR4yLlZ8n_J6juOqhvEe5S3tvFOYspHInh9Nj_n8AohsHF_9laypg1sMGL6YIUJx2TsZLgxYxWiSspEinwAuvpCErVVbt-eyICRkp4mJOCGxD1ysd5IKLQhDHHivJDquW4W4OZzjOKidsLDVPqKuxGMAsq_uGiiheF_9AEk31WP-KAsfOj3R08BbUz_STqYRke4WYn1ES_z7DaNLNfe-J1lYyFXIOayretmiIumccxqYHNzOAPrQ3MgPybEVZyCl1G0ebtXBARO0-Q0l8D4X2GKzjsNE4idVNZkLZpaM2Rw5argUUaKZ2Uazez3HQxCjA_bBE_; path=/; samesite=none; httponly
Content-Security-Policy: script-src 'unsafe-inline'
X-Content-Security-Policy: default-src 'none'; script-src 'sha256-orD0/VhH8hLqrLxKHD/HUEMdwqX6/0ve7c5hspX5VJ8='
Referrer-Policy: no-referrer
```


**3rd call:**

{: .small}
**request:**

{: .small}
```
POST /signin-oidc HTTP/1.1
Host: host.docker.internal:5100
Connection: keep-alive
Content-Length: 2261
Cache-Control: max-age=0
Origin: null
Upgrade-Insecure-Requests: 1
DNT: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,ja;q=0.8
Cookie: .AspNetCore.OpenIdConnect.Nonce.CfDJ8NrRmhUhbrhOjfxyAV4PYe3JHLK25Norm5LsgSd6bivHRQ4csCggp_pow1_S0L5DJiACuPLvHK8bf9LZIRQHSKtjjAj2iksGFSPNdsP_qhGpj7_QiwIXIm4ranX4koDqoPmM55Krmww5UKYhB4o-6Up3RI-LIMEgxRnzLo7pHxuoFnKg1h8uBsvlra2RtDIxq1QhNTdav5p2fQyN8Ni4RyHe9AyBHrt1UjJZRaGDQlyVaantnF5zriVPHYuEjIDW4CFzGrJvN705gZJuddCoRGs=N; .AspNetCore.Correlation.OpenIdConnect.8pgRX_wtoYfy9hnfHHS2mDvmKJOotASo4lYK42nA830=N; .AspNetCore.Antiforgery.9TtSrW0hzOs=CfDJ8K9Ehnko85pHtZ-5L4xRwA9P8XrBJ_95B7lEZ9kX21Ezh87o5vGSmCqEf83ishLyA0EMoQSD3LOpvci9Bn8jmxddGH47scc78qbjGfYKAyip3oVfCRLkZabrXcQoNpXmj27RnFMxDbcTuU_LIYTHDYw; idsrv.session=1kWSRVmsC8FWnqTprEEyLQ; .AspNetCore.Identity.Application=CfDJ8K9Ehnko85pHtZ-5L4xRwA-A5Ob5-mj6wsQ3W0xNU5_QRHFMI9SOWdK8rsePEe6FuL9ZvMaed8ptjBsjajaL2EQUwbYllhStmm8pFmlTWkZuWdMY9pFsywefnXZ8QaGJv8B3G6Uvn9aJH86_ZK_NbvCYfB5REGJx1NSaA5-EGlUtvt-4B594lNOSwAxjIEKE223JO5XXfxDzA647iOvvu1850duz7xeCOtH-Ze_GgO2OKXT7nlILwuA016c68okuPE9QzBGMA9zGp7MWUZBpzR8TQUXvP-krp5q6zaBX_TxBb8nl7CJNNiCiAbh6bR4uEPLNNFKiN9kD28-WMbwzQAdz7fbaZ3yfE51KTi1d_dueUAlJhQPGVfZypQlb-IGEx1HHPo7HG97c6b3Nh88CoyCpPJ3IE_aev7aiKRscm2vmkOVYk-_F1hxnVIrs78ba0_iAfH5GktQTsTQvxxictr5982NVFQn0Avnf6S0trdV9bpYrt_KUd90VQ-T_1SuYm8LkNxfUkyUmdRuUCZDG6K-6H_W7IINx_0fwromhH5gkXgyN3csQPDu9kph0kOKpGBbccreEUEyfbMwqEbuKoyxXVoGhiz0L-NrAuga5kXFG2pVGTUxI53o17nbRTHvaX1fPUzD4TZkF1CUKilkhviTC8CT2uvc-fCJPbnJ5WZ1gYsQr_91BsbM57AqJDUDWh4qkG9SbLMLCxflRPTS64wKR1EP5ojo0hkkN3FCRkEe4jrkS2wsP_Dvbc9gX3oU8-dd9fAerR3_dF73b9QzQMWv1N1QU1JnIY3qIM2wYlUbuZvTnrO-p5CdSEkBqyRlbMyV6837FFw3OQN90krPw8BxhxN1cZRRssWIvMpGri2xq_rR-jTHXUE06SGTe2iujpbzIIxKg_8ind65Scas1dfgj1JWY6r1j7e52XJQ-CUlnCJQXyCdZWKSoiTPJ1tVvaz87zMxKDbWuFbr-4bEMhb3MpA8VLwjduv-8Ebdcem1OiF5F0eMHR2GruZN9iG6eXE6InM04vZcrbSmyu0q_ORMkIBxSrBy70ox10aEtx7MN6F0v-cxwPzu_nFkRzLQY7CUzwi6wbdR5YXXKDVsRRRmmTkRk1OjA6VLFFz5j_CiYB7tl9dqccgf44Rclu-k0wFRRJbbInctfQQ3G9YMKIqABaHtqgO3MtU5VqVhKYpxhZqoD4ZkGmnqw8N8wuyQl9mOXEV_SFWbw7RGqMdX0xyQJ0-EOuxGcXmCG4wWmAnL281iV_5LI0PwqX28GbtkVM8PmXvvDX7JBuA6aF5ByACXqYuAxjql3lhR_dsBKe_iCm3KfjXAHMxIZwzlJ6SpkF0bjekqmfEsdKc-Bm6c3s4l-OIUXQeO6vovBYjCN-HaOJDnubowi403ge4Pkn8numOxjbod3ewT_NWVRT7Xyhz7S46nQqfpuMS0RxQT5R5lCj2x0f6FGXwbJOWp2oAJS0rbkPDPrBAxFbWvq0uaHcvCoeRC53JBjhokMo8euzVjpxA-GZG322U3bkK-J4fY1vc7hxmZps3ob2R4Qq_KOqNPkKL6kKSNprkXA2sBiODaLKNVbf-SJ9J6_BTa4_MPKZq4DwQYAYNAWqhpd5Jzh9VwnCfkXMDGQrEg3h0Jfa_kTdADpP9ApMvjNLTKlaMR-NynVMKiWhAIjpqJkP7hHffA7tZ_sLgmtbelfZle_uxi2x_5SIO3RVNm8i7W44FUUNA8tG8UypXZt1_PGCyYMWHfW6X_quT7n7b6mD-CrS8_uI1WvUzcEXCmWhQ6WUfIKm4U59zta2wcT-ouxbzp38ARxaGHBpVRkA3EPnqVwV-rd-mOEC_aDyVzVclQNsLTmfMlBTsAOhYqHZAkp1o6ZsXML7_vJvxhO2rDMkuxR4yLlZ8n_J6juOqhvEe5S3tvFOYspHInh9Nj_n8AohsHF_9laypg1sMGL6YIUJx2TsZLgxYxWiSspEinwAuvpCErVVbt-eyICRkp4mJOCGxD1ysd5IKLQhDHHivJDquW4W4OZzjOKidsLDVPqKuxGMAsq_uGiiheF_9AEk31WP-KAsfOj3R08BbUz_STqYRke4WYn1ES_z7DaNLNfe-J1lYyFXIOayretmiIumccxqYHNzOAPrQ3MgPybEVZyCl1G0ebtXBARO0-Q0l8D4X2GKzjsNE4idVNZkLZpaM2Rw5argUUaKZ2Uazez3HQxCjA_bBE_
```


with **form data:**

{: .small}
```
code: ajwMYdl0p7gKAuUR2aV6eOqnDj0vfN-RhO9cIoWbDRQ
id_token: eyJhbGciOiJSUzI1NiIsImtpZCI6IjZCN0FDQzUyMDMwNUJGREI0RjcyNTJEQUVCMjE3N0NDMDkxRkFBRTEiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJhM3JNVWdNRnY5dFBjbExhNnlGM3pBa2ZxdUUifQ.eyJuYmYiOjE2MDAzMjIyMDUsImV4cCI6MTYwMDMyOTQwNSwiaXNzIjoibnVsbCIsImF1ZCI6Im12YyIsIm5vbmNlIjoiNjM3MzU5MTg5OTc5NTQzOTE0Lk56ZGpNbVZqWW1FdE1EVTRaaTAwT0dFMExUZ3lNRGt0T0dZd1pqUXlOamRsTWpka09XTTVZek15WWpRdFlUVTFZeTAwWlRSa0xUZzRZVEV0TURBME1qVTNZbVE0WWpabSIsImlhdCI6MTYwMDMyMjIwNSwiY19oYXNoIjoiaUYzZEp0Snk4VkM4bDJPa1NQYkxHQSIsInNfaGFzaCI6Ik80OHM0M1l3bnl0VUkzelUydmNYYlEiLCJzaWQiOiIxa1dTUlZtc0M4RlducVRwckVFeUxRIiwic3ViIjoiMTBlNWVmMDEtNTBkZC00NzQ5LWIyMDYtY2EyYTUyNTEzYTZiIiwiYXV0aF90aW1lIjoxNjAwMzIyMjA1LCJpZHAiOiJsb2NhbCIsInByZWZlcnJlZF91c2VybmFtZSI6ImRlbW91c2VyQG1pY3Jvc29mdC5jb20iLCJ1bmlxdWVfbmFtZSI6ImRlbW91c2VyQG1pY3Jvc29mdC5jb20iLCJuYW1lIjoiRGVtb1VzZXIiLCJsYXN0X25hbWUiOiJEZW1vTGFzdE5hbWUiLCJjYXJkX251bWJlciI6IjQwMTI4ODg4ODg4ODE4ODEiLCJjYXJkX2hvbGRlciI6IkRlbW9Vc2VyIiwiY2FyZF9zZWN1cml0eV9udW1iZXIiOiI1MzUiLCJjYXJkX2V4cGlyYXRpb24iOiIxMi8yMCIsImFkZHJlc3NfY2l0eSI6IlJlZG1vbmQiLCJhZGRyZXNzX2NvdW50cnkiOiJVLlMuIiwiYWRkcmVzc19zdGF0ZSI6IldBIiwiYWRkcmVzc19zdHJlZXQiOiIxNTcwMyBORSA2MXN0IEN0IiwiYWRkcmVzc196aXBfY29kZSI6Ijk4MDUyIiwiZW1haWwiOiJkZW1vdXNlckBtaWNyb3NvZnQuY29tIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwaG9uZV9udW1iZXIiOiIxMjM0NTY3ODkwIiwicGhvbmVfbnVtYmVyX3ZlcmlmaWVkIjpmYWxzZSwiYW1yIjpbInB3ZCJdfQ.ToPcg23exe5gT-KhKS2MMthcVdYGuvh3gXqfPY34Oe-yl19nslkdcmtrmowT7laXO8_k9MBpi6iqXxZUNo0odtw8Lps2aom2j_ZFlXQ2kvQEILo5VZvCnFZEgi1oEPhP57uIDhJw-LClUhuexcR-eBGg_nZ3Z0y-XnkYWftisca7QzM3VosMO9n5dEvwbMCo5ophH7oSblQ_DOqTxJ7K6cpXNrarZWBsfKahPIufKmEzNA2NeXOOXaUy8CRuqjsGn7uZDFHU7gAbnDi-iEX_Q01HhD-E5ERyWW-MIR8t5hyc04RvnEpdhsgnsSWQU2lymRYEDYBAZzm8wSLAT0-Knw
scope: openid profile orders basket marketing locations webshoppingagg orders.signalrhub
state: CfDJ8NrRmhUhbrhOjfxyAV4PYe1k19sd5Qs5QXKEB0AE9RoYgy85343eHyYdFDBuEbEYgLZiRmWxUBJksi_D9A3IwHNadD_gkPRlPW-zH4IYQCbrcUUCunwZ37FQzTlX2lO6ea_7k2Jw_NDRp7Vpd02h1VQF5AuptjpbANwR64WhZhzRl2I1N3_N2iyDAGKQt70f6KWEvyB2cRQTkCS5q9WvjN0ETDKBp1bjGtYJZJZuljP6bhY4gChGeWnrCVl1N0NLIN6e9GnTMGCQUo2nFtm-UKVXK25ZPPO-V0JW6AuG_kB_FfamO7Rszdw9jn79pCI8BDD2ltge-x7rS5mJPya9ENk
session_state: xHmoNvYDXBX445ijHYfUSCavC8FJXaWvx8aADLfH4f0.hbWhYpEgkQ8FU8exAbcM3Q
```


{: .small}
**response:**

{: .small}
```
HTTP/1.1 302 Found
Date: Thu, 17 Sep 2020 05:56:45 GMT
Server: Kestrel
Content-Length: 0
Cache-Control: no-cache
Pragma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Location: /Account/SignIn
Set-Cookie: .AspNetCore.Correlation.OpenIdConnect.8pgRX_wtoYfy9hnfHHS2mDvmKJOotASo4lYK42nA830=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/signin-oidc; samesite=none; httponly
Set-Cookie: .AspNetCore.OpenIdConnect.Nonce.CfDJ8NrRmhUhbrhOjfxyAV4PYe3JHLK25Norm5LsgSd6bivHRQ4csCggp_pow1_S0L5DJiACuPLvHK8bf9LZIRQHSKtjjAj2iksGFSPNdsP_qhGpj7_QiwIXIm4ranX4koDqoPmM55Krmww5UKYhB4o-6Up3RI-LIMEgxRnzLo7pHxuoFnKg1h8uBsvlra2RtDIxq1QhNTdav5p2fQyN8Ni4RyHe9AyBHrt1UjJZRaGDQlyVaantnF5zriVPHYuEjIDW4CFzGrJvN705gZJuddCoRGs=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/signin-oidc; samesite=none; httponly
Set-Cookie: .AspNetCore.Cookies=chunks-2; path=/; samesite=lax; httponly
Set-Cookie: .AspNetCore.CookiesC1=CfDJ8NrRmhUhbrhOjfxyAV4PYe26Flq9shou-ao_ZMtNbtLQyma0n-9EERGKdlvaIfpNgjmJlr-oOVBxJwmD181BzEo9Jt_hEDvruZrZsVWdTsJsvP63Fofk8wCGALlv3I-fSlx8JVBNfMXdNnZTGuqjDy_aYwI1z6RdhrHQ21-JOG0NJCsCBwaXDckxa5CIqqAPS4LOyHEmcdUMblCUKWkss392uC_S3eq1R8naobk9AQSxVhXzS7BlDabwTZNnX13pXiJSaPQ1ErYAD4tvPZJnp3xXIpR8xPHX0RTImBmkqMsgF_WDF22G17ZJ3oRGVS9UnvlRmi-ucScLyMrW4srSBZCyWzbMvglic8ioXs_1EgHsPh9v09CPw5KWZ_NtbwQC2Z6UFVpNAzpMR3UkpsTtQsGGmtqs1Fc2dMoLFtYLJLNQ4ab9RAE3blUn1EllfC3ccnEQ-FWUnH0VoZFGukmBBa-TlXBHH10I24HG9YfkSatZQkZ8K9x1JC3NxioS8VCXy2ukeAXgmeXTB2qKzPI8XyZYBY2QwwfGsv1ZYUK-4CKLGk_mC_j-ZO23McGdwCtjQ9lw-huUZblwkh-YJ4d5kBSeF1D0AeL5PYIanlQi7XtF07To8hn7srYjZSBxkvjCNvfslltcm2TmjZDeoUpCPY6fbUq2oHvWEXfLSe38z85idBxTvE8j14Oj4jeK0lzI7KVJFE2uukMIWMrwMYkt8MJiwr6VCEKWhK2L6N0mBNB_Sp4ll2EUvACX2xYKqfa-cvIHAnh0lFqkqUSIkK1daHp75vQcTTu9A1VhatNkRq6-90uTIRmPyNDrS9ff09bRdVoWILp4_64c3wwqjBM71gxGC3pynz0pPxyOS82kF86EslcLD9JxWiyDy7IIGPcmev40i8ugKc6oPd2MU1PocaCUpkf77mzEEeVshjpJg6E8hjgfiOlcVH77PW6VaDNwASTGmXgVKFb7Esx-76OS1XBKy8tQeFIZrnlJWSo8DRHVTDzdIu5drM6mBOPZp7dzMmsKm1FxUaQM7yZ6d1kYmLj7GilKZdsdEeA-XEK-_3nqLknuFF97OE7Lj68rL4jWtxF6RKG7Wx70gOcHcaRjU_bFEeg0589dPIYFxPEgbEzWeNO2HTEImpEaJOQGyafozmMb7apCkmvz5QUT2k4DwK1m1gECFdnT933wXBJy7UGBYTOIRDd1DovbDa2OwyTUPaHhYM14RLYdfXRkUD87ECiEPhnWraFjf2oMY6QhKcA7bTugK5a1u0w6Txuw2hWAwZc2bRIDDJMp-q6zttDtNJcNliuRpKoJc5k-RYWHEX2AgumubGOA7o4gtiA5hl-rSzbxPhtcPNf-MKHmx6tGBeTFEDFH2aifsqAtCd1-uni-p9Kc3lkYBYqX9nsMvI8bayEWltcgJ_jTCd8eXAaPGuhi3JuuWLLI4W-U_hKJ6gXsHI4_73T4kq1E9eTLeN69sShkRO3YSZJxr4pru6Z_ZAYmy8aOPnIH6X46z9I7fFBdr4ZAIC_C3YIshVDpLxdJ5eu-2a-JH6zn9bnTEaZo3pSaSpniWcB6cz7hVIygaqCXc0LIne1rCbPwGN6i_VOddD6I_vxbdtdbD5PGtQw90rDSFUhEGpSWsGKVOysZD2uNUqY_6xNtV8f36zsdSCaMakTY-c-1SajyoP0aiRuA-lgONsuKOmxdU-rI7GVuamekC3WOeYS6PW6oyBPiX6QuCLX9XWyDXEI75VDjIVXjYksJeOMEa-BurlPP_AHufB7EGB-4yqjQ-MJktzdHhxwZmxwoNQ00Hez7lkMhWogujFPjKjfT637TKf8JZ02FuQocfSahfdsctqe0kNli_wNwgX7IsvY6zEPAkDqKM16BJwKo4WK3Y7gibhieG3paE8pCivWD0vi8voHr0RaTpMIc0kHELT9PNYUt6qbFS74Z0UDch_fsr5tpdmyB2xO5XKueXibYqGi7YqmUbidCSuQRXUJfu8fP8Jp5au6JoqoPiAJYyNJTQ67pLfGyCPYjxvIv337lYgWflPYMyRH3t1qSycc0r9gdNMBTuaJV_IQdYoPBtM6E5Wu-ssjYdQUzD30o5aGUorSHTEsbQQK5FnUVapOFbqfMg6wOmPv1SqSzonUTw-9z1MJQzbICRfYVvkcv_kuA_lkNbAc4USku3O4jW-OpYi6aZ4x3AySDyEX3zavmWpylg-giLXby4NBD2DZPVhblDjofwZs2N1crLZ9FAGLa_LnKuB1S3y96_TSDcZiFAleMCJIro29j0V5QiNbGBoqZU1pLXommZczbnpjb3VyYK_-oGe0xSamYLI1qpsvgmQ435iRDjnPEfJ50LWtgJ_enXeND0MX9866uE15gmfCz-q44XIbs5oCIeDqOZ87LzquuB082O9iWkDa07_Pjzm40_h--qlv5pkrqciXPOFiMFFn4H0KUuK6zDz7nXLXpmVwFq8f1xa8HESB9cpbrTY4EXwreFDS8E2-VJmsfAkW7tA62yTtvbFxrwH9Gf30ZLQLi8AuNBbuygfUnIf-8Z85bmh5PVXyEuC1S2_vHrc8GkRmIXIOXTuR9IiCc_HrB_7PQM9GxRvWa9nbcuE91wldzfXrRmONJ6Ocv_aJUZgzsA-Gz9aNC3Vy4iHBZSot9X0AjvaR-p2E9nFYEa3V5OTf_AxOzUejPIVSDf88XYxTGYG2CrjsQlXuTZxOHyKAl9ODTZaqd1XB1P_a9RCL9l0pDig2hO-pi44jystJC7UiHeSu8jvzmSCbEu-x-rO6xmclNOshyXIJqzeITHEcGDR3ozz2PELzZD0kA1Ncv3l2WE_daQ185-zycuBKRbn7OF06jUJ-G7W8nreKdCs_FWt4ch9N5VNo4efxKW0TE_6bKGm1q1P2EMht0ZzaEVAnCGzdpBLdzMA_B_DyXwsYBrxPS8rs5_OMFBx_FhpBaJs9pWmS1TVpqavQueju_SKldU8HcgqiKBj3BFwkQJq6Knb08aJgxbTftZeKgciWAg0nF5vVbVnbWAfVyMOn7VyldR75pjl1SOURf2Etq17lkC0enoE8xqlappTW2D3nwNdTwUBjZ-MQSNhWuLHC8hizCtaGUZdSzh02gWT7eVavzBTg0ItzmHMn22vmWuTtliryrbUT79wx77-Em67FJPKLqkCHCdyhe3bAjGsBoMLl3y4E7xrDHaZ7W6YdQDk50mel7qEwWeOpprRxJhfqXqmB6IEMOS_Pax5syLi9sg5f03QeYqbxWIz0KNnsUWzs6U96wCb31V91Jdw0NhBzMclnOhVYrryaB5mAuY94OgKD2ASdta-JXnhwjw5aBHo0QgW9vGAGjxQU7DsRgiLkK5cbhrGPO-bQXo51KrlrllExgHRw2qQrpvrkzSR0gNv2ES04pBZ2jEnCbimbykGoQHrw93xFEU_3YC4K75ZixNaIMdKpF03seugt3UJhNMN8HOnU7vGNw_FcOMjCUxsZU5UFctQTqRMc1PLsAy72aAGO_7EQRlz-uIdsrMecXEXCjwOEfpd7xaQCW5yW_pzVA3rg2zLyHXNAHYlm32nCmHd4OLcAtmWczJW3HPgggO0fdGItIdaT7QTgNXqYP20-Dwt1iJGo7MQRg13SoiWMiC_Z_cfO-vuLU8ih4KNki_6giIgW-28yXtv6EPymEbV4ZIIvwmjfa2GOWf9PkXXkSD3estqvFBX6UpRggYRcpZrURfh-phOyH_XlmeMfa2Z_OMLaCZiE0JkYcEfGyxP7_LLyzPnEb0nwvGWfTyRzTbefYiMYbbbVYSaQfr9fC4tbf3OpF3nJh1-kMLCib377fPd4fhvd1zugsUVJs6N3CtKKFgcs69rMJnDe8og8-ZT1eiVdO9GU48lD0p1HIt5rVGgDuegJ0U3-Naq9EdAeQlM0WDzo34JGv3rVnQeki-VY7KBuDyVv7vgdCk_-BQxPokI_81Pm6QkgYE2B-SvPi7mPlhGoUahORQ72NVFl819ycwsr; path=/; samesite=lax; httponly
Set-Cookie: .AspNetCore.CookiesC2=7Gus_2Dai0Jmwh4xUzlBkKO4yowK85UZteI-12eG3DnMN4odZevaH8ihoVanbp_G6FkZN8vpLMFHkOV8i0kogfI0t0qDoOkR_PqMbF33hYpNKRAHDJbVq20c_QYjHBAdnZQ026tD6xXCSWRNKgJcFySkVnynQfloqGDXPQq3O4bqYWzkn2zc5pOeJ_j4O6Cb81VfeBiDehVgYYq2SsR_T6b8JDaPoczSscpWTac7MVnjtuOabEI8yyenAr0J6VcaeBqCokd3dUAksfPX3Boq4lRU6o1mDNEXW4wpTQA5_LYMhnXBxpZ7RWcardxnQwIODnyzI-kUh6Kn45IvuWf5q5N3lCYctzPz0pbSTY775afdvjISAGDG8LWikDlu6I2y9f-vEa7QfrgOLOowpX7pxUCh3y6ESiFkimLNl38r-rWmVX-QV2Z2txNZmy7_o0I-WndoC-gcYYih5zD_48bMBJmFgfVOKr4SdxMwDGxigy1522YSzNrq_9XdK3QdrotGLQ-KBIBK4A1OYsRGACmZSRDNQ_jc0WLnwWjlGjTElZiy57-g8GkQzg04E0ZjLvA3NehM4ppp6Wk0U28L6emeaX0Ff70eHOcPW9GUZ943d5UCUPipkq63g2HshUpw7ZLBchkJ5kP5oMzj2izXok0Aijq5ckqBJjWsys-DiNmth6_BXPjhsZd6wGyUCsGqYbi5YBxxhyxYHRsv1E9ytsfU9QP4F0yHSXPH3SXsQQj5f-rjDoLTCrwgz_I6FgRtyOS7dA6NbVlC_r2TBZfZBpa4S_eyvAmyoMtwZWNsC_JhgFjOXzUV48jnpWnkiaZH-rKKJMQ89SMrad05pjY0Ux3XcHq-FAzXLPChl__XRjEP_OnLJHP8SYqrZ2MeNBLoIHHFUManujwYNukVKKZEWba3TlwRrR_KJBqNvYqz8Qg7uyy3ySfgronPSXJnPyQnzhsq_5B5U2ozndgo2V81TBc2eVY22PSus3CEeMP9xOo7R7UEhGNN1ZInp5U997u5MbIERkrDtjTNXqadQb9uJqRDvp1k0jCYsq30a-ZQ5C2ChrfzoMV-qlDv7prMZQkhFUNiJYNUOxHPu6lmHfurx1Yul4R-lJyHF86tWM9aAK-QvFOEfNnUcT0a5Yr4q3e_wDeRujnzKcx3u8Esa7_tMBVFIRAWfns3zk9iKDalerIgG3MwOdrLHwWl4ZKJ8MsNlaZ7V_LQWr8fxJgHmbs3kFQU9f1t0WuP4MNpQtgDfE5C-Qh0LzQiZZUmlRYunBvga2C7j4_bUN8mJhxqMqXDj0Bs4JaPu8LrRhN3L-wmOZwA1n7qgDZx5XABNNJ8PSjDdCRKBl6sFEsrLZevRlKJv8UL0kvcPU2wBONIyFR979qy9JAgoCaUpnVrqUF1Ju2r5vNRHOLjEqRIBK1N8dkuVWtrqzsojF_LX2D79krjiSiilZCUQYf23OaTgTnfyGvSquD8GxQkyj4rqJaW8t7LH5gvt4qYHBl3OP7a7VVLgspYkdyAWAmtOHHYs_NYzHGMeudsMSfrFKnHqaxyeD8r_roRCf5LcQ3wPM1vjJgBAwTRGc_qDgZmhaibcTtgbD5fLl43p_q2-gYHcyIRgis6zZpWEUWiikKuHlI6TAgTJ0mVzx7_e63m62aGTakKlaRG7SGE2xHUPzFACEVnYlq7pDin8Sbvfa8UvWxzIeeywsAJ4UJwVdrwCBHYzMHaHUreJ4NYCdurOEQLmo8wFhYZqBLOYUE4iQkAxEdiEC3kMlqpJaze0X3NoLPED4NZ5yS6lt704DyYKILoHgBXgJRqafcmH9R0NwGZNX0z0zA7pDp34RyS6QB8726PW4Syx1fQ7fQmULl0rJDQZTBUYMKw1K4uok_67WmVP-z53xaW1zZxk26UNfp4FGHQ4126F1MJEEbyBLYoXTT61T7-ZyOoCFplo_as0tOZvQEgqIuNgPV7ZM_tgjFCgz_hhDV5OTg1-8kTkiAovhBb_94RiP-Ro2xGtOeScI-V27lBTgPxt4eBm2O3tKSUjpGoCBbOdG3j3fsxPwHMNEE7hqi78-42az9uZawYcG1bAJLFWo27UizEDIWJ49XkeMZVMwhA9APOWt2TQh3ZjxovhMOysXevlrHTcZU41UQNrPlz0eTMQN4BMalALEP2BjbjZSe6s_b6-mnU6yvG7nGroZh0O7Nd5BZAIF_DFs9kr32vLTcOZ5KVn20-uBs4_mo_IzIbPvzLhXQyx5Hy9iN4-B4Xu_wyqBxVhUWeZPhxjYzuUvL3FGdQx-YANe8STpiLUybxodQVFUKRF4Iu_cAvu4SC6VzElKjq9oiirWwAlakN6kiJN9MD7umPooU8nqvpd5zov6TMJxfL-wJSp6zgrZpwWK7K7YgtWrui59yoPd_svlb5sxOtmE9fTQXbDcoiUy7kIPUn5yOuwkSvzkZLu5vaJJPIVZKRSyckjyyMfeoGv0CL9aokP2sl5SVhYY4K6ipUBYxg6gz_eKs2u1gA8RKJ-ShNMLvuraB9Eivd7YOlrTfDNO6A8piuZpbHiD04nD2CYQw9W-StD4_OXfCJTg_qLJY0OJbdJI6s_k4cLiY2qSEOmeVwjgoVEdsyxx8PT_bXReMuJIhaCZKN2DyRDmKZ5q3aQSCeR4YO-Y22BjHT34noOXSpECx_QMT_CQcuPh9e4RZjdPpZOXP1CyyVWUViF3t54Tv2nMmc8VJhfS2YSnXuMGGMM1T4r1MLrcA6PirFKYZ38MzUWX0xWyoBz_6j8sGq2CtHsarHWVEDrs9D7fL_f8NwFlKi9YJfyeNLoCNnpEwO22pATFlpWuQDZzoUf65VHZTbp5MmdDcScWZL_jz9LwfNkqEeiG18vpv8KH2z1VdMT1mhhUB6IDYoZJVuNEWxWLtXTGbB1Gz3-gdsvSSW8Q1FGJ_oAGmU2V6Yw5OJ59JL_-JdJTEVPq3GZMTFhwZ784twqsAwTObTkQsSzZCaxGTMCJ5o6JJdbbmegUW7SZm809pZ1_gZQAfZlTnL3BQ9s7agokJfG2-2mDsFGkthAIm4rrP7FvtyFud7qCQ5RtMbwrxhekExH9AsfigXXrjQLG2oS3KNebdhyr-77AAi2emOcHUCYFoWRemRd9HNAucVsKSW-CTy-KpHWJeYD-qU9CchZnh4JWLV72a-AGLWStXvMDbeQrs6R4Bbx_HhmVUcA_ZGXc0uaLbBqjetB19iHAcMVR8qvcZ57bD0ZVta0iknvSv1LbzOI887N9j8G7JrPQ49Aq94QtSwpxck-6BudvPMN; path=/; samesite=lax; httponly
```


So this is the call which sets the chunked encrypted cookie...:

![chuncked encrypted cookie created by HttpContext.Authentication.SignInAsync()](https://web.archive.org/web/20170809191300im_/https://i.stack.imgur.com/p2Gy8.png)

So this is the call where the exchange for an access token happens. Remember that the access token is stored in that chunked encrypted cookie.

It has `code` in its form data, which I believe is the authorization code:

```
code: ajwMYdl0p7gKAuUR2aV6eOqnDj0vfN-RhO9cIoWbDRQ
```

... but **where did it come from?** I cannot see it in previous calls. Perhaps it was encoded or encrypted???


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
- [OpenID Connect explained](https://connect2id.com/learn/openid-connect) from Connect2id
- [By default, the redirect URL will use the `/signin-oidc` path.](https://www.scottbrady91.com/Identity-Server/Getting-Started-with-IdentityServer-4)

</div>
