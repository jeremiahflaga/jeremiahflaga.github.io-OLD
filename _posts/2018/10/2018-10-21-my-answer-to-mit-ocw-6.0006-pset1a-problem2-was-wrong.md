---
layout: post
title: 'My answer to Problem 2 of PSet 1A of MIT OCW 6.0002 was wrong!'
categories: [Programming]
tags: [Programming, Python, MIT OCW 6.0002]
date: 2018-10-21 03:30:00 PM UTC
---

<!-- Oct 21, 2018 11:30:00 PM Philippine Time -->

[I finished MIT OpenCourseWare 6.0001](/2017/08/05/finished-mit-ocw-6.0001/) last August 2017. After that, I tried to do [MIT OCW 6.0002](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0002-introduction-to-computational-thinking-and-data-science-fall-2016/index.htm), but I finished only the first three lectures and the first problem set. :disappointed:

More than a year later...

... a few days ago, I found out that a similar course to 6.0002 is being offered at edX - [MITx: 6.00.2x Introduction to Computational Thinking and Data Science](https://www.edx.org/course/introduction-computational-thinking-data-mitx-6-00-2x-7)

_Wow!_

_I'm going to take this course! ... So that I will have an idea on what data science is all about._

<!--more-->

I enrolled... watched the video lectures... answered the finger exercises (they are short exercises given between each video lecture)...

Then came Problem Set 1.

> A colony of Aucks (super-intelligent alien bioengineers) has landed on Earth and has created new species of farm animals! The Aucks are performing their experiments on Earth, and plan on transporting the mutant animals back to their home planet of Aurock. In this problem set, you will implement algorithms to figure out how the aliens should shuttle their experimental animals back across space.
<br />
...


_Hmmm... seems like the same problem as in [pset 1 of 6.0002](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0002-introduction-to-computational-thinking-and-data-science-fall-2016/assignments/) last year!!_

_Should I just use my code last year? Or should I do it all over again from scratch!_

I'm going to do it from scratch!

_Wait... I already forgot how to write code in python..._

Let's take a peek at your old code.

_Hmmm.. that's how Python code looks like!_

Let's copy the first line... just to get you started.

``` python
keys_sorted_by_weight = sorted(cows.keys(), key = lambda key: cows[key], reverse=True)
```

[click... click... click --- coding]

_Finished! Yeeey!_

``` python
# Problem 1
def greedy_cow_transport(cows,limit=10):
    """
    Uses a greedy heuristic to determine an allocation of cows that attempts to
    minimize the number of spaceship trips needed to transport all the cows. The
    returned allocation of cows may or may not be optimal.
    The greedy heuristic should follow the following method:

    1. As long as the current trip can fit another cow, add the largest cow that will fit
        to the trip
    2. Once the trip is full, begin a new trip to transport the remaining cows

    Does not mutate the given dictionary of cows.

    Parameters:
    cows - a dictionary of name (string), weight (int) pairs
    limit - weight limit of the spaceship (an int)
    
    Returns:
    A list of lists, with each inner list containing the names of cows
    transported on a particular trip and the overall list containing all the
    trips
    """
    keys_sorted_by_weight = sorted(cows.keys(), key = lambda key: cows[key], reverse=True)
    all_cow_list_of_trips = []

    cow_list_for_a_trip = []
    available_space_left = limit
    for name in keys_sorted_by_weight:
        weight_of_current_cow = cows[name]

        if weight_of_current_cow > available_space_left:
            all_cow_list_of_trips.append(cow_list_for_a_trip)
            cow_list_for_a_trip = []
            available_space_left = limit

        if weight_of_current_cow <= limit:
            cow_list_for_a_trip.append(name)
            available_space_left -= weight_of_current_cow

    # append the last list because the loop already ended before we have the chance to add it
    if len(cow_list_for_a_trip) > 0:
        all_cow_list_of_trips.append(cow_list_for_a_trip)

    return all_cow_list_of_trips
```

Time to submit!...

...

> :x: &nbsp; Incorrect (0/20 points)

_Incorrect?! Why?_

Let's see the test results...

![MITx-6.00.2x-pset1-part1-test-results-for-first-submission](/images/2018/MITx-6.00.2x-pset1-part1-test-results-for-first-submission.png)

_Ahh... okay... this problem is much harder than I thought._
<!-- 
I thought they just want me write an O(n) implementation for this part. _(I do not really know Big-O, but I know that my code is O(n) because it just goes through the input list once.)_
 -->
_... I feel tired... I mean, lazy :grin: ... let's just submit your code from last year and be done with this..._

[submitting `greedy_cow_transport()` found [here](https://github.com/jeremiahflaga/mit-ocw-6.0002/blob/f17155864481d4adc6eeba5c4acc1a7b65f960a6/ps1/ps1a.py)]

...

> :x: &nbsp; Incorrect (0/20 points)

_What! Incorrect also?!... So my solution, **which I posted on GitHub,** was incorrect!_

Shame. :laughing: :laughing: :laughing:

![MITx-6.00.2x-pset1-part1-test-results-for-old-code](/images/2018/MITx-6.00.2x-pset1-part1-test-results-for-old-code.png)

Well, part of the reason why I posted online my solutions to the problems sets of these free courses is so that they will be available for others to see and correct (if there are mistakes in them). My only problem is that I'm the only one who cares about that code, and so it has to wait for a very long time before someone is able to see the error.

This reminds me of the what they call "misplaced confidence" that programmers like me usually have in themselves. :laughing:

But sometimes it's not really misplaced confidence but laziness. :laughing:

And sometimes it is tiredness...

Sometimes, perhaps oftentimes, we just need someone else to check our confidence or our attitude when writing code. This makes me remember what Victor Rentea said about pair programming in his talk ["Brainstorming a Clean, Pragmatic Architecture"](https://www.youtube.com/watch?v=mBxpOvlbAow&ab_channel=JUG.ru): 

> Many consultants believe that pair programming is the universal solution to any problem that a software project can have.
> 
> - It spreads the technical skills
> - It spreads the functional knowledge
> - It builds the team
> - It's fun

I hope pair programming will become a common practice for programmers in the near future.

And about the problem?... I already solved it.

> :heavy_check_mark: &nbsp; Correct (20/20 points)

_Yehey!_

But I'm not allowed to post the code in here so I'm just going to post part of the test results:


![MITx-6.00.2x-pset1-part1-test-results-for-correct-submission](/images/2018/MITx-6.00.2x-pset1-part1-test-results-for-correct-submission.png)

Happy coding!!!