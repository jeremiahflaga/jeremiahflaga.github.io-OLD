---
layout: post
title: On maintainability and the separation of the business rules
categories: [Programming]
tags: [TDD, Software Design, Michael Feathers, Eddie Bush, Robert Martin]
date: 2018-03-19 06:20:00 PM UTC
---

<!-- January 1, 2018 2:20:00 AM Philippine Time -->

_Story time..._

During college, I became interested about solving algorithmic problems. But because of lack of mentorship and my lack of knowledge in higher maths and algorithms, I decided to concentrate my energies into studying how to create [line-of-business (LOB) applications](https://blogs.msdn.microsoft.com/dragoman/2007/07/19/what-is-a-lob-application/) (or what is called _"representational-transactional systems"_ [here](https://aryehoffman.com/entry/classifying-software/)).

I thought to myself that creating LOB applications is much easier to do --- it does not require lots of knowledge in maths --- and I thought that I can easily get jobs in the future if I concentrate on creating LOB apps rather than solving algorithmic problems.


During those times, I was curious about how people structure their code in real-world projects.

<!--more-->


I found out about this 3-Layers Architecture thing: you have a **_Presentation Layer_**, **_Business Logic Layer_ (BLL)**, and **_Data Access Layer_ (DAL)**.

I understood what a _Data Access Layer_ is... because during college, _everybody_ knows about databases. During college, we were kind of [_brainwashed_ into thinking that the database is the center of our application](http://blog.cleancoder.com/uncle-bob/2012/05/15/NODB.html). Creating relational data models was the first thing we do when creating software projects during those times.

I also understood what a _Presentation Layer_ is --- it has something to do with the user interface or UI.

_But, "what is this business logic thing??"..._

_"Hmmm..."_

I did not yet understand what it was during that time.

I also learned that if the app is very simple, one can choose to have _two_ layers only: a **_Data Layer_** and an **_Application Layer_**.

This 2-Layers thing is much easier to comprehend! I know what a _Data Layer_ is... 

_"Anything that does not involve SQL must be part of the Application Layer!!"_ 

Great!

So I followed this 2-Layers model in my projects during that time.

A few years later, I came to understand that the validation of user inputs is part of the Business Logic Layer.

So I used that knowledge in one project. I placed my validations in the BLL module.

[_Then the web happened!..._](insert link here of Uncle Bob saying those --- Agility and Architecture OOP2015 at 16:00 min) I mean, I realized that I need to know how to create web applications.

I started thinking in [monoliths](insert link here) again. 

Luckily I found a job. After a few months in that job, DDD or Domain-Driven Design was introduced to us. Our employer gave us some articles to read about DDD becase we will be involved in an in-house project where concepts from DDD will be used.

That was my introduction to DDD.

I really liked the idea of using the Ubiquitous Language in our code, because you know, _"naming things"_ is one of the hardest things to do in programming. The Ubiquitous Language thing can greatly help on this problem.

I also liked this idea of tight collaboration between the programmers and the business people to produce the right product.

Also, the idea of bounded contexts... it makes things easier to manage.

And, most important of all (at least during my early years as a programmer), I already had another thing to google for when deciding on how to structure my projects ---- [_"How do I structure a DDD prroject?"_](insert here link to google searching those words)

Then... came... Uncle Bob Martin... with his [charismatic presentations of Clean Architecture!](2017-04-15-agility-and-architecture-by-uncle-bob-martin-oop-2015-keynote)

He suggests _(preaches)_ that we keep our business rules separate from the UI, from the database, from the web! And he gave a [sample diagram on how to do it.](insert link here) You can know more about this diagram in the links I will give below.

Whew!

_Okay... End of story._

That was just a very long introduction to the following list of materials and quotes from our masters, which emphasizes the importance of separating the business rules from the other parts of a software system:




## Great materials about separating the business rules



### Uncle Bob Martin's talks and articles on Clean Architecture**

- ["Agility and Architecture" (OOP 2015 Keynote)](insert link here) talk

- ["Architecture: The Lost Years"](https://www.youtube.com/watch?v=Nsjsiz2A9mg) talk

- etc. (google for it)

- ["Clean Architecture"](http://blog.cleancoder.com/uncle-bob/2011/11/22/Clean-Architecture.html) article

- ["The Clean Architecture"](http://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) article

In these talks and articles, Uncle Bob emphasizes that the business rules part is the **most important part** of a software system, and it must be separated from the other parts.



### ["Policy, Mechanism, And The Preservation Of Business Value"](http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/) of Eddie Bush

> "**The business value in our applications is represented by the policy [or business rules].** Policy may change over time, and new policies may be introduced, but --- if separation of policy and mechanism is followed faithfully --- its value can be longer lasting than any user interface or persistence mechanism ever thought about."

> "Let’s face it, developers want to work on contemporary technologies. **If you’re not separating policy and mechanism, it’s inevitable that you will find yourself in a less favorable position when it comes to recruiting top talent.**"

> "**Don’t Ignore Simple Applications**
<br /><br />
> "Simple applications are the first place you should begin practicing the separation of policy and mechanism because they’re simple. Once they have that formality applied, they can be great testbeds for new UI and persistence frameworks."


### ["This Is How We Do It"](https://terencemcghee.com/Articles/Tech/2015/10/25/A0B2606228759D1A888E0AFFDB9DADE0.html) of Terence McGhee

> "One of the most important things that you can learn from me or from anyone else regarding professional software creation, is that you must build your systems for **easy maintainability**."

And separating the business rules from the other parts of a software system is very important to achieve maintainability. 



### ["Codesmith's Delight"](https://terencemcghee.com/Articles/Tech/2016/10/15/551B3828CD47198C7C5A58903228DA71.html) of Terence McGhee

> "The primary value of software is that it not only meets today’s needs for today’s users, but that it can **continue** to meet the future needs of all of its users."



### ["Reasons For Isolation"](https://blogs.msdn.microsoft.com/ploeh/2007/05/30/reasons-for-isolation/) of Mark Seeman

He listed here three reasons why we should separate things that need to be separated in our software systems.



### ["Is Layering Worth the Mapping?"](http://blog.ploeh.dk/2012/02/09/IsLayeringWorththeMapping/) of Mark Seeman

Mark Seemann gives here an example of how to separate things in a software system.

He also said that separation is an "all-or-nothing proposition"

> "... the point of the post is that as soon as you soften separation of concerns just a bit, it becomes impossible to maintain that separation. It's an all-or-nothing proposition. Either you maintain strict separation, or you'd be building a monolithic application whether you know it or not."

And remember what Eddie Bush said above: **_"Don’t Ignore Simple Applications... Simple applications are the first place you should begin practicing..."_**

Happy coding!!!
