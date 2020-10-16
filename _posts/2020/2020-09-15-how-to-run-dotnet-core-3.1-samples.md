---
layout: post
title: 'How to run the ASP.NET Core 3.1 samples'
subtitle: ''
categories: [Programming]
tags: [Programming, eShopOnContainers, .NET, ASP.NET]
date: 2020-09-15 01:00:00 AM UTC
---

<!-- started September 11, 2020 01:18 PM Philippine Time -->
<!-- finished September 12, 2020 -->
<!-- updated September 25, 2020 -->

I was trying to run the [Cookie authentication sample](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies) from [ASP.NET Core 3.1 code repository](https://github.com/dotnet/aspnetcore). I encountered some errors. These steps mights save you from those errors:

<!--more-->


### Step 0: Clone the repository

``` shell
git clone https://github.com/dotnet/aspnetcore.git
```

### Step 1: Select the correct branch

``` shell
git checkout release/3.1
```

### Step 2: Open `/global.json` to know the correct SDK

Open the file `/global.json` in a text editor. It contains the SDK version being used in the project.

``` json
{
  "sdk": {
    "version": "3.1.107"
  }
  ...
```

Open a command window and check the .NET SDK versions already installed in your machine by executing this command:

``` shell
dotnet --list-sdks
```

```
1.0.0-preview2-003121 [C:\Program Files\dotnet\sdk]
...
3.0.100 [C:\Program Files\dotnet\sdk]
3.1.301 [C:\Program Files\dotnet\sdk]
3.1.401 [C:\Program Files\dotnet\sdk]
3.1.402 [C:\Program Files\dotnet\sdk]
```

If the version from your `/global.json`, version `3.1.107`, is not listed, go to [.NET Core 3.1 SDK downloads site](https://dotnet.microsoft.com/download/dotnet-core/3.1) then search for version `3.1.107`, then download the installer for the SDK.


### Step 3: Restore dependencies then run

Go to folder `\aspnetcore\src\Security\samples\Cookies` from the command line then

``` shell
dotnet restore
```

then

``` shell
dotnet run
```

(NOTE: I was unsuccessful when I tried to run it in Visual Studio 2019. I will just update this post later when I already figured out how.)


-----

<div class="small" markdown="1">

#### References:

- [Run the samples](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/samples?view=aspnetcore-3.1)
- [How to get daily builds of ASP.NET Core](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md)
- [Download .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1)
- [Download .NET 5.0](https://dotnet.microsoft.com/download/dotnet/5.0)

</div>
