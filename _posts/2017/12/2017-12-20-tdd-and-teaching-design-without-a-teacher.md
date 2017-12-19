---
layout: post
title: TDD might help in teaching about design, even without a teacher
categories: [Programming]
tags: [TDD, Software Design, Michael Feathers, Martin Fowler]
date: 2017-12-18 06:00:00 PM UTC
published: false
---

<!-- December 19, 2017 2:40:00 AM Philippine Time -->


In his talk, "The Deep Synergy Between Testability and Good Design (NDC 2010)", [Michael Feathers](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055) said these:

> "If your code is not testable, then it is not a good design."
<br /><br />
> "In general, every time you encounter a testability problem, there is an underlying design problem."
<br /><br />
> "If you're finding trouble testing things, reconsider your design and end up with something better."


Perhaps TDD can be used in teaching design when no teacher is available to teach design??

<!--more-->

We can just tell the student, _"If it is hard to test, the design must be wrong. Try to find another solution."_

Perhaps the reason why the rule _"Hard to test means bad design"_ works is because tests are a measure of the quality of the design?? --- "If it endured the many tests, it might be a good design."

Christianity (and perhaps other belief systems) has this idea of _"people who endure testings becomes stronger"... "Gold tried with fire becomes purer."_

What if this _"testing or trial rule"_ thing is universal? No? _(Perhaps I'm just using the same words which have different meanings in here!)_

But maybe tests are _not_ a measure of the quality of the design at all... because Michael Feathers also said that being testable _does not imply_ good design, but well designed systems are (always?) testable. There is a direction to this synergy of testability and good design, he said.

So it's still possible to create poorly designed systems which are still testable!?

I don't know...

But... maybe the reason why a poorly designed system might still be testable is bacause it has _not yet_ gone through _many_ testings?? Maybe the process is not yet complete? Maybe the tests only cover a small percentage of the production code? 

Or maybe the tests are wrong?


Maybe we also should have tests for our tests, so we can be sure that our tests are right!

What should be the tests for our tests? How will we know that our tests are right?

Maybe we can use the production code itself to determine if the tests are right. If our tests produce good design then our tests must be right!

<!-- _What? Arguing in circles?!!!_ -->

_What? With that, you are already assuming that you know what good design is!!!_

Of course! Of course!

We always assume that we know what good design is before we can produce good design. It doesn't matter whether we are writing tests or not.

<!-- We first must know what good design looks like before we can know that tests produce good design. -->

<!-- 
Okay... That was just a joke :smile:...

I think we are now confusing things here. 
-->

We should remember that the tests are just tools. They are just a means to an end. They are tools to help us when designing software (just as software are just tools in helping make the lives of people better _[I hope]_, or easier _[I hope, still]_, at least).

> "... [TDD] practitioners discovered that writing tests first made a significant improvement to the design process." --- Martin Fowler


We are not going to write tests just for the sake of writing tests; just so we have something to brag about --- _"See! I'm writing tests!!!"_. If the tests do not serve any useful purpose, we should not be writing them at all.


Okay... back to the _"teaching design"_ thing...


> "If you're finding trouble testing things, reconsider your design and end up with something better." --- Michael Feathers


And remember that TDD cannot produce design. It can only _help_ produce design.




<!-- 

Ahh! Perhaps what Uncle Bob said on "being stuck" helps here... _"When you get stuck, do the simplest thing you can do"_ or something like that.

Perhaps we can say the the measure of the quality of the tests is the _simplicity_ of the tests??

Urghhh! I'm confused. I need to get back to this later.





When we get stuck our tests must be wrong. We have to backtrack. Or start again, and write the simplest tests first.

So if you are not getting stuck at testing, you are going in the right direction.

 -->



<!--
-->