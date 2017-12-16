---
layout: post
title: TDD might help in teaching about design without a teacher
categories: [Programming]
tags: [TDD, Software Design]
date: 2017-12-14 05:00:00 PM UTC
published: false
---

<!-- December 15, 2017 2:00:00 AM Philippine Time -->


In his talk, "The Deep Synergy Between Testability and Good Design (NDC 2010)", Michael Feathers said these:

> "If your code is not testable, then it is not a good design."
<br /><br />
> "In general, every time you encounter a testability problem, there is an underlying design problem."
<br /><br />
> "If you're finding trouble testing things, reconsider your design and end up with something better."


Perhaps TDD can be used in teaching design when no teacher is available to teach design??

<!--more-->

You can just tell the student, _"If it is hard to test, the design must be wrong. Try to find another solution."_

Perhaps the reason why the rule _"Hard to test means bad design"_ works is because tests are a measure of the quality of the design?? --- "If it endured the many tests, it might be a good design."

Christianity has this idea of "people who endure testings becomes stronger" --- "Gold tried with fire becomes purer."

What if this _"testing or trial rule"_ thing is universal? No? _(Perhaps I'm just using the same words which have different meanings in here?)_


And... Michael feathers also said that being testable does _not_ imply good design, but well designed systems are (always?) testable.

But... maybe the reason why a poorly designed system might still be testable is bacause it has not yet gone through many testings?? --- Maybe the tests only cover a small percentage of the production code.

Or, perhaps, the tests are wrong?

Maybe we also should have tests for our tests, so we can be sure that our tests are right!?

What should be the tests for our tests? How will we know that our tests are the right ones?

Maybe we can use the production code itself to determine if the tests are right. If our tests produce good design then our tests must be right!

What? Arguing in circles?

Ahh! Perhaps what Uncle Bob said on "being stuck" helps here... _"When you get stuck, do the simplest thing you can do"_ or something like that.

Perhaps we can say the the measure of the quality of the tests is the simplicity of the tests??



<!-- 
When we get stuck our tests must be wrong. We have to backtrack. Or start again, and write the simplest tests first.

So if you are not getting stuck at testing, you are going in the right direction.

Urghhh! I'm confused. I need to get back to this later. 
 -->



<!--
-->