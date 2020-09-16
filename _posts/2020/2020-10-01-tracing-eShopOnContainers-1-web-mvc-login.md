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

Another thing: I cannot figure out yet in which call IdentityServer passes the authorization code to the client.

These are the **first three** HTTP calls when logging in:

**1st call:**

```
POST http://host.docker.internal:5105/Account/Login?returnurl=%2Fconnect%2Fauthorize%2Fcallback%3Fclient_id%3Dmvc%26redirect_uri%3Dhttp%253A%252F%252Fhost.docker.internal%253A5100%252Fsignin-oidc%26response_type%3Dcode%2520id_token%26scope%3Dopenid%2520profile%2520orders%2520basket%2520marketing%2520locations%2520webshoppingagg%2520orders.signalrhub%26response_mode%3Dform_post%26nonce%3D637357619619673058.MmUxY2ZjZTgtZjRhYy00NzViLWEzYmEtZDUwOTZkZTczZjIwZmIyMDQwZDItNjBkMy00Y2IwLThlYzgtZGJlMjgyNzY0NGM5%26state%3DCfDJ8E_I7LFHT6hPsBUfhTnrPb_NWieVYlUyUrIfWmIdwiDRWmhTrT9F5U114GZKwEOfKsrmG4apBnCt1zE6BQj1JDDqgwPXeyANLt7BczGzOqaj-aZABtvd_GuQK7V6r_lCmpPqIGfYiOtgyYbvA5eY2mCnxX7NpN4oYGsobS0LSOB2ACAcN3TAmzgirIdZO1aJH27Lbfb2krVzgLPQOWsXNW4yY7c_RJg2X9FjWpiYr4kbC399zaCTFLStVtc05DDphAJMy0hvdzCUQyO2vaSUlF7BQus6JqyYFUR1WXTzzZgwb4NJKHu0AtD0YsgMWugffwW_sMp9GZOEfNj9pRFTUUc%26x-client-SKU%3DID_NETSTANDARD2_0%26x-client-ver%3D5.5.0.0

```


with response

```
HTTP/1.1 302 Found
Date: Tue, 15 Sep 2020 10:24:14 GMT
Server: Kestrel
Content-Length: 0
Cache-Control: no-cache
Pragma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Location: /connect/authorize/callback?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637357619619673058.MmUxY2ZjZTgtZjRhYy00NzViLWEzYmEtZDUwOTZkZTczZjIwZmIyMDQwZDItNjBkMy00Y2IwLThlYzgtZGJlMjgyNzY0NGM5&state=CfDJ8E_I7LFHT6hPsBUfhTnrPb_NWieVYlUyUrIfWmIdwiDRWmhTrT9F5U114GZKwEOfKsrmG4apBnCt1zE6BQj1JDDqgwPXeyANLt7BczGzOqaj-aZABtvd_GuQK7V6r_lCmpPqIGfYiOtgyYbvA5eY2mCnxX7NpN4oYGsobS0LSOB2ACAcN3TAmzgirIdZO1aJH27Lbfb2krVzgLPQOWsXNW4yY7c_RJg2X9FjWpiYr4kbC399zaCTFLStVtc05DDphAJMy0hvdzCUQyO2vaSUlF7BQus6JqyYFUR1WXTzzZgwb4NJKHu0AtD0YsgMWugffwW_sMp9GZOEfNj9pRFTUUc&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0
Set-Cookie: idsrv.session=1X32dF58FqRogFIcYjSNFg; path=/; samesite=lax
Set-Cookie: .AspNetCore.Identity.Application=CfDJ8DJ1hFk8a29Innv16U0hf1MT8uSF7lYGrJKQ49SBfPBMlOQCaASPBlLnRaKt7kIG3S8L_o2LpTu0D8h9ClvJqJDBMka62_JZt6rgm7ZWpxKiU7tx5s26Yp9yWaQ-WIMz1O-DR5uVpqBYBBFBr1i967D9k9aR54ti5vn_4v-aHxInyCNe6eIof-sJMo9SOgrSOjqGPzGDWdDQhhHMFLIqGJzrhu--fQVKfJWYl4TZBbO0gk6OYRZ56UXqTXuufnORaGk2nar-RBRnKhfyZohcKa8D_gnJFsp2eywBKGth_K5hIddDyJHnm1qM3CK86bXzb1757KGuvJi0dy23i-h-22gwrThirotxWUXMaldodELtAIcauWpXcgUYVx246xc2hKxweuWEPVD9WO8U-HW6TGfT28cXz5wqaH1j20s3xiW8rNZgo02VYl5Fk1KqUd9QCyVD1XHKeegYfgPOoXGOBDytVtsmGZ808VVMptnIGGvV0plJYGJgIJ0XG4p1v95HCatRYAQLwmKSo-jMrg7IckndMxysowouyV0HjsYB-tsgjRt1UFsIZ8XK-uxrG9eKWVbFWI1CfMZB50Hz6cZFjSrh-7W_J09jkfBOrm0Gchwi7yBs3hd2ulc8JonWa0arf-fJn7EXUIv6_cpQzIDfT4Aep0wrGN96j9dTNE9ylWiwHMHFTiK9s2MOEceVl4eCs4_59J6CYA0JnzCc_jCI-qCx_Xth5qkBsbWh3OwBZK7po7Z3UoCosVrT8c521dJMmCWkuDTour3avVz5LjM8ATCDCqltPm71klfosAotsePJOsTYIUEanoDrlyESr5Ry6xtrVfSHJXHKSQ1w3lZUZm00nWnzvOZIO63p2ThKRGVn5z9Xj76ibc3Xm5-II7_104lNo1yD9Ucqp-Jt6JkCB2pWK3BJVtF4oCMgcRMAoIZH3HZZS4Na5kQhstPqi-Y8Y40TKvwKxNVkJNP93zfp39YCiTB7UEhTqxnZv84DT653Dh4lajjmQJ8g4Pz5zVaO-9e5_BjtlvYBc4hV7pnzK6LxehhkxqQVOc57-W8p9DumnQDUL4QgYTIxEx-a6dBkUGV2iHQlW7GG2w-q4rfpEv0EguKqPk5Qa6wAc74_O4llLA4Rz_0pSCA3VCj0y1JfshJtYiuLEVutWvuS3ZaatgF9dR-k4W3wV4pyhLx1HK2lwEnKOLg2_nryzxPl4ienERlgT6SOlRgUJbIn0Fr_q713OOwvP6j0g_kJYuqs4lQ_myZwNB-GHyGyQaPHTiWKBaRdzIHkkRodHJvHDUsAgRN5Y6krY2fpL9IP-mN1woYWyru9QuQI3qtMTqQAQb-jWAxnITxien7wHo62uzD5ypg124LIsH-SOgnxlIbDi-qA4WhhER3UykY3Ih05SoR6MdGDr12wStf2-jHpO8K-M4SQ6GVAD4P6kNhT_DXpqtvMngIEXQ63T-U85B9vzi1OjxzbE49WwHZQuCQzJZCMa5Jt3ulrfFPahOT8xCXhK26zShf2ui2Cei-C1ODD0zhfaRM_Bc4v4pSW1NICHnTUFQnSMk1imoZD0h0GIOy3dp8iMW4e5seVfrq8ld0VQOnXpNJwkxyRYaaqJtF4H3skKAZTXnyu4fkm3Zn40En_eOS4ZbcNAOArAMFiTzq-sF5--uqthZzW7-tCr8iGEfpuB39KuMqUeuNft6bPsq0RJ2Jk8mkdKOLqnlEbHf7qQM4gL8d2mezQNp9EPemxMe02Wyt5Kd4N-8TV3pphh_S7N29fW3BiJQYpbCS6EkNWVbvYk1WoNQZSxOFdn3J-wNRWA75sz5XWq-MQzczh8jpdwizkQobKXJrADB2Y26VV9bQpTLFSINPmC_oQv4pvjsVhuk7-YHNqt_ax4dOMYBD4C6plcovfRPD0Nm7FZKCI3rJqkcECWxxSB9u2XVEe8EpZgOnEWqHnyqqozPMme1mJpij91zo67DhBoShKRZrZX5-rTh51i1rvDKP-zVhPtopsZU2SB6Dvv9DFSS7EJs-XU6s2gxAVtvvFW-uKh1RIKAE6kZP6SKCfk9aJ1_kO6L5wU6DLDqX4t1RcQdfYWMiXWKzOF7iN2lwETcbhw-ek8Fp2JQZvr349KSOqjwaXv3isF3Z0RQFg_RUf-BMd4pA8uDrw_O4tFon9yD1a_wnM6k1ptA; path=/; samesite=lax; httponly
Content-Security-Policy: script-src 'unsafe-inline'
```


<div class='message' markdown="1">

The guide ["OpenID Connect explained" from Connect2id](https://connect2id.com/learn/openid-connect) seems to suggest that this step is where the authorization code, `code` in the sample below, should be passed from the Authorization Server to the Client:

```
HTTP/1.1 302 Found
Location: https://client.example.org/cb?
          code=SplxlOBeZQQYbYS6WxSbIA
          &state=af0ifjsldkj
```

But I cannot find a `code` in the query string of the `Location` in the response of the **1st call**. But I can see a `state`, `state=CfDJ8E_I7LFHT6hPsBUfhTnrPb_NWieVYlUyUr...`

Hmmm...

</div>


**2nd call:**

```
GET http://host.docker.internal:5105/connect/authorize/callback?client_id=mvc&redirect_uri=http%3A%2F%2Fhost.docker.internal%3A5100%2Fsignin-oidc&response_type=code%20id_token&scope=openid%20profile%20orders%20basket%20marketing%20locations%20webshoppingagg%20orders.signalrhub&response_mode=form_post&nonce=637357619619673058.MmUxY2ZjZTgtZjRhYy00NzViLWEzYmEtZDUwOTZkZTczZjIwZmIyMDQwZDItNjBkMy00Y2IwLThlYzgtZGJlMjgyNzY0NGM5&state=CfDJ8E_I7LFHT6hPsBUfhTnrPb_NWieVYlUyUrIfWmIdwiDRWmhTrT9F5U114GZKwEOfKsrmG4apBnCt1zE6BQj1JDDqgwPXeyANLt7BczGzOqaj-aZABtvd_GuQK7V6r_lCmpPqIGfYiOtgyYbvA5eY2mCnxX7NpN4oYGsobS0LSOB2ACAcN3TAmzgirIdZO1aJH27Lbfb2krVzgLPQOWsXNW4yY7c_RJg2X9FjWpiYr4kbC399zaCTFLStVtc05DDphAJMy0hvdzCUQyO2vaSUlF7BQus6JqyYFUR1WXTzzZgwb4NJKHu0AtD0YsgMWugffwW_sMp9GZOEfNj9pRFTUUc&x-client-SKU=ID_NETSTANDARD2_0&x-client-ver=5.5.0.0
```

with response

```
HTTP/1.1 200 OK
Date: Tue, 15 Sep 2020 10:24:15 GMT
Content-Type: text/html; charset=UTF-8
Server: Kestrel
Cache-Control: no-store, no-cache, max-age=0
Pragma: no-cache
Transfer-Encoding: chunked
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: .AspNetCore.Identity.Application=CfDJ8DJ1hFk8a29Innv16U0hf1Pvy6YYIelglYWW2wzxbrdZWsWvunYCi2f429CDzvKiP7h4KAB-7h-F_p_E9MUI22F5HxQ_LJNXAUhpe3Y7EK4GJxVcbaP5zzwSNBk3rnT5twMUf8rlNPbV29Ve3I-LBXc5tcSlzsEYBwJzMEkjZWbXpqzU4PHFCI2hHTUSRe58bXCeKA2uw5TYBLt_i-KymyUKXrELzrMvOkxwZKqZSO8NKLk6qQ9Agey2XBOm4weFY1uK9ZtttxdWsUj6svQj2mkJUldaRfGWkEq5suTPIEqdwBn_UjwNBXA6ypVDjav20lWTXbG3fsUdUer5OMNiphTZHc5aR2x7JMqS-jpE5E9rAgKCHsXeZ-gmJU8_3YvvHAubU3Jq7dOqDxw2dLaGgVTDxi9-PWUhEeeWYhIIHf-MCeHQehwbXfhEzku5qe0usJHSP970qXRzMJ_pSs641z9_vW1Ht0QLDTbWnuU3cj7lRpRlU7osuLQH7mnXU7pDDdPcA7B9YXtPB2x5yXkKVg6Q_VXColMKDlclVYPabUgBOgjx2CCAyaPslBxWXqhmvHtS0evnhtPloICEr2WY55EeOOpT0Vi3HAjQBLg4CjXC6UWC-Rzo0rpJtovovKzPu1c6a3mrnYwIv7Nmyt1kHPcecDU7gAkxQp2DYcSYgWZCaNWe3jVFwZIRygBIIBF65wOLzWd6JUH_VvaS1V_fiUONDH_1lcn08zzWEOH-lBtwCpY05SScoW9VyNtvDVUK5VAuwIP_xGwYqLCbvsMUQVEoJrphddWtP5wblrMvr3spxzGVwyqJkRt-ZTHtK2duRx-rZVtKtpH5BsBwBdmWWLTurHYhhTw8ORmRyuYI-8M7JrULWkd1UgFoB0NWk1dGB5bTtEQ8tLkucYbpo-XWh4DNNcWNNQkbUHp17vyny6Y_znpUMAyPvRqckw9mn4p76NcxGVf4mKYTT1sOUfO64gyw-CCg8nytkwRLT8a_eTzGHLY7-MK3jHkmPOYNQBraiwX6MerleBVfbKBJ-GTBva25qBoTQrLglafQL4fbLxyvCCVY0dAaX2NvZ0bEFOpBvq61-EqxCNlFDV3LJMF29BDAww4WJEtFuWJfbzyoUb5f5NBczTH9bexiyChp_1HGUtvyRQnGKJzYwtf1bVjmcORB1oFT-DZjS_kfxb1liqBn_YQO_jK6YGHmEUEFxA84dI-cP-n6UEJFDVjfkyN2Z8T7QKTMpTK8XXsmz29iv-SB2htWVpJ0Yucp7Oj_NgGH8v6CD6ZgcCwnXI7p0GKL0nSCaVRmRaSbYeL9GqnmZ4o6nMLpoWglt9jNT-c43QrN98mY5DxnGBZ8QWMj5ESUlCXcpUWDkY4YqYqlMjsGcvhMhtaIy3UqsuMCDm19CQ6ta0Y_MJg4Ez4KPfUyvPVfB4dMgi2wJh96wbg5yyN9rZNe6HRM1ZOkVI1ixDlYAIqBInlIezX5x8m9VYi0lnmdgaLKBYXjUBwyH-NrqId44X0AjevsC-Bj5a_zwr221dKEvcnQ1zKrTAL9B3_AcOBvzOh93faX8Yw_m12RsE4tPqd7nTKpw-CuZj7bZtWR5-PG5pNDke9vGFC0sU2Xzm-YxNUH5oIFTtB79NjpD_WEhazW4xS11ugsiFSNI8SE7lB9Skj5rbssLp4L3oAfGq1JeE5SqRQStB84ELT33DBfDoIuq6lqLkA3Bh6ifix2bxdmAZ1-J7WEAsp7Djj-K3kwXGdLIjpOoNc4AMblXJp9-7z8Fuw5wAX6ybjdkscyuUwKov5qFBoG272Iqa8py_3ZRIk5gp0Ko7QIPeDTQCnfoeX-hByB7vEamB9HQuJcY7KO8FflBpTQ90keJl8BZfVnD87ebunssqpIwDVjGgC8CJe6AosGlASCoA47cf6A85fSnYke6dXC9Bs7dw6Z-UYeqxp_1hZg9duDMUXiLE-wkMqeeA0HZqDDd5PjQWG9U3unekj6vzquNOOLFqQpPIkSaKEL-U6H2LS-GzWS-WWZpPyCgxg94Cjjo-B_UJxcNrXBIidieTXzykCCz5Ia7CH8RnbvePDUHVJSiz2dtYdY0ouFpHPpVrhI-yCtNjurKQGC9TImGOesQ_IJM3JVKrGyVZNigLJAin4GNiWCkZBsWiNi7xfpCnogytaXQtLtRv43GTVbkCEUT4PvdR_jpBC_fhASAZv6vDVepaTX159J77pO; path=/; samesite=none; httponly
Content-Security-Policy: script-src 'unsafe-inline'
X-Content-Security-Policy: default-src 'none'; script-src 'sha256-orD0/VhH8hLqrLxKHD/HUEMdwqX6/0ve7c5hspX5VJ8='
Referrer-Policy: no-referrer
```


**3rd call:**

```
POST http://host.docker.internal:5100/signin-oidc
```

with form data:

```
code: uz1rsr7j9_6gqxPGvLQ8_FTovCqw6mvj4bA-VHHc2GU
id_token: eyJhbGciOiJSUzI1NiIsImtpZCI6IjZCN0FDQzUyMDMwNUJGREI0RjcyNTJEQUVCMjE3N0NDMDkxRkFBRTEiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJhM3JNVWdNRnY5dFBjbExhNnlGM3pBa2ZxdUUifQ.eyJuYmYiOjE2MDAxNjU0NTUsImV4cCI6MTYwMDE3MjY1NSwiaXNzIjoibnVsbCIsImF1ZCI6Im12YyIsIm5vbmNlIjoiNjM3MzU3NjE5NjE5NjczMDU4Lk1tVXhZMlpqWlRndFpqUmhZeTAwTnpWaUxXRXpZbUV0WkRVd09UWmtaVGN6WmpJd1ptSXlNRFF3WkRJdE5qQmtNeTAwWTJJd0xUaGxZemd0WkdKbE1qZ3lOelkwTkdNNSIsImlhdCI6MTYwMDE2NTQ1NSwiY19oYXNoIjoiN1pPX1EzUGM3N3Nkc19RdUxzcGlqdyIsInNfaGFzaCI6Img1dGVnYUROTnR4Y3J1UVpwWGR2SnciLCJzaWQiOiIxWDMyZEY1OEZxUm9nRkljWWpTTkZnIiwic3ViIjoiMTBlNWVmMDEtNTBkZC00NzQ5LWIyMDYtY2EyYTUyNTEzYTZiIiwiYXV0aF90aW1lIjoxNjAwMTY1NDQyLCJpZHAiOiJsb2NhbCIsInByZWZlcnJlZF91c2VybmFtZSI6ImRlbW91c2VyQG1pY3Jvc29mdC5jb20iLCJ1bmlxdWVfbmFtZSI6ImRlbW91c2VyQG1pY3Jvc29mdC5jb20iLCJuYW1lIjoiRGVtb1VzZXIiLCJsYXN0X25hbWUiOiJEZW1vTGFzdE5hbWUiLCJjYXJkX251bWJlciI6IjQwMTI4ODg4ODg4ODE4ODEiLCJjYXJkX2hvbGRlciI6IkRlbW9Vc2VyIiwiY2FyZF9zZWN1cml0eV9udW1iZXIiOiI1MzUiLCJjYXJkX2V4cGlyYXRpb24iOiIxMi8yMCIsImFkZHJlc3NfY2l0eSI6IlJlZG1vbmQiLCJhZGRyZXNzX2NvdW50cnkiOiJVLlMuIiwiYWRkcmVzc19zdGF0ZSI6IldBIiwiYWRkcmVzc19zdHJlZXQiOiIxNTcwMyBORSA2MXN0IEN0IiwiYWRkcmVzc196aXBfY29kZSI6Ijk4MDUyIiwiZW1haWwiOiJkZW1vdXNlckBtaWNyb3NvZnQuY29tIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwaG9uZV9udW1iZXIiOiIxMjM0NTY3ODkwIiwicGhvbmVfbnVtYmVyX3ZlcmlmaWVkIjpmYWxzZSwiYW1yIjpbInB3ZCJdfQ.YXv_tE-7KjU7wSIhTx-e8Qnaph6ZvC4_zyPolfNYADLsmhP3h8jqH8Ded_9rrsMfQb1BQ6WNCQENWcHcAOPZn8R7AmD7mNnlvxbKofBFIityrjNr6BNZmH906KbGG1_dDxAqgIx272422Ebi6zE_-hfLYs4YSwoUVKhIRslhOq5mvu72bD4coOcs2izvzcnYQgbowyg75rqF0rLlpyVcw3M7FbWLl0MEUscj4ixJ4rQIYiT-5NaqsnbAVZSBnPO9UTDSMQL2xvRZuReY7sc-W671CqoCx622mRPwGwL0EjYctoc5sCKq4mCBSrU34B3ZZ1hx3QA2Ujm5wxTr9QP7gg
scope: openid profile orders basket marketing locations webshoppingagg orders.signalrhub
state: CfDJ8E_I7LFHT6hPsBUfhTnrPb_NWieVYlUyUrIfWmIdwiDRWmhTrT9F5U114GZKwEOfKsrmG4apBnCt1zE6BQj1JDDqgwPXeyANLt7BczGzOqaj-aZABtvd_GuQK7V6r_lCmpPqIGfYiOtgyYbvA5eY2mCnxX7NpN4oYGsobS0LSOB2ACAcN3TAmzgirIdZO1aJH27Lbfb2krVzgLPQOWsXNW4yY7c_RJg2X9FjWpiYr4kbC399zaCTFLStVtc05DDphAJMy0hvdzCUQyO2vaSUlF7BQus6JqyYFUR1WXTzzZgwb4NJKHu0AtD0YsgMWugffwW_sMp9GZOEfNj9pRFTUUc
session_state: FrJd5dyPLjyeTJKx17aBsOQ_SbwB266-PkRfBoWaD38.vxzSowoA-RAIUEPf_4eG3g
```

<div class='message' markdown="1">

This call has `code` in its form data, `code=uz1rsr7j9_6gqxPGvLQ8_FTovCqw6mvj4bA-VHHc2GU&`, but **where did it come from?**

</div>


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
