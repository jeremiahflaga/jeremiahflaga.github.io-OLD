---
layout: post
title: 'Encountering "Vertical Slice Architecture"... Is it incompatible with Clean Architecture?'
subtitle: '... for now, it''s okay to have duplicates in code, as long as there is very clear separation between features'
categories: [Programming]
tags: [Vertical Slice Architecture, Clean Architecture, Jimmy Bogard, Robert Martin, Steven van Deursen]
date: 2019-05-20 7:20:00 AM UTC
dateLastUpdated: 2021-05-23 06:00:00 AM UTC
pinned: true
---

<!-- May 20, 2019 3:20:00 PM Philippine Time -->
<!-- UPDATED: May 21, 2019 2:00:00 PM Philippine Time -->
<!-- UPDATED: May 23, 2021 02:00:00 PM Philippine Time - added "defactoring"; updated section taken from youtube comments -->
<!-- UPDATED: May 24, 2021 06:48:00 PM Philippine Time - added link to Mar Seemann's article on using decorators to implement cross-cutting concerns -->

<!-- 
<small>
**_(Updated May 21, 2019: added more explanation about the difference between Vertical Slice Architecture and Clean Architecture)_**
</small>
 -->

While browsing through Scott Hanselman's blog, I came accross [a post](https://www.hanselman.com/blog/ExampleCodeOpinionatedContosoUniversityOnASPNETCore20sRazorPages.aspx) where something called [_"Vertical Slice Architecture"_ by Jimmy Bogard](https://jimmybogard.com/vertical-slice-architecture/) is mentioned. (Anything related to software architecture easily catches my attention these days :smile:) 

I clicked on the [link](https://jimmybogard.com/vertical-slice-architecture/) and read the article...

<!--more-->


> A traditional layered/onion/clean architecture is monolithic in its approach...
> 
> !["The Clean Architecture" digram of Uncle Bob Martin](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)
> 
> The problem is this approach/architecture is... you start to get many abstractions around **concepts that really shouldn't be abstracted** (Controller MUST talk to a Service that MUST use a Repository).


That diagram above is from Uncle Bob Martin's ["The Clean Architecture"](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) blog post! Is Jimmy Bogard criticizing Clean Architecture?

Hmmmm...






I need to learn more... So I watched his talk titled ["SOLID Architecture in Slices not Layers"](https://www.youtube.com/watch?v=wTd-VcJCs_M)...


<div class="message m-3" markdown="1">

One of the interesting things in that talk is his comparison of "Traditional N-Tier" and "DDD-style N-Tier architecture". Regarding that, he said this:

> "... looks like they just changed the name of things"
> 
> ![Traditional N-Tier vs DDD-style N-Tier by Jimmy Bogard](/images/2019/traditional-vs-ddd-style-n-tier-by-jimmy-bogard.png)

That statement and picture talks to my unconscious and says _"Learning more about DDD is not that important for now. You have other more important things to learn."_ Okay...

Another interesting thing is in about 6:00 to 8:00 minutes in the video, when he described his **first step** in changing a complicated code into a simpler one; what he calls the **defactoring step**, where he removed the existing patterns and **hardcoded everything into the controller**:

> In the applications I build these days, I really try to have the code tell me when I need patterns through code smells and design issues, and not try to just put patterns in place just because.

I think that is a good strategy to remember when trying to understand a complicated code: code with lots of branching if/else.

But those are not the focus of this blog post... :smile:

Okay back to my point...

</div>

In the talk, he gave some code examples. On first look, the examples look the same as the [Clean Architecture examples](/2017/08/16/clean-architecture-sample-projects/) I found more than a year ago, except that he is directly using what Uncle Bob calls _a concretion_ (which is Entity Framework's DbContext) in his Interactor classes or _use case handlers_, such as the `CreateStudentHandler` below:

<!-- (or _task handlers_; I will call classes such as the CreateStudentHandler below as **_task handlers_** in this post)... -->

``` csharp
public class CreateStudentHandler
{
    private readonly SchoolContext db;

    public CreateStudentHandler(SchoolContext db) 
        => this.db = db;

    public void Handle(CreateStudentCommand message)
    {
        var student = new Student() 
        {
            Name = message.Name,
        }
        this.db.Students.Add(student);
    }
}
```

... while the [Clean Architecture](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) projects uses _abstractions_  in their _use case handlers_, such as the IStudentRepository interface below:

``` csharp
public class CreateStudentHandler
{
    private readonly IStudentRepository repository;

    public CreateStudentHandler(IStudentRepository repository) 
        => this.repository = repository;

    public void Handle(CreateStudentCommand message)
    {
        var student = new Student() 
        {
            Name = message.Name,
        }
        this.repository.Add(student);
    }
}

public interface IStudentRepository
{
    void Add(Student student);
}

public class StudentRepository : IStudentRepository
{
    private readonly SchoolContext db;

    public CreateStudentHandler(SchoolContext db) => this.db = db;

    public void Add(Student student)
    {
        this.db.Students.Add(student);
    }
}
```

Also, if he is directly using DbContext in the _use case handlers_ then he must not have unit tests for them!

Looking at the [example project on GitHub](https://github.com/jbogard/ContosoUniversityDotNetCore), it does not have unit tests. But it has what he calls Integration Tests. 
<!-- (which I believe is the same as what others call [Functional Test or Acceptance Test or End-to-End Test](https://www.obeythetestinggoat.com/book/chapter_02_unittest.html)).  -->

At least it still has automated tests, right? It still deserves the attention of those who want tests in their application.

Okay... So the only difference between Vertical Slice Architecture and Clean architecture is that Vertical Slice Architecture does not use indirections (such as IStudentRepository in the code sample above) and therefore does not have unit tests(??) (Because unit testing will be hard without these indirections.)

_Let's try reading the [article](https://jimmybogard.com/vertical-slice-architecture/) again..._

> Instead of coupling across a layer, we couple vertically along a slice. **Minimize coupling between slices, and maximize coupling in a slice.**

Ahhhh.. so there it is... his goal --- he wants to "**_minimize_** coupling between slices, and **_maximize_** coupling in a slice."


But we can also <strong><em>min</em></strong>imize coupling between slices, and _also_ <strong><em>min</em></strong>imize coupling within a slice... Would that be better?

I don't know... The answer is always _"it depends"_ on what your goals are, I guess. :smile:

<!-- 
Here's another one from the article: 
-->

> New features only add code, you're not changing shared code and worrying about side effects. Very liberating!

Ahh! That might be the reason why Jimmy Bogard wants to <strong><em>max</em></strong>imize coupling in a slice --- because he wants to minimize code sharing between slices. **That sounds very good to me**, because when I am new to a project and does not yet understand the codebase, I would rather see code duplication than see code reuse with lots of if/else in them. :laughing:




<div class="message m-3" markdown="1">

Also, remember [what Sandi Metz said in "All the Little Things"](https://www.youtube.com/watch?v=8bZh5LMaSmE): 

> duplication is far cheaper than the wrong abstraction

And also, duplication might not be a problem **if** the system is divided into what is called bounded contexts in DDD:

> Even if the code is the same and it never changes, usually the duplication causes no problem. Duplication is a problem because you may update code in one place, and forget to update it in another. However, this is rarely a problem when you have loosely coupled bounded contexts that are intended to run in isolation. There are very few reasons that the same concept in two bounded contexts should be changed at the same time. So don’t worry about duplicating similar code, and instead focus on isolating your bounded context and maintaining their boundaries.
>
> --- Scott Millett, PPPDDD p. 157

</div>



<!-- 
if we will _not_ maximize coupling in a slice that means we will not have control over the implementation of our dependencies, and the implementation might have code sharing in there (someday, if not today).
 -->



Ultimately, I think, the difference between Clean Architecture and Vertical Slice Architecture lies on their focus or aim:

- Clean Architecture aims to separate the business rules from the I/O (thus the indirections)

- Vertical Slice Architecture aims to separate the code by features, aiming to minimize code sharing between features.


I think they are not necessarily mutually exclusive. I think you can combine them --- Clean Vertical Slice Architecture --- wherever your project and team might lead you to combine them, as long as you are aware of the tradeoffs of your chosen architecture, because, as Kent Beck says, there will always be [tradeoffs](https://twitter.com/kentbeck/status/702662471547355137?lang=en)...

<a href="https://makeagif.com/gif/kent-becks-tradeoffs-ZULuNO" title="Kent Beck's tradeoffs"><img src="https://i.makeagif.com/media/5-20-2014/ZULuNO.gif" alt="Kent Beck's tradeoffs"></a>

<small>_(I cannot find a Kent Beck quote on tradeoffs, so I will just put this related quote I found :smile:)_</small>

> ["First you learn the value of abstraction, then you learn the cost of abstraction, then you're ready to engineer" --- Kent Beck](https://twitter.com/kentbeck/status/258316233068396544?lang=en)

<!-- 

In the example code above, the IStudentRepository, which is passed to the constructor of CreateStudentHandler, is an abstraction. There is value in abstraction. It makes you write code without trying to remember everything there is to remember about your system. But it does that for a cost --- You _might_ lose the control over "<strong><em>max</em></strong>imizing coupling in a slice". You _might_ not be able to control whether the concrete class which will implement IStudentRepository will have code sharing all over the place. You _might_ not ... But if you know where you are going, and your team knows where it is going, and you communicate your goals well to the next team who will be working on that project, then you can be able to get things under control, I believe.

Here are my key takeaways:

1. If I am trying to refactor an existing codebase which does not have unit tests, then I will make Vertical Slice Architecture as my goal.

2. I I will be working on a new project, and my teammates does not want to write unit tests, then I will convince them to make Vertical Slice Architecture as our goal.

3. 

 -->

One last thing... For now, I do not like using the MediatR library used in the example project (perhaps because I have not yet given much time to study how it works?). I would rather use the interfaces introduced by Steven van Deursen [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/) and [here](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/)


----------

{: #enlightening-articles-of-Steven-van-Deursen }
## Steven van Deursen's enlightening articles!

Reading more about Vertical Slice Architecture also led me to these very enlightening articles of Steven van Deursen (on the same level as the article ["A Little Architecture"](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) had enlightened me):

- [Meanwhile... on the command side of my architecture](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/)

> "This article describes how a single interface can transform the design of your application to be much cleaner, and more flexible than you ever thought possible." --- Steven van Deursen


- [Meanwhile... on the query side of my architecture](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/)

> "Two simple interfaces will change the look of your architecture… forever." --- Steven van Deursen

These two articles are kind of more elaborate explanation of what I had read from Mark Seemann on the use of decorators to implement cross-cutting concerns in his blog post ["Dependency Injection is Loose Coupling"](https://blog.ploeh.dk/2010/04/07/DependencyInjectionisLooseCoupling/) (also [here](https://blog.ploeh.dk/2010/09/20/InstrumentationwithDecoratorsandInterceptors/))

Enjoy!



----------

<br />

<div class="small" markdown="1">

## More...

Googling to understand more about this Vertical Architecture thing led me to this [youtube comment thread](https://www.youtube.com/watch?v=SUiWfhAhgQw&lc=UgzDmpq_2SHwmuSgIL54AaABAg) (I think it's good if conversations like this be not lost in the comments section of youtube, so I'm going to paste them here):

> **Doormouse:** 
If you've had to cry with the Onion Architecture, you were doing it wrong.
> 
> **Jimmy Bogard:** 
We were doing onion architecture with the person that invented the term. I don't think we could have had a better chance to get it "right".
> 
>  **Doormouse:** 
> Jeffrey Palermo? And there's Robert C. Martin with Clean Architecture. 
> 
> So if it failed, please enlighten me why it did? 
> 
> We're having a blast with it, ever expanding our monorepo using it. Doesn't say it doesn't have its drawbacks or limitations, but it's definately better than classical architecture.﻿
> 
> **Jimmy Bogard:** 
> Yes, it was about 10 years ago we started a large project with onion architecture. We had done some small projects with it, this was the first large one. About 3 months in we ripped it all out. 
> 
> I have another talk, [SOLID Architecture in Slices not Layers](https://www.youtube.com/watch?v=wTd-VcJCs_M), that goes more in depth with where layers goes wrong. I blogged about this too [https://lostechies.com/jimmybogard/2013/10/10/put-your-controllers-on-a-diet-redux/](https://lostechies.com/jimmybogard/2013/10/10/put-your-controllers-on-a-diet-redux/)
> 
> But the gist was - **all this layered architecture guidance presumes a value in these layers and abstractions. Things like "don't depend directly on your ORM in case you want to swap it out". It turns out that all these ideas and scenarios weren't actually tested in practice**, and when we actually hit these scenarios, like wanting to swap our ORM, layers and abstractions actually made things *harder*. So we got rid of all those layers and abstractions, and called YAGNI on it, vowing only to add those layers and abstractions in when we felt true pain. 7-8 years later, still no pain.
> 
>  **Doormouse:** 
> ​@Jimmy Bogard Thank you for this clean and elaborate breakdown of your experience with Onion Architecture. While I sympathise with the YAGNI argument to a certain extent, because not every abstraction is needed to enable pluggability of other implementations, that's just a utopia, this is not the only reason for wanting to apply DI on key places (usually when you exit your system). 
> 
> For us having the abstractions in place for every (let's call it) IO device client, we were able to test all of our application's business logic in memory with every IO device stubbed in seconds. I don't see this being possible without DI and Onion Architecture provides a solid template to apply it in...
>
> Also we had a requirement to deploy our applications as WAR on an old application server as well as as microservices in the cloud. There were definately cases we could get away with only writing a new implementation for the cloud, without touching our core application. This would be the Open-closed principle in SOLID. 
> 
> So we're having three layers: Core, Integration and Delivery, no more, no less. Core contains all business logic and entities, basically most code. Integration all repository logic (JPA repositories, WS/Rest clients, etc, we just call them all repositories). Delivery contains how you want to deliver the Integration (like Spring Boot, WAR, Test etc). Also there proved to be a surprising amount of reusablity in the integration layer, because the repositories are so generic, containing no business logic at all. And a good amount of flexibility, because we could wire everything up in delivery the way we want it. All in one JVM for test or as smaller deliverables in the cloud. 
> 
> I wonder how all these benefits work out in vertical slices. I do have to stand by my initial point that someting must have gone wrong for this not to work. Especially for big projects it's so suitable. Endlessly scalable. I've used it now for almost 6 years successfully in different size projects. Keep it clean and don't break the architecture in a monorepo though, or there will be hell to pay.
>
> **Brandon Pearman**
> @Jimmy Bogard  So did Jeffrey Palermo build this project with you, and see it fail just like you? I think you are attributing the problems to the architecture where he may have other reasons for the failure... what is his side of the story?
> 
> **Jimmy Bogard**
> Brandon Pearman He disagreed with us ripping it out and applied it on a couple subsequent projects, which we ripped out again. He didn’t actually have to code against it, we did. We saw what it did to our code, and ditched it.
> 
> **Brandon Pearman**
> @Jimmy Bogard  yeah i get it. In my experience any application with significant complexity is going to have its issues. It's a trade off, and I think we need to be more open about the these risks and trade offs. For example you only hear about the good stuff of event sourcing, CQRS, microservices, OOP, DRY, etc. But it's rare to hear the issues and when NOT to use it or what details to avoid. When people have differences in opinion it is good because it highlights the bs. There are some issues with onion but it's stuff I adapt to and often live with to avoid the alternative problems


Let me repeat the interesting part of Jimmy Bogard's statement above:

> "... all this layered architecture guidance **presumes a value in these layers and abstractions**. Things like "don't depend directly on your ORM in case you want to swap it out". **It turns out that all these ideas and scenarios weren't actually tested in practice**, and when we actually hit these scenarios, like wanting to swap our ORM, layers and abstractions actually made things **\*harder\***." --- Jimmy Bogard

Hmmmm...

Reading ["A Little Architecture"](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) of Uncle Bob Martin and other resources on Clean Architecture was kind of an [enlighnement to me](/2017/08/09/clean-architecture-an-equivalent-to-one-language/) because it seemed to have with it attached the promise of control with our software projects.

But reading that experience of Jimmy Bogard and his team makes me rethink about using Clean Architecture in all of my future software projects :laughing:

A big plus for this Vertical Slice Architecture is that it seems to help new programmers quickly understand how the components of a project communicate, as hinted in another [youtube comment](https://www.youtube.com/watch?v=SUiWfhAhgQw&lc=UgxKn5DIimiDXq1MWqB4AaABAg):

> **César Afonso:**
As a team leader who had to deal with many many trainees entering one projects without knowledge on architectures or even programing, and leaving after a few months, I started to use a similar architecture sometime in 2015 on my projects. 
<br /><br />
On my experience, it makes a project look uglier to expert developers, because the separation of layers is not pretty well defined...
<br /><br />
But on the other [hand], ... **it's easier to new delevopers [to] understand the project and the components... they are not so afraid of changing something**... the projects have less bugs and are easier to find and fix it. 

... The indirections in projects employing the Clean Architecture will confuse programmers who do not _yet_ understand the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle). :smile: I think making beginning programmers understand the mindset behind Vertical Slice Architecture will help them stucture their code well even when they do not yet know much about architecture --- _"for now, it's okay to have duplicates in code, as long as there is very clear separation between features; we can refactor things later on when we already know how to refactor them"_.


</div>


<!-- 
----------

<small>(Added on May 23, 2019)</small>

## Blog posts of a fellow Filipino software developer, OJ Raqueño


While googling for "vertical architecture in angularjs", I came across a blog post by a fellow Filipino developer

http://www.ojdevelops.com/2017/09/vertical-slices-with-mediatr-part2.html





I'm not sure why nationality matters so much today

Why 

Origin of 

language barriers which, given my assumptions about the history of

 -->


----------

**(Update: May 23, 2019)**

While googling for "vertical architecture in angularjs", I came across a series of blog posts, OJ Raqueño, which will give you ideas on _why_ you should use Vertical Slice Architecture in your projects:

- [The Tyranny of Horizontal Architectures (and How You Might Escape): Part 1](http://www.ojdevelops.com/2018/07/the-tyranny-of-horizontal-architectures.html)

- [The Tyranny of Horizontal Architectures (and How You Might Escape): Part 2](http://www.ojdevelops.com/2018/07/the-tyranny-of-horizontal-architectures2.html)

You might also be interested in these (from the same author):

- [Vertical Slices Application Design with MediatR: Part 1](http://www.ojdevelops.com/2017/08/vertical-slices-with-mediatr.html)

- [Vertical Slices Application Design with MediatR: Part 2](http://www.ojdevelops.com/2017/09/vertical-slices-with-mediatr-part2.html)
