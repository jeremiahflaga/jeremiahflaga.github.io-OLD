---
layout: post
title: 'Encountering "Vertical Slice Architecture"... Is this the same as Featurized Architecture?'
categories: [Programming]
tags: [Vertical Slice Architecture, Featurized Architecture, Jimmy Bogard, Matthias Käppler]
date: 2019-05-10 3:15:00 AM UTC
published: false
---

<!-- May 10, 2019 11:15:00 AM Philippine Time -->

While browsing through Scott Hanselman's blog, I came accross [a post](https://www.hanselman.com/blog/ExampleCodeOpinionatedContosoUniversityOnASPNETCore20sRazorPages.aspx) where something called [_"Vertical Slice Architecture"_ by Jimmy Bogard](https://jimmybogard.com/vertical-slice-architecture/) is mentioned. (Anything related to software architecture easily catches my attention these days :smile:) 

I clicked on the [link](https://jimmybogard.com/vertical-slice-architecture/) and read the article...

> "A traditional layered/onion/clean architecture is monolithic in its approach..."
<br /><br />
> !["The Clean Architecture" digram of Uncle Bob Martin](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)


<!--more-->


That diagram is from Uncle Bob Martin's ["The Clean Architecture"](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) blog post! Is Jimmy Bogard critisizing Clean Architecture?

Hmmmm...

I need to learn more... So I watched his talk titled ["SOLID Architecture in Slices not Layers"](https://www.youtube.com/watch?v=wTd-VcJCs_M)...

One of the interesting things in that talk is his comparison of "Traditional N-Tier" and "DDD-style N-Tier architecture". Regarding that, he said this:

> "... looks like they just changed the name of things"
<br /><br />
![Traditional N-Tier vs DDD-style N-Tier by Jimmy Bogard](/images/2019/traditional-vs-ddd-style-n-tier-by-jimmy-bogard.png)

That statement and picture talks to my unconscious and says _"Learning more about DDD is not that important these days. You have other more important things to learn."_ Okay...

But that is not the point of this blog post... :smile:

Okay back to my point...

Through the article and the talk, I learned that Vertical Architecture means slicing the project be feature instead of by 

But I heard somthing like that about a year ago... from the talk ["Going Reactive, An Android Architectural Journey" by Matthias Käppler](https://www.youtube.com/watch?v=R16OHcZJTno)

... _Featured Architecture!_ ...

Are they the same?

This is what Matthias Käppler said about Featurized Architecture:

> "About a year later. . . We came to realize that his platform driven approach that we were taking didn't work very well anymore. So we were pretty much all focused around technology; so we had an Android team, we had an iOS team, we had a web team; and this didn't align well with the emphasis that we wanted to put on particular features that drive business value.
<br /><br />
"So we actually moved to a model where we would **split up our organization around features rather than around technology**."

So this Featurized Architecture is something which is not just about the structure of a software project but includes the structure of an organization or teams working on a project.






collective ownership of the codebase



In the talk, he gave some code samples. On first look the examples look the same as the [Clean Architecture examples](https://jeremiahflaga.github.io/2017/08/16/clean-architecture-sample-projects/) I found more than a year ago, except that he is directly using DbContext in his UseCase or Interactor objects...

``` csharp
    public class StudentCreateHandler
    {
        private readonly SchoolContext this.db;

        public StudentCreateHandler(SchoolContext db) => this.db = db;

        public void Handle(StudentCreateCommand message)
        {
            var student = new Student() 
            {
                . . .
            }
            this.Students.Add(student);
        }
    }
```

... while the Clean Architecture projects uses repository interfaces... 

``` csharp
    public class StudentCreateHandler
    {
        private readonly IStudentRepository repository;

        public StudentCreateHandler(IStudentRepository repository) => this.repository = repository;

        public void Handle(StudentCreateCommand message)
        {
            var student = new Student() 
            {
                . . .
            }
            this.repository.Add(student);
        }
    }
```

If it is directly using DbContext then it must not have unit tests!

Looking on the example project on GitHub, it does not have unit tests. But it has what he calls [Itegration Tests](https://github.com/jbogard/ContosoUniversityDotNetCore/tree/master/ContosoUniversity.IntegrationTests) (which I believe is is the same as what others call [Functional Test or Acceptance Test or End-to-End Test](https://www.obeythetestinggoat.com/book/chapter_02_unittest.html))





https://github.com/jbogard/ContosoUniversityDotNetCore




I think this is the same as what 

Matthias Käppler-GOTO 2015 • Going Reactive, An Android Architectural Journey

https://www.youtube.com/watch?v=R16OHcZJTno

I watched that video when I was learning about RxJava (for an Android project) more than a year ago.



Øredev 2016 - Jimmy Bogard - SOLID Architecture in Slices not Layers

From <https://www.youtube.com/watch?v=wTd-VcJCs_M> 





The implementation in code seems similar to the many examples of Clean Architecture...


Perhaps the _Vertical_ in the name "Vertical Achitecture" does not necessarily mean another method of implementing things but just to change the mindset or focus of programmers.... just like _Clean_ in Clean architecture




[Clean Architecture sample projects](https://jeremiahflaga.github.io/2017/08/16/clean-architecture-sample-projects/)




The focus of Jimmy Bogart's Vertical Slice Architecture is clear separation of features.


The focus of Uncle Bob's Clean Architecture is separation of business rules from I/O.

The focus of Matthias Featured Architecture is separation by features on the team level and on the code level.


![](http://www.commitstrip.com/en/2019/04/10/new-api-version/)



