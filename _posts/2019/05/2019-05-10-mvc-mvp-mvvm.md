---
layout: post
title: 'What is MVC, MVP, and MVVM? What are their similarities? What are their differences?'
categories: [Programming]
tags: [MVC, MVP, MVVM, Presentation Model, Erwin Vandervalk]
date: 2019-05-10 3:15:00 AM UTC
---

<!-- May 10, 2019 11:15:00 AM Philippine Time -->

<small>[This post is not my own answer to that question --- _"What is MVC, MVP, MVVM?..."_. This post just contains _a link_ to the **_best_** :grin: explanation there is about MVC, MVP and MVVM.]</small>

-----

While searching my notes for [a blog post I read before about the _two ways_](https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/) of how the "Use Cases/Interactors", "Presenters", and "Controllers" interact in an application employing Clean Architecture, I found a [note](https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data) with this [image explaining MVC, MVP and MVVM](https://i.stack.imgur.com/Y82D3.png):

![image explaining MVC, MVP and MVVM - by Erwin Vandervalk](https://i.stack.imgur.com/Y82D3.png)


<!--more-->

It's from an [article written by Erwin van der Valk.](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/) --- in 2009!

One can clearly see the difference between _MVC_ and _MVP-(Passive View)_ in that image; and between _MVP-(Passive View)_ and _MVP-(Supervising Controller)_. The _MVVM_ diagram looks like the _MVP-(Passive View)_ diagram, but you will see the difference if you read the [article]((https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/)) and the [next article](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/18/implementing-the-model-view-viewmodel-pattern/) in the series. (I think the difference is that we can do two-way data binding in MVVM while we cannot (or should not) do it in MVP??)

The article also contains code samples which greatly helps programmers understand the patterns. :smile:

_Go on... Read it!..._

_Don't you think it is the best explanation of MVC, MVP, and MVVM? their similarities? and their differences?_

I think it is the best explanation.

I was searching a few weeks ago for a good explanation of MVC, MVP, and MVVM, but I was not able to find this article. It does not show up in search engine results _perhaps_ because its title does not contain the words "MVC" and "MVP". I hope this blog post will _help_ search engines find this [article by Erwin van der Valk](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/) and include it in their top results.

Enjoy!

<br />

----------

**Update (a few hours after writing this post):**

That [image](https://i.stack.imgur.com/Y82D3.png) above first caught my attention because it puts in one picture some things that I already know about these patterns:

- The _MVC_ diagram seems to match how ASP.NET MVC works: the Controller uses the Model; the View also uses the Model; the Controller chooses what View to show the user; the View tells what Controller methods (or "action" methods) to invoke based on user gestures (for example, if a user clicks on a button in the View, it will invoke an "action" method in the Controller)
- _MVP_ has two variants: Passive View and Supervising Controller
- In the _MVP-(Passive View)_ the View does not directly access the Model, while in _MVP-(Supervising Controller)_ the View can access the Model
- _MVVM_ is the same as Presentation Model (from this [stackoverflow answer](https://stackoverflow.com/a/101561/1451757))

The image also clearly shows the difference between _MVC_ and _MVP-(Passive View)_ because they are placed side by side.

But, after further inspection (and thinking about how each of them will be implemented) ...

seems like _MVC_ and _MVP-(Supervising Controller)_ looks **_the same_**

... and ...

_MVVM_ and _MVP-(Passive View)_ looks **_the same_**: 

Let's look at that image again:

![image explaining MVC, MVP and MVVM - by Erwin Vandervalk](https://i.stack.imgur.com/Y82D3.png)

Hmmmm...

Also, the sample code for the Controller of MVC has the View passed into the constructor:

```
   1: public class Controller
   2: {
   3:     private readonly IView _view;
   4:  
   5:     public Controller(IView view)
   6:     {

   . . .
```

I experience using MVP in a project before, and that code looks like a Presenter to me. It seems like I can just rename the "Controller" class into "Presenter" to transform it from MVC into MVP!

```
   1: public class Presenter
   2: {
   3:     private readonly IView _view;
   4:  
   5:     public Presenter(IView view)
   6:     {

   . . .
```


See!? :grin:

These patterns look the same to me now... They look the same... Really, they do.

I wonder if they are just the same pattern given different names by different people who encountered somewhat **_similar problems_**, but are living in different places from different times in history.

_If_ they are all the same then that explains the confusion of many (including me) when reading stackoverflow answers to questions like _"What is the difference between MVC, MVP and MVVM?"_.

Consider these images which I found being used to compare MVC, MVP and MVVM in [**an** answer to a stackoverflow question](https://stackoverflow.com/a/45850979/1451757):

![](https://i.stack.imgur.com/CFbNC.png)

![](https://i.stack.imgur.com/ENBf1.png)

Confusing, right? In the MVC of the top image, the arrow points from the Model to the View; while in the MVC of the bottom image, the arrow points from the View to the Model.

Then consider these comments regarding that answer:

> "Please don't just copy images - especially when **they don't agree among themselves**. See [the examples for] MVC ---  'browser talks to view' in top picture, but 'talks to controller' in lower picture." --- peter.fr 

and this:

> "**In the first diagram what is the difference between MVVM and MVP**? As I see it, it is only the links between the V and the VM/P. Which in one case has the back and forth messages as a bidirectional link and in the other they are represented as two unidirectional links. **I don't see any functional difference between them.** What am I missing?" --- iCyberPaul

_Confusing!_

This makes me remember what Jimmy Bogard said during the first few minutes of [one of his recorded talks](https://www.youtube.com/watch?v=wTd-VcJCs_M) --- when he compared the Traditional N-Tier architecture with DDD-style N-Tier architecture:

> "looks like they just changed the name of things"

![Traditional N-Tier vs DDD-style N-Tier by Jimmy Bogard](/images/2019/traditional-vs-ddd-style-n-tier-by-jimmy-bogard.png)

It seems like MVC, MVP and MVVM also are just like that --- _"looks like they just changed the name of things"_. Seems to me that confusion like this is prevalent in our industry :laughing: (perhaps because it is still very young, as they say). 

Also, the [person who used that "MVC, MVP, MVVM" image where I first saw it said this](https://softwareengineering.stackexchange.com/a/357066/283196): 

> You can use many different ways to communicate between modules and call it MVC. Telling me something uses MVC doesn't really tell me **how the components communicate**. That **isn't standardized**. All it tells me is that **there are at least three components** focused on their three responsibilities.
<br /><br />
Some of those ways have been **given different names**: . . .
<br /><br />
[[image](https://i.stack.imgur.com/Y82D3.png) inserted here]
<br /><br />
And **every one of those can justifiably be called MVC**.

Hmmmm...

Perhaps they are all the same --- They are! --- _they separate the Presentation Logic from the View_. Perhaps they will only differ due to some limitations or/and features of the UI framework that we use(?)

This gets me into thinking that if we are employing a Presentation Pattern in our application, perhaps it's best if we just call it _MVCPVM_ (or something like that) instead of MVC or MVP. And if a new team member asks _"What is MVCPVM?"_ then we just let him see our code and show him **_"how the components communicate"_**... because if we tell him _"We are using MVC!"_ (or _"We are using MVP!"_), he might already have a different understanding of what MVC (or MVP) is from how we do it in our codebase. And if he study our codebase and comments _"I don't think that's MVC"_, it might ignite a war. :laughing: 

