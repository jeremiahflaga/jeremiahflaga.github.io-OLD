---
layout: post
title: 'Encountering "Vertical Slice Architecture"... Is it incompatible with Clean Architecture?'
categories: [Programming]
tags: [Vertical Slice Architecture, Clean Architecture, Jimmy Bogard, Robert Martin, Steven van Deursen]
date: 2019-05-20 7:20:00 AM UTC
---

<!-- May 20, 2019 3:20:00 PM Philippine Time -->

While browsing through Scott Hanselman's blog, I came accross [a post](https://www.hanselman.com/blog/ExampleCodeOpinionatedContosoUniversityOnASPNETCore20sRazorPages.aspx) where something called [_"Vertical Slice Architecture"_ by Jimmy Bogard](https://jimmybogard.com/vertical-slice-architecture/) is mentioned. (Anything related to software architecture easily catches my attention these days :smile:) 

I clicked on the [link](https://jimmybogard.com/vertical-slice-architecture/) and read the article...

> A traditional layered/onion/clean architecture is monolithic in its approach...
<br /><br />
> !["The Clean Architecture" digram of Uncle Bob Martin](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)
<br />
> The problem is this approach/architecture is... you start to get many abstractions around **concepts that really shouldn't be abstracted** (Controller MUST talk to a Service that MUST use a Repository).


<!--more-->


That diagram above is from Uncle Bob Martin's ["The Clean Architecture"](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) blog post! Is Jimmy Bogard critisizing Clean Architecture?

Hmmmm...

Googling to understand more about this Vertical Architecture thing led me to this [youtube comment thread](https://www.youtube.com/watch?v=SUiWfhAhgQw&lc=UgzDmpq_2SHwmuSgIL54AaABAg) (it's good if conversations like this be not lost in the comments section of youtube, so I'm going to paste them here):

> **Doormouse2:** 
If you've had to cry with the Onion Architecture, you were doing it wrong.
<br /><br />
**Jimmy Bogard:** 
We were doing onion architecture with the person that invented the term. I don't think we could have had a better chance to get it "right".
<br /><br />
> **Doormouse2:** 
Jeffrey Palermo? And there's Robert C. Martin with Clean Architecture. 
<br /><br />
So if it failed, please enlighten me why it did? 
<br /><br />
We're having a blast with it, ever expanding our monorepo using it. Doesn't say it doesn't have its drawbacks or limitations, but it's definately better than classical architecture.﻿
<br /><br />
**Jimmy Bogard:** 
Yes, it was about 10 years ago we started a large project with onion architecture. We had done some small projects with it, this was the first large one. About 3 months in we ripped it all out. 
<br /><br />
I have another talk, [SOLID Architecture in Slices not Layers](https://www.youtube.com/watch?v=wTd-VcJCs_M), that goes more in depth with where layers goes wrong. I blogged about this too [https://lostechies.com/jimmybogard/2013/10/10/put-your-controllers-on-a-diet-redux/](https://lostechies.com/jimmybogard/2013/10/10/put-your-controllers-on-a-diet-redux/)
<br /><br />
But the gist was - **all this layered architecture guidance presumes a value in these layers and abstractions. Things like "don't depend directly on your ORM in case you want to swap it out". It turns out that all these ideas and scenarios weren't actually tested in practice**, and when we actually hit these scenarios, like wanting to swap our ORM, layers and abstractions actually made things *harder*. So we got rid of all those layers and abstractions, and called YAGNI on it, vowing only to add those layers and abstractions in when we felt true pain. 7-8 years later, still no pain.
<br /><br />
> **Doormouse2:** 
​@Jimmy Bogard Thank you for this clean and elaborate breakdown of your experience with Onion Architecture. While I sympathise with the YAGNI argument to a certain extent, because not every abstraction is needed to enable pluggability of other implementations, that's just a utopia, this is not the only reason for wanting to apply DI on key places (usually when you exit your system). 
<br /><br />
For us having the abstractions in place for every (let's call it) IO device client, we were able to test all of our application's business logic in memory with every IO device stubbed in seconds. I don't see this being possible without DI and Onion Architecture provides a solid template to apply it in...

<!--
 Also we had a requirement to deploy our applications as WAR on an old application server as well as as microservices in the cloud. There were definately cases we could get away with only writing a new implementation for the cloud, without touching our core application. This would be the Open-closed principle in SOLID. 
<br /><br />
So we're having three layers: Core, Integration and Delivery, no more, no less. Core contains all business logic and entities, basically most code. Integration all repository logic (JPA repositories, WS/Rest clients, etc, we just call them all repositories). Delivery contains how you want to deliver the Integration (like Spring Boot, WAR, Test etc). Also there proved to be a surprising amount of reusablity in the integration layer, because the repositories are so generic, containing no business logic at all. And a good amount of flexibility, because we could wire everything up in delivery the way we want it. All in one JVM for test or as smaller deliverables in the cloud. 
<br /><br />
I wonder how all these benefits work out in vertical slices. I do have to stand by my initial point that someting must have gone wrong for this not to work. Especially for big projects it's so suitable. Endlessly scalable. I've used it now for almost 6 years successfully in different size projects. Keep it clean and don't break the architecture in a monorepo though, or there will be hell to pay.

-->

Let me repeat the interesting part of Jimmy Bogard's statement above:

> "... all this layered architecture guidance **presumes a value in these layers and abstractions**. Things like "don't depend directly on your ORM in case you want to swap it out". **It turns out that all these ideas and scenarios weren't actually tested in practice**, and when we actually hit these scenarios, like wanting to swap our ORM, layers and abstractions actually made things **\*harder\***." --- Jimmy Bogard

Hmmmm...

Reading ["A Little Architecture"](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) of Uncle Bob Martin and other resources on Clean Architecture was kind of an [enlighnement to me](https://jeremiahflaga.github.io/2017/08/09/clean-architecture-an-equivalent-to-one-language/) because it seemed to have with it attached the promise of control with our software projects.

But reading that experience of Jimmy Bogard and his team makes me rethink about using Clean Architecture in all of my future software projects :laughing:

A big plus for this Vertical Slice Architecture is that it seems to help new programmers quickly understand how the components of a project communicate, as hinted in another [youtube comment](https://www.youtube.com/watch?v=SUiWfhAhgQw&lc=UgxKn5DIimiDXq1MWqB4AaABAg):

> **César Afonso:**
As a team leader who had to deal with many many trainees entering one projects without knowledge on architectures or even programing, and leaving after a few months, I started to use a similar architecture sometime in 2015 on my projects. 
<br /><br />
On my experience, it makes a project look uglier to expert developers, because the separation of layers is not pretty well defined...
<br /><br />
But on the other [hand], ... **it's easier to new delevopers [to] understand the project and the components... they are not so afraid of changing something**... the projects have less bugs and are easier to find and fix it. 

----------

## Steven van Deursen's enlightening articles!

Reading more about Vertical Slice Architecture also led me to these very enlightening articles of Steven van Deursen (on the same level as the article ["A Little Architecture"](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) had enlightened me):

- [Meanwhile... on the command side of my architecture](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/)

> "This article describes how a single interface can transform the design of your application to be much cleaner, and more flexible than you ever thought possible." --- Steven van Deursen


- [Meanwhile... on the query side of my architecture](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/)

> "Two simple interfaces will change the look of your architecture… forever." --- Steven van Deursen

Enjoy!
