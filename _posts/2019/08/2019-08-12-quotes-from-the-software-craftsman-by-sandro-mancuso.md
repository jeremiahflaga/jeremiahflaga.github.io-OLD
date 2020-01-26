---
layout: post
title: 'Some quotes from &quot;The Software Craftsman&#58; Professionalism, Pragmatism, Pride&quot; by Sandro Mancuso'
categories: [Programming]
tags: [Sandro Mancuso]
date: 2019-08-12 08:55:00 PM UTC
---

<!-- Aug 11, 2019 08:40:00 PM Philippine Time -->
<!-- Aug 13, 2019 04:55:00 AM Philippine Time -->


[Arcanys](https://www.arcanys.com), the company I currently work for, has this mini-library, which is great because I don't have to buy some very good books to be able to read them. I can just borrow from our library. 

A few weeks ago I borrowed the book ["The Software Craftsman"](https://www.bookdepository.com/Software-Craftsman-Sandro-Mancuso/9780134052502?a_aid=jflaga). Here are some great quotes from the book:

## Part I

> Agile does **not solve** any problem, it **exposes** them.

> Effective processes work from the premise that, in order to achieve quality, we need to provide a great feedback system, at all levels.



<!--more-->

<!-- Ch 3 -->

> Software Craftsmanship is a better metaphor for software development than software engineering.

> ... do not think that just experienced professionals should write blogs. All software developers should have their own blogs, regardless of how much experience they have. We should all share our experiences and findings and help to
create a great community of professionals. Sometimes we may think that we are
not good enough or do not have much to say. We may think that we don’t have
an original idea and no one will read our blog anyway. First of all, **we should treat our blog as a record of our own learning and progression --- a history of our thoughts, ideas, and views of the world over our careers. We should not worry too much about what other people will think about it.** We should first
write it for ourselves. Even if developers more experienced than us have written
about the subject many times before, it is worth writing whatever we are currently learning anyway. Every year there are thousands of new developers joining our industry and they will need to learn many of the things we are learning
now. Maybe for them, our blogs will be very useful since we will be writing
them from the perspective of a beginner. **Do not worry about being judged by more senior developers because that is not going to happen.** Whenever we
Google for something and the first link we click leads to something we already
know, we just jump to the next link. All developers should appreciate the effort
that other developers make to write and share their views with the rest of the
world, for free.

<!-- copied from http://ptgmedia.pearsoncmg.com/images/9780134052502/samplepages/9780134052502.pdf -->


<small>_(Yehey!)_</small>



> When teaching, we are forced to **structure our thoughts**, making us really understand our quite often half-baked ideas so we can make someone else understand them.


> It can be a lot **faster** to learn something from someone with more experience than trying to learn on our own.

<!-- 
> When I hear developers saying that their code is good, that they know what they are doing, and that they don't need to write tests, I always say that they are being egocentric and selfish. Software projects are not about a single developer. Software projects are not about you. What is easy or clear for one developer may not be so easy or clear for other developers on the team, and as the system grows, everyone will pay the price of these smal selfish decisions.

> It is easy to say that a piece of code is badly written. It is easy to complain or even laugh. But the question is: are you good enough to make it better?
 -->

> Practices must be backed by values, which are shared by all members of the team, to be efficient... But saying you have values is not enough. We recognize someone's values according to his or her actions and not by what they say. Practices are the validation of values. Practices and values have different scopes. XP practices are specific to software projects. **Values are things you live by.**

> Trying to _sell_ technical practices for what they are is pointless. It doesn't work. Don't focus on the practices themselves when trying to convince managers or team members to adopt them. **Focus the discussion on the benefit they bring** and how they compare to the practices they currently have.

> Although TDD has "test" in its name, TDD is actually a design practice. **When test-driving our code, it becomes very difficult to write complex code.** First, because we write just enough code to satisfy the requirements --- represented as tests --- we discourage overengineering and big design up front (BDUF). Second, whenever our code becomes a bit too complex and bloated, it also becomes difficult to test. Complexity in our code and bad design choices are highlighted by the complexity in maintaining and writing new tests. These tests lead us to re-analyze the design of our code and refactor to make it simpler.



## Part II

> Recruiting when you are desperate will make you bring the wrong people to your organization...

> Once we know something, we tend to forget how much time we spent learning it.

> By nature, algorithms are procedural and better implemented in a functional style.

> Switch projects for an iteration: ... Pairing with developers from different teams exposes developers to different technologies, techiniques, and ways of working and thinking.

> You just need **two people** to start a community, internal or external.

> You don't need to change every person to make the organization more effective or a better place.

> The most efficient way to inject passion into a team, and help them embrace different ways of working (or different technologies), is to **lead by example**. Many developers are not comfortable working in different ways, even if they feel that it could be a good idea. Having someone who is not necessarily experienced but has enthusiasm leading the way may be what is needed to ignite a whole team.

> It is not difficult to bring a culture of learning into an organization. The only thing needed is a passionate developer willing to start it. Stop finding excuses and be this developer.

<small>_(I have an excuse: writing these quotes here so that you, the one reading this, will have no excuse. :smiling_imp:)_</small>

> Many developers suffer from what Kent Beck calls **_adolescent surety_**. They think they have the secret formula for delivering great software and nothing else is worth considering.

<small>_(That's me. :grin:)_</small>

> There are a few things you must have if you really want to change things around you. The most important one is **courage**.

> We can't change anything if people don't trust us. As a developer, the best way to **build trust** (with fellow developers, delivery managers, or clients) is by consistently delivering quality software.

> ... You can speed up their learning curve if you go through the learning curve yourself and teach them.

> It is important to **include people in the decision-making process**... _iteration boundary trick_: "Let's try this approach for one iteration and then we revisit."

> ... **always speak the truth**... just be careful not to be an asshole.

<small>_(How not to be an asshole?)_</small>

> Refactoring for the sake of refactoring is a waste of time. Unless you have nothing else to do, there is no point in opeing a piece of code that you don't need to change and spencing days refactoring it. The Boy Scout rule says "leave the campground cleaner than you found it," not "clean the whole campground until it is so clean that you could lick the floor."

> Good practices and processes are good until we find better ones to replace them. That's the same for programming languages and tools... 
<br /><br />
As craftsmen, we should not believe that TDD and all the XP practices are the best practices we will ever have...
<br /><br />
Don't call people unprofessional just because they don't use certain practices. Ask them what practices they use instead and then compare the benfits their practices have over the practices you use.

> As professionals, we need to understand that a sofware project is not about us and our egos. The I-know-what-I'm-doing-and-I-don't-need-to-write-tests attitude is selfish and arrogant. Even if that were true, the project is not about you. It's not about one or two great developers. We need to think about who is going to maintain the software when we leave. We need to think about how difficult it will be for the company to keep evolving that software. **If the value we add to a project is conditional to our continuous involvement in the project, it's not value. It's a point of failure.**

> The best line of code is the one you don't write.

<small>_("No code!")_</small>

> [p. 218: Four Rules of Simple Design --- Kent Beck version and J.B. Rainsberger version]

> Design patterns are, and should always be, part of our toolkit. However, that doesn't mean we should always use them.

> One of the best ways we have to help the business to achieve their goals is to be able to change the code almost as quickly as they change their minds.

> My advice is to stay in a job for as long as it remains aligned with our individual career aspirations.

> Companies benefit from unhappy people leaving their jobs; it creates opportunities for them to bring in new people, with fresh ideas and more energy and the willingness to challenge the status quo and do a great job.

> The values you have are defined by your actions, not your words.

> Craftsmanship versus XP: Craftsmanship is an idealogy. XP is a methodology.




## More Sandro Mancuso quotes <small>I found while googling for places where I can copy these quotes so I don't have to type them all</small>

> Code coverage is a side effect, not a goal. The only time I find the metric useful is when I’m working with legacy and want to have an indication if there are tests for the part of the code I’m interested in. --- from [this tweet](https://twitter.com/sandromancuso/status/1091701221390516224)