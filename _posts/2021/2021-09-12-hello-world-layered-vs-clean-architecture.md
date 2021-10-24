---
layout: post
title: '"Hello World" Layered Architecture vs Clean Architecture'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2021-09-12 12:00:00 AM UTC
pinned: true
---

<!-- Started May 7, 2021 12:00 AM Philippine Time -->
<!-- Ended August 16, 2021 9:54 PM Philippine Time -->

<!-- 
{: .img-fluid .float-left .ml-5 .pl-5}
![Layered Architecture Simple Diagram](/images/2021/2021-05-22-simple-diagram-layered-architecture.png)

{: .img-fluid .float-right .mr-5 .pr-5}
![Clean Architecture Simple Diagram](/images/2021/2021-05-22-simple-diagram-clean-architecture.png)

<div class="clearfix"></div>
 -->

Many weeks ago, I read [an article](https://xianminx.github.io/2013/02/28/computer-system-a-programmer-perspective/) which says this:

> the best way to learn a field is to know the overall structure, but not many single isolated points

I think I'm very inclined to finding articles like that one because my mother is a preschool teacher and I might have gotten from her this inclination or desire of wanting to find a way to make things understandable to beginners or children. :smile:

That thing aside...

The article is saying that in learning something it's best to have a bird's eye view of what we want to learn before delving into the details (and maybe constantly review the bird's eye view while learning about the details). 
<!-- It's also very helpful if we [compare it with something we already know](https://betterexplained.com/articles/adept-method/). -->

And that's what this article is about. It just contains a bird's eye view diagram of the Clean Architecture (with sample code), and its [comparison](https://betterexplained.com/articles/adept-method/) to the Layered Architecture which many programmers are already familiar with:

{: .mb-0 }
<small>(Layered Architecture on the left, Clean Architecture on the right)</small>

<img width="300" class="float-left img-fluid" src="/images/2021/2021-05-22-hello-world-layered-architecture-code-with-diagram.png" alt="">

<img width="400" class="float-right img-fluid" src="/images/2021/2021-05-22-hello-world-clean-architecture-code-with-diagram.png" alt="">

<div class="clearfix"></div>

Notice that in the Clean Architecture diagram, the Presentation and Data layers are pointing towards the Domain layer. And that the Domain layer owns the interface for the data repository (because as what Uncle Bob Martin said, "the client is the owner of the abstract interface").

And of course, the object construction will be done in the `main` or the entry point of the application, also called the [composition root](https://blog.ploeh.dk/2011/07/28/CompositionRoot/):

{: .small }
``` csharp
static void Main(string[] args)
{
    var greetingsController = 
        new GreetingsController(
            new GreetingsService(
                new GreetingsRepository()));

    ...
}
```


If you want code you can execute, go [here](https://github.com/jeremiahflaga/hello-world-layered-vs-clean-architecture).

If you want to read an artistic way of describing the Clean Architecture, to go Uncle Bob Martin's article, titled "A Little Architecture", [here](http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html).










<!-- 

One of the things -- my mind -- preoccupied with since the beginning of my  programming is how to best structure a software system -- is software architecture.



I'm going to write a few blog post on the simplest possible example of Layers architecture and clean architecture I can think of, and then compare them and point out the differences between them.


But who knows, these posts might someday help a poor young soul who is trying to understand Clean Architecture.






One of the things I have observed about life so far is that people can have more courage or patience to deal with evil if they have a clear picture of what good looks like. But sometimes people becomes overwhelmed with the evil, they also become discouraged to face it.

	As a corollary: people will know what things to avoid doing what they know is evil, but they will have no direction to go if evil is all they know. They will only have direction if they know what is good.
	
	... So even if people do not know evil, and they only know good, --- it is more advatageous because first is that they have direction, and second is that they 
	
	
		If one knows what evil is, he knows what to avoid, but he does not know where to go.
		But if one knows what is good,  he has an idea on what to avoid (avoid whatever that do not look like good), and he also has a direction; he knows where to go.
		
		(There's another thing: it seems to me that knowing what is good does not necessarily make one want to do what is good, or it does not make one move towards the direction of goodness.)


	(I'm not saying that I know what good is.. But there was someone who claimed to be good, but he also said that only God is good.)






Uncle Bob gave this diagram as an example of how to implement a Clean Architecture







---------- 

-->
