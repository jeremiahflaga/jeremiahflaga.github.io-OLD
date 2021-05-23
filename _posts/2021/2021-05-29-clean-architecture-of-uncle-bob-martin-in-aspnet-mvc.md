---
layout: post
title: An ASP.NET MVC (or ASP.NET Web API) implementation of Uncle Bob Martin's Clean Architecture example
subtitle: Does the Interactor have to know about the Presenter...
categories: [Programming]
tags: [Programming]
date: 2021-05-23 04:00:00 PM UTC
published: false
---

<!-- Started May 21, 2021 5:39 AM Philippine Time -->

In his talks on Clean Architecture, Uncle Bob gives this diagram as an example of how to implement it:

![CleanArchitectureDesignByUncleBobMartin.png](/images/2017/CleanArchitectureDesignByUncleBobMartin.png)

<!--more-->

If you do not yet understand what that diagram means, I encourage you to first watch his talks titled "Architecture: The Lost Years" or "Clean Architecture and Design", then come back here to read the rest of this post, because **this post focuses only on the dance between the **Interactor**, and the **Presenter** of that diagram** and will not talk about the other parts of the diagram.

If you have watched his talks before but needs to refresh your memory, I posted my handwritten notes on his talks [here](/2021/05/22/notes-on-architecture-the-lost-years-of-uncle-bob-martin). (WARNING: those notes might be hard to read; you might want to re-watch his videos instead)


## the **Controller**, the **Interactor**, the **Presenter**

Let's zoom in on that diagram (still using an image from Uncle Bob's slides) and give a short description of some of the components that we will touch on in this blog post.

![The dance between the Controller, the Interactor, and the Presenter](/images/2021/2021-05-24-controller-interactor-presenter-dance-fom-slides-of-uncle-bob.png)


##### The **Controller** 

--- The one who recieves **InputData** from the user or the client, converts that input into a **RequestModel**, then passes the **RequestModel** to the **Interactor**. (Note that the _meaning_ of **Controller** in that diagram is not to be confused with the `MVC_Controller` in ASP.NET. But we will still use the `MVC_Controller` to implement the **Controller** in that diagram.)

{: .small }
```
class Controller
{
    void Execute(InputData inputData)
    {
        var requestModel = new RequestModel { ... };
        ...
        this.interactor.Execute(requestModel);
    }
}
```

##### The **Interactor**

--- Accepts the **RequestModel** given by the **Controller**, does the work described in the use case document, then creates a **ResponseModel** which it passes to the **Presenter**.  (Maybe you have heard of things called Application Services, or Command Handlers, or Query Handlers, or Use Case Handlers. The **Interactor** has the same purpose as those things. And remember that the **ResponseModel** here is NOT the same as the the HTTP Response that web developers are familiar with.)

{: .small }
```
class Interactor
{
    void Execute(RequestModel requestModel) 
    {
        ...
        var responseModel = new ResponseModel { ... };
        ...
        this.presenter.Present(responseModel);
    }
}
```

##### The **Presenter**

--- Accepts the **ResponseModel** given by the **Controller**, converts that into a **ViewModel**, then gives the **ViewModel** to the **View**

{: .small }
```
class Presenter
{
    void Present(ResponseModel responseModel)
    {
        var viewModel = new ViewModel { ... };
        ...
        this.view.Show(viewModel);
    }
}
```

Now, to the main focus of this post...


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

Now, there are lots of objects called Boundary in Uncle Bob's example, so we will name the **Boundary** that the **Presenter** implements to `PresenterBoundary`. Or maybe name it `IPresenterBoundary` (because it is customary in .NET to prefix interfaces with "`I`"). Or much better, name it `IPresenter` so it will be shorter:

``` csharp
interface IPresenter 
{
    void Present(ResponseModel responseModel);
}

class Presenter : IPresenter { ... }
```

Nice.

The **Interactor** points to the **Boundary** (or `IPresenter`) with a normal arrow head. 

![Interactor pointing to Boundary](/images/2021/2021-05-24-interactor-pointing-to-boundary-interface.png)

That means that **Interactor** contains or uses an `IPresenter`. We will name our interactor as `UseCaseHandler`:

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

## Implementation in ASP.NET MVC

Now, if we try to implement that in ASP.NET MVC (or ASP.NET Web API) we will bump into a problem, because in ASP.NET MVC the `MVC_Controller` is the one who both accepts input from the user and gives output back to the user. 

I'm not going to get into the details here, but if we try to experiment on implementing that in ASP.NET MVC, we will end up with something that is quirky to work with. And I think it's something that does not look good to the eye, because we will have to make the **Controller** know about the **Presenter**, which is something we do NOT see in Uncle Bob's diagram --- there is no arrow from the **Controller** to the **Presenter**.

I'm not saying that it's wrong to put an arrow from the **Controller** to the **Presenter**, but the code is harder to work with if we do that (at least to me that seems to be the case).

(There are discussions about this problem in other places too. For example, [here](https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/) and [here](https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data).)

But we can solve that problem by making the `MVC_Controller` to be both the **Controller** and the **Presenter**. We do that by making the `MVC_Controller` implement the `IPresenter` interface.

``` csharp
class MVC_Controller : IPresenter
{
    private UseCaseHandler interactor;

    // constructor
    public MVC_Controller(UseCaseHandler interactor) 
    {
        this.interactor = interactor;
    }

    [HttpGet]
    public IActionResult Get()
    {
        var requestModel = new RequestModel {...};
        this.interactor.Execute(requestModel);
    }    
}
```



<!-- 
Other discussion about this:
- Clean Architecture: A Tale of Two Stories - https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/
- Clean Architecture: Use case containing the presenter or returning data? - https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data?rq=1
 -->







----------
