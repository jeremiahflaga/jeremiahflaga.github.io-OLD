---
layout: post
title: 'An ASP.NET MVC Web API implementation of Uncle Bob Martin''s Clean Architecture example'
subtitle: Does the Controller have to know about the Presenter?
categories: [Programming]
tags: [Programming]
date: 2021-05-30 04:00:00 PM UTC
dateLastUpdated: 2021-06-12 07:40:00 AM UTC
published: true
pinned: false
---

<!-- Started May 21, 2021 5:39 AM Philippine Time -->
<!-- Updated June 5, 2021 6:00 PM Philippine Time -->
<!-- Updated June 12, 2021 3:40 PM Philippine Time - added Uncle Bob's link to example of Controller knowing about the Presenter -->

<div class="small alert alert-danger" markdown="1">

This post is just a personal inquiry into how to do a clean architecture implementation in ASP.NET MVC Web API without making the Controller know about the Presenter.

This might not give any value to you. :grin: If you want more valueable stuffs, read Steven van Deursen's posts [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/) and [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/) instead.

</div>

In his [talks on Clean Architecture](/2021/05/22/notes-on-architecture-the-lost-years-of-uncle-bob-martin), Uncle Bob gives this diagram as an example of how to implement it:

![CleanArchitectureDesignByUncleBobMartin.png](/images/2017/CleanArchitectureDesignByUncleBobMartin.png)

<!--more-->

In the diagram, the **Controller** does not point to the **Presenter**, which means that the **Controller** does not know about the **Presenter**. That means that there is no code like `var presenter = new Presenter()` inside the **Controller**. And there is no instance level field like `private Presenter presenter`, or instance level property like `private Presenter presenter { get; set; }` inside the **Controller**. <small>(Maybe he knows intuitively that making the Controller know about the Presenter will make things harder to manage?? I don't know. Maybe.)</small>

More on this later...

<div class="small alert alert-danger" markdown="1">

**Update, June 12, 2021:** I just learned that Uncle Bob has an example of the Controller knowing about the Presenter [here](https://softwareengineering.stackexchange.com/questions/423333/clean-architecture-how-does-the-use-case-interactor-generate-different-outputs). This shows yet another example of the fact that the relationship between the Controller and the Presenter depends on the kind of application you are building.

So, like I said above, this post might not give any value to you. Don't waste your time reading this, Read Steven van Deursen's posts [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/) and [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/) instead.

But if you have time to waste, this post might give you an idea on what **NOT** to do in your ASP.NET MVC Web API application. :smile:

</div>


**This post focuses only on the interaction between the Controller, the Interactor, and the Presenter of that diagram** (of course, because ASP.NET MVC Web API only touches on those parts of the diagram). This post does not talk about the other parts of the diagram. If you want to understand everything in that diagram, I encourage you to watch his [talks titled "Architecture: The Lost Years" or "Clean Architecture and Design"](/2021/05/22/notes-on-architecture-the-lost-years-of-uncle-bob-martin). And to read his artistically done article on it: ["A Little Architecture"](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html).


## Descriptions

<!-- 
_(If you are in a hurry, go ahead to the ["Implementation in ASP.NET MVC Web API" section](#implementation-in-aspnet-mvc-web-api) of this post.)_ 
-->

Let's zoom in on that diagram (still using an image from Uncle Bob's slides) and give a short description of some of the components that we will touch on in this blog post.

{: .w-75 }
![The dance between the Controller, the Interactor, and the Presenter](/images/2021/2021-05-24-controller-interactor-presenter-dance-fom-slides-of-uncle-bob.png)


#### The **Controller** 

--- the one who receives **InputData** from the user or the client, converts that input into a **RequestModel**, then passes the **RequestModel** to the **Interactor**. (Note that the **Controller** in that diagram is not necessarily the same as the **MVC-Controller** in ASP.NET. But if we implement that diagram in ASP.NET, we will be using the **MVC-Controller** as our **Controller**.)

{: .small }
```
class Controller {
    void Process(InputData inputData) {
        var requestModel = new RequestModel { ... };
        ...
        this.interactor.Process(requestModel);
    }
}
```

#### The **Interactor**

--- accepts the **RequestModel** given by the **Controller**, does the work described in the use case document, then creates a **ResponseModel** which it passes to the **Presenter**.  (Maybe you have heard of things called Application Services, or Command Handlers, or Query Handlers, or Use Case Handlers. The **Interactor** has the same purpose as those things. And remember, if you watched Uncle Bob's talks, that the **ResponseModel** here is NOT the same as the the HTTP Response that web developers are familiar with.)

{: .small }
```
class Interactor {
    void Process(RequestModel requestModel)  {
        try {
            ...
            var responseModel = new ResponseModel { ... };
            ...
            this.presenter.Present(responseModel);
        } catch (Error error) {
            this.presenter.PresentError(error.Message);
        }
    }
}
```

#### The **Presenter**

--- accepts the **ResponseModel** given by the **Controller**, converts that into a **ViewModel**, then passes the **ViewModel** to the **View** which is the UI in our case.

{: .small }
```
class Presenter {
    void Present(ResponseModel responseModel) {
        var viewModel = new ViewModel { ... };
        ...
        this.view.Show(viewModel);
    }
    
    void PresentError(string errorMessage) {
        this.view.ShowError(errorMessage);
    }
}
```
<!-- 
Now, to the main focus of this post...
 -->

## The dance between the **Interactor**, and the **Presenter**

This is the diagram Uncle Bob uses to show the interaction between the **Interactor** and the **Presenter**.

![The dance between the Interactor and the Presenter](/images/2021/2021-05-24-interactor-presenter-dance-fom-slides-of-uncle-bob.png)

The **Boundary** is an interface:

{: .small }
```
interface Boundary 
{
    void Present(ResponseModel responseModel);
}
```

We can see in that diagram that the **Presenter** points to the **Boundary** using an arrow with a hollow triangle-head. 

![Presenter pointing to Boundary](/images/2021/2021-05-24-presenter-pointing-to-boundary-interface.png)

That means that the **Presenter** implements the **Boundary**, like this:

{: .small }
```
class Presenter : Boundary { ... }
```

(In C#, the **Boundary** is either an abtract class or an interface.)

Now, there are lots of objects called Boundary in Uncle Bob's example, so we will name the **Boundary** that the **Presenter** implements as `PresenterBoundary`. Or maybe name it `IPresenterBoundary` (because it is customary in .NET to prefix interfaces with "`I`"). Or make it shorter, like `IPresenter`:

``` csharp
interface IPresenter 
{
    void Present(ResponseModel responseModel);
    void PresentError(string errorMessage);
}

class Presenter : IPresenter { ... }
```

Nice.

The **Interactor** points to the **Boundary** (or `IPresenter`) with a normal arrow head. 

![Interactor pointing to Boundary](/images/2021/2021-05-24-interactor-pointing-to-boundary-interface.png)

That means that **Interactor** contains or uses an `IPresenter`. We will use the name `UseCaseHandler` for our **Interactor**:

``` csharp
class UseCaseHandler
{
    private IPresenter presenter;

    // constructor
    public UseCaseHandler(IPresenter presenter) 
    {
        this.presenter = presenter;
    }
}
```

{: #implementation-in-aspnet-mvc-web-api}
## Implementation in ASP.NET MVC Web API

When implementing Uncle Bob's diagram in ASP.NET MVC Web API, the **MVC-Controller** (which I will call `MVC_Controller` here) will play the role of the **Controller** of Uncle Bob's diagram, because in ASP.NET the `MVC_Controller` is the one who accepts the input (or **InputData**) from the user.

Now, the **MVC-Controller**, or `MVC_Controller`, is also the one who gives back the output (or **ViewModel**) to the user:

``` csharp
[ApiController]
[Route("api/clean-architecture-example")]
class MVC_Controller : ControllerBase
{
    [HttpPost]
    public IActionResult Process(InputData inputData)
    {
        ...
        var viewModel = new ViewModel();
        ...
        return Ok(viewModel);
    }
}
```

... that means that the `MVC_Controller` will also play the role of the **Presenter**.

We do that by making the `MVC_Controller` implement the `IPresenter` interface.

``` csharp
[ApiController]
[Route("api/clean-architecture-example")]
class MVC_Controller : ControllerBase, IPresenter
{
    private readonly UseCaseHandler interactor;

    private IActionResult Result;

    public MVC_Controller(UseCaseHandler interactor)
    {
        this.interactor = interactor;
    }

    #region "Controller"
    [HttpPost]
    public IActionResult Process(InputData inputData)
    {
        var requestModel = new RequestModel {...};
        this.interactor.Process(requestModel);
        return Result;
    }
    #endregion

    #region "Presenter"
    [NonAction]
    public void Present(ResponseModel responseModel)
    {
        var viewModel = new ViewModel();
        this.Result = Ok(viewModel);
    }

    [NonAction]
    public void PresentError(string errorMessage)
    {
        this.Result = Problem(errorMessage);
    }
    #endregion
}
```

Now, the **Presenter** needs a way to pass the **ViewModel** it produced to the `MVC_Controller`. That is what the private property `IActionResult Result` is for: 

``` csharp
    ...
    private IActionResult Result;
    ...
```

the Presenter will put the ViewModel it produced into the `Result` property, then the Controller will access it after calling the Interactor's `Process` method.

(That's the most awkward code in this implementation --- we have to remember to call the `interactor.Process()` before returning the `Result`. I might find a way someday to remove that awkward code. For now, writing a unit test to enforce that code sequence might help.)

Then in the [`main` or entry point of the application](https://blog.ploeh.dk/2011/07/28/CompositionRoot/), we construct our objects and register them to our IoC container:

``` csharp
services.AddScoped<UseCaseHandler>();
services.AddScoped<IPresenter, MVC_Controller>();
services.AddScoped<IRepository, Repository>();
```

<div class="small alert alert-danger" markdown="1">

Please note that the clean architecture implementation presented in this blog post --- an ASP.NET implementation where the Controller does not know about the Presenter --- might not be useful to softwares being built these days --- the client-server kinds of applications.

Steven van Deursen's implementation presented [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/) and [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/) seems best for those kinds of applications. If you follow that implementation, there is no need for a Presenter (or an `IPresenter` interface); the Controller will be the one who will create the ViewModels.

The implementation presented in this post is useful to software where there is a need for different kinds of Controllers and Presenters, which reuses the module where the Interactor resides.

For example, if you need to have Web API endpoints in the server, a console app in the server, and a rich-client app in the server, all of which uses the same Interactors --- these three will have different Controllers and Presenters, but they reuse the Interactors.

</div>


**WARNING:** The code above will actually throw a [circular dependency error](https://medium.com/software-ascending/circular-dependencies-in-dependency-injection-403b790daebb) like this:

<!-- 
> circular dependencies in software are solvable because the dependencies are always self-imposed by the developers. To break the circle, all you have to do is break one of the links. 
>
> --- Brian Mearns (Circular Dependencies in Dependency Injection)
-->

```
System.AggregateException: 'Some services are not able to be constructed ...
A circular dependency was detected for the service of type ...
```

To solve that, we [create a factory for our use case handlers](https://stackoverflow.com/a/41072001/1451757): 

``` csharp
public class UseCasesFactory
{
    private readonly IRepository repository;

    public UseCasesFactory(IRepository repository)
    {
        this.repository = repository;
    }

    private UseCaseHandler __useCaseHandler;
    public SendGreetingHandler CreateUseCaseHandler(IPresenter presenter)
    {
        if (__useCaseHandler == null)
            __useCaseHandler = new UseCaseHandler(presenter, repository);
        return __useCaseHandler;
    }
}
```

Then we remove the `UseCaseHandler` from our IoC container, and register our `UseCasesFactory` instead:

``` csharp
services.AddScoped<UseCasesFactory>();
services.AddScoped<IPresenter, MVC_Controller>();
services.AddScoped<IRepository, Repository>();
```

And then we use our `UseCasesFactory ` in our Controller, passing the presenter instance during the creation of the `UseCaseHandler`, like this:


``` csharp
[ApiController]
[Route("api/clean-architecture-example")]
class MVC_Controller : ControllerBase, IPresenter
{
    private readonly UseCasesFactory factory;

    private IActionResult Result;

    public MVC_Controller(UseCasesFactory factory)
    {
        this.factory = factory;
    }

    [HttpPost]
    public IActionResult Process(InputData inputData)
    {
        var requestModel = new RequestModel {...};
        // pass in the presenter instance to the factory
        var interactor = this.factory.CreateUseCaseHandler(this);
        interactor.Process(requestModel);
        return Result;
    }
    
    ...
}
```

Have fun coding!

If you want code you can run, I've created a simple example [here](https://github.com/jeremiahflaga/hello-world-layered-vs-clean-architecture). Go to the `csharp` folder, open the solution file in Visual Studio 2019, then set the project named `CleanAspNet.WebApi` as the startup project. Then run the application.


<!-- 
<div class="message" markdown="1">

I think this Clean Architecture idea is most useful only to the software that is expected to become big, or software that is expected to be used for many years. This will not be very useful, and might be a waste of time to implement, to software that will live only for a few months.

Actually, it will still be useful for you and for other devs if you use this Clean Architecture idea in smaller or short-lived softwares --- you can use smaller softwares to practice doing clean architecture, because [it's better to practice doing clean architecture in the small](http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/). But, depending on your situation, you might have to ask permission from your employer, so that he will not be surprised if the initial phase of the project will take a long time to finish.

</div>
 -->

<div class="message small" markdown="1">

I think this Clean Architecture idea is most valuable during the maintenance phase<sup id="maintenance-phase-footnote-indicator">[[1]](#maintenance-phase-footnote)</sup> of a software system, because it is during that phase when lots of changes in the business rules are going to happen, and changes in business rules can be implemented easily only if the software is structured properly. 

I think that means that this Clean Architecture idea is very valuable only when the software is expected to become big, or when the software is expected to be used for many years, because in those cases the maintenance phase will be longer.

This Clean Architecture idea might not be very useful, and might be a waste of time<sup id="waste-of-time-footnote-indicator">[[1]](#waste-of-time-footnote)</sup> to implement, if your software will live only for a few months.

Actually, it will still be useful for you and for other devs if you use this Clean Architecture idea in smaller or short-lived softwares --- you can use smaller softwares to practice doing clean architecture, because [it's better to practice doing clean architecture in the small](http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/). But, depending on your situation, you might have to ask permission from your employer, so that he will not be surprised if the initial phase of the project will take a long time to finish.

</div>

-----

### PS:

The ASP.NET MVC Web API code given in this post implements Uncle Bob's diagram exactly the way it is drawn --- where the Controller does not know about the Presenter. But this is not the only way to implement a clean architecture in an MVC-framework (so called). Other kinds of implementation are also given by others, for example [here](https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/) and [here](https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data).

Some of these implementations make the Controller know about the Presenter, but with the tradeoff of making some duplicate logic in the Controller and the Interactor, especially on managing errors.

Some of these implementations are implemented the way Uncle Bob draws them in the diagram, but might not be compatible with the very interesting idea of using decorators to implement cross-cutting concerns, as presented by Steven van Deursen [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/) and [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/), and by Mark Seemann [here](https://blog.ploeh.dk/2010/04/07/DependencyInjectionisLooseCoupling/) and [here](https://blog.ploeh.dk/2010/09/20/InstrumentationwithDecoratorsandInterceptors/).



-----

**Footnotes**:

<sup id="maintenance-phase-footnote">[1]</sup>
<small>
Of all the aspects of a software system, maintenance is the most costly. The 
never-ending parade of new features and the inevitable trail of defects and 
corrections consume vast amounts of human resources.
...
A carefully thought-through architecture vastly mitigates these costs. By 
separating the system into components, and isolating those components through 
stable interfaces, it is possible to illuminate the pathways for future features and 
greatly reduce the risk of inadvertent breakage. 
(Uncle Bob Martin, Clean Architecture: A Craftsman's Guide to Software Structure and Design <!-- chapter 15 -->)
</small>
[&#8617;](#maintenance-phase-footnote-indicator)


<sup id="waste-of-time-footnote">[1]</sup>
<small>
... architectural boundaries exist everywhere. We, as architects, must be careful to recognize when they are needed. We also have to be aware that such boundaries, fully implemented, are expensive. On the other hand, we also have to recognize that when such boundaries are ignored, they are very expensive to add in later — even in the presence of comprehensive test-suites and refactoring discipline.
(Uncle Bob Martin, Clean Architecture: A Craftsman's Guide to Software Structure and Design <!-- chapter 25 -->)
</small>
[&#8617;](#waste-of-time-footnote-indicator)

<!-- 

Clean Architecture: A Tale of Two Stories - https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/
Clean Architecture: Use case containing the presenter or returning data? - https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data?rq=1

"Meanwhile... on the command side of my architecture" by Steven van Deursen
"Meanwhile... on the query side of my architecture" by Steven van Deursen
"Dependency Injection is Loose Coupling" by Mark Seemann
"Instrumentation with Decorators and Interceptors" by Mark Seemann
"Policy, Mechanism, And The Preservation Of Business Value" by Eddie Bush
 -->



<!-- 

-----

## A less desirable implementation

We can implement Uncle Bob's example in ASP.NET MVC Web API by making the **Controller** know about the **Presenter**, like this:

``` csharp
[ApiController]
[Route("api/clean-architecture-example")]
class MVC_Controller : ControllerBase
{
    [HttpPost]
    public IActionResult Process(InputData inputData)
    {
        var presenter = new Presenter();
        var interactor = new UseCaseHandler();
        var requestModel = new RequestModel { ... };
        var responseModel = interactor.Process(requestModel);
        
        if (responseModel.HasError) {
            if (responseModel.Error == <some-error>)
                return <some-error-message-to-user>;
            else if (responseModel.Error == <some-other-error>)
                return <some-other-error-message-to-user>;
        }

        var viewModel = presenter.GenerateViewModelFrom(responseModel);
        ...
        return Ok(viewModel);
    }
}
```

The problem with this kind of implementation is that (duplication of logic in controller and interactor)

also Uncle bob said about `main`


=====
Now, if we try to implement that in ASP.NET MVC Web API we will bump into a problem, because in ASP.NET MVC Web API the **MVC-Controller** (which I will call `MVC_Controller` here) is the one who both accepts input from the user and gives output back to the user. It is the one who accepts the **InputData** and the one who passes the **ViewModel** to the **View**.

I'm not going to get into the details here, but if we try to experiment on implementing that in ASP.NET MVC (or even in other MVC frameworks on other platforms), we will end up with something that is quirky to work with, because we will have to make the **Controller** know about the **Presenter**

(Please note that in Uncle Bob Martin's diagram there is no arrow from the **Controller** to the **Presenter**. Maybe he knows intuitively that making the Controller know about the Presenter will make things hard to work with.)

I'm not saying that it's wrong to put an arrow from the **Controller** to the **Presenter**, but if we do that, it seems to me, based on my little experiments, that the code will be harder to work with in the long run, especially when the software gets big.
=====



But, you might have noticed that  in Uncle Bob's diagram, there is no arrow from the Controller to the Presenter. That means that the Controller does not know about the Presenter.

Is there a way 

How are we going to do that without making the **Controller** know about the **Presenter**? --- You might have noticed that there is no arrow from the Controller to the Presenter in Uncle Bob's diagram. That means that the Controller does not know about the Presenter.


=====
... at compile time they are do no
=====


But we can solve that problem by making the `MVC_Controller` to be both the **Controller** and the **Presenter**. We do that by making the `MVC_Controller` implement the `IPresenter` interface.

``` csharp lineno
[ApiController]
[Route("api/clean-architecture-example")]
class MVC_Controller : ControllerBase, IPresenter
{
    private readonly UseCaseHandler interactor;

    private IActionResult Result;

    public MVC_Controller(UseCaseHandler interactor) 
    {
        this.interactor = interactor;
    }

    #region "Controller"
    [HttpPost]
    public IActionResult Process(InputData inputData)
    {
        var requestModel = new RequestModel {...};
        this.interactor.Process(requestModel);
        return Result;
    }
    #endregion

    #region "Presenter"
    [NonAction]
    public void Present(ResponseModel responseModel)
    {
        var viewModel = new ViewModel();
        this.Result = Ok(viewModel);
    }

    [NonAction]
    public void PresentError(string errorMessage)
    {
        this.Result = Problem(errorMessage);
    }
    #endregion
}
```

Remember that the `MVC_Controller` is also the one who will give the output to the user (the **ViewModel** is the output for the user), so the **Presenter** needs a way to pass the **ViewModel** it produced to the `MVC_Controller`. That is what the private property `Result` in line number 5 is for: the Presenter will put the ViewModel it produced into the `Result` property, then the Controller will access it after calling the 

``` csharp
    private IActionResult Result;
```


=====
Not all duplication is bad. Even in the real world there are duplicates, and they are not bad --- smaller organizations are still electing or appointing presidents even when we already have a president in our country. And I think there is such a thing as premature de-duplication --- sometimes, we have to wait for a few days or weeks before we can be sure if something that looks the same are really the same. Also, sometimes, it's also okay to make a temparary kind of re-duplication (what Jimmy Bogard calls **defactoring** in [one of his talks](/2019/05/20/vertical-slice-architecture-is-it-incompatible-with-clean-architecture)) to make it easier to understand things and to fix things; and I think it's even okay to make a permanent re-duplication if we discovered  that something we thought is the same are not really the same.
=====

 -->
