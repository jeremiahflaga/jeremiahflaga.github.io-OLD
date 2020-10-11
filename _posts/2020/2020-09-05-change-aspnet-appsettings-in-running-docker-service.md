---
layout: post
title: 'Changing appsettings value of an ASP.NET app in a running Docker service'
subtitle: ''
categories: [Programming]
tags: [Programming, Docker]
date: 2020-09-05 01:00:00 AM UTC
---

<!-- started September 02, 2020 11:54 AM Philippine Time -->

(This tutorial is based on ["Scale ASP.NET Core Apps with Docker Swarm Mode"](https://www.pluralsight.com/guides/scale-asp-net-core-apps-with-docker-swarm-mode) tutorial by Stefan Prodan.)


### Windows 10 prerequisites:

- [Docker for Windows](https://docs.docker.com/docker-for-windows/)
- [Visual Studio 2019 Community Edition](https://visualstudio.microsoft.com/vs/)
- [.NET Core SDK](https://dotnet.microsoft.com/download#windows)

### Step 1: Create an ASP.NET project

Using Visual Studio 2019, create an ASP.NET Web API project named `TokenGen`.

<!--more-->

Be sure to **"Enable Docker Support"** by clicking on the chechbox as shown in the image below.

![Visual Studio 2019 - New ASP.NET Web API project with Docker support enbled](/images/2020/aspnet-web-api-enable-docker-support.png)

It will create a `Dockerfile` with these contents:

{: .small }
``` shell
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["TokenGen/TokenGen.csproj", "TokenGen/"]
RUN dotnet restore "TokenGen/TokenGen.csproj"
COPY . .
WORKDIR "/src/TokenGen"
RUN dotnet build "TokenGen.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TokenGen.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TokenGen.dll"]
```

(For an explanation on what these `Dockerfile` contents mean, you can refer to this: ["How Visual Studio builds containerized apps (version vs-2019)"](https://docs.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2019))


After the projects is created, go to the `Controllers` folder and add a new controller named `TokenController` with the following content:


``` csharp
namespace TokenGen.Controllers
{
    [Route("api/[controller]")]
    public class TokenController : Controller
    {
        [HttpGet]
        public dynamic Get()
        {
            return new
            {
                Guid = Guid.NewGuid().ToString(),
                Expires = DateTime.UtcNow.AddHours(1),
                Issuer = Environment.MachineName
            };
        }
    }
}
```


### Step 2: Update `appsettings.json`

Add `SampleSettings` to your `appsettings.json` file like so:

``` json
{
  "Logging": {
    ...
  },
  ...
  "SampleSettings": {
    "Setting1": "setting-1",
    "Setting2": "setting-2"
  }
}
```


### Step 3: Create `SampleSettings` class

Create a class that will hold your new settings:

``` csharp
public class SampleSettings
{
    public const string SETTINGS_NAME = "SampleSettings";

    public string Setting1 { get; set; }
    public string Setting2 { get; set; }
}
```


### Step 4: Configure and use `SampleSettings`

Configure `SampleSettings` to get data from the `appsettings.json`. Then add it to your DI container by updating the `ConfigureServices()` method in your `Startup` class like this:

``` csharp
public class Startup
{
    ...    
    public void ConfigureServices(IServiceCollection services)
    {
        ...
        services.Configure<SampleSettings>(Configuration.GetSection(SampleSettings.SETTINGS_NAME));
    }
    ...
}
```

Then inject the `SampleSettings` in your `TokenController` using the `IOptionsMonitor` interface, then use it inside the `Get()` action method to retrieve the settings from the `appsettings.json`.

Using `IOptionsMonitor` allows your app to be able to get the latest changes in your `appsettings.json` file without the need to restart the app.

(For more information about `IOptionsMonitor`, go [here](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.1))

``` csharp
[Route("api/[controller]")]
public class TokenController : Controller
{
    private readonly IOptionsMonitor<SampleSettings> sampleSettings;

    public TokenController(IOptionsMonitor<SampleSettings> sampleSettings)
    {
        this.sampleSettings = sampleSettings;
    }

    [HttpGet]
    public dynamic Get()
    {
        return new
        {
            Guid = Guid.NewGuid().ToString(),
            Expires = DateTime.UtcNow.AddHours(1),
            Issuer = Environment.MachineName,
            Setting1 = sampleSettings.CurrentValue.Setting1,
            Setting2 = sampleSettings.CurrentValue.Setting2,
        };
    }
}
```

(You can see the resulting project in this public GitHub [repository](https://github.com/jeremiahflaga/containers-playground/tree/master/2020-08-28-docker-swarm-with-config).)


### Step 5: Create Docker image

Open a command window in **root folder** of the project created in Step 1 (the folder where the `.sln` file is located).

Execute this command to build a Docker image named `tokengen-img`:

``` shell
docker build -t tokengen-img -f ./TokenGen/Dockerfile .
```

After that, when you execute 

``` shell
docker images
```

you will see `tokengen-img` as one of the images listed.

```
REPOSITORY                             TAG                 IMAGE ID
tokengen-img                           latest              be76db52e663
mcr.microsoft.com/dotnet/core/sdk      3.1-buster          9ab567a29502
mcr.microsoft.com/dotnet/core/aspnet   3.1-buster-slim     bdca989bc8d3
```


### Step 6: Use Docker Swarm

First, enable Docker Swarm mode by executing this command:

``` shell
docker swarm init
```

Then create a service. (For an explanation of the difference between a Docker container and a Docker service, go [here](https://stackoverflow.com/a/46646524/1451757).)

``` shell
docker service create --publish 5000:80 --name tokengen tokengen-img
```

Execute this command to check if the service is running:

``` shell
docker service ls
```

```
ID             NAME      MODE        REPLICAS  IMAGE                PORTS
y547fhp4q76x   tokengen  replicated  1/1       tokengen-img:latest  *:5000->80/tcp
```

Now, open a **PowerShell** window then run this:

``` shell
Invoke-RestMethod http://localhost:5000/api/token
```

You will get a response that looks similar to this:

```
guid     : 16488c14-5e91-470e-819f-1a36423b8049
expires  : 2020-09-02T05:41:22.8778958Z
issuer   : f579e9da1691
setting1 : setting-1
setting2 : setting-2
```


### Step 7: Update app settings

Update the app settings by running this command:

``` shell
docker service update --env-add SampleSettings:Setting1=new-setting-1-value tokengen
```

Note that we are `SampleSettings:Setting1`, with the **colon**. That is how you refer to the `Setting1` setting inside the `appsettings.json` file:

``` json
"SampleSettings": {
    "Setting1": "setting-1"
...
```


Open the **PowerShell** window again then run

``` shell
Invoke-RestMethod http://localhost:5000/api/token
```

You will see that `setting1` has now changed to the new value, `new-setting-1-value`:

```
guid     : b88f9715-5a78-4cd5-899e-2a05210cde25
expires  : 2020-09-02T05:48:58.228479Z
issuer   : 2e3b9e0e4949
setting1 : new-setting-1-value
setting2 : setting-2
```

The end. 

Bow. :bow:



-----

**Update (Oct 11, 2020):**

I found a very interesting talk about Docker: ["Heresy in the Church of Docker"](https://www.youtube.com/watch?v=RB6MvSEaMKI&ab_channel=DevOpsDaysSiliconValley) by Corey Quinn, a Cloud Economist. Watch it if you have time :smile: 



-----

<div class="small" markdown="1">

#### References:

- ["Scale ASP.NET Core Apps with Docker Swarm Mode"](https://www.pluralsight.com/guides/scale-asp-net-core-apps-with-docker-swarm-mode) by Stefan Prodan
- ["Dockerize an ASP.NET Core application"](https://docs.docker.com/engine/examples/dotnetcore/) from Docker docs
- ["How Visual Studio builds containerized apps (version vs-2019)"](https://docs.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2019)
- [difference between Docker container and Docker service](https://stackoverflow.com/a/46646524/1451757)
- ["Configuration should use the options pattern."](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-3.1)
- ["Options pattern in ASP.NET Core"](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.1)
- ["How to set an environment variable in a running docker container"](https://stackoverflow.com/questions/27812548/how-to-set-an-environment-variable-in-a-running-docker-container)

</div>
