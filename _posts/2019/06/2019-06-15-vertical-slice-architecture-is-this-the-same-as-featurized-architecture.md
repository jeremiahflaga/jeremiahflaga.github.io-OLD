---
layout: post
title: 'Encountering "Vertical Slice Architecture"... Is this the same as Featurized Architecture?'
categories: [Programming]
tags: [Vertical Slice Architecture, Featurized Architecture, Jimmy Bogard, Matthias Käppler]
date: 2019-06-15 02:15:00 AM UTC
---

<!-- June 15, 2019 10:15:00 AM Philippine Time -->

After [encountering Jimmy Bogard's "Vertical Slice Architecture"](/2019/05/20/vertical-slice-architecture-is-it-incompatible-with-clean-architecture) I watched his talk["SOLID Architecture in Slices not Layers"](https://www.youtube.com/watch?v=wTd-VcJCs_M) to learn more about it. While listening to him explain what a vertical slice is, I remember hearing a similar thing before... It was by someone named Matthias... and it had "feature" in it's name.

<!--more-->

Searching my notes... I found it... It's called "Featurized Architecture":

["Going Reactive, An Android Architectural Journey (GOTO 2015)"](https://www.youtube.com/watch?v=R16OHcZJTno&t=195) by Matthias Käppler.

<iframe width="560" height="315" src="https://www.youtube.com/embed/R16OHcZJTno?start=195" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br />

I watched that video when I was learning about RxJava (for an Android project) more than a year ago. In about 3:18 in the video, he said something about what he calls "Featurized Architecture". This is what he said:


> About a year later... We came to realize that this platform driven approach that we were taking didn't work very well anymore. So we were pretty much all focused around technology; so we had an Android team, we had an iOS team, we had a web team; and this didn't align well with the emphasis that we wanted to put on particular features that drive business value.
<br /><br />
So we actually moved to a model where we would split up our organization around features rather than around technology.
<br /><br />
Featurized instead of platform driven approach (where we have android teams, ios teams, web teams)

<center>
<img src="/images/2019/featurized-architecture-featurized-layers.png" alt="Featurized Architecture" >
</center>

Seems like what he calls "Featurized Architecture" is the same as what Jimmy Bogard calls "Vertical Slice Architecture". Only that in the project he was involved in, this Featurized thing has a larger scope because it involved vertical divisions in teams and not just vertical divisions in the codebase.

> "We moved to a model where we would split up our organization around features rather than around technology."

It seems like if a team employs this "Featurized" team structure then everyone has access to all codebases that team is involved in --- the backend people has access to the frontend codebase and vice versa. This seems to agree with what they call ["Collective ownership of the codebase"](https://martinfowler.com/bliki/CodeOwnership.html).


This "Featurized" approach is stuck in my mind because during the time I learned about this I was involved in a project where I was encountering bugs where I was not sure if the bug is coming from the API/backend side or from our side (the Android frontend).

I was thinking, _"If only we had this featurized approach in our team I would have access to the codebase of the API/backend and I can easily see if the error is coming from there or not."_ :laughing: (My aim for wanting to look at the backend code was not to throw blames on the backend people of course. :laughing: It was to see if I can do something to help fix the problem.)


[!["New API version" from CommitStrip](https://www.commitstrip.com/wp-content/uploads/2019/04/Strip-Ba-jte-lai-dit-650-finalenglishV3.jpg)](http://www.commitstrip.com/en/2019/04/10/new-api-version/)



> "At the root of every seemingly technical problem is a human problem." - Eric Ries (from ["The Lean Startup"](https://www.bookdepository.com/Lean-Startup-Eric-Ries/9780670921607?a_aid=jflaga))


:grin:
