---
layout: post
title: 'ASP.NET MVC Lesson: Be careful when passing messages between two action methods through the URL'
categories: [Programming]
tags: [ASP.NET MVC]
date: 2018-07-05 10:00:00 AM UTC
published: false
---

<!-- July 5, 2018 06:00:00 PM Philippine Time -->

You might be tempted to do something like this when you want to pass data between two action methods:

``` csharp
public ActionResult Index(MessageEnum messageEnum)
{
    switch (messageEnum)
    {
        case MessageEnum.Approved:
            ViewBag.StatusMessage = "User has been Approved."
            break;
        case MessageEnum.UnApproved:
            ViewBag.StatusMessage = "User has been Unapproved.";
            break;
        ...
    }
    
    var users = ...
    return View(users);
}

[ChildActionOnly]
public ActionResult Approve(string id)
{
    ...
    if (successful)
        return RedirectToAction(nameof(Index), new { messageEnum = ManageMessageId.Error })
}
```


The problem with that approach is that you are basically including data in the URL which is not supposed to be in there:

> `https://localhost:44444/Data?messageEnum=Approved`

See the `messageEnum=Approved` in the query string?






I have this `ExecuteThing()` action method
I want to pass this message, "The thing you want to execute was successful", from this `ExecuteThing()` action method into the `HomePage()` action method. How do I do that?

> You can store it in the database then retri

<!--more-->
