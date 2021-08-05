---
layout: post
title: 'How to write clean code'
subtitle: 
categories: [Miscellaneous]
tags: [Miscellaneous]
date: 2021-12-01 12:00:00 AM UTC
published: false
---

<!-- Started July 28, 2021  06:24 PM Philippine Time -->


A few months ago on youtube, I found a very interesting [comment by someone named veganaiZe](https://www.youtube.com/watch?v=WV2Ed1QTst8&lc=Ugwjp5QVpxe_AQK7YNF4AaABAg). I'm going to paste the entire comment here so that it will not be lost in the comments section of youtube.


> This is one of the most important, as well as overlooked, topics in software engineering.  Tech focuses a lot more on the "Patterns" during this talk than on "Principles".  These software design patterns offer ways to accomplish certain behaviors, within a program, and also serve to benefit programmer communication.  They all stand upon and are justified by principles.
> 
> I think the most important principles are the following:
> 
> D.R.Y. - Don't Repeat Yourself - Pretty much self-explanatory.  If you are re-writing the same block(s) of code in various places then you are ensuring that you'll have multiple places to change it later.  This is probably the #1 warning flag that you should look into an alternative/pattern.
> 
> Y.A.G.N.I - You Ain't Gonna Need It - Don't code out every little detail that you can possibly think of because, in reality, you probably won't end up needing much of it.  So just implement what is absolutely necessary (save the GUI until later, for example!)
> 
> K.I.S.S. - Keep It Stupid Simple - When in doubt, make it easy to understand.  Don't try to build in too much complexity.  If a particular design pattern overly complicates things (vs. an alternative or none at all) then don't implement it.  This applies heavily to user interface design and APIs (application programming interfaces).
> 
> P.O.L.A. - Principle Of Least Astonishment - This has overlap with KISS...  Do the least astonishing thing.  In other words: DON'T BE CLEVER.  It's nice that you can squeeze a ton of detail into a single line of code.  But your future self and other developers will judge it by the number of "F"-words per minute when they open your source file.  It should be relatively easy to jump into what your code is doing, even for an outsider who isn't quite as clever as you are.
> 
> Last, and most certainly NOT least...
> 
> S.O.L.I.D.  (I saved this for last because it's more specific, but it should probably land around #2 on this list)
> 
> Single Responsibility - A class should have only one reason to change;  Do one thing and do it well.
> Open/Closed Principle - A class should be open for "extension" but closed for "modification".
> L. Substitution Principle - Derived (sub) classes should fit anywhere that their base (super) class does.
> Interface Segregation - Multiple specific interfaces are better than one general-purpose interface.
> Dependency Inversion - Depend on abstractions NOT on concrete implementations.  (Depend on abstract classes and interfaces, which can't be instantiated, rather then any specific instance)


I would like to add the Uncle Bob updated the definition for the SRP so that it will be clearer. Read about that here and here.

<!-- 
["The Single Responsibility Principle"](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html) by Uncle Bob Martin

["Single Responsibility Principle – do you know the real one?"](https://www.e4developer.com/2018/10/04/single-responsibility-principle-do-you-know-the-real-one/) by Bartosz Jedrzejewski
 -->







The Clean Code book by Uncle Bob Martin


Clean Code talk by Victor Rentea





Name by role by Mark Seemann


