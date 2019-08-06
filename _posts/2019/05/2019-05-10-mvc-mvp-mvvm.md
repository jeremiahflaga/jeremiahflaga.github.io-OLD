---
layout: post
title: 'What is MVC, MVP, and MVVM? What are their similarities? What are their differences?'
categories: [Programming]
tags: [MVC, MVP, MVVM, Presentation Model, Erwin van der Valk]
date: 2019-05-10 3:15:00 AM UTC
---

<!-- May 10, 2019 11:15:00 AM Philippine Time -->

<small>_[This post is not my own answer to that question --- "What is MVC, MVP, MVVM?...". This post just contains _a link_ to the **_best_** :grinning: explanation there is about MVC, MVP and MVVM.]_</small>

-----

While searching my notes for [a blog post I read before about the _two ways_](https://craftsmanshipcounts.com/clean-architecture-a-tale-of-two-stories/) of how the "Use Cases/Interactors", "Presenters", and "Controllers" interact in an application employing Clean Architecture, I found a [note](https://softwareengineering.stackexchange.com/questions/357052/clean-architecture-use-case-containing-the-presenter-or-returning-data) with this [image explaining MVC, MVP and MVVM](https://i.stack.imgur.com/Y82D3.png):

![image explaining MVC, MVP and MVVM - by Erwin Vandervalk](https://i.stack.imgur.com/Y82D3.png)


<!--more-->

It's from an [article written by Erwin van der Valk.](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/) --- in 2009!

One can clearly see the difference between _MVC_ and _MVP-Passive-View_ in that image; and between _MVP-Passive-View)_ and _MVP-Supervising-Controller_. The _MVVM_ diagram looks like the _MVP-Passive-View_ diagram, but you will see the difference if you read the [article]((https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/)) and the [next article](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/18/implementing-the-model-view-viewmodel-pattern/) in the series. (I think the difference is that we can do two-way data binding in MVVM while we cannot (or should not) do it in MVP??)

The article also contains code samples which greatly helps programmers understand the patterns. :smile:

_Go on... Read it!..._

_Don't you think it is the best explanation of MVC, MVP, and MVVM? their similarities? and their differences?_

<!-- I think it is the best explanation there is. -->

I was not able to find this article when I was searching a few weeks ago for a good explanation of MVC, MVP, and MVVM. This article does not show up in search engine results _perhaps_ because its title does not contain the words "MVC" and "MVP" and "MVVM". I hope this blog post will _help_ search engines find this [article by Erwin van der Valk](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/).

Enjoy!

<br />

----------

**Update (May 11 & 12, 2019):**

That [image](https://i.stack.imgur.com/Y82D3.png) above first caught my attention because it puts in one picture some things that I already know (some vaguely at least) about these presentation patterns:

- The _MVC_ diagram seems to match how ASP.NET MVC works: the Controller uses the Model; the View also uses the Model; the Controller chooses what View to show the user; the View tells what Controller methods (or "action" methods) to invoke based on user gestures (for example, if a user clicks on a button in the View, it will invoke an "action" method in the Controller)
- _MVP_ has two variants: Passive View and Supervising Controller
- In the _MVP-Passive-View_ the View does not directly access the Model, while in _MVP-Supervising-Controller_ the View can access the Model
- _MVVM_ is the same as Presentation Model (from this [stackoverflow answer](https://stackoverflow.com/a/101561/1451757))

The image also clearly shows the difference between _MVC_ and _MVP-Passive-View_ because they are placed side by side.

But, after further inspection (and thinking about how each of them will be implemented) ...

seems like _MVC_ and _MVP-Supervising-Controller_ looks **_the same_**

... and ...

_MVVM_ and _MVP-Passive-View_ looks **_the same_** (perhaps their only difference is the two-way data binding in MVVM?)

Let's look at that image again:

![image explaining MVC, MVP and MVVM - by Erwin Vandervalk](https://i.stack.imgur.com/Y82D3.png)

Hmmmm...

Also, the sample code for the Controller of MVC has the View passed into the constructor...

```
   1: public class Controller
   2: {
   3:     private readonly IView _view;
   4:  
   5:     public Controller(IView view)
   6:     {

   . . .
```

... that looks like a Presenter class in a project I was involved in a few years ago. It seems like I can just rename the "Controller" class into "Presenter" to to be able to transform it from MVC to MVP!

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

These patterns look the same to me now... At least MVC and MVP... They look the same...

I wonder if they are just the same pattern given different names by different people who encountered somewhat **_similar problems_**, but are living in different places from different times in history.

Perhaps not. :smile:

But _if_ they are all the same then that explains the confusion of many (including me) when reading stackoverflow answers to questions like _"What is the difference between MVC, MVP and MVVM?"_.

Consider these images which I found being used to compare MVC, MVP and MVVM in [**an** answer to a stackoverflow question](https://stackoverflow.com/a/45850979/1451757):

![](https://i.stack.imgur.com/CFbNC.png)

![](https://i.stack.imgur.com/ENBf1.png)

Confusing, right? In the MVC of the top image, the arrow points from the Model to the View; while in the MVC of the bottom image, the arrow points from the View to the Model.

Then consider these comments regarding that answer:

> "Please don't just copy images - especially when **they don't agree among themselves**. See [the examples for] MVC ---  'browser talks to view' in top picture, but 'talks to controller' in lower picture." --- peter.fr 


> "**In the first diagram what is the difference between MVVM and MVP**? As I see it, it is only the links between the V and the VM/P. Which in one case has the back and forth messages as a bidirectional link and in the other they are represented as two unidirectional links. **I don't see any functional difference between them.** What am I missing?" --- iCyberPaul

_Confusing!_

This makes me remember what Jimmy Bogard said during the first few minutes of [one of his recorded talks](https://www.youtube.com/watch?v=wTd-VcJCs_M) --- when he compared the Traditional N-Tier architecture with DDD-style N-Tier architecture:

> "... looks like they just changed the name of things"

![Traditional N-Tier vs DDD-style N-Tier by Jimmy Bogard](/images/2019/traditional-vs-ddd-style-n-tier-by-jimmy-bogard.png)

It seems like MVC, MVP and MVVM also are just like that --- _"looks like they just changed the name of things"_. Seems to me that confusion like this is prevalent in our industry :laughing: (perhaps because it is still very young, as they say). 

(They also say that these "architecture" things in software --- Onion Architecture, Hexagonal Architecture, Ports and Adapters Architecture and Clean Architecture --- [are all the same](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html), but look at all those names!)

Also, the [person who used that "MVC, MVP, MVVM" image where I first saw it said this](https://softwareengineering.stackexchange.com/a/357066/283196):

> You can use many different ways to communicate between modules and call it MVC. Telling me something uses MVC doesn't really tell me **how the components communicate**. That **isn't standardized**. All it tells me is that **there are at least three components** focused on their three responsibilities.
<br /><br />
Some of those ways have been **given different names**: . . .
<br /><br />
[[image](https://i.stack.imgur.com/Y82D3.png) inserted here]
<br /><br />
And **every one of those can justifiably be called MVC**.

Hmmmm...

Perhaps they are all the same --- They are in a sense! --- _they separate the Presentation Logic from the View_. Perhaps the implementation in code will only differ due to some limitations or/and features of the UI framework that we use(?)

This gets me into thinking that if we are employing a Presentation Pattern in our application, perhaps it's best if we just call it _MVCPVM_ (or something like that) :grinning: instead of MVC or MVP. And if a new team member asks _"What is MVCPVM?"_ then we just let him see our code and show him **_"how the components communicate"_**, and that **_"we intend to separate the Presentation Logic from the View"_**... because if we tell him _"We are using MVC!"_ (or _"We are using MVP!"_), he might already have a different understanding of what MVC (or MVP) is than how we do it in our codebase. And if he studies our codebase and comments _"I don't think that's MVC (or MVP)"_ there will be war.  :laughing: 


<!--

 _(not necessarily because the team members are prideful or not willing to learn :grin:, but because there is no standardized definition for MVC or MVP [Or is there not? If there is, please tell me in the comments section below.])_.


<br />

**P.S.**

Another thing which made me attracted to [Erwin van der Valk's article](https://blogs.msdn.microsoft.com/erwinvandervalk/2009/08/14/the-difference-between-model-view-viewmodel-and-other-separated-presentation-patterns/) is his statements about MVC...

> "Most examples of the MVC pattern focus[ed] on very small components, such as building a text box or building a button."

... which seems to match Uncle Bob Martin siad in one of his talks: "MVC was designed for the small, not for the architecture of the entire system"

. . .

If someone tells you that their app employs the _MVC Architecture_ or the _MVP Architecture_, that _might_ (_might_, I say) indicate that their app is a big-ball-of-mud.

MVC, MVP, and MVVM are patterns only for the Presentation Layer of your application, not for the entire app.
-->

