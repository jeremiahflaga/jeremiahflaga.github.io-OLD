---
layout: post
title: 'An exploration of NDepend'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2022-04-18 07:20:00 AM UTC
dateLastUpdated: 2022-04-27 12:00:00 AM UTC
---

<!-- May 24, 2021  Philippine Time - started -->
<!-- April 18, 2022 3:20 PM Philippine Time - finished-->
<!-- Updated April 27, 2022 08:00:00 AM Philippine Time - more explanation -->

Last year, I was given a free professional license for [NDepend](https://www.ndepend.com/) and agreed to write something about it. This very simple blog post is me trying to satisfy that agreement. :smile:

## Rules

The first feature of NDepend which got my attention was the "Rules violated" pane, which looks like the image below.

![](/images/2021/2021-11-07-ndepend-all-issues-pane.png)

You can view all these rules in the [NDepend Rules Explorer](https://www.ndepend.com/default-rules/NDepend-Rules-Explorer.html) --- Rules such as ["Avoid methods too big, too complex"](https://www.ndepend.com/default-rules/NDepend-Rules-Explorer.html?ruleid=ND1003#!), and ["Boxing/unboxing should be avoided"](https://www.ndepend.com/default-rules/NDepend-Rules-Explorer.html?ruleid=ND1314#!)

I think this kind of rules will help programmers write better code. It will help them produce more [readable code](2021/09/01/guidelines-and-resources-on-writing-clean-code), especially when used very early in a project.

An example of violations found during my exporations with NDepend was from [this code from eShopOnContainers](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/enumeration-classes-over-enum-types)

{: .small }
``` csharp
public class CardType : Enumeration
{
    public static CardType Amex = new(1, nameof(Amex));
    public static CardType Visa = new(2, nameof(Visa));
    public static CardType MasterCard = new(3, nameof(MasterCard));

    public CardType(int id, string name)
        : base(id, name)
    {
    }
}
```

Analyzing that code using NDepend produces this rules vaiolations report:

![](/images/2021/2021-11-07-ndepend-violation-CardType-Enumeration.png)

<!-- 
Fields should not be declared as private
Avoid non-readonly static fields
Avoid static fields with a mutable field type
 -->
 

To make those issues go away, we just need to make the fields read-only, like in this code:

{: .small }
``` csharp
public class CardType : Enumeration
{
    public static readonly CardType Amex = new(1, nameof(Amex));
    public static readonly CardType Visa = new(2, nameof(Visa));
    public static readonly CardType MasterCard = new(3, nameof(MasterCard));

    public CardType(int id, string name)
        : base(id, name)
    {
    }
}
```

One good thing about the rules in NDepend is that it gives an explanation of why such rules exist. For example, the rule _"Avoid non-readonly static fields - Immutability"_ has this explanation:

{: .small }
> ... Such mutable static fields create confusion about the expected state at runtime and impairs the code testability since the same mutable state is re-used for each test.
> 
> More discussion on the topic can be found here: https://blog.ndepend.com/the-proper-usages-of-the-keyword-static-in-c/

If you have programmed for so many years already, you might already know the reason why non-readonly static fields are bad. You might even have a first hand experience of why it's bad. But, as C.S. Lewis said, 

> "We have to be continually reminded of what we believe."

Our limited memory prevents us from remembering everything we have experienced since we started programming. [NDepend](https://www.ndepend.com/) is a great tool that can constantly remind us of what we believe or experienced to be bad or good in our .NET code.

We also have to remember that because tools like NDepend do not think like us humans do, they might suggest fixes that are not applicable to our application. So we also need to [understand the reasoning behind the rules](http://www.softwareonthebrain.com/2022/01/the-misunderstood-single-responsibility.html) that NDepend  

<!-- 
I do not know what the other implications of those changes are.

Also, you might not be able to do those changes in your project. But that is okay. We have to remember that because these tools do not think like us humans do, they sometimes produce false warnings --- warnings or suggested fixes that do not apply in a specific situation in our project --- and we have to learn to be wise when to heed the tool's advice and when not to. Because you know, robots cannot fix all of our problems for us :). They will be a big help to us. For example, with their help, we can have lots of food without spending most of our time producing food. But they cannot fix everything. But that is actually good for us, because that could eliminate our [fear of being ruled by them](https://www.youtube.com/watch?v=z0HsPBKfhoI&ab_channel=TED). That will give us the comfort that they are not going to rule the world anytime soon (even, they are not going to rule the world anytime forever?! Yaeey!)
 -->



## Diagram

Another good thing about NDepend is that it can generate a diagram of the structure of your codebase.

Following are digrams from the simple codebase I used in [a blog post on Clean Architecture vs the traditional Layered Architecture](/2021/09/12/hello-world-layered-vs-clean-architecture/):

![](/images/2021/2021-11-07-ndepend-clean-architecture-dependency-graph.png)

![](/images/2021/2021-11-07-ndepend-layered-architecture-dependency-graph.png)

We can see from the diagram whether or not our codebase adheres to the architectural idea we want to follow.


Here is another diagram generated from the codebase used in [another blog post](/2021/05/30/clean-architecture-of-uncle-bob-martin-in-aspnet-mvc-web-api/) of mine:

![](/images/2021/2021-11-07-ndepend-clean-aspnet-dependency-graph.png)

We can see in that diagram that our codebase is adhering to the [Clean Architecture](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) idea because both the Presentation (Web API) layer and the Data layer are pointing to the Domain layer.

Now, if we try to reference and use the ASP.NET module from our Domain layer (which we should not do, because the ASP.NET module is supposed to be used only in the Presentation layer), like this: 

![](/images/2021/2021-11-07-ndepend-referencing-aspnet-core-in-domain-layer.png)

... NDepend can show that to us in the diagram (we need to uncheck the filter named "Exclude third-party code elements" to be able to see that):

![](/images/2021/2021-11-07-ndepend-graph-of-referencing-aspnet-core-in-domain-layer.svg)

The red box in the lower-right part of the diagram is the assembly named `Microsoft.AspNetCore.Mvc.Core`. We can see from that diagram that both the Presentation layer (`CleanAspNet.WebApi`) and the Domain layer (CleanAspNet.Domain) are pointing to that assembly. But that assembly is supposed to be used only in the Presentation layer of our application, because we want to follow the Clean Architecture idea in this case.

We can see from these examples that NDepend can help us find out whether or not we are violating coding principles and architectural ideas we want to adhere to in our projects.


Those are just some of the capabilities of NDepend.

You can learn more from these links:

- [https://www.ndepend.com/docs/ndepend-use-cases](https://www.ndepend.com/docs/ndepend-use-cases)

- [https://www.ndepend.com/docs/videos](https://www.ndepend.com/docs/videos)


Have fun exploring :smile:



-----

### Other NDepend reviews:

["NDepend - Keep Technical Debt And Code Quality Under Control"](https://ivanderevianko.com/2020/01/ndepend-keep-technical-debt-and-code-quality-under-control) by Ivan Derevianko


