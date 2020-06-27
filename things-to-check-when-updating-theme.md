---
layout: page
title: 'Things to check when updating theme'
date: 2020-06-27 10:30:00 AM UTC
background: '/images/Jboy2017-Real-2(200x200).jpg'
---

<!-- first: June 27, 2020 06:30:00 PM Philippine Time -->

# date: UTC

refer to ["Problem: Different URLs for different timezones"](http://127.0.0.1:4000/2017/04/08/problems-encountered-with-jekyll-powered-blog#different-url-for-different-timezone)

---

# blockquotes

> blockquotes

---

# messages

<div class="message" markdown="1">
messsage inside `div` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</div>

<p class="message" markdown="1">
    messsage inside `p` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</p>

do not use span with `class="message"`; use `div` tag and `p` tag only:

<span class="message">
    messsage inside `span` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</span>

<p class="message message-compressed" markdown="1">
    messsage-compressed inside `p` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</p>

<p class="message message-compressed float-right" markdown="1">
    messsage-compressed inside `p` tag - `float-right` - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</p>

<p class="message message-compressed float-left" markdown="1">
    messsage-compressed inside `p` tag - `float-left` - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</p>

<div class="clearfix"></div>

---

# code inside single backticks

The template for use case handler `ICommandHandler` ...

The property `ICommandResult.Succeeded` ...

---

# code inside triple backticks

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

should have `box-shadow` to make it look good to the eye:

``` css
div.highlighter-rouge {
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
}
```

---

# images

![Argue to learn, not to win - from GOOSGBT](/images/2017/Argue-to-learn-from-GOOSGBT.jpg)

should not overflow to the right and should have `box-shadow` to make it look good to the eye:

``` css
img {
    max-width: 100%;
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5)
}
```

---

# comments section: disqus

---

# analytics

