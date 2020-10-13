---
layout: post
title: 'IoC container lifetime of DbContext, Unit of Work, and Repositories'
subtitle: ''
categories: [Programming]
tags: [DbContext, Unit of Work, Repository, .NET]
date: 2020-10-17 01:00:00 AM UTC
---

<!-- started October 11, 2020 PM Philippine Time -->

I have always googled for the IoC container lifetime of DbContext, Unit of Work, and Repositories every time I create practice projects from scratch. I [tend to forget](https://www.hanselman.com/blog/TheVBEquivalentToCTypeofKeyword.aspx) what their lifetime should be, perhaps because I don't know the _why_ behind it.


<!--more-->

Good thing I found this [peer reviewed](https://www.wsj.com/articles/what-the-pandemic-has-taught-us-about-science-11602255638) resource named [".NET Microservices Architecture for Containerized .NET Applications"](https://github.com/dotnet-architecture/eBooks/blob/master/archives/microservices/NET-Microservices-Architecture-for-Containerized-NET-Applications-v3.1.1.pdf), which I am using in one of my trainings in the company I am currently working for, [Arcanys](https://www.arcanys.com/).

It says that the DbContext and Unit of Work should have the lifetime of _scoped_. And it gives the _why_ behind that recommendation:

> The `DbContext` object (exposed as an `IUnitOfWork` object) **should be shared among multiple repositories within the same HTTP request scope**. For example, this is true when the operation being executed must deal with multiple aggregates, or simply because you are using multiple repository instances... 
> 
> In order to do that, the instance of the `DbContext` object has to have its service lifetime set to `ServiceLifetime.Scoped`.

And regarding repositories, it says that it can have the _socped_ or the _transient_ lifetime:

> ... repository’s lifetime should usually be set as scoped (InstancePerLifetimeScope in Autofac). It could also be transient (InstancePerDependency in Autofac), but **your service will be more efficient in regards memory when using the scoped lifetime**.
> 
> Note that using the singleton lifetime for the repository could cause you serious concurrency problems when your DbContext is set to scoped (InstancePerLifetimeScope) lifetime (the default lifetimes for a DBContext).

I hope brain will not forget this ever...
