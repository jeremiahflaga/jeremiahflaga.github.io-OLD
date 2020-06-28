---
layout: page
title: 'Things to check when updating theme'
date: 2020-06-27 10:30:00 AM UTC
background: '/images/Jboy2017-Real-2(200x200).jpg'
---

<!-- first: June 27, 2020 06:30:00 PM Philippine Time -->

**NOTE:** your additional CSS can be found in `/_sass/additional-styles.scss`

---

# date: UTC

refer to ["Problem: Different URLs for different timezones"](http://127.0.0.1:4000/2017/04/08/problems-encountered-with-jekyll-powered-blog#different-url-for-different-timezone)

---

# blockquotes

> blockquotes

---

# code inside single backticks

The template for use case handler `ICommandHandler` ...

The property `ICommandResult.Succeeded` ...

---

# code inside triple backticks

{: .mt-5 }
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

# emojis

:smile: :smiling: :laughing: :blush: :+1:

should not have box-shadow

``` css
img.emoji {
    box-shadow: none;
    display: inline;
    margin: 0;
    border-radius: 0;
}
```

---

# messages

<div class="message" markdown="1">
messsage inside `div` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</div>

<p class="message" markdown="1">
    messsage inside `p` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</p>

`class="message"` inside `span` is not being used in this site, so it's okay even if it does not look good

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

# comments section: disqus

---

# analytics


---

# links

## [jeremiahflaga.github.io](/)

[jeremiahflaga.github.io](/)

---

# youtube videos

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6S8RCKRIoY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

should not have box-shadow

``` css
iframe[src*="https://www.youtube.com/embed/"] {
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
}
```