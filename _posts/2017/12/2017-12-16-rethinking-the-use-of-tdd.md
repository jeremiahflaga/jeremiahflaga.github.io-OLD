---
layout: post
title: Rethinking the use of TDD
categories: [Thoughts]
tags: [Greg Howlett, Music]
date: 2017-12-13 03:30:00 PM UTC
published: false
---

<!-- December 10, 2017 11:30:00 PM Philippine Time -->

This is what Jimmy Bogard said in the conclusion of his talk, "Crafting Wicked Domain Models" (NDC 2012):

> DDD is all about building domain models that encapsulate the behavior.
<br /><br />
> So I'm not hiding behavior. I'm just saying it's up to the domain model itself to perform all these operations itself and its gonna take care of its own. It defines its boundaries --- it does not let anyone do whatever it wants. It wraps up everyting nicely in a nice neat bowl.
<br /><br />
> **This is how I was able to eliminate bugs in our application a lot more easily than just writing a whole bunch of tests. Writing a bunch of tests still requires me to know how to write those tests, but in this case the domain model is offering me the right path.**
	
That last sentence makes me rethink the use of TDD in creating (almost) bug-free software.


Adding to that is what Dijstra said about tests:

> "Testing shows the presence, not the absence of bugs."

What if we instead focus on 






Which of course Uncle Bob Martin also talked about in his book, Clean Architecture... Let's try look what he said...


> Dijkstra once said: "Testing shows the presence, not the absence of bugs." In other words, a program can be proven incorrect by a test; but cannot be proven correct. Therefore all that tests can do, after sufficient testing effort, is allow us to deem a program to be correct enough for our purposes.
<br /><br />
> The implication is stunning. Software development is not a mathematical endeavor; even though it seems to manipulate mathematical constructs. Rather, software is like a Science. We show correctness by applying our best efforts, and failing, to prove incorrectness.

(Oh! He quoted Dijkstra... I did not remember that...)

<!--more-->
