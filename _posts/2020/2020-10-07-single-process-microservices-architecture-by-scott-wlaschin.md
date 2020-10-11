---
layout: post
title: '"Single Process Microservices Architecture" or SPMSA by Scott Wlaschin'
subtitle: ''
categories: [Programming]
tags: [Programming, Microservices, Monoliths, Scott Wlaschin, Robert C. Martin]
date: 2020-10-07 01:00:00 AM UTC
---

<!-- October 6, 2020 5:43 PM Philippine Time -->

{: .message .small }
**NOTE:** The purpose of this post is just to force my brain to remember something I consider very important. Another purpose is so that I can [easily search for it when I need it in the future](https://www.hanselman.com/blog/TheVBEquivalentToCTypeofKeyword.aspx).

-----

In his talk on Youtube titled ["FP & DDD - Functional Programming and Domain-driven Design - a match in heaven!"](https://www.youtube.com/watch?v=zhA-2JQ8XKc&ab_channel=codecentricAG), Marco Emrich gave these three alternatives to Microservices:

1. Modulith (Dan Haywood / Xavier Dury)
2. Structured Monolith (Neil Ford)
3. Single Process Microservices Architecture (Scott Wlaschin)

The third one intrigued me: Single Process Microservices Architecture.

_**Single process** microservices???_

_What's that??_

<!--more-->

I googled for it and found this tweet by [Scott Wlaschin](https://scottwlaschin.com/):

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I&#39;m developing a new alternative to microservices that addresses many of the problems that people run into. I call it &quot;SPMSA&quot;. Bear with me -- I&#39;ll explain the acronym in a later tweet. 1/</p>&mdash; Scott Wlaschin (@ScottWlaschin) <a href="https://twitter.com/ScottWlaschin/status/1183023838487044096?ref_src=twsrc%5Etfw">October 12, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


I'm going to paste the whole thread here so that it will not be lost in the internets (emphasis mine): 

> I'm developing **a new alternative to microservices that addresses many of the problems that people run into**. I call it "SPMSA". Bear with me -- I'll explain the acronym in a later tweet.
> 
> First, **design your microservices in the usual way**. No more than 500 lines each, say, and with well-defined interfaces. But here's the first trick: you **put them all in the same repo.** This means that you have a single commit that encompasses all services at once.
>  
> That means when you commit or refactor, you can build and test the services as a whole system and be sure that they *always* all work together! Awesome!
>  
> The next trick is that when you deploy, you **deploy *all* the services for a given commit at the same time!** If that means redeploying a unchanged service, no worries -- you should have a fast CI/CD pipeline, yes?
> 
> Here's the cool part: you deploy all the services in such a way that **they access some shared memory**. This means that **they can communicate super-fast**, without having to do all that JSON serialization (up to 50% of servicing time). This can make your system twice as fast!
> 
> And of course, there are none of the issues associated with distributed systems. No network latency or availability issues, etc. 👍
[en.wikipedia.org/wiki/Fallacies…](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
>
> So basically, all the benefits of a modular system composed of small single-responsibility services, yet with none of the issues associated with traditional microservice architecture.
> 
> Now, one of the advantages of traditional microservices is that you can use different languages. This is harder with shared memory. Luckily, some SPMSA-friendly platforms let you use different languages easily: Java/Scala/Kotlin/Clojure on JVM and C#/F#/VB on .NET.
> 
> What about scaling? If you need scaling but you're not Google, see if your platform supports something called "threads" or "forking" (of course if you are Google, please ignore this whole thread)
>  
> Anyway, **I call this approach "Single Process Microservices Architecture" or SPMSA**. When microservices drops down the hype curve, I'm predicting that this will become the new hotness.
> 
> (A big /s for this whole thread, in case anyone missed the point 😀) 
>
> ![It's such a fine line between stupid and clever.](https://pbs.twimg.com/tweet_video_thumb/EGs6W1lXkAYRHWs.jpg)

_(me: processing... reading the replies... processing...)_

Ah okay... Single process microservices is just another name for **monoliths**. :laughing:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">One of the reasons I did my recent thread on monoliths is demonstrate that you if describe something without using labels -- or you make up a new label👍 -- you can often get a different perspective on it.<a href="https://t.co/IGM3413xPn">https://t.co/IGM3413xPn</a></p>&mdash; Scott Wlaschin (@ScottWlaschin) <a href="https://twitter.com/ScottWlaschin/status/1183320324835819520?ref_src=twsrc%5Etfw">October 13, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

:blush: :+1: :+1:

I remember a similar thing Uncle Bob Martin said about microservices. It was from his blog post ["Microservices and Jars"](http://blog.cleancoder.com/uncle-bob/2014/09/19/MicroServicesAndJars.html) (emphasis mine):

> ... After reading this you might think I’m totally against the whole notion of Micro-Services. But, of course, I’m not. I’ve built applications that way in the past, and I’ll likely build them that way in the future. It’s just that I don’t want to see **a big fad tearing through the industry leaving lots of broken systems in it’s wake.**
> 
> For most systems the independent deployability of jar files (or DLLS, or Gems, or Shared Libraries) is more than adequate. For most systems the cost of communicating over sockets using REST is quite restrictive; and will force uncomfortable trade-offs.
> 
> My advice:
> 
> > **Don’t leap into microservices just because it sounds cool.** Segregate the system into jars using a plugin architecture first. If that’s not sufficient, then consider introducing service boundaries at strategic points.




