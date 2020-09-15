---
layout: post
title: 'An eShopOnContainers error - "Sorry, there was an error: unauthorized_client"'
subtitle: ''
categories: [Programming]
tags: [Programming, DDD, PPPDDD, Scott Millett, Robert C. Martin, Jonathan Boccara, Eric Evans]
date: 2020-08-18 01:00:00 PM UTC
---

<!-- started August 17, 2020 07:08 PM Philippine Time -->
<!-- ended August 18, 2020 09:00 PM Philippine Time -->



{:.message}
_"... When you face a tough problem in your work then you find a breakthrough, it’s probably something others will face. Blog about it."_ - [Vaughn Vernon](https://web.archive.org/web/20180827140727/https://vaughnvernon.co/?p=879#comment-1938)


-----

Are you getting this error when you click on the Login button of the [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) project? Or when you click on the Authorize button of a swagger page in that project?:

{: .alert .alert-danger }
Sorry, there was an error : unauthorized_client

<!--more-->

Here's what solved it for me:

#### Step 1: db

After running the application, [open the SQL Server database](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Explore-the-application#all-applications-and-microservices) (connect with SSMS to `tcp:localhost,5433` with `User Id=sa;Password=Pass@word;`)

Then look at the `ClientRedirectUris` table in the `Microsoft.eShopOnContainers.Service.IdentityDb` database, using this script:


{: .small }
``` sql
SELECT * FROM [Microsoft.eShopOnContainers.Service.IdentityDb].[dbo].[ClientRedirectUris]
```

{: .table .table-sm .table-bordered .small }
**Id**  |	**RedirectUri**                                                   |	**ClientId**
1       |	http://**host.docker.internal**:5109/swagger/oauth2-redirect.html |	6
2       |	http://**host.docker.internal**:5110/swagger/oauth2-redirect.html |	7
...     | ...                                                               | ...
11      | http://10.121.122.162:5105/xamarincallback                        |	2
12      | http://**host.docker.internal**:5104/                             |	1


Note the `host.docker.internal` in that table


#### Step 2: .env

Open the `eShopOnContainers/src/.env` file. You will see this in about line #7:

{: .small .alert .alert-light }
\# Use this values to run the app locally in Windows <br />
ESHOP_EXTERNAL_DNS_NAME_OR_IP=**docker.for.win.localhost** <br />
ESHOP_STORAGE_CATALOG_URL=http://**docker.for.win.localhost**:5202/c/api/v1/... <br />
ESHOP_STORAGE_MARKETING_URL=http://**docker.for.win.localhost**:5110/api/v1/... <br />


Change the `docker.for.win.localhost` into `host.docker.internal` like this:

{: .small .alert .alert-light }
\# Use this values to run the app locally in Windows <br />
ESHOP_EXTERNAL_DNS_NAME_OR_IP=**host.docker.internal** <br />
ESHOP_STORAGE_CATALOG_URL=http://**host.docker.internal**:5202/c/api/v1/... <br />
ESHOP_STORAGE_MARKETING_URL=http://**host.docker.internal**:5110/api/v1/... <br />


#### Step 3: http://...:5100

[Open the MVC Web app using `http://host.docker.internal:5100/`](https://github.com/dotnet-architecture/eShopOnContainers/issues/1258#issuecomment-593800988) instead of `http://localhost:5100/`

Then click on the Login button...

Tada!!!


-----

<div class="small" markdown="1">

#### References:

- [After Docker-up get the issue on the login page](https://github.com/dotnet-architecture/eShopOnContainers/issues/1236)
- [Cannot get into login page](https://github.com/dotnet-architecture/eShopOnContainers/issues/1258)
- [Getting error of unauthorized_client when clicking on Login button from web mvc](https://github.com/dotnet-architecture/eShopOnContainers/issues/1273)
- [unauthorized_client error on Login](https://github.com/dotnet-architecture/eShopOnContainers/wiki/unauthorized_client-error-on-login)
- [eShopOnContainers: Explore the application](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Explore-the-application) 
- [Stop / remove all Docker containers](https://coderwall.com/p/ewk0mq/stop-remove-all-docker-containers)
- Restart machine

</div>





<!-- 
The solution is so simple, but I'm going to put it here so that [I will know where to look when I forget this in the future](https://www.hanselman.com/blog/TheVBEquivalentToCTypeofKeyword.aspx):

> You must use http://docker.for.win.localhost:5100 url.
> 
> --- [by KooshkakiH](https://github.com/dotnet-architecture/eShopOnContainers/issues/1258#issuecomment-593800988)

That's it.

Enjoy!
 -->
