---
layout: page
title: 'Things to check when updating theme'
date: 2020-06-27 10:30:00 AM UTC
background: '/images/Jboy2017-Real-2(200x200).jpg'
---

<!-- first: June 27, 2020 06:30:00 PM Philippine Time -->

# date: UTC

refer to ["Problem: Different URLs for different timezones"](http://127.0.0.1:4000/2017/04/08/problems-encountered-with-jekyll-powered-blog#different-url-for-different-timezone)


# blockquotes

> blockquotes


# messages

<div class="message">
    messsage inside div tag
</div>

<p class="message">
    messsage inside p tag
</p>

<span class="message">
    messsage inside span tag
</span>

<p class="message message-compressed">
    messsage-compressed inside p tag
</p>

<p class="message message-compressed float-right">
    messsage-compressed inside p tag - float-right
</p>

<p class="message message-compressed float-left">
    messsage-compressed inside p tag - float-left
</p>

<br />
<br />
<br />

# code

``` csharp
public interface ICommandHandler<TCommand>
{
    ICommandResult Handle(TCommand command);
}

public interface CommandResult
{
    bool Succeeded { get; }
    Error Error { get; }
    IEnumerable<Error> ErrorList { get; }
}
```


# images

![Argue to learn, not to win - from GOOSGBT](/images/2017/Argue-to-learn-from-GOOSGBT.jpg)

``` css
img {
    max-width: 100%;
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5)
}
```


# comments section: disqus


# analytics

