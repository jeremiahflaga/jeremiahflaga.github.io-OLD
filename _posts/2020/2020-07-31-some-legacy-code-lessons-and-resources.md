---
layout: post
title: 'Some legacy code lessons from PPPDDD of Scott Millett'
subtitle: '"Don’t strive for perfect code; strive for perfect boundaries."'
categories: [Programming]
tags: [Programming, DDD, PPPDDD, Scott Millett, Robert C. Martin, Jonathan Boccara, Eric Evans]
date: 2020-07-31 10:25:00 AM UTC
dateLastUpdated: 2021-06-05 10:00:00 AM UTC
pinned: true
---

<!-- July 29, 2020 Philippine Time -->
<!-- finished July 31, 2020 06:24 PM Philippine Time -->
<!-- Updated June 5, 2021 6:00 PM Philippine Time -->

When I first heard (and read a little) of DDD, I thought that it only contains lessons for dealing with greenfield projects. But while reading ["Patterns, Principles, and Practices of Domain-Driven Design"](https://www.bookdepository.com/Patterns-Principles-Practices-Domain-Driven-Design-Scott-Millett/9781118714706?a_aid=jflaga) (PPPDDD) of Scott Millett I learned that DDD also contains some gems on how to properly deal with legacy codebases. 

PPPDDD gives a very good reminder to NOT try to clean up everything. :grin:


> If you are working in a legacy codebase or are integrating with a legacy code, it is vital to ensure that your code is not contaminated by the mess that already exists. (If there is mess; remember that **legacy doesn’t mean bad code!**) 
> 
> **It may be tempting to clean up the legacy codebase, but this is a task that can quickly become time consuming** and distract from your real goal of introducing new functionality. Instead, **lean on the anticorruption layer pattern to create a boundary between your new code and the existing code**. This protection boundary enables you to create a clean model that is isolated from other teams’ influences. (p. 140)


<!--more-->

> ... you can use an anticorruption layer to wrap communication with legacy or
third‐party code to protect the integrity of a bounded context. An anticorruption layer manages the transformation of one context’s model to another.  (p. 95)

> **Not all of a system will be well designed.** Focus effort on the core domain. For legacy BBoM systems define an anti‐corruption boundary to avoid new code becoming tangled within the mess of old. (p. 40)


> **Don’t strive for perfect code; strive for perfect boundaries.** (p. 140)

> If you are working in a legacy environment, ensure that you protect yourself from external code, don’t trust anyone, and enforce your borders. Carve out an area to add new functionality. **Don’t try to clean up everything.** (p. 147)

_"Don’t try to clean up everything."_ That sounds like what Uncle Bob Martin said in his talk, ["Expecting Professionalism"](/2017/05/13/expecting-professionalism-by-uncle-bob-martin/) (I have forgotten this one already; good that I found it again.):

<blockquote markdown="1">

... how do you deal with a legacy environment?

- **Don't make a project out of making your legacy environment testable (or something like that) because that project will fail.** That's what will happen if you try to make a project to correct a long term lack of discipline.

- **The solution is to change attitude: the boy scout rule.**
	- From now on whenever I or anyone in my team touches this code, we are going to make it a little bit better.
	- If you can make tests do it. If you can't, just do a small change to make it approach testability -- break some coupling, change some function arguments.

{:.mt-2}
- Secondly, sometimes you will be asked to add some new features and you'll realize very quickly that there are two ways to add a new feature.
	- One way is to smear a bunch of code in the existing legacy code.
	- The other way is to write the new feature in a completely separate module and then couple that module in from the outside of the legacy code.
	- That second way is the harder of the two and should be the one that you do (because you can write tests).

</blockquote>


In the Introduction of [PPPDDD](https://www.bookdepository.com/Patterns-Principles-Practices-Domain-Driven-Design-Scott-Millett/9781118714706?a_aid=jflaga), Scott Millet has this to say about the relationship of DDD and legacy code:

> "**Following the DDD philosophy** will give developers the knowledge and skills they need to tackle large or complex business systems effectively. Future enhancement requests won’t be met with an air of dread, and developers will no longer have stigma attached to the legacy application. In fact, **the term legacy will be recategorized in a developer’s mind as meaning this: a system that continues to give value for the business**."

Hmmmm... I hated working on legacy codebases when I was starting with my programming career because, you know, I was never told that code running in production can be so bad. All code I had been seeing in books and in programming tutorials was relatively good code. (The only _bad_ code I had seen by that time was my own code. :laughing:)

I remember the first time I worked on a legacy code base; I thought to myself, "This might just be some kind of a test; they are just testing us to see if we can work on code like this; after the test they will allow us to work on the real project." Later on I learned that the one we were working on was already the real project. :laughing: (I would just like to add that one of the major reasons why a code base might look like spaghetti code to a programmer is [unfamiliarity with the domain](https://terencemcghee.com/Articles/Tech/2016/06/26/262AFE44011BA22125FC0376AF532D01.html#easytounderstand) for which the code was written, just like the case with my first legacy code base --- I was not familiar with the domain.)

<!-- "Tradecraft 101" - Terence McGhee -  https://terencemcghee.com/Articles/Tech/2016/06/26/262AFE44011BA22125FC0376AF532D01.html#easytounderstand -->

> Code that's easy to understand is largely determined first by the author's adherence to good naming and grouping practices and secondly by
the reader's familiarity with the domain of the problem space that the code is addressing. --- Terence McGhee

Another thing I remember is that working on a codebase which uses old technologies made me think that I was left out from learning all the [new shiny technologies popping up from everywhere every millisecond of the day](/2018/05/21/software-development-has-not-changed-in-40-years/)... because, you know, young programmers often want to work on new technologies.

But I now understand that legacy code does not necessarily mean bad code, even though it might still be hard to work on it.

> "... What’s the right attitude, then? I was lucky to learn it very early in my career from my fantastic manager, Patrice Dalesme. A few weeks after I came in, Patrice gave us the following piece of advice: **consider that the code you’re working on is your code. Even if you haven’t written it yourself, and regardless of how good or bad you think it is, this is your code, and you have responsibility over it.**"
> 
> --- Jonathan Boccara (["The Right Attitude to Deal with Legacy Code"](https://simpleprogrammer.com/2017/03/01/deal-with-legacy-code/))

> While developers often lament working on legacy systems, **Eric Evans places high value on legacy systems, as they are often the money makers for companies**. His encouragement for developers to "hope that someday, your system is going to be a legacy system" was met with applause.
> 
> --- from ["Domain-Driven Design Even More Relevant Now"](https://www.infoq.com/news/2017/09/evans-ddd-relevant)

I hope learning more about DDD will help me more in dealing with legacy codebases.


<div class="message" markdown="1">

**Other resources for dealing with legacy codebases:**

- ["Working Effectively with Legacy Code"](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga) by Michael Feathers
- ["Refactoring: Improving the Design of Existing Code"](https://www.bookdepository.com/Refactoring-Martin-Fowler/9780201485677?a_aid=jflaga) by Martin Fowler ([2nd edition here](https://www.bookdepository.com/Refactoring-Martin-Fowler/9780134757599?a_aid=jflaga))
- ["Object-Oriented Reengineering Patterns"](http://scg.unibe.ch/download/oorp/) by Serge Demeyer, Stéphane Ducasse, & Oscar Nierstrasz

</div>


## More quotes about legacy code

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">&quot;Legacy&quot; code is underrated. The very fact that it lived long enough to become legacy code is evidence of its value. <br><br>Keeping this in mind helps me feel grateful in the face of frustration refactoring old code. Without this code would the company have survived to employ me? ❤️</p>&mdash; Merrick Christensen (@iammerrick) <a href="https://twitter.com/iammerrick/status/1299141939166457856?ref_src=twsrc%5Etfw">August 28, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>