---
layout: post
title: 'On the tension between DRY (Don''t Repeat Yourself) and SRP (Single Responsibility Principle)'
subtitle: Code duplication is not always evil
categories: [Programming]
tags: [Programming]
date: 2021-10-01 12:00:00 AM UTC
pinned: true
---

<!-- Started August 13, 2021  11:13 PM Philippine Time -->

<!-- 
> ...debate with someone in the future by posting your refutation in the past
>
> Uncle Bob Martin, [Types and Tests](https://blog.cleancoder.com/uncle-bob/2019/06/08/TestsAndTypes.html)

-----
 -->

The [DRY principle](/2021/09/01/guidelines-and-resources-on-how-to-write-clean-code) or guideline is one of the easiest of the coding guidelines to understand and remember. Maybe that is the reason why we easily conclude that there is duplication when we see code which look the same. And it seems to me that we have developed this instinct of wanting to remove duplication when we see them (or maybe it's just me :smile: ).

But there are also times when we are hesitant to de-duplicate code which look the same, because it seems unnatural to do so in those cases --- we want to keep the duplication, because we can somewhat foresee that if we de-duplicate this particular code it might make our codebase messier and more difficult to maintain later.

In those cases, we are being pulled between following the DRY (Don't Repeat Yourself) principle and following the SRP (Single Responsibility Principle).

<!-- 
<div class="message-compressed float-right small" markdown="1">

We can see an example of that [here](https://www.brainstobytes.com/the-single-responsibility-principle/). Take note of the method `get_tabulated_inventory_info()`

</div>
 -->

How do we decide which of the two principles to break when we are in this kind of dilemma?

First, let's go back to the definitions of DRY and SRP.

[DRY](/2021/09/01/guidelines-and-resources-on-how-to-write-clean-code) is pretty strightforward. It means to remove duplicates.

SRP is more complicated, so let's read again its [definition from Uncle Bob Martin](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)

> The Single Responsibility Principle (SRP) states that **each software module should have one and only one reason to change**... However it begs the question: What defines a reason to change?
> 
> ... the Single Responsibility Principle... _is about people._
> 
> Another wording for the Single Responsibility Principle is:
> > **Gather together the things that change for the same reasons. Separate those things that change for different reasons.**
> 
> ... as you think about this principle, remember that **the reasons for change are _people_**. It is people who request changes. And you don’t want to confuse those people, or yourself, by mixing together the code that many different people care about for different reasons.

In his book "Clean Architecture", he made the [definition of SPR much clearer](https://www.e4developer.com/2018/10/04/single-responsibility-principle-do-you-know-the-real-one/):

> **A module should be responsible to one, and only one, actor.**


That seems to indicate that code duplication --- breaking the DRY principle --- is okay when we know that the code in question is being written for different actors. 

But what if you do not know whether or not the code in question is being written for different actors? Which one to break then?

<!-- I would say that it's preferable to break DRY than to break SRP. I would rather have code duplication than have code which does a lot of different things and with lot's of if/else in them :grinning:. -->

It's up to you to decide. But I would prefer to break DRY than to break SRP in that case.
Remember [what Sandi Metz said in "All the Little Things"](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction): 

> duplication is far cheaper than the wrong abstraction

<!-- "All the Little Things" - https://www.youtube.com/watch?v=8bZh5LMaSmE -->

<!-- Code duplication is not always evil. -->


-----

One last thing: Scott Millett in [PPPDDD](https://www.bookdepository.com/Patterns-Principles-Practices-Domain-Driven-Design-Scott-Millett/9781118714706?a_aid=jflaga) said something about duplication and [bounded contexts](/2021/07/18/microservices-is-not-the-same-as-bounded-context) in page 157 of the book:

> Even if the code is the same and it never changes, usually the duplication causes no problem. Duplication is a problem because you may update code in one place, and forget to update it in another. However, this is rarely a problem when you have loosely coupled bounded contexts that are intended to run in isolation. There are very few reasons that the same concept in two bounded contexts should be changed at the same time. So don’t worry about duplicating similar code, and instead focus on isolating your bounded context and maintaining their boundaries.

Given that, I would say that one can argue that some code that look the same are not duplicates in the first place. They just look the same. And so we are not breaking the DRY principle when we refuse to de-duplicate them in those particular cases.

Happy coding!!!
