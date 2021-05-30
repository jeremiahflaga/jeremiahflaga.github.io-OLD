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
> 
> > blockquotes
> 
> blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes blockquotes
---

# emojis

:smile: :smiley: :laughing: :grin: :grinning: :+1: :blush: :grimacing: :bow: :disappointed: :worried: :innocent: 

should not have box-shadow

{: .small }
``` css
// CSS for removing box-shadow in emojis
img.emoji {
    box-shadow: none;
    display: inline;
    margin: 0;
    border-radius: 0;
}
```

More emojis at [https://phatblat.com/2016/01/03/jemoji.html](https://phatblat.com/2016/01/03/jemoji.html)



-----

# code inside single backticks

The template for use case handler `ICommandHandler` ...

The property `ICommandResult.Succeeded` ...



-----

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

{: .small }
``` css
// CSS for adding box-shadow
div.highlighter-rouge {
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
}
```



-----

# code inside {% raw %}{% highlight %}{% endraw %}, and with line numbers

**NOTE: Using {% raw %}{% highlight %}{% endraw %} syntax throws an exception when you're not online:**

`Liquid Exception: Liquid syntax error (line 177): Unknown tag 'highlight' in <filename>`

Without line numbers:

{% highlight javascript %}
function some(code) { /*goes here*/ }
let x = 21;
{% endhighlight %}

With line numbers:

{% highlight javascript linenos %}
function some(code) { /*goes here*/ }
let x = 21;
{% endhighlight %}




-----

# images

![Argue to learn, not to win - from GOOSGBT](/images/2017/Argue-to-learn-from-GOOSGBT.jpg)

should not overflow to the right and should have `box-shadow` to make it look good to the eye:

{: .small }
``` css
// CSS for adding box-shadow
img {
    max-width: 100%;
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5)
}
```


---

# messages

<div class="message" markdown="1">

messsage inside `div` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message

```
code
here
```

</div>

<p class="message" markdown="1">

messsage inside `p` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message

```
code
here
```

</p>

`class="message"` inside `span` is not being used in this site, so it's okay even if it does not look good

<span class="message">
messsage inside `span` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message
</span>

<div class="message message-compressed" markdown="1">

messsage-compressed inside `div` tag - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message

```
code
here
```

</div>

<div class="message message-compressed float-right" markdown="1">

messsage-compressed inside `div` tag - `float-right` - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message

```
code
here
```

</div>

<div class="message message-compressed float-left" markdown="1">

messsage-compressed inside `div` tag - `float-left` - a long long long long long long long long long long long long long long long long long long long long long long long long long long long long  message

```
code
here
```

</div>

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

should have box-shadow

{: .small }
``` css
// CSS for adding box-shadow in videos
iframe[src^="https://www.youtube.com/embed/"] {  // ^ means "starts with"
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
}
```

---

# embedded pdfs

should have box-shadow

{: .small }
``` css
// CSS for adding box-shadow in PDFs
embed[src$=".pdf"] { // $ means "ends with"
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
}
```

<embed src="/files/music/2020-06-28-the-girl-from-ipanema.pdf" width="100%" height="300px"/>


---


# be sure that [Bootstrap](https://getbootstrap.com/) is working

**code block with `.small` vs noromal**

{: .small }
``` shell
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
```

vs

``` shell
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
```


**background color**

<div class="p-3 mb-2 bg-primary text-white">.bg-primary</div>
<div class="p-3 mb-2 bg-secondary text-white">.bg-secondary</div>


**accordion/collapse**
<div class="accordion" id="accordionExample">
  <div class="card">
    <div class="card-header" id="headingOne">
      <h2 class="mb-0">
        <button class="btn btn-link" type="button" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          Collapsible Group Item #1
        </button>
      </h2>
    </div>

    <div id="collapseOne" class="collapse show" aria-labelledby="headingOne" data-parent="#accordionExample">
      <div class="card-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
  <div class="card">
    <div class="card-header" id="headingTwo">
      <h2 class="mb-0">
        <button class="btn btn-link collapsed" type="button" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Collapsible Group Item #2
        </button>
      </h2>
    </div>
    <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordionExample">
      <div class="card-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
  <div class="card">
    <div class="card-header" id="headingThree">
      <h2 class="mb-0">
        <button class="btn btn-link collapsed" type="button" data-toggle="collapse" data-target="#collapseThree" aria-expanded="false" aria-controls="collapseThree">
          Collapsible Group Item #3
        </button>
      </h2>
    </div>
    <div id="collapseThree" class="collapse" aria-labelledby="headingThree" data-parent="#accordionExample">
      <div class="card-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
</div>