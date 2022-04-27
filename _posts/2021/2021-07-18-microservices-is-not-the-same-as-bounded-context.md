---
layout: post
title: 'So, microservices are not the same as bounded contexts'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2021-07-18 12:00:00 AM UTC # happy 3rd month with mluv
dateLastUpdated: 2021-08-14 10:19:00 PM UTC
---

<!-- July 15, 2021  5:00 AM Philippine Time -->
<!-- Finished July 16, 2021  6:18 AM Philippine Time -->
<!-- Updated July 22, 2021  6:45 AM Philippine Time -->
<!-- Updated August 15, 2021  6:19 AM Philippine Time - "I would recommend starting with just one microservice per Bounded Context." -->



I heard someone in the past who said that the Building Microservices book is the "DDD: The good parts" version of the Domain Driven Design book of Eric Evans (please note that none of these book have I read yet :smile:). 
<!-- --- I tend to remember statements like that because it it a comparison between two famous books, and to be economical ... and I prefer reading a smaller book than a large one, and I prefer buying a cheaper one, and a more recently published one. ---  -->
That statement, later on, I interpreted it to mean that _microservices are the same as bounded contexts_.

But a few days ago, as I was watching an interview of Uncle Bob Martin --- ["A Path to Better Programming • Robert "Uncle Bob" Martin & Allen Holub • GOTO 2021"](https://www.youtube.com/watch?v=QnmRpHFoYLk&t=747s&ab_channel=GOTOConferences) --- the interviewer, Allen Holub, made a statement which implies that microservices are not the same as bounded contexts! <small>(in time 5:00 in the video)</small>


_--- Of all the good things in that video, this is the one you remember most? ---_


Uhuh!


I don't really know that much about microservices or bounded contexts. But because microservices seems hyped these days, those words coming out from master programmers, who have lots of skills and experience in software development, are very valuable --- needs to be put in storage for future reference.



## Related materials

**["Bounded Contexts are NOT Microservices"](https://vladikk.com/2018/01/21/bounded-contexts-vs-microservices/)** by Vladik Khononov

**["Tackling Complexity in Microservices"](https://vladikk.com/2018/02/28/microservices/)**  by Vladik Khononov

**["Bounded Contexts, Microservices, and Everything In Between - Vladik Khononov - KanDDDinsky 2018"](https://www.youtube.com/watch?v=dlnu5pSsg7k&t=211s&ab_channel=KanDDDinsky)**

> Heuristic #1: Do not implement conflicting models in the same service. **Always decompose to Bounded Contexts.**



**["The Single Responsibility Principle"](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)** by Uncle Bob Martin

> ... **each software module should have one and only one reason to change**... **the reasons for change are _people_**

**["Single Responsibility Principle – do you know the real one?"](https://www.e4developer.com/2018/10/04/single-responsibility-principle-do-you-know-the-real-one/)** by Bartosz Jedrzejewski

> **A module should be responsible to one, and only one, actor.**

**["The Single Responsibility Principle"](https://www.brainstobytes.com/the-single-responsibility-principle/)** by Juan Orozco Villalobos

**[The Misunderstood Single Responsibility Principle](http://www.softwareonthebrain.com/2022/01/the-misunderstood-single-responsibility.html)** by Joseph Lynch

<!-- from Uncle Bob's tweet - https://twitter.com/unclebobmartin/status/1485224521875562498 -->


**["Designing Microservice Architectures for People"](https://cloudnative.ly/designing-microservice-architectures-for-people-fcb4fb92397)**

> **I would recommend starting with just one microservice per Bounded Context.** Others can be extracted out only when there is a good reason to do so... This way, you defer all the additional complications that microservices bring until you know you need to face them.



**["Domain-driven design needn't be hard. Here's how to start"](https://www.thoughtworks.com/insights/blog/domain-driven-design-neednt-be-hard-heres-how-start)** by Andrew Hamel-Law

