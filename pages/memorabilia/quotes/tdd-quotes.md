---
layout: page
title: TDD Quotes
categories: [Programming]
tags: [TDD]
permalink: /memorabilia/quotes/tdd/
---

Through Uncle Bob Martin's charismatic presentations on TDD, I had been convinced (and is still am) that TDD is a very important practice for programmers who want to have control over their software (instead of their software controlling them).

But because I am **not** yet very experienced on TDD, I will have to be less dogmatic about it than Uncle Bob is: I will just hold to the same position as John Sonmez holds on TDD for now:

<!-- 
But because I have not yet experienced using TDD in a real world project, and I am barely writing automated tests in the projects I have been involved in, I have to back off a little bit from that _belief_, because as what [Jordan Peterson](https://www.goodreads.com/quotes/9237559-you-can-only-find-out-what-you-actually-believe-rather) is saying, _"You can only find out what you actually believe (rather than what you think you believe) by watching how you act."_ :grin: And at the moment, I think I'm not acting the way I'm supposed to act if I truly am a TDD advocate.

For now, I will just hold the same position as John Sonmez has on TDD:
 -->

> ... the biggest benefit for me for doing test driven development that I found is **it helps you become better at writing good code**.
>
> The reason why this is true is because when you write the test first, **it forces you to use the APIs of your code**... When you write unit test first and then you write the code to make those unit test pass, **you're basically starting out by defining the API** which is really valuable for writing quality clear code that’s easy to understand.
>
> **Let me tell you what I think now. I think now that test driven is like training wheels for writing good code. It should be used in certain cases but not all the time.**
>
> ... Overall, with my view, I think test driven development is good. It’s really, really good for beginners. If you’re starting out you need to start practicing this in order to become better at development. As you get more experience, you have to use your common sense. **You have to be pragmatic about it. If you feel like the tests aren’t adding value or they’re just wasting time, then that might be true.**
>
> --- John Sonmez (from ["My Views On Test Driven Development"](https://simpleprogrammer.com/views-test-driven-development/))

(Take note also of what Mark Seemann says about rules in his article
["Curb code rot with thresholds"](https://blog.ploeh.dk/2020/04/13/curb-code-rot-with-thresholds/#6c5e4b0817f742bf9ecc0a9050ecb7b5): _"Rules make great training wheels. Once you become proficient, the rules are no longer required. They may even be in your way. When that happens, get rid of them."_)

-----

<!-- 
## Original contents

I believe, like [Uncle Bob Martin does](http://blog.cleancoder.com/uncle-bob/2014/05/02/ProfessionalismAndTDD.html), that, _today_, TDD (or Test-Driven Development) is one of the hallmarks of professionalism in the software development world.

So 
 -->

... I would like to convince myself and my fellow software developers to embrace TDD. :blush:

Ergo, these quotes...



### Uncle Bob Martin <small>([So... You want your code to be maintainable?](https://sites.google.com/site/unclebobconsultingllc/so-you-want-your-code-to-be-maintainable))</small>

> "**Nothing makes a system more flexible than a suite of tests.** 
>
> Nothing. 
>
> Good architecture and design are important; but the effect of a robust suite of tests is **an order of magnitude greater**. It’s so much greater because those tests enable you to improve the design.
>
> "This can’t be overstated. If you want your systems to be flexible, write tests. If you want your systems to be reusable, write tests. If you want your systems to be maintainable, write tests.
>
> "And write your tests using the Three Laws of TDD."


-----

### Roy Osherove <small>[www.simform.com/unit-testing-tdd](https://www.simform.com/unit-testing-tdd/)</small>

> "**Tests are stories we tell the next generation of programmers on a project.**" --- Roy Osherove



-----

### Michael Feathers <small>([Working Effectively with Legacy Code](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga))</small>

> "**Code without tests is bad code.** It doesn't matter how well written it is; it doesn't
matter how pretty or object-oriented or well encapsulated it is.
>
> "With tests, we can change the behavior of our code quickly and verifiably. Without them, we really don't know if our code is getting better or worse."



-----

### Uncle Bob Martin <small>([TDD Harms Architecture](http://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html))</small>

> "The idea that the high level design and architecture of a system emerge from TDD is, frankly, absurd. **Before you begin to code any software project, you need to have some architectural vision in place. TDD will not, and can not, provide this vision.** That is not TDD’s role."
>
> However, this does not mean that designs do not emerge from TDD –-- they do; just not at the highest levels. **The designs that emerge from TDD are one or two steps above the code**; and they are intimately connected to the code, and to the red-green-refactor cycle.
>
> It works like this: ..."



-----

### Micheal Feathers <small>([The Deep Synergy Between Testability and Good Design - NDC 2010](https://www.youtube.com/watch?v=4cVZvoFGJTU))</small>

> "If your code is not testable, then it is not a good design."

> "In general, every time you encounter a testability problem, there is an underlying design problem."

> "There is a big difference between mentally knowing about coupling and feeling the pain of coupling... But **when we actually write tests, we feel concrete pain**. The concrete pain is not because testing is difficult, it's because we need to change our design."

> "I can have all these interesting thoughts about design. But if I write my tests first, it's an instant concrete reminder of whether things are actually aligned pretty well with good design principles or not."

> "If you're finding trouble testing things, reconsider your design and end up with something better."



-----

### Uncle Bob Martin <small>(Expecting Professionalism - Kuppelsalen, Copenhagen - December 15, 2015)</small>


> **TDD minimizes debugging time.**

> TDD is a good way to make code example for the programmer to read.

> TDD makes writing code fun.

> **TDD makes you write decoupled code** (decoupled code == testable code) which can make the design of the system better.


> TDD gives you a suite of tests that you trust with your life. And when you have that suite of tests, you can do many other things like cleaning code and cleaning the design.
>
> Nothing makes code more flexible and maintainable than having a suite of tests by a huge order of magnitude.
>
> You give me an application that is beautifully designed but there's no tests. I'll be afraid to touch it. It will rot.
>
> You give me an application that is badly designed but has a comprehensive suite of tests. I can make the design better because I have a suite of tests.
>
> A suite of tests is more important than a good design.



-----

### Joshua Kerievsky <small>(Refactoring to Patterns)</small>

> "Test-driven development and continuous refactoring, two of the many excellent XP practices, have dramatically improved the way I build software. 
>
> "I’ve found that these two practices have helped me and the organizations I’ve worked for **spend less time over-engineering and under-engineering and more time creating high-quality, function-rich code, produced on time**."



-----

### Harry Percival <small>(["Test-Driven Development with Python"](https://www.obeythetestinggoat.com/book/praise.harry.html))</small>

> I’ve lost count of the number of times I’ve thought “Thanks, tests”, as a functional test uncovers a regression we would never have predicted, or a unit test saves me from making a really silly logic error. **Psychologically, [TDD] made development a much less stressful process. It produces code that’s a pleasure to work with.** (from [Preface](https://www.obeythetestinggoat.com/book/preface.html))

> Ultimately, programming is hard. Often, we are smart, so we succeed. TDD is there to help us out when we’re not so smart. Kent Beck (who basically invented TDD) uses the metaphor of lifting a bucket of water out of a well with a rope: when the well isn’t too deep, and the bucket isn’t very full, it’s easy. And even lifting a full bucket is pretty easy at first. But after a while, you’re going to get tired. **TDD is like having a ratchet that lets you save your progress, take a break, and make sure you never slip backwards. That way you don’t have to be smart all the time.** (from [Chapter 4](https://www.obeythetestinggoat.com/book/chapter_philosophy_and_refactoring.html))


-----

### Michael ([GeePaw](http://anarchycreek.com/whosgeepawhill/)) Hill <small>([How TDD and Pairing Increase Production](http://anarchycreek.com/2009/05/26/how-tdd-and-pairing-increase-production/))</small>

> ... the hard part of programming is the thinking, not the typing...
>
> TDD increases your production because it serves as a thinking-aid.  It limits back-tracking and gold-plating  and it reduces code-study and debugging.  Pairing increases your production for the very same reason.  Two developers together don’t type as fast as two developers separately, but we don’t care: **the bottleneck is the thinking, and pairing and TDDing both improve it.**


-----

### John Sonmez <small>(from ["My Views On Test Driven Development"](https://simpleprogrammer.com/views-test-driven-development/))</small>

> ... the biggest benefit for me for doing test driven development that I found is **it helps you become better at writing good code**.
>
> The reason why this is true is because when you write the test first, **it forces you to use the APIs of your code**. It forces you to use that production code --- so that influences you to make the structure of that code better for being used and to be more understandable.  When we just write a bunch of code first where you don’t think about how it’s going to be used, we are guessing at how people are going to use it or how easy this is going to be to use. When you write unit test first and then you write the code to make those unit test pass, **you're basically starting out by defining the API** which is really valuable for writing quality clear code that’s easy to understand.
> 
> **Let me tell you what I think now.  I think now that test driven is like training wheels for writing good code.  It should be used in certain cases but not all the time.**

> When you feel like tests aren’t adding value, this is my personal belief, stop writing those tests.  You want to be able to test code with your test driven development or when you’re writing a unit test that makes sense where it’s going to give you some value from doing that.  If you feel like it’s not creating value, stop doing it because chances are it’s not creating value.

> Overall, with my view, I think test driven development is good.  It’s really, really good for beginners.  If you’re starting out you need to start practicing this in order to become better at development.  As you get more experience, you have to use your common sense.  You have to be pragmatic about it.  If you feel like the tests aren’t adding value or they’re just wasting time, then that might be true.



-----

### Sandro Mancuso <small>(from page 100 of ["The Software Craftsman"](https://www.bookdepository.com/Software-Craftsman-Sandro-Mancuso/9780134052502?a_aid=jflaga))</small>

> Although TDD has "test" in its name, TDD is actually a design practice. **When test-driving our code, it becomes very difficult to write complex code.** First, because we write just enough code to satisfy the requirements --- represented as tests --- we discourage overengineering and big design up front (BDUF). Second, whenever our code becomes a bit too complex and bloated, it also becomes difficult to test. Complexity in our code and bad design choices are highlighted by the complexity in maintaining and writing new tests. These tests lead us to re-analyze the design of our code and refactor to make it simpler.




-----

### Emily Bache <small>(from page 113 of ["The Coding Dojo Handbook"](https://www.bookdepository.com/The-Coding-Dojo-Handbook-Emily-Bache/9789198118032?a_aid=jflaga))</small>

> **Once you've worked on a system with extensive automated tests, I don't think you'll want to go back to working without them. You get this incredible sense of freedom to change the code, refactor, and keep making frequent releases to end users.**
>
> Actually, you design automated functional tests for two main purposes. Initially you want to clarify your understanding of what to build. In fact, at that point, they're not really tests; you usually call them scenarios, or examples. Later, the main purpose of the tests becomes to detect regression errors, although you continue to used them to document what the system does.
>
> When I'm designing a test case for a particular piece of functionality, I want to assess whether it will document that functionality and provide regressions protection, whilst being cost effective to write and maintain. **As with most software it's the maintenance cost that will dominate.**

> ... The cost needs to be in proportion to the benefits you get: helping you understand what the system does, and regression protection.



-----

### J. B. Rainsberger <small>([on twitter](https://twitter.com/jbrains/status/167297606698008576))</small>

> Worried that TDD will slow down your programmers?
>
> Don't. They probably need slowing down.



-----


## Other useful TDD quotes


> "**TDD isn’t something that comes naturally. It’s a discipline**, like a martial art, and just like in a Kung Fu movie, you need a bad-tempered and unreasonable master to force you to learn the discipline."
>
> --- Harry Percival (from [Chapter 1 of "Test-Driven Development with Python"](https://www.obeythetestinggoat.com/book/chapter_01.html))

> "TDD is a discipline, and that means it’s not something that comes naturally; **because many of the payoffs aren’t immediate but only come in the longer term, you have to force yourself to do it in the moment.**"
>
> --- Harry Percival (from [Chapter 4 of "Test-Driven Development with Python"](https://www.obeythetestinggoat.com/book/chapter_philosophy_and_refactoring.html))