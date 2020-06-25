---
layout: post
title: 'What is a rich domain model?'
categories: [Programming]
tags: [Jimmy Bogard, MattVladimir Khorikov]
date: 2019-10-27 08:55:00 PM UTC
published: false
---

<!-- Oct 10, 2019 08:40:00 PM Philippine Time -->


<!-- 
"I write because I don't know what I think until I read what I say." --- [Flannery O'Connor](https://www.goodreads.com/quotes/315733-i-write-because-i-don-t-know-what-i-think-until)

Hmmm... Nice words!

Okay. So what is a rich domain model? -->

Whatever that is I'm sure it's not _poor_ :smile:

Lame joke...

I've been hearing about this rich domain model thing for years by now but because I have been distracted with these other "more important" things (which included procastination and laziness of course) I never had a chance to dig deeper into this topic... until last summer this year.

<!--more-->

Uhhhmm... Ooops... My notes says that I watched [Jimmy Bogard's "Crafting Wicked Domain Models"](https://www.youtube.com/watch?v=UYmTUw5LXwQ) last December 2017, that's about two years ago. That only shows how much time I need to wrap something around my head.


But don't mind that. Let me pretend that I started learning about rich domain models last April, okay. So what did I learn?

I learned that I need to ["write because I don't know what I think until I read what I say. [Flannery O'Connor]"](https://www.goodreads.com/quotes/315733-i-write-because-i-don-t-know-what-i-think-until).

I mean... 

<!-- sorry about that. I just have to defend why I am writing from time to time because I'm not really good at writing. I'm just writing mostly for me. -->

I learned that rich domain models are models which contain not just data but also methods (or behavior). 

I'm going to give an example. I will first show a piece of code whose domain model is not "rich". They call them "anemic domain models":

```
public class Student
{
    public Guid Id { get; set; }
    public string Name { get; set; }

    public IList<Course> Courses { get; set; }
}

public class Course
{
    public Guid Id { get; set; }
    public string Name { get; set; }
}

```

When we have a domain model like that, we usually have corresponding Service classes which contain the behavior we want for our models, like this:

```
public class StudentServices
{
    public void AddCourseToStudent(Guid studentId, Course course) 
    {
        var student = studentRepository.Get(studentId);
        student.Courses.Add(student);
        // save student to database
    }
}
```

Notice that we can put that `AddCourseToStudent()` method inside the `Student` model, like this:


```
public class Student
{
    public Guid Id { get; set; }
    public string Name { get; set; }

    public IList<Course> Courses { get; set; }
    
    public void AddCourse(Course course) 
    {
        student.Courses.Add(student);
        // save student to database
    }
}





But these methods are not just ordinary methods. They are methods that describes a business process in the real world. I'm going to give an example later.





<!-- So is "rich" equatable to "wicked"? It's not. I think the reason why Jimmy Bogard used "wicked" in his title is because he knows that we humans are inclined to think that being wicked is being cool. :laughing: -->


It turns out that my joke that rich domain models are not "poor" is not a joke after all because it is a truth. Rich domain models are not poor on methods.