---
layout: post
title: 'The key to clean code'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2021-08-08 12:00:00 AM UTC
pinned: true
---

<!-- July 24, 2021  09:30 PM Philippine Time -->
<!-- Updated August 06, 2021  12:30 AM Philippine Time - knowledge is only potential power -->


In the introduction videos of the [Nand2Tetris Part 1 course](https://www.coursera.org/learn/build-a-computer), Noam Nisan and Shimon Schocken said that _the main secret of computer science is this_:

> Don't worry about the **"how"**, only about the **"what"**.
> 
> In other words, don't worry about the "implementation", only about the "abstraction".

![The Main Secret of Computer Science](/images/2017/main-secret-of-computer-science.png)


<small>(Please don't misinterpret their usage of the phrase "don't worry". To better understand what they mean, You have to watch the entire introduction video, or visit their [site](https://www.nand2tetris.org/).)</small>


-----

Steve Freeman and Nat Pryce, the authors of the [GOOS book](https://www.bookdepository.com/book/9780321503626?a_aid=jflaga), said something similar in page 252 of the book: 

> All code should emphasize **"what"** it does over **"how"**, including test code.


-----

Jonathan Boccara also said that it is [_"the principle to rule them all"_:](https://simpleprogrammer.com/2017/01/27/respecting-abstraction/). 

> ...respecting levels of abstraction... , [focusing on] **what** a particular piece of code **intends** to do as opposed to **how** it is implemented... is the one principle to rule them all because it automatically applies all the best practices... When you follow it, you’ll find yourself naturally writing code with a high-quality design.


<div class="message" markdown="1">

But of course, knowledge is only potential power, so... sad life... but at least now we _can_ have a direction...

</div>

## An example:

When computing the sum of a list of numbers, it is easier to read this

``` c
sum = listOfNumbers.computeSum();
// use sum here
```

or this

``` c
sum = computeSumOf(listOfNumbers);
// use sum here
```

than this

``` c
sum = 0 
index = 0
while index < listOfNumbers.length {
    n = listOfNumbers[index]
    sum += n
    index++
}
// use sum here
```

The first two code snippets focuses on **what** the code does instead of **how** it does it. They are shorter compared to the third one. They have intention-revealing names, which make them easier to read.
