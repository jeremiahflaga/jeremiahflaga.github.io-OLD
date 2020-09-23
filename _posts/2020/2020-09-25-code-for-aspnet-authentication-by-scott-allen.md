---
layout: post
title: 'Code for the "Experimenting with ASP.NET Core Authentication Schemes" article by Scott Allen'
subtitle: ''
categories: [Programming]
tags: [Programming, ASP.NET, Authentication, Scott Allen, eShopOnContainers]
date: 2020-09-25 01:00:00 AM UTC
---

<!-- started September 11, 2020 Philippine Time -->

I was trying to trace the code execution of [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) at runtime, and I stumbled upon this:

``` csharp
public class AccountController : Controller
{
    ...
    [Authorize(AuthenticationSchemes = "OpenIdConnect")]
    public async Task<IActionResult> SignIn(string returnUrl)
    {
        ...
    }
    ...
}
```

<!--more-->

I'm not used to seeing this `AuthenticationSchemes` parameter on the `Authorize` attribute, so I googled for an explanation of what it is. I found this tutorial by Scott Allen: ["Experimenting with ASP.NET Core Authentication Schemes"](https://odetocode.com/blogs/scott/archive/2019/01/02/experimenting-with-asp-net-core-authentication-schemes.aspx). But it does not point to a code repository which I can download and execute and play with. So I created one.

You can download the code from [this GitHub repository](https://github.com/jeremiahflaga/authentication-authorization-playground). Go to the folder named `2020-09-11-aspnet-authentication-schemes`.

## Experimentation

You first need to read [Scott Allen's article](https://odetocode.com/blogs/scott/archive/2019/01/02/experimenting-with-asp-net-core-authentication-schemes.aspx) before proceeding with the experimentation part below. In case his site is down, I saved a PDF of that article which you can download [here](https://github.com/jeremiahflaga/authentication-authorization-playground/blob/master/2020-09-11-aspnet-authentication-schemes/Experimenting%20with%20ASP.NET%20Core%20Authentication%20Schemes%20-%20Scott%20Allen.pdf).

... finished reading that article?...

Now **open** the `2020-09-11-aspnet-authentication-schemes/AspNetAuthenticationSchemes.sln` in Visual Studio 2019. You will see an ASP.NET Razor Pages web app.

Look for `Page1Model`. It contains this code:

``` csharp
[Authorize]
public class Page1Model : PageModel
{
    ...
}
```

It has `Authorize` _without_ an `AuthenticationSchemes` parameter. That means that the app will use the _default_ authentication scheme configured in our `Startup` class --- the one named `cookie1`, in our case:

``` csharp
public class Startup
{
    ...
    public void ConfigureServices(IServiceCollection services)
    {
        ...
        services.AddAuthentication(options =>
        {
            options.DefaultScheme = "cookie1";
        })
        .AddCookie("cookie1", "cookie1", options =>
        {
            options.Cookie.Name = "cookie1";
            options.LoginPath = "/loginc1";
        })
        .AddCookie("cookie2", "cookie2", options =>
        {
            options.Cookie.Name = "cookie2";
            options.LoginPath = "/loginc2";
        });
    }
    ...
}
```
<!-- 
Take note of that `"/loginc1"` path associated with the `cookie1` scheme.
 -->

Now **run** the app then go to `https://localhost:44325/Page1`. It will give you this output:

<div class="message">
    <h2>User</h2>
        <div>Authentication Type: cookie1</div>
        <table class="table">
                <tbody><tr>
                    <td>name</td>
                    <td>Alice-c1</td>
                </tr>
        </tbody></table>
</div>

That page shows that you are now authenticated as `Alice-c1`

How were you authenticated as `Alice-c1` when you did not do anything yet?... That is because this `ctx.SignInAsync(...)` line in the `case "/loginc1":` part of the code in your `Startup` was **automatically** executed when you visited Page 1.

``` csharp
switch (ctx.Request.Path)
{
    case "/loginc1":
        var identity1 = new ClaimsIdentity("cookie111");
        identity1.AddClaim(new Claim("name", "Alice-c1"));
        await ctx.SignInAsync("cookie1", new ClaimsPrincipal(identity1));
        break;
    ...
}
```

How did Page 1 know that it should execute that code?... It's because the default authentication scheme has the `/loginc1` path in its configuration:

``` csharp
...
options.LoginPath = "/loginc1"
...
```


## More experiments

Now, to continue with the experiments, try the **combination** of logging in and logging out using these URLs in your browser:

```
https://localhost:44325/loginc1
https://localhost:44325/loginc2

https://localhost:44325/logoutc1
https://localhost:44325/logoutc2
```

Then try to visit

```
https://localhost:44325/Page1
```

and 

```
https://localhost:44325/Page2
```

and 

```
https://localhost:44325/Page3
```

after each login and logout and see what these pages output on the screen.


## 302 Found redirect

Now, **logout** using `https://localhost:44325/logoutc1`.

Then **stop** the app.

Then comment out the `ctx.SignInAsync(...)` for `loginc1` in your `Startup` class, like this:

``` csharp
case "/loginc1":
    var identity1 = new ClaimsIdentity("cookie1");
    identity1.AddClaim(new Claim("name", "Alice-c1"));
    //await ctx.SignInAsync("cookie1", new ClaimsPrincipal(identity1));
    break;
...
```

Now, when you try **visit** `https://localhost:44325/Page1`, you will get a `302 Found redirect` response with `Location: https://localhost:44325/loginc1?ReturnUrl=%2FPage1`.

Interesting...


## Lesson learned

The string `"OpenIdConnect"` in 

``` csharp
[Authorize(AuthenticationSchemes = "OpenIdConnect")]
```

of [eShopOnContainers' WebMVC project](https://github.com/dotnet-architecture/eShopOnContainers/blob/85aea20046ef495ba44c5d6031c75960aa316eb4/src/Web/WebMVC/Controllers/AccountController.cs) is just the _name_ of the authentication scheme to use, the same _name_ used in configuring authentication in the `Startup` class:

``` csharp
services.AddAuthentication(...)
.AddOpenIdConnect("OpenIdConnect", options =>
{
    ...
});
```
 


-----

## AuthDump

If you navigate to `https://localhost:44325/AuthDump`, you will be given this output:

<div class="message">            
  <h2>Auth Schemes</h2>
  <table class="table">
      <tbody><tr>
          <th>DisplayName</th>
          <th>Name</th>
          <th>Type</th>
      </tr>
          <tr>
              <td>cookie1</td>
              <td>cookie1</td>
              <td>Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationHandler</td>
          </tr>
          <tr>
              <td>cookie2</td>
              <td>cookie2</td>
              <td>Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationHandler</td>
          </tr>
  </tbody></table>
  <div>DefaultAuthenticate : cookie1</div>
  <div>DefaultForbid: cookie1</div>
  <div>DefaultSignIn: cookie1</div>
  <div>DefaultSignOut: cookie1</div>
</div>

