---
layout: post
title: Some problems I encountered with my Jekyll powered blog -- and their solutions
category: [Miscellaneous, Evolving Notes]
tags: [Jekyll, Blogging, Timezones]
date: 2017-04-08 03:30:00 PM UTC
dateLastUpdated: 2021-05-23 06:00:00 AM UTC
---

<!-- April 8, 2017 11:30:00 PM Philippine Time -->
<!-- UPDATED: May 23, 2021 02:00:00 PM Philippine Time - added line numbers -->

<div class="message" markdown="1">

This post will be one of those [_"evolving publications"_](https://martinfowler.com/bliki/EvolvingPublication.html), as Martin Fowler calls them.

But I will call them "_evolving notes_" here in my blog, because most of my posts are not really publications but are just my personal _notes_ on what others have published.

</div>

-----

This post contains some of the problems I encountered while adding more functionality or fixing some errors to my Jekyll-powered blog.

<!--more-->

{:#different-url-for-different-timezone}
## Problem: Different URLs for different timezones {#different-urls-for-different-timezones}

A few days ago, I noticed that Jekyll generates different URL's for different timezones.


For example, I have this as my publish date:

```
date: 2017-04-05 05:05:00 PM UTC
```

When I build my blog locally, it generates this URL:

> http://127.0.0.1:4000/**2017/04/06**/the-specific-generic-cycle-of-uncle-bob/

...because in my local machine the date is **April 6, 2017 01:05:00 AM**.

But when I publish it to GitHub Pages, it generates a different URL:

> http://127.0.0.1:4000/**2017/04/05**/the-specific-generic-cycle-of-uncle-bob/


**The solution** is to add `timezone: UTC` in `_config.yml`.

After that, when executing `jekyll serve --watch`, an error popped up.

```
jekyll 3.4.3 | Error:  No source of timezone data could be found.
Please refer to http://tzinfo.github.io/datasourcenotfound for help resolving this error.
```

I needed to install the gem `tzinfo-data`.

When I tried `gem install tzinfo-data -install-dependencies`, my machine installed it in the current folder instead of the folder where all the global gems are installed. (In my case, `C:\Ruby23-x64\lib\ruby\gems\2.3.0`)

So I googled and found that I needed to do this: `gem install tzinfo-data -include-dependencies --install-dir C:\Ruby23-x64\lib\ruby\gems\2.3.0`

That's it!


{:#add-pagination}
## Problem: How to add pagination {#how-to-add-pagination}

Just go to the docs [jekyllrb.com/docs/pagination/](https://jekyllrb.com/docs/pagination/)


{:#add-pagination}
## Adding line numbers in code blocks

[https://www.richwerden.com/2017/line-numbers-in-jekyll-code.html](https://www.richwerden.com/2017/line-numbers-in-jekyll-code.html)


{:#other}
## Some helpful stuffs from other people:

["Little Stuff about Markdown I Always Forget and have to Google"](
https://css-tricks.com/little-stuff-markdown-always-forget-google)  by Chris Coyier
