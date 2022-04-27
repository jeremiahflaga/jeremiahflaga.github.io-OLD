---
layout: post
title: 'Some guidelines and resources on writing clean and readable code'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2021-09-01 12:00:00 AM UTC
---

<!-- Started July 28, 2021  06:24 PM Philippine Time -->


A few weeks ago on youtube, I found a very interesting [comment on coding principles by someone named veganaiZe](https://www.youtube.com/watch?v=WV2Ed1QTst8&lc=Ugwjp5QVpxe_AQK7YNF4AaABAg). I'm going to paste the entire comment here so that it will not be lost in the comments section of youtube.


<!-- 
> This is one of the most important, as well as overlooked, topics in software engineering.  Tech focuses a lot more on the "Patterns" during this talk than on "Principles". These software design patterns offer ways to accomplish certain behaviors, within a program, and also serve to benefit programmer communication.  They all stand upon and are justified by principles.
> 
-->

> ...I think the most important principles are the following:
> 
> **D.R.Y. - Don't Repeat Yourself** - Pretty much self-explanatory.  If you are re-writing the same block(s) of code in various places then you are ensuring that you'll have multiple places to change it later.  This is probably the #1 warning flag that you should look into an alternative/pattern.
> 
> **Y.A.G.N.I - You Ain't Gonna Need It** - Don't code out every little detail that you can possibly think of because, in reality, you probably won't end up needing much of it.  So just implement what is absolutely necessary (save the GUI until later, for example!)
> 
> **K.I.S.S. - Keep It Stupid Simple** - When in doubt, make it easy to understand.  Don't try to build in too much complexity.  If a particular design pattern overly complicates things (vs. an alternative or none at all) then don't implement it.  This applies heavily to user interface design and APIs (application programming interfaces).
> 
> **P.O.L.A. - Principle Of Least Astonishment** - This has overlap with KISS...  Do the least astonishing thing.  In other words: DON'T BE CLEVER.  It's nice that you can squeeze a ton of detail into a single line of code.  But your future self and other developers will judge it by the number of "F"-words per minute when they open your source file.  It should be relatively easy to jump into what your code is doing, even for an outsider who isn't quite as clever as you are.
> 
> Last, and most certainly NOT least...
> 
> **S.O.L.I.D.**  (I saved this for last because it's more specific, but it should probably land around #2 on this list)
> 
> **Single Responsibility** - A class should have only one reason to change;  Do one thing and do it well.
> 
> **Open/Closed Principle** - A class should be open for "extension" but closed for "modification".
> 
> **L. Substitution Principle** - Derived (sub) classes should fit anywhere that their base (super) class does.
> 
> **Interface Segregation** - Multiple specific interfaces are better than one general-purpose interface.
> 
> **Dependency Inversion** - Depend on abstractions NOT on concrete implementations.  (Depend on abstract classes and interfaces, which can't be instantiated, rather then any specific instance)


<!-- Instead of repeating ugly logic, why don't you just push that logic into its own function and name the function after the comment that was above said ugly logic? That should leave your code nice and declarative (ie. readable), and hey, it'll even keep the code DRY. -->

I would like to add the Uncle Bob updated the definition for the SRP to make it clearer:

> A module should be responsible for one, and only one actor

You can read about that 
[here](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)
and [here](https://www.e4developer.com/2018/10/04/single-responsibility-principle-do-you-know-the-real-one/)
and [here](https://www.brainstobytes.com/the-single-responsibility-principle/)
and [here](http://www.softwareonthebrain.com/2022/01/the-misunderstood-single-responsibility.html) <!-- The Misunderstood Single Responsibility Principle by Joseph Lynch - from Uncle Bob's tweet - https://twitter.com/unclebobmartin/status/1485224521875562498 -->


<!-- 
["The Single Responsibility Principle"](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html) by Uncle Bob Martin

["Single Responsibility Principle – do you know the real one?"](https://www.e4developer.com/2018/10/04/single-responsibility-principle-do-you-know-the-real-one/) by Bartosz Jedrzejewski

["The Single Responsibility Principle"](https://www.brainstobytes.com/the-single-responsibility-principle/) by Juan Orozco Villalobos
 -->





## Other resources on how to write clean code


["Clean Code: A Handbook of Agile Software Craftsmanship"](https://www.bookdepository.com/Clean-Code-Robert-C-Martin/9780132350884?a_aid=jflaga) book by Uncle Bob Martin

> Clean code always looks like it was written by someone who cares. 
> 
> --- Micheal Feathers



["The Most Important Design Guideline"](https://oguzpedia.blogspot.com/2016/01/scott-meyers-most-important-design.html) by Scott Meyers

> Make interfaces easy to use correctly and hard to use incorrectly.



["The Art of Clean Code"](https://youtu.be/AeWbJ5LIFNg?t=205) by Victor Rentea

> The true cost (80%) of software lies in its maintenance, not in its initial development. <small>(time 03:17)</small>



["Name by role"](https://blog.ploeh.dk/2020/11/30/name-by-role/) by Mark Seemann



<!-- Shameless plug:  -->
["The key to clean code"](/2021/08/08/the-key-to-clean-code/)



["SOLID: the next step is Functional"](https://blog.ploeh.dk/2014/03/10/solid-the-next-step-is-functional/) by Mark Seemann

["Referential transparency fits in your head"](https://blog.ploeh.dk/2021/07/28/referential-transparency-fits-in-your-head/) by Mark Seemann

<!-- 
Good names are skin-deep by Mark Seemann - https://blog.ploeh.dk/2020/11/23/good-names-are-skin-deep/
 -->

["Readability verification"](https://blog.ploeh.dk/2021/10/18/readability-verification/) by Mark Seemann

> In [Code That Fits in Your Head](https://blog.ploeh.dk/code-that-fits-in-your-head), I suggest heuristics for writing readable code, but ultimately, the only reliable test of readability that I can think of is simple:
>
> > Ask someone else to read the code.

[Robert C. Martin's book "Clean Code" adapted with JavaScript examples](https://www.reddit.com/r/node/comments/5pvzo2/robert_c_martins_book_clean_code_adapted_with/)


