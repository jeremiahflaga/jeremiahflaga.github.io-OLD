---
layout: page
title: Great Quotes from "The Craftsman Series" of Uncle Bob Martin
permalink: /the-craftsman-series-quotes/
---


You can see the whole series [here](https://github.com/jeremiahflaga/the-craftsman-book), a github repository I forked from [Igor Popov](https://github.com/sensui).


<h3 id="3">
  The Craftsman #3: Clarity and Collaboration
</h3>

> “So far”, he said, “We’ve been refactoring fragments. Now we want to see if the whole program hangs together as a readable whole.”
<br /><br />
I asked: “Jerry, do you do this with your own code too?”
<br /><br />
Jerry scowled: **“We work as a team around here, so there is no code I call my own. Do you consider this code yours now?”**
<br /><br />
“Not anymore.” I said, meekly. “You’ve had a big influence on it.”
<br /><br />
“We both have.” he said, “And that’s the way that Mr. C likes it. He doesn’t want any single person owning code. But to answer your question: Yes. We all practice this kind of refactoring and code clean up around here. It’s Mr. C’s way.”


> “Remember this. **From now on when you write a module, get help with it and keep it clean. If you hand in anything below those standards you won’t last long here.**”



<h3 id="4">
  The Craftsman #4: Clarity and Collaboration
</h3>

> “Look.” Jerry said, clearly trying to calm me down. “**Don’t become vested in your code.** This was just thirty minutes worth of work. It’s not that big a deal. You need to be ready to throw away a lot more code than that if you want to become any kind of a programmer. Often the best thing you can do with a batch of code is throw it out.”
<br /><br />
“But that’s such a waste!” I blurted.
<br /><br />
**“Do you think the value of a program is in the code?” he asked. “It’s not. The value of a program is in your head.”**

> **“Did you notice whether the new code was better or worse than the code you lost?”** he asked.
<br /><br />
**“Oh, it was much better.”** I said, regretting my words the instant I said them. “I was able to use a much better structure the second time.”
<br /><br />
He smiled. **“So for an extra 25% effort, you wound up with a better solution.”**



<h3 id="5">
    The Craftsman #5 Baby Steps
</h3>

``` java
	public static int[] factor(int multiple) {
		return new int[] {2};
	}
```

> He ignored me and continued. “**Every time you add a new test case, you have to make it pass by making the code more general.** Now go back and make the simplest change that is more general than your first solution.”
<br /><br />
I thought about this for a minute. At last Jerry had asked me something that might require a few brain cells. Yes, there was a more general solution. I took the keyboard and typed:

``` java
	public static int[] factor(int multiple) {
		return new int[] {multiple};
	}
```


<h3 id="6">
    The Craftsman #6: Once Is Not Enough
</h3>

> “What I’m doing here is called **Intentional Programming**.” Jerry said. **I’m calling code that doesn’t yet exist.** As I do so, I **express my intent** about what that code should look like, and how it should behave.”


> “But how do you know `SocketService` will need the `connections` method?”
<br /><br />
“Oh, it probably doesn’t. I’m just putting it there so I can test it.”
<br /><br />
“Isn’t that wasteful?” I queried?
<br /><br />
Jerry looked me sternly in the eye and replied: **“Nothing that makes a test easy is wasted, Alphonse. We often add methods to classes simply to make the classes easier to test.”**
<br /><br />
I didn’t like the `connections()` method, but I held my tongue for the moment.


> “What’s going on?” I asked.
<br /><br />
Jerry smiled. “See if you can figure it out, Alphonse. Trace it through.”
<br /><br />
“OK, let’s see. The test program calls `serve`, which creates the socket and calls `accept`. Oh! `accept` doesn’t return until it gets a connection! And so `serve` never returns, and we never get to call `connect`.”
<br /><br />
Jerry nodded. “So how are you going to fix this Alphonse?”
<br /><br />
I thought about this for a minute. I needed to call the `connect` function after calling `accept`, but
when you call `accept`, it won’t return until you call `connect`. At first this sounded impossible.
<br /><br />
**“It’s not impossible, Alphonse.”** said Jerry. **“You just have to create a thread.”**
<br /><br />
I thought about this a little more. Yes, I could put the call to accept in a separate thread. Then I could invoke that thread and then call connect.


> I repeatedly pushed the button. In ten attempts I saw three failures. Was I losing my mind? How can
a program behave like that?
<br /><br />
“How did you know, Jerry? Are you related to the oracle of Aldebran?”
<br /><br />
“No, **I’ve just written this kind of code before, and I know a little bit about what to expect.** Can you work out what’s happening? Think it through carefully.”


> “I think you’re right.” Jerry said. “**The two events – accept and close – are asynchronous, and the system is sensitive to their order.** We call that a **race condition**. We have to make sure we always win the race.





<h3 id="11">
    The Craftsman #11: What's main() got to do with it
</h3>

> "Excuse me!" Jerry Interrupted. "Do you have a test for that?"
<br /><br />
"Huh? What do you mean? This is trivial code, why should we write a test for it?"
<br /><br />
**"If you don't write a test for it, how do you know you need it?"**


> "You see?" He explained. **"I like to evaluate each command line argument in its own function, rather than mixing all that parsing and evaluating code together."**



<h3 id="12">
    The Craftsman #12 :Three Ugly Lines
</h3>

> "It's just a four lines of code." I said. "Hardly major duplication."
<br /><br />
"True." Jerry replied. "But **duplication should always be removed as soon as possible. If you allow duplicate code to accumulate in your project you'll find yourself with a huge source of confusion and bugs**."




<h3 id="13">
    The Craftsman #13: Objects
</h3>

> Jennifer stepped up to me and said: "Yes, that's all you did. And, no, I don't suppose you think all that much of it. But to us, it's a big deal."
<br /><br />
"But why?"
<br /><br />
"Because", said Jeremy, **"the most important trait of a good programmer is the ability to think abstractly. Very few programmers can actually do that. You have just proven that you can."**



