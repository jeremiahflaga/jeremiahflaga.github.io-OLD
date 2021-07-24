---
layout: post
title: What is the most basic rule when writing code?
subtitle: 'Focus on the "what", not the "how"'
categories: [Programming, Let's ask our masters]
tags: [Computer Science, Abstraction, Noam Nisan, Shimon Schocken, Jonathan Boccara, GOOS, Duke Ellington, Greg Howlett, Albert Einstein, RObert Martin, Kent Beck]
date: 2017-11-30 03:30:00 AM UTC
dateLastUpdated: 2021-06-27 10:30:00 AM UTC
pinned: true
---

<!-- November 30, 2017 11:30:00 aM Philippine Time -->
<!-- Updated July 24, 2021 09:45 PM Philippine Time - added link to Nand2Tetris course, and changed "the principle that rules them all" to "the principle to rule them all" -->


Do you like simple rules?

### Simple rules

I like simple, but/and/yet all-encompassing rules _(partly because I'm not very good at remembering lots of things :smile: ):_

> You can start anywhere.

> "If it sounds good, it is good." --- Duke Ellington

> "Everything should be as simple as possible, but not simpler." --- Albert Einstein

<!--
> If it is hard, it must be wrong.
-->

<!--more-->

> "The only way to go fast is to go well" --- Uncle Bob Martin

> "First make it work. Then make it right. Then make it fast" --- Kent Beck

### Another simple rule

I found another simple rule!

In the introduction videos of the [Nand2Tetris Part 1 course](https://www.coursera.org/learn/build-a-computer), Noam Nisan and Shimon Schocken calls it _"The Main Secret of Computer Science"_:

![The Main Secret of Computer Science](/images/2017/main-secret-of-computer-science.png)

> Don't worry about the **"how"**, only about the **"what"**.
> 
> In other words, don't worry about the "implementation", only about the "abstraction".


Steve Freeman and Nat Pryce, the authors of the [GOOS book](https://www.bookdepository.com/book/9780321503626?a_aid=jflaga), also said something about it (p. 252): 

> All code should emphasize **"what"** it does over **"how"**, including test code.


Jonathan Boccara declared it as [**"the principle to rule them all"**:](https://simpleprogrammer.com/2017/01/27/respecting-abstraction/). 


> ...respecting levels of abstraction... , [focusing on] **what** a particular piece of code intends to do as opposed to **how** it is implemented... is the one principle to rule them all because it automatically applies all the best practices... When you follow it, you’ll find yourself naturally writing code with a high-quality design.

<!-- 
> ... abstraction is characterized by **what** a particular piece of code intends to do as opposed to **how** it is implemented
 -->

### "what" over "how"

Simple, yet all-encompassing rule!

> Focus on the **"what"**, not the **"how"**

Of course we might still be the one to write the _"how" part_. But if we focus on the _"what" part_ when writing code, we will end up with code that is, what they call, **_"intention revealing" code_**.

> "what does this code do?..."
> 
> "ahhh!"

And **_"intention revealing" code_** is easy to read! Remember that we, and our coleagues _(we should [think about our coleagues also](/2017/09/30/fear-that-produces-no-fear) :smile:)_ spend _more_ time reading code than writing code!


I should put this in mind always then...

_"what" over "how"_

_"what" over "how"_

_"what" over "how"_

_"what" over "how"_

:smile:

**_"what does this button do?"_**

<iframe width="560" height="315" src="https://www.youtube.com/embed/MtaTKXJ89jk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## An example:

When computing the sum of a list of numbers, it is easier to read this

``` python
sum = listOfNumbers.computeSum();
```

or this

``` python
sum = computeSumOf(listOfNumbers);
```

than this

``` python
sum = 0 
index = 0
while index < listOfNumbers.length
    n = listOfNumbers[index]
    sum += n
    index++
```

The first two code snippets focuses on **what** the code does instead of **how** it does it.
