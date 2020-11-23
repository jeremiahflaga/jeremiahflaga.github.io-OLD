# Anti-résumé



<!-- [I also think that antiresume matches with this idea of TDD, or writing tests] -->

<!-- 
<small>_(Note: I'm not a sexist. In the following section(s), I'm using the terms 'his', 'him', and 'he' to refer to any human being: male or female, old or young etc..)_</small>
-->










## **I have almost no experience deploying things and scaling things**

... But a colleague of mine once said that it is just almost the same as clicking the build button in an IDE. 

So if I am tasked of deployment and no one else is to guide me, I will just google how to do it. :smile:


<!--
## **I only have basic knowledge about functional programming**

All I know about functional programming is that there is no assignment statements and that there is heavy use of recursion.
-->



## **I know nothing about estimating**

Today, perhaps the only method I have in mind to honestly estimate the time to be spent to finish a project is to do at least two of the most important features of the app, then use the time spent on those two features to estimate the time that will be spent on the whole project.

But even then, my estimates might still be far from accurate.


## **I have almost no knowledge about software security**

When it comes to security, I only know about SQL injection, and I know that it can be prevented by not using string concatenation when building SQL queries. But even though I know that using string concatenation is wrong when building SQL queries, I still do it sometimes --- when I'm tired or bored, and there is an existing code in the project that I can just copy-and-paste. :grinning: But if you already have a set of guildelines for coding which I need to follow, or you practice code review or pair programming, then this will not be a problem. 

<!-- And I will be happy to work with a teammate who will correct me if I do not follow correct guidelines. -->

Oh, I also know about _not_ storing passwords in plaintext, but instead hashing them before storing them. And I also know about not sending _forgotten passwords_ to users (because, of course, if we do not store passwords as plaintext, we will not know what password to send to them), but instead send them link to a form/resource that will let them change their passwords.

I also read in the past about Cross-site Scripting (XSS) and Cross-site Request Forgery (CSRF), but I have to review them to bring them back to mind.

<!-- 
but I already forgot what they are. All I know is that, to prevent these attacks in an ASP.NET MVC application, we only have to put a _specific_ Attribute in our Controller class or in a function inside a Controller class (I already forgot what that Attribute is), and that we must be cautious about the properties in our Model (or view model) that we expose to our users.
 -->

But I have a copy of the book ["Foundations of Security: What Every Programmer Needs to Know"](https://www.bookdepository.com/Foundations-Security-Christoph-Kern/9781590597842?a_aid=jflaga) (but I have read only some chapters from it). So if you need me to become familiar with software security when working at your company, I can start with this book (or with any material you can recommend me to consume).



<br />


### _**More next time...**_


<br />


**_Thank you for your time!_**


<br />



<!--

## **It's hard for me to stay long with a team who**

I cannot stay long with a team when I sense that it's members have the kind of attitude that sounds  like "I might not be the one to maintain this project, why should I care if its codebase is clean or not!"

Of course I understand that there are existing projects that are very messy already so that is't hard to clean it up.



## **I sometimes get frustrated when I know that something is wrong and I cannot do anything about it**

I will get frustrated sometimes (oftentimes perhaps) when I know that we can do better with things but the team are not doing anything about it.


I would be great if there is a kind of system where a programmer can be able to raise his concerns about the existing codebase if he thinks that there is something wrong in the codebase... and that we do something about it, or at least plan to do something about it... to fix it later... because if a team does not do this, it seems to me that we are being dishonest to our employer, and to our clients.



## **I sometimes think about justice when I see messy code...**

> "Let those who produced messy code be the ones to fix them. Don't give them another new project to start until they fix their mess!... Do you really want them to create another mess?"

Well, I am very bold in saying that because I never experienced being involved in a software project _from the very start of the project_. I only experienced working on software projects that are in the maintenance phase already.

So when I see code that looks like it was abandoned by their creators, I'm torn inside. --- _Why!?... Don't they care?_

But perhaps I should stop thinking like this. I should be [empathetic](http://chadfowler.com/2014/01/19/empathy.html) towards other programmers because I do not know what they have been through while working on the projects with messy codebase. Perhaps they had tight deadlines, and that is the reason why they did not give time cleaning up their mess.

Okay...

I think I can help in fixing messy software... But I'm not [Micheal Feathers](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga), so if the mess is very big already, you might need someone more skillful than me to help fix it. If the mess is still small, I might be able to help (I hope).



## **I sometimes get frustrated when I know that the codebase of the project I am working on is messy and I cannot do anything about it (and my teammates are not planning to do anything about it)**

> "Do unto others what you want others to do to you."

That's what Jesus said.

If you don't want to hear it from Jesus, hear it from Confucius who said something quite similar:

> "Do not do to others what you do not want others to do to you"

Our master programmers have their own version (or application) of this motto:

> "Always leave the campground cleaner than you found it."

They call that "The Boyscout Rule".

I want to live by that kind of rule. If 
-->


<!--
## **I intend to separate the business rules from the other parts of a software system**

Being influenced by people who promote DDD _(Note: I only know a little about DDD)_, and Clean Architecture, I am the kind of person who _would like to_ focus more on learning about the _domain_ of the business... and to focus on trying to model that domain instead on focusing on the specifics of the frameworks or libraries being used in the software system I am involved in.

If I am hired, I plan to spend at least half of my time (during the first few weeks) learning about the business and half of the time learning about the specific frameworks/libraries/technologies your team is using.

And if your domain is not googleable, or there are no books available to learn about it, then I might need _lots of guidance_ from my fellow programmers who are already familiar with the domain, or from the business people themselves, while learning about your domain.
-->

<!-- 

## **I work very well, even during trying hours, _only if_ there is rapid feedback from the system I am working on**

I'm a patient kind of man, I think, but I sometimes get impatient especially when it takes so much time to fix a problem which I think should be very easy to fix.

This impatience will be bolstered when it takes too long a time before I can be able to see whether the code I just wrote works or not :laughing: --- this is what they call a _slow feedback loop_.

A very long compilation time might be one of the reasons for a very slow feedback.

This problem can be solved if your system allows me to write fast (not slow) unit tests. So this will not be a problem if I can write fast unit tests.

If it takes a lot of time to compile your project, and you want me to be involved in that project, I would like to suggest to the team working on that project to give time to make the critical parts of that project to be unit testable, so that if there are bugs in those critical parts it will not take too much time to fix it.
 -->

<!-- 
If the unit tests of your system run very slow, you might be interested in hiring me to _help_ fix the running time of your unit tests. Please note that I used the word _"help"_ because I will be needing the help of _at least_ one of your developers who is already very familiar with the system whose unit tests' running time you want to fix. It might be possible for me to fix it alone, but it will take way too much time and will give me (and you) too much unecessary frustrations if you allow me to do it alone.
 -->



<!-- 
But, as every adult might have already realized, we do not always get justice in this life. So I intend to help those who experience injustice by helping them fix the mess that was the result of the original creators' abandonment of the system.

But if the creators of the system did not abandon their work, and is still working on it right now, and they know that their project is a mess but do not know how to fix it, I am also willing to _help_ clean up the mess.

I think I can help in fixing messy software... But I'm not [Micheal Feathers](https://www.bookdepository.com/Working-Effectively-with-Legacy-Code-Michael-Feathers/9780131177055?a_aid=jflaga), so if the mess is very big already, you might need someone more skillful than me to help fix it. If it is still small, I might be able to help.



You see, I believe in a personal Designer of the universe --- a Designer who is still cares about his creation (even though his creation do not seem to care about him)

But of course in the real world, that does not always happen.
-->



<!-- 
## **Cannot work on Android projects**

I cannot work on Android projects for my next job because I promised my current employer that I will not be working on an Android project in my next job.


## **Cannot work on Android projects, except...**

I cannot work on Android projects for my next job because I promised my current employer that I will not be working on an Android project in my next job... except when I will be hired in _helping_ decouple your business logic layer from the other parts of you software system, because in that case, I will not be _directly_ involved in working on the _Android-framework-specific_ parts of project.
 -->



<!--
## **I don't know how to do these CQRS and Event Sourcing things**

Even though I think this CQRS thing will have a very important role in software development in the next few years, I do not know how to do this yet. But I will learn about it after I finish the other things that I need to learn.
 -->



<!--
## **I know almost nothing about network programming**

I learned a little bit about this sockets thing in Uncle Bob Martin's ["The Craftsman Series"](/memorabilia/books/the-craftsman-series/), and the 1 hour video ["Network Programming Seminar"](http://cs50.tv/2007/fall/#about,seminars) of CS50 Fall 2007. I will learn more later if time permits.
-->



<!-- 
--------------------
 
<sup id="footnote-1">[[1]](#footnote-indicator-1)</sup> <small>_(I have already forgotten most things I read from this book. But if you have also read this book, you can use some quotes in it to correct me someday when I'm already working with you and I did something wrong. But, of course, if the quote you will use is in disagreement with some of my core values, then I might try to defend myself. :smile: I hope you will also listen during those times when I try to defend myself.)_</small>
 -->


<!--






### I'm the kind of person who is not comfortable accepting praise if I am not very confident about the project I'm working on. 


### Wehn I get frustrated

when i get frustrated I sometimes think this: let those who make the mess be the ones to clean the mess. DOn't let them work on new projects unless they first clean up the mess that they made. If you want other people to clean up the mess that someone else made, be sure to pay him more that what you paid the one who made the mess.






It gives me this feeling that the memebers of the team do not seem to care.

It would be great if your company have this kind of system where I can be able to voice my opinion, for example, when I see something that we are doing which is bad. 

This kind of system will breed trust to each member of the team...


I don't want to play the blaming game because I don't know what my coleauges, managers or employer in the past were experiencings when they decided to do things the way they did them.


If you want to hear examples about this please ask me during the interview.


I understand that there are times when I don't need to ask for permission when I do something which I think will benefit my employers or my coleagues, but I just don't want to play politics, so I have to ask for permission about these things before I get hired.


Examples:

not decoupled code that I need to test

using RxJava










### I almost wanted to give up on software development in the past

In my first job, I was very excited

  -  leaned about twitter ...


In my second job


So i will not work with you unless I'll be working in a new project where I will have influence about he structure/architecture of the project, or I will be working on an existing project that does not look like it was abandoned by it's designers, or I will be fixing an existing mess.... AND I can mentor the next person that will someday take care of the project I am currently working on (Or the current best designers in you company will mentor me)


Being influenced by people who promote DDD (Note: I only know a little about DDD), I already got past being a framework-focused developer to being a business-model-focused developer.

If your team is still in the stage of being framework-focused, I might not work well with them. Except maybe if you want someone to be influential into making your team business-focused.




### I have a hidden agenda

I want to help spread this idea of professionalism in the sovtware development comunity.



--------------------


**_If you are looking for my [Resum&eacute;](/resume), you can view it [here](/resume)_**



 -->