---
layout: post
title: 'Is Micheal Feathers backing out from his <em>&quot;Code without tests is bad code&quot;</em> statement?'
categories: [Thoughts, Programming]
tags: [Michael Feathers]
date: 2019-08-06 12:40:00 PM UTC
---

<!-- Aug 6, 2019 08:40:00 PM Philippine Time -->

<!-- toinks!!! Micheal Feathers said, "WELC was fully ingrained in that 'if we don't have tests, we can't do much' attitude", last 2011! -->

<!-- toinks!!! It seems like Micheal Feathers is backing out from the "Code without tests is bad code" statement from his book WELC! -->

<!-- Seems like Michael Feathers is saying it's sometimes okay to refactor even without tests! -->

<!-- 
The book ["Working Effectively with Legacy Code"](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga) of Michael Feathers was published in 2005. I bought a copy last 2017 because it is one of the books recommended by many respected software developers; and two out of three of the sofware projects I was involved in by that time
 -->

Story time...

About six weeks ago, I was deciding whether to buy _this_ another interesting book, or to wait for Michael Feathers' upcoming book, ["Brutal Refactoring"](https://www.bookdepository.com/Brutal-Refactoring-Michael-C-Feathers/9780321793201?a_aid=jflaga). But I forgot its release date, so I googled for it.

One of the top results is an article of his with the same title as his upcoming book: ["Brutal Refactoring"](https://michaelfeathers.typepad.com/michael_feathers_blog/2011/03/brutal-refactoring.html). It was posted last 2011, about seven years after his book, ["Working Effectively with Legacy Code"](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga) or WELC, was published!

Ha! He's been preparing this book since 2011!

<!--more-->

But in that post he said this:

> "These days, I'm much more aggressive in my approach to old code.  WELC was fully ingrained in that "if we don't have tests, we can't do much" attitude.  **I think that part of that was a sign of the times, and part of it was a reflection of my natural fear as a consultant.**  You walk in and you don't know anything about the code base so you can't even come close to accurately assessing risk. Nicely, though, people who are embedded in teams often can, and I've learned a lot from people who've tried things out long term in their code and have lived to tell the tale."

... toinks!!! 

It seems to me like Micheal Feathers is [backing out](https://www.youtube.com/watch?v=SdtKDRn1S9E) from the [_"Code without tests is bad code"_ statement from his book](/memorabilia/books/quotes-from-working-effectively-with-legacy-code/)... since 2011! 

_Is he?_

<small>(To me, after reading the book, that statement meant _"you should not do much refactoring if you do not have tests in place to show that your refactorings did not break anything"_)</small>

You know, I'm interested about things like this because at this moment I have been convinced that [TDD is a very useful discipline for programmers](/memorabilia/quotes/tdd/) (even though I have not yet used it in a real world project :grin:, partly because I have never been invovled yet in starting a project from scratch at work). And as a corollary to that, I am also convinced that doing things that can make us introduce TDD in a existing project later on --- things such as writing tests for and refactoring an existing code base to make it more testable (and readable, of course) --- is very useful and extremely helpful to software developers who are and will be working on that project. <small>(I think they are useful not only at work but also in life outside of work --- in life as a whole.)</small>

And because of that, I am very interested in knowing if there are some [what I consider] _master_ developers, one of whom is Michael Feathers, who have [_apostasized_](https://www.askdifference.com/apostatize-vs-apostasize/) from that kind of mindset. Because if someone did, that means it's time to rethink my stand on some things.


So, back to the quote...

_It seems to me_ that he is saying that it's okay for me to refactor my code base even when I do not have automated tests to show that my refactorings did not break anything, _as long as_ I am already familiar with the code base! (_Is he? Or is it just me?_) If that is the case, then, Yehey! That's the kind of mindset I had before reading WELC. But after reading WELC, whose Preface says this, ...

> "**Code without tests is bad code.** It doesn't matter how well written it is; it doesn't
matter how pretty or object-oriented or well encapsulated it is.
<br /><br />
> "With tests, we can change the behavior of our code quickly and verifiably. Without them, we really don't know if our code is getting better or worse."

..., I feel somewhat guilty when refactoring without tests.

But even after reading WELC I still kept on doing that :grin: --- refactoring without tests (with a bit of guilt of course) --- because changing existing code with the goal of making it testable is a skill that cannot be learned over night. It takes time to learn it.

<!-- 
And in a previous project I was involved in, I tried to do some changes to mold the project into something that is testable, but I encountered some obstacles which made me abandon the endeavor. <small>(_Did you encounter obstacles or did you become lazy?_ I can't remember anymore; or don't want to. :grin:)</small>
 -->

And worse, by the time I already know how to change my code to make it testable, I also am already very familiar with my code base and have come to realize (or have come to fool myself into thinking?) that refactoring and writing tests will be a waste of time and effort at that point in time. (Poor _next_ developer. :worried:)

<!-- 
(assuming of course that I will be the one working on the project forever, which certainly is not the case most of the time... Poor _next_ developer. :worried:)
 -->

Okay, back to the quote again...

I hope that my initial thoughts, that Micheal Feathers is backing out from his _"Code without tests is bad code"_ statement, is _not_ 100% accurate. I may never know until I read his upcoming book, ["Brutal Refactoring : More Working Effectively with Legacy Code"](https://www.bookdepository.com/Brutal-Refactoring-Michael-C-Feathers/9780321793201?a_aid=jflaga), about a year from now, May 2020. I cannot wait.


### Update (July 17, 2020):

Forgot about the release date again...

While googling, I found this: ["Brutal refactoring, lying code, the Churn, and other emotional stories from Legacy Land"](https://matthiasnoback.nl/talk/brutal-refactoring-lying-code-the-churn-and-other-emotional-stories-from-legacy-land/) by Matthias Noback:

> Working effectively with legacy code isn’t all about creating test harnesses before refactoring algorithms. The “safety first” strategy doesn’t always apply. Not if the code you’re looking at is LYING IN YOUR FACE anyway.


(here's [the video](https://vimeo.com/338475467) of his talk)


<!-- 
in the previous months I feel of



so it's okay to do these what they call "hacks" if one is new to a project and does not yet know how to refactor the code base



scratch refactoring .. page 212



I hope that what he is saying is something like "we still can do refactorings even without tests, but that doesn't mean that we remove 'writing tests' as part of our goal while refactoring" 
-->
