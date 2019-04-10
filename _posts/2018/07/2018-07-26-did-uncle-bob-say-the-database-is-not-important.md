---
layout: post
title: 'Did Uncle Bob say the database is not important?'
categories: [Programming]
tags: [Robert Martin, Clean Architecture]
date: 2018-07-26 09:30:00 PM UTC
---

<!-- July 27, 2018 05:30:00 AM Philippine Time -->

First of all I would just like to say that I'm not an Uncle Bob fanatic :laughing:. I'm a fan but not a fanatic. I have even [written before on something I disagree with Uncle Bob,](/2017/10/28/this-is-a-lie/) but it was not about programming.

And I'm also aware that Uncle Bob is against this fanaticism about the ideas and practices he is presenting:

> ... In 1999, when Kent Beck and I decided to put our energies into the promotion of Extreme Programming, we feared that we could be starting a religion instead of a movement, and vowed to fight ritualism when it arose. This concern and vow was expressed again in the 2001 meeting that produced the Agile Manifesto...
<br /><br />
 --- from [The True Corruption of Agile](https://blog.cleancoder.com/uncle-bob/2014/03/28/The-Corruption-of-Agile.html)

<!--more-->

Okay... Here I go... defence mode :laughing:


About two weeks ago, in a facebook group I am a member of, I posted a message with a link to Uncle Bob's article ["A Little Architecture"](https://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html).

_Part_ of my message in that post are these words:

> if you were brainwashed into thinking that the database is the center of our application, and you still think the same today... 
<br /><br />
...
<br /><br />
you have to repent :laughing: :laughing:

The haha (:laughing: :laughing:) is part of the post to indicate my frivolous<sup id="footnote-indicator-1">[[1]](#footnote-1)</sup> phrasing and of the use of the words _brainwashed_ and _repent._ (I borrowed the word _repent_ from Christianity; it means _"to change ones mind"_)

_Some_ disagreed with this idea of the business rules as the _center_ of the application.

But there was one who seem to be very angry about that article.

At first, I did not _fully_ understand why he was angry. I thought he disagrees only with the idea of the database being _not_ the center of the application. But I later discovered, through the exchanges in the comments, that he (and perhaps the others) thinks that Uncle Bob is saying in the [article](https://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html) that the database is **_not important_**:

> **Seeming angry person:** Dude, if the database decision is not really important, then...

> **Me:** What! who said the database decision is not important... 
<br />
We never said things like that

> **Me:** You should take into account the context of the article... "Context is King"...

> **Neutral person (observer):** Did you even read what you shared?
<br />
HAHAHAHAHA
<br />
&gt; expanded brain: meme
<br />
&gt; brainlet: Burn the enemy
<br />
&gt; expanded brain: Self burn

"The database decision is not important." --- Someone with even a short experience in software development will not say something stupid like that, I thought haha. How much more someone who has worked in the software develpment industry for so many years, like Uncle Bob Martin!

So I opened the article and began reading it again...

Wow! I have read this article many times in the past and I cannot even remember noticing that the phrase _"the database isn't an important decision"_ is even in the article! :laughing: And the phrase is located at the very beginning of the article!

> What do you mean? The Database **isn't an important decision?** Do you know how much money we spend on them?

> > Too much probably. And, no; the database is not one of the most important decisions.

_(Self burn... :laughing: ... Failed...)_

I was consumed by the grandness of the idea presented in the article that I was not able to notice the _detail_, that _short phrase,_ that the _seeming angry person_ was able to notice.

<!-- 
I suspect that the _seeming angry person_ did not read the entire article before pouring out his rants about that short phrase (that short phrase is even ended with a question mark)! 
-->

> ... The Database **isn't an important decision?**

Firstly, to answer the question of the "Neutral person"  (whom I later learned is someone actually rooting for me :smile: - yehey! ): 

Yes!... I had read the article before, and I visit it from time to time. The first time I read it, it took me about three days of rereading it and analyzing the example before I was able to make sense of what Uncle Bob is trying to say. (I was not yet very familiar with things like this in software development so it took time for me to understand them.)

And about that statement: _"the database isn't an important decision"_ ...

I think Uncle Bob is misinterpreted in here. I can see how he can be easily misinterpreted by how those sentences were constructed. I experienced the misinterpretation firsthand when I read the article again after seeing the "Neutral person's" comment of _"Did you even read what you shared?"_ above. After discovering that part, I said to myself, _"What? Uncle Bob said that? .... Wait, wait... Let's read it again..."_

(Perhaps Uncle Bob did that on purpose? Perhaps not? Perhaps it's just his style of writing.)

So I beg to differ on how those sentences should be interpreted. And here is my explanation:

Did you notice that that sentence is not a _statement_ but a _question_? It ended with a question mark!

> What do you mean? **The Database isn't an important decision?**

And it is the _architect wannabe_, and _not_ Uncle Bob, who said that.

The architect wannabe is asking a question:

> "What do you mean? The Database **ISN'T AN** important decision?"

It is as if he is trying to _put words_ in Uncle Bob's mouth.

Then Uncle Bob replied,

> "... no; the database **IS NOT ONE OF THE MOST** important decisions."

Uncle Bob said **_"ONE OF THE MOST"_**, not **_"AN"_**.

And if one argues that the architect wannabe in the article is _still_ Uncle Bob speaking, remember that the architect wannabe also said this:

> "[the database]... It’s where all the data is organized, sorted, indexed, and accessed. **Without it there would be no system!**"

Whoever will say something like that understands that the database is important.

Based on that statement, it can still be concluded that Uncle Bob is **_NOT_** saying that the database isn't an important decision.

What he is saying is that when it comes to the _"architecture"_ (not when it comes to the purpose of the universe) the database decision is irrelevant, or it is _supposed to be_ irrelevant, or _should be_ irrelevant, _ought to be_ irrelevant --- when it comes to the architecture of the system.

(I think that we can be certain also that Uncle Bob will never say that the architecture of a software system is the most important thing in the entire universe. :laughing:)

In his other writings and talks, Uncle Bob is saying that [we are making the _architecture_ to be about databases and frameworks when it should _not_ be about them.](https://www.infoq.com/news/2013/07/architecture_intent_frameworks)

... So, in conclusion, even if someone says that he can point out a software application where the database decision is more important than the architecture... he is missing the point of the article, because the article is _not_ talking about the "importance of the _'architecture decision' over the 'database decision'"_ in relation to the overall state of affairs; it is talking about, among other things, the importance of the database decision in relation to the architecture of the system.

:bow:

---------------

<sup id="footnote-1">[[1]](#footnote-indicator-1)</sup> <small>I'm not sure if I'm using the word "frivolous" correctly in here. You are free to correct me if I'm not.</small>
