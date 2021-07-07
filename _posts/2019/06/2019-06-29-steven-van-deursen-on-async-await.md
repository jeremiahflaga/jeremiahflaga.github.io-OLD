---
layout: post
title: 'Steven van Deursen on async/await'
categories: [Programming]
tags: [async/await, Stephen Cleary, Steven van Deursen]
date: 2019-06-29 03:00:00 PM UTC
---


<!-- finished on June 29, 2019 12:51:00 PM Philippine Time -->
<!-- auto-publish later at specified datetime above -->


I was surfing the web last April for articles on how to use `async/await` with ASP.NET Core WebAPI, because, you know, I want to know [whether or not ASP.NET Core sends back _`"please wait"`_ response messages](https://blog.stephencleary.com/2012/08/async-doesnt-change-http-protocol.html) to the user/machine accross a network when it encounters an `await` in the code. :grin:

One of the articles I found was this: ["Building fully Asynchronous ASP.NET Core 2 Web API"](https://www.mithunvp.com/fully-asynchronous-aspnet-core-2-web-api/).


<!--more-->


In that article the author recommends reading two other articles to better understand `async/await`. I clicked on [one of the links](https://blog.stephencleary.com/2012/02/async-and-await.html)... Oh! This page looks familiar... this is the blog of a [Christian](https://stephencleary.com/god/) programmer I found many months ago... the blog I had been searching for because I forgot to bookmark it in my browser... Stephen Cleary!

Good for me! But that story aside, these are the lessons I learned from Stephen Cleary's article(s):

> 1. "Async all the way down."
<br /><br />
> 2. "A good rule of thumb is to use ConfigureAwait(false) unless you know you do need the context."

<small>(That second rule... I think I will forget that... I think I will.)</small>

One of the interesting things in Stephen Cleary's [article](https://blog.stephencleary.com/2012/02/async-and-await.html) is what he said in the beginning:

> "First, the punchline: Async will fundamentally change the way most code is written.
<br /><br />
"Yup, **I believe async/await will have a bigger impact than LINQ. Understanding async will be a basic necessity just a few short years from now.**"

He said that in 2012 --- seven years ago.

That means that I have to start to really really master asynchronous programming now!<sup id="footnote-indicator-1">[[1]](#footnote-1)</sup>


But... 

A few weeks after reading Stephen Cleary's article(s), I read another hero-of-mine-to-be, Steven van Deursen, [recommending _against_ using async/await by default](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/#steven---11-october-15):

> "Whether or not the use of async is useful and actually beneficial depends on a lot of factors, but in general **I’m against just making everything asynchronous by default**, as I explained [here](https://codereview.stackexchange.com/questions/84379/viewmodel-creator-design/84402#84402)."

Oooo...

I hope Steven van Deursen can save me from the troubles of asynchronous programming!

Perhaps not. :laughing:

But it's good to know what some of our master programmers has to say about asynchronous programming...

I'm going to paste the [entire statement of Steven van Deursen about asynchronous programming from codereview.stackexchange](https://codereview.stackexchange.com/questions/84379/viewmodel-creator-design/84402#84402), which he referred to in his quote above: 

> A last suggestion I would like to make is to get rid of the complete async programming model. **Asynchronous methods tend to spread through your application like a virus and make both programming and debugging your application much harder.** Yes, this asynchronous programming has become WAY easier than it used to be, but it is STILL harder than synchronous programming and it will probably stay harder untill the .NET runtime has been rewritten from the ground up (if that's even possible).
> 
> I know this is against popular opinion, but there is hardly ever a reason to polute your entire code base with this asynchronous programming model. Main reason for Microsoft to push this programming model really hard is because it is more efficient when running in the cloud. This makes sense, because in Azure, you pay per CPU cycle and per the number of machines you need. But on the other hand, asynchronous programming costs way more developer cycles, and because developers are quite expensive, it is quite unlikely that your savings on the Azure bills will actually compensate the extra developer costs. But obviously, you will have to do the math yourself.
> 
> Don't get me wrong, of course we want --- and need --- responsive UIs so **we might need a few async/await calls inside your Window, Page or View Model classes in our presentation layer** built with WPF, Silverlight, Win Forms or some other client technology. **You can still do this, even though your whole code base below is synchronous, with synchonous calls to the database, web services and the file system.** When doing that, you will still be able to make your UI responsive, but the only difference is that you'll have a background thread sleeping most of the time, instead of using I/O completion ports. But I've never ever worked on an application where having this single extra background thread was a problem. Even for Windows Phone applications this is a non-issue.
> 
> But since you're building an MVC application, don't bother in making your controller code asynchronous, **the user's browser will wait anyway, even if you make your controller asynchronous.**
> 
> Asynchronous programming may be the new shiny thing in the .NET world, and with some training and experience, we can become quite effective as developers in applying it, but even than it is more painful than synchronous progamming (which is hard enough by itself), and **instead of spending money on training developers learning how to do async, I rather spend this money in training them to learn the SOLID design principles, Test Driven Development, Functional Programming or writing clean code**. There are so many other skills that are probably more important and more effective in reducing the total cost of ownership, that I rather have my money on that first.


I like that idea of having async/await calls _only_ in the presentation layer and making everything else below it synchronous.

I would like to highlight that for my future self... when he reads this to review async/await:

{:.message}
If possible, have async/await calls _only_ in the presentation layer and make everything else below it synchronous.

I'm going to repeat it, in red:

{:.message .text-danger .font-weight-bold}
If possible, have async/await calls _only_ in the presentation layer and make everything else below it synchronous.

Okay... :smile:

Let me copy&paste more statements from the exchanges in the comments section:

> This is a great answer; specially that ASP.NET MVC thing. Browsers would always wait, so it make them use await keyword by default; although they don't. Deserves a +1. 
<br />
--- `Afzaal Ahmad Zeeshan Mar 19 '15 at 10:55`
<br /><br />
The caveat needs to be made that **sync will be faster during light load** but as load increases it will slow down when there aren't enough threads available. Then eventually the queue will be full and requests will be refused. Using **async allows the server to continue to be responsive under higher load**. This has it's limits as well, but it means I can get more utilization from my individual servers. That's the reason for using async, not because individual requests will be faster than using sync methods - because they won't due to the overhead of async. 
<br />
--- `Mark J Miller Mar 20 '15 at 14:52`
<br /><br />
<span class="quoted-text">`@MarkJMiller`: I addressed this in my post, where I say that the sync model might increase "the number of machines you need". **Don't forget that we can increase the default thread pool size in ASP.NET, which allows us to have more parallel requests.** This costs more memory and won't take us as far as the async model. But again you should ask yourself: do you really need this? 
<br />
--- `Steven Mar 20 '15 at 23:01`
</span>


He said those last March 2015. Today, we already have .NET Core. Like me, you might be also wondering what he has to say about using `async/await` in .NET Core.

Here is what he has to say about that... from the same comment thread:


> `@Steven` have your thoughts on async changed in the re-writing/porting of netcore? 
<br />
--- `Shawn Mclean Dec 11 '16 at 1:01`
<br /><br />
<span class="quoted-text">
`@ShawnMclean` no, not really. **Core might be async from the ground up, but unless you have a really compelling reason to do so, I would advise making all your own code synchronous.**  
<br />
--- `Steven Dec 11 '16 at 1:10`
</span>
<br /><br />
`@Steven` I disagree about async. If you do it from the start the amount of extra code you write is minimal. A web server will be able to handle a lot more requests with fewer resources which will save a lot of money in the long run.  
<br />
--- `Daniel Lorenz Nov 28 '17 at 14:51`







----------

## Stephen Cleary's uncoming book

And oh. Stephen Cleary has an upcoming book on asynchronous programming in .NET --- a [2nd edition to his book "Concurrency in C# Cookbook"](https://www.bookdepository.com/Concurrency-C-Cookbook-2e-Stephen-Cleary/9781492054504)

I was going to buy his first edition, but then I found out that his 2nd edition is coming out soon. I'm going to buy it when this comes out. I hope you will too. :smile:

I'm really hoping that someone will completely save us from the pain of asynchronous programming, but until that time comes I think this book by Stephen Cleary will greatly help ease the pain.




<br />

----------

<sup id="footnote-1">[[1]](#footnote-indicator-1)</sup> <small>I heard that functional programming will save us out of this difficulty(?) Well I do not yet know functional programming, so perhaps I misunderstood what I heard about it. :smile: </small>
