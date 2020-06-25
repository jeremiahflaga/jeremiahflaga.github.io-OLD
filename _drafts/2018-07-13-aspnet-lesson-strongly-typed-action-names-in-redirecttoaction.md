---
layout: post
title: 'ASP.NET MVC Lesson: Strongly-typed action names in RedirectToAction() method'
categories: [Programming]
tags: [ASP.NET MVC]
date: 2018-07-05 10:00:00 AM UTC
published: false
---

<!-- July 5, 2018 06:00:00 PM Philippine Time -->

Magic strings are bad.

Do not use them in your RedirectToAction()


Use

```
return RedirectToAction(nameof(Index))
```

... instead of using magic strings:

```
return RedirectToAction("Index"))
```


<!--more-->
