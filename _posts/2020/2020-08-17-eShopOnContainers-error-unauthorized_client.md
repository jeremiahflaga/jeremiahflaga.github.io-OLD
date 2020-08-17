---
layout: post
title: 'A eShopOnContainers error: "Sorry, there was an error: unauthorized_client"'
subtitle: '"Don’t strive for perfect code; strive for perfect boundaries."'
categories: [Programming]
tags: [Programming, DDD, PPPDDD, Scott Millett, Robert C. Martin, Jonathan Boccara, Eric Evans]
date: 2020-08-31 10:25:00 AM UTC
background: /images/background/small/arif-riyanto-vJP-wZ6hGBg-unsplash.jpg
published: fase
---

<!-- started August 17, 2020 07:08 PM Philippine Time -->

Are you getting this error when you click on the Login button of the eShopOnContainers project? Or when you click on the Authorize button of a swagger page in that project?:

```
Sorry, there was an error : unauthorized_client
```

<!--more-->

Here's what solved it for me:

**Step 1:**

Open the file `src/.env`


The solution is so simple, but I'm going to put it here so that [I will know where to look when I forget this in the future](https://www.hanselman.com/blog/TheVBEquivalentToCTypeofKeyword.aspx):

> You must use http://docker.for.win.localhost:5100 url.
> 
> --- [by KooshkakiH](https://github.com/dotnet-architecture/eShopOnContainers/issues/1258#issuecomment-593800988)

That's it.

Enjoy!

**References:**

- https://github.com/dotnet-architecture/eShopOnContainers/issues/1236

- https://github.com/dotnet-architecture/eShopOnContainers/issues/1258

- https://github.com/dotnet-architecture/eShopOnContainers/issues/1273

- https://github.com/dotnet-architecture/eShopOnContainers/wiki/unauthorized_client-error-on-login


- [Stop all docker containers at once on Windows](https://stackoverflow.com/a/48813850/1451757)

> In Git Bash or Bash for Windows you can use this Linux command:
> 
> `docker stop $(docker ps -q)`

- [Delete all Stopped Containers](https://stackoverflow.com/a/42458318/1451757)
  
> `docker rm $(docker ps -a -q)`

- Restart machine