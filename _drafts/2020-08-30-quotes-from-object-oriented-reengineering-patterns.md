---
layout: post
title: 'Quotes from Object-Oriented Reengineering Patterns'
subtitle: ''
categories: [Programming]
tags: [Programming, Serge Demeyer, Stéphane Ducasse, Oscar Nierstrasz]
date: 2020-07-31 12:00:00 AM UTC
published: false
---

<!-- first edits: June July 15, 2020 08:15:00 AM Philippine Time -->

["Object-Oriented Reengineering Patterns"](http://scg.unibe.ch/download/oorp/)

I found this free book last 2017, but managed to read up to the first chapter only. :frowning:

<!--more-->


> For a long time it’s puzzled me that most books on software development processes talk about what to do when you are starting from a blank sheet of editor screen. It’s puzzled me because that’s not the most common situation that people write code in. Most people have to make changes to an existing code base, even if it’s their own. In an ideal world this code base is well designed and well factored, but we all know how often the ideal world appears in our career.
>
> --- from the Foreword by Martin Fowler

Read that!? Priceless...

Tsk tsk! Too bad I forgot to continue reading this book the first time I found it...

> If you have a lot of experience with software reengineering then you probably won’t learn much from this book. **You should read it anyway**, because you’ll want to give copies to people you work with, and you will want to use the vocabulary of the book when you talk with them. If you are new to reengineering, you should read the book, learn the patterns, and try them. You will learn a lot that will be valuable. Don’t expect to understand the patterns completely before you try them, because patterns are practical, and **practical knowledge has to be experienced to be fully understood.  Nevertheless, the book will give you a big advantage. It is much easier to learn when you have a path to follow**, and this book provides a reliable guide.
>
> --- from the Foreword by Ralph E. Johnson

Here are some interesting quotes from the book to awake your excitement:

-----

## Preface

> ... we are kidding ourselves if we think that we can know all the requirements and build the perfect system. The best we can hope for is to build a useful system that will survive long enough for it to be asked to do something new.

> ... the most interesting and challenging side of software engineering may not be building brand new software systems, but rejuvenating existing ones.

> Patterns as a form of documentation are most useful and interesting when the problem being considered entails a number of conflicting _forces_, and the solution described entails a number of _tradeoffs_.
> ... Understanding these tradeoffs is essential to successfully applying the patterns.

> ... If you take an extreme viewpoint, you could say that every software project is a reengineering project.

## Chapter 1

> A legacy is something _valuable_ that you have _inherited_. Similarly, legacy software is valuable software that you have inherited.

> Experience shows that the right sequence is “first do it, then do it right, then do it fast”, so you
might want to reengineer to clean up the code before thinking about performance.

<small>This is similar o Kent Beck's "first make it work, then make it right, then make it fast"</small>

> It is not age that turns a piece of software into a legacy system, but the rate at which it have
been developed and adapted without having been reengineered. The wrong conclusion to draw from these experiences is that

> One of the conclusions you should draw from this book is that, well, objects are pretty good, but you must take good care of them.

> ... there is good evidence to support the notion that a culture of continuous reengineering is necessary to obtain healthy, maintainable software systems.

> ... design patterns have to do with choosing a particular solution to a design problem, whereas reengineering patterns have to do with _discovering an existing design_, determining what _problems_ it has, and _repairing_ these problems.

## Chapter 2

> “You cannot do it right the first time.”

<small>... Interesting!</small>

> A key point to remember is that any maxim may only have a limited lifetime. It is important to periodically reevaluate the validity of any maxims that have been adopted. A project can get completely off track if you agree on the wrong maxims, or the right ones but at the wrong time.

> Appoint a specific person whose responsibility in role of navigator is to ensure that the architectural vision is maintained.

<small>This is similar to what Frederick P. Brooks in [The Mythical Man-Month](https://www.bookdepository.com/Mythical-Man-Month-Frederick-P-Brooks-Jr/9780201835953?a_aid=jflaga): "... I will certainly not contend that only the architects will have good architectural ideas. Often the fresh concept does come from an implementer or from a user. However, all my own experience convinces me, and I have tried to show, that the conceptual integrity of a system determines its ease of use. Good features and ideas that do not integrate with a system's basic concepts are best left out. If there appear many such important but incompatible ideas, one scraps the whole system and starts again on an integrated system with different basic concepts."</small>

> Knowledge and understanding of a legacy system is always distributed and usually hidden... The information that is extracted from a legacy system is a valuable asset that must be shared for it to be exploited.

> Which problems should you focus on first? ... Start working on the aspects which are most valuable to your customer.

> If you fail to deliver good initial results, you will learn a lot, but you risk losing credibility. It is therefore critical to choose carefully initial tasks which not only demonstrate value for the customer, but also have a high chance of success.

> 80% of software defects typically occur in 5% of the code

> Software artifacts that are stable and do not threaten the future of the legacy system are not “broken” and do not need to be reengineered, no matter what state the code is in.


## Chapter 3

> Software maintenance is generally considered a second-class job, often left to junior programmers and often leading to a maintenance team which changes frequently.

<small>Oh. Does this mean that system maintenance is a niche market?... I should aspire to become very good at this then. :blush:</small>


> “The major problems of our work are not so much technological as sociological in nature.” — Tom De Marco

<small>This is similar to what Eric Ries said in ["The Lean Startup"](https://www.bookdepository.com/Lean-Startup-Eric-Ries/9780670921607?a_aid=jflaga), in the section about the 'five whys': "At the root of every seemingly technical problem is a human problem." Interesting... </small>

> “Organizations which design systems are constrained to produce designs which are copies of the communications structure of these organizations.” — Melvin Conway
>
> Conway’s law is often paraphrased as: “If you have 4 groups working on a compiler; you’ll get a 4-pass compiler”
>
> One particular reason why it is important to know about the way the development team was organized, is because it is likely that this structure will somehow reflect the structure of the source code.

> “Maintenance fact #1. In the late ‘60s and throughout the 70’s, production system support and maintenance were clearly treated as second-class work.
> Maintenance fact #2. In 1998, support and maintenance of production systems continues to be treated as second-class work.” — Rob Thomsett


> We learned the hard way that maintainers are very proud about their job and very sensitive to critique. Therefore, we emphasize that such a kick-off meeting must be “maintainer oriented”, i.e. aimed to let the maintainers show what they do well and what they want to do better. Coming in with the attitude that you — the newcomer — will teach these stupid maintainers how to do a proper job will almost certainly lead to disasters.

<small>Very very interesting... I hope I will not forget this.</small>






<small></small>


**That's all I will put here folks. I hope those quotes made you interested to read the book. [Dowload the book](http://scg.unibe.ch/download/oorp/) to learn more...**

