---
layout: post
title: On maintainability and the separation of the business rules
categories: [Programming]
tags: [TDD, Software Design, Michael Feathers, Eddie Bush, Robert Martin]
date: 2018-01-01 06:20:00 PM UTC
published: false
---

<!-- January 1, 2018 2:20:00 AM Philippine Time -->

During college, I became interested about solving algorithmic problems. But because of lack of mentorship and my lack of knowledge in higher maths, I decided to concentrate my energies into studying how to create [line-of-business (LOB) applications](https://blogs.msdn.microsoft.com/dragoman/2007/07/19/what-is-a-lob-application/) (or what is called _"representational-transactional systems"_ [here](https://aryehoffman.com/entry/classifying-software/)).

I thought to myself that creating LOB applications is much easier to do --- it does not require lots of knowledge in maths --- and I thought that I can easily get jobs in the future if I concentrate on creating LOB apps rather than solving algorithmic problems.


During those times, I was curios about how people structure their code in real-world projects.

I found out about this 3-Layers Architecture thing: you have a _Presentation Layer_, _Business Logic Layer_ (BLL), and _Data Access Layer_ (DAL).

I understood what a _Data Access Layer_ is because during college I think _everybody_ knows about databases. I think that during college we were kind of [_brainwashed_ into thinking that the database is the center of our application](http://blog.cleancoder.com/uncle-bob/2012/05/15/NODB.html). Creating relational data models was the first thing I do when creating software projects during those times.

I also understood what a _Presentation Layer_ is --- it has something to do with the user interface or UI.

_But what is this business logic thing??..._

I also learned that if the app is very simple, one can choose to have two layers only: a _Data Layer_ and an _Application Layer_.

This 2-Layers thing is much easier to comprehend! I know what a _Data Layer_ is... _anything that does not involve SQL must be part of the Application Layer!_ Great!

So I followed this 2-Layers model in my projects during that time.

A few years later, I came to understand that the validation of user inputs is part of the Business Logic Layer.

So I used that knowledge in a project.

_Then the web happened!..._ I mean, I realized that I need to know how to create web applications.

I started thinking in [monoliths](insert link here) again. 

Luckily I found a job and [DDD] was introduced to us in that job. Our employer gave us some articles to read about DDD becase we will be involved in an in-house project where concepts about DDD will be used




This Is How We Do It of Terence McGhee

https://terencemcghee.com/Articles/Tech/2015/10/25/A0B2606228759D1A888E0AFFDB9DADE0.html

> One of the most important things that you can learn from me or from anyone else regarding professional software creation, is that you must build your systems for **easy maintainability**.


Codesmith's Delight

https://terencemcghee.com/Articles/Tech/2016/10/15/551B3828CD47198C7C5A58903228DA71.html



Policy, Mechanism, And The Preservation Of Business Value of Eddie Bush

http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/





Mark Seeman's article on Isolation





<!--more-->





(I actually have a secret agenda for why I am promoting the separation of the business rules from the other parts of the software system --- when I will become a part of a team that do not practice TDD I will still do TDD 
