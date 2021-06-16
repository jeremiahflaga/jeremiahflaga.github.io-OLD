---
layout: post
title: 'IoC container lifetime of DbContext, Unit of Work, and Repositories'
subtitle: ''
categories: [Programming]
tags: [DbContext, Unit of Work, Repository, .NET]
date: 2020-10-17 01:00:00 AM UTC
dateLastUpdated: 2021-06-16 08:00:00 AM UTC
---

<!-- started October 11, 2020 PM Philippine Time -->
<!-- Updated June 15, 2021 11:50:00 AM Philippine Time - added statements from Steven van Deursen, Mark Seemann, and others -->
<!-- Updated June 16, 2021 4:00:00 PM Philippine Time - added statements from Steven van Deursen, Mark Seemann, and others -->

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


-----

#### Other related and helpful statements:

> Since `DbContext` instances contain request-specific data and are not thread-safe, each request should get its own `DbContext` instance...
> 
> ...
> 
> Letting `ShoppingBasketRepository` have a Singleton Lifestyle would cause DbContext to be kept alive for the application’s lifetime. This is dreadful because that would cause it to be used by multiple requests simultaneously—a horrible prospect. Again: DbContexts are not thread-safe. 
> 
> --- Steven van Deursen, [The Closure Composition Model](https://blogs.cuttingedge.it/steven/posts/2019/closure-composition-model/)



> The ProductService class is a stateless service, and therefore thread-safe, so it's an excellent candidate for the Singleton.
> 
> --- Mark Seemann, [Captive Dependency](https://blog.ploeh.dk/2014/06/02/captive-dependency/)



> the rule of thumb is the inner object should have an equal or longer lifetime than the outer one --- akazemis, [comment on StackOverflow](https://stackoverflow.com/questions/38138100/addtransient-addscoped-and-addsingleton-services-differences)

-----


#### Related resources:

[**Captive Dependency**](https://blog.ploeh.dk/2014/06/02/captive-dependency/) by Mark Seemann

> Since then, I've thought of the name Captive Dependency, which may not be super-catchy, but at least accurately describes the problem. A longer-lived object (e.g. a Singleton) holds a shorter-lived object captive, past its due release time. Although the shorter-lived object should be released, it's not, because of a bureaucratic error.

> A Captive Dependency is a dependency that’s inadvertently kept alive for too long because its consumer was given a lifetime that exceeds the dependency’s expected lifetime. --- from [The Closure Composition Model](https://blogs.cuttingedge.it/steven/posts/2019/closure-composition-model/)


> Simple Injector has built in support for a number of container verifications including lifestyle mismatches (Captive Dependency is a lifestyle mismatch) through its Diagnostic Services.
> 
> ...
> 
> And for completeness we should also mention how to solve the captive dependency problem. From the really awsome [SimpleInjector documentation:](http://simpleinjector.readthedocs.io/en/latest/LifestyleMismatches.html)
> 
> - Change the lifestyle of the component to a lifestyle that is as short or shorter than that of the dependency.
> 
> - Change the lifestyle of the dependency to a lifestyle as long or longer than that of the component.
> 
> - Instead of injecting the dependency, inject a factory for the creation of that dependency and call that factory every time an instance is required.
> 
> For the above example you would probably want to introduce a factory for the DbContexts.
>
> --- qujck, from the [comments section of "Captive Dependency"](https://blog.ploeh.dk/2014/06/02/captive-dependency/#comments-header)

-----

[**Should I Use Entity Framework**](https://www.iamtimcorey.com/blog/137806/entity-framework) by Tim Corey

> My answer is typically "No". [But...]


-----

[**EntityFramework: Yes or No?**](https://www.cleverti.com/blog/entityframework-yes-or-no/) by Dinis Ferreira

> Like everything else, EF has qualities and flaws...


-----

Uncle Bob Martin also talked about ORM's in his blog posts ["Dance you Imps!"](https://blog.cleancoder.com/uncle-bob/2013/10/01/Dance-You-Imps.html) and ["Classes vs. Data Structures"](https://blog.cleancoder.com/uncle-bob/2019/06/16/ObjectsAndDataStructures.html)

