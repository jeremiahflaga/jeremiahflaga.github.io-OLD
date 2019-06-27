---
layout: post
title: 'Steven van Deursen advises against async/await'
categories: [Programming]
tags: [Vertical Slice Architecture, Featurized Architecture, Jimmy Bogard, Matthias Käppler]
date: 2019-06-15 02:15:00 AM UTC
published: false
---

<!--  -->

Last April I was surfing the web for articles on how to use async/await with ASP.NET Core WebAPI.

One of the articles I found was this: ["Building fully Asynchronous ASP.NET Core 2 Web API"](https://www.mithunvp.com/fully-asynchronous-aspnet-core-2-web-api/)

I that article the author recommends reading Stephen Clear

	First, the punchline: Async will fundamentally change the way most code is written.
Yup, I believe async/await will have a bigger impact than LINQ. Understanding async will be a basic necessity just a few short years from now.



Whether or not the use of async is useful and actually beneficial depends on a lot of factors, but in general **I’m against just making everything asynchronous by default**, as I explained [here](https://codereview.stackexchange.com/questions/84379/viewmodel-creator-design/84402#84402).

https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/


<!--more-->




About Async Programming

A last suggestion I would like to make is to get rid of the complete async programming model. Asynchronous methods tend to spread through your application like a virus and make both programming and debugging your application much harder. Yes, this asynchronous programming has become WAY easier than it used to be, but it is STILL harder than synchronous programming and it will probably stay harder untill the .NET runtime has been rewritten from the ground up (if that's even possible).

I know this is against popular opinion, but there is hardly ever a reason to polute your entire code base with this asynchronous programming model. Main reason for Microsoft to push this programming model really hard is because it is more efficient when running in the cloud. This makes sense, because in Azure, you pay per CPU cycle and per the number of machines you need. But on the other hand, asynchronous programming costs way more developer cycles, and because developers are quite expensive, it is quite unlikely that your savings on the Azure bills will actually compensate the extra developer costs. But obviously, you will have to do the math yourself.

...



Asynchronous programming may be the new shiny thing in the .NET world, and with some training and experience, we can become quite effective as developers in applying it, but even than it is more painful than synchronous progamming (which is hard enough by itself), and instead of spending money on training developers learning how to do async, I rather spend this money in training them to learn the SOLID design principles, Test Driven Development, Functional Programming or writing clean code. There are so many other skills that are probably more important and more effective in reducing the total cost of ownership, that I rather have my money on that first.





@Steven have your thoughts on async changed in the re-writing/porting of netcore? – Shawn Mclean Dec 11 '16 at 1:01

@ShawnMclean no, not really. Core might be async from the ground up, but unless you have a really compelling reason to do so, I would advise making all your own code synchronous. – Steven Dec 11 '16 at 1:10

@Steven I disagree about async. If you do it from the start the amount of extra code you write is minimal. A web server will be able to handle a lot more requests with fewer resources which will save a lot of money in the long run. – Daniel Lorenz Nov 28 '17 at 14:51

