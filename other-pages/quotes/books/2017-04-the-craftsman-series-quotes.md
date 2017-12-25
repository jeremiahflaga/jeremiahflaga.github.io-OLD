---
layout: page
title: Great Quotes from "The Craftsman Series" of Uncle Bob Martin
permalink: /quotes/the-craftsman-series-quotes/
---

<style>
  h3 {
    color: #6a9fb5;
  }
</style>



You can see the whole series [here](https://github.com/jeremiahflaga/the-craftsman-book), a github repository I forked from [Igor Popov](https://github.com/sensui).


<h3 id="3">
  The Craftsman #3: Clarity and Collaboration
</h3>

> “**We work as a team around here, so there is no code I call my own.** Do you consider this code yours now?”

> “Remember this. **From now on when you write a module, get help with it and keep it clean. If you hand in anything below those standards you won’t last long here.**”



<h3 id="4">
  The Craftsman #4: Clarity and Collaboration
</h3>

> “**Don’t become vested in your code.** This was just thirty minutes worth of work. It’s not that big a deal. You need to be ready to throw away a lot more code than that if you want to become any kind of a programmer. **Often the best thing you can do with a batch of code is throw it out.**”
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
    The Craftsman #5: Baby Steps
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


<span class="message message-compressed float-right">
    `accept` is a [blocking function](https://www.codeproject.com/Articles/13071/Programming-Windows-TCP-Sockets-in-C-for-the-Begin)
</span>

> ... I needed to call the `connect` function after calling `accept`, but
when you call `accept`, it won’t return until you call `connect`. At first this sounded impossible.
<br /><br />
**“It’s not impossible, Alphonse.”** said Jerry. **“You just have to create a thread.”**
<br /><br />
I thought about this a little more. Yes, I could put the call to `accept` in a separate thread. Then I could invoke that thread and then call `connect`.


> “How did you know, Jerry? Are you related to the oracle of Aldebran?”
<br /><br />
“No, **I’ve just written this kind of code before, and I know a little bit about what to expect.** Can you work out what’s happening? Think it through carefully.”


> “I think you’re right.” Jerry said. “**The two events – accept and close – are asynchronous, and the system is sensitive to their order.** We call that a **race condition**. We have to make sure we always win the race.


> **“When you are dealing with multiple threads, you have to keep an eye out for possible race conditions. Hitting the test button multiple times is a good habit to get into.”**
<br /><br />
**“It’s a good thing we found this in our test cases.”** I said. **“Finding it after the system was running would have been a lot harder.”**




<h3 id="7">
    The Craftsman #7: Once Is Not Enough
</h3>

> I didn’t care for that. I said: “**Why should we pollute SocketService with a “Hello” message just to satisfy our tests?** It would be good to test that the SocketService can send a message, but we don’t want the message to be part of the SocketService code!”
<br /><br />
“Right!” Jerry agreed. “We want the message to be specified by, and verified by, the test.”
<br /><br />
“How do we do that?” I asked.
<br /><br />
Jerry smiled and said: “We’ll use the **Mock Object pattern**. In short, we create an interface that the SocketService will execute after receiving a connection. We’ll have the test implement that interface to send the “Hello” message. Then we’ll have the test read the message from the client socket and verify that it was sent correctly.”


<span class="message message-compressed float-right">
    Comment: seems like some of the code that should be in here, such as those for `setUp` and `tearDown`, are missing... **they are in #8!**
</span>


> The tests all passed.
<br /><br />
“That was pretty easy.” Said Jerry.
<br /><br />
“Yeah. **That Mock Object pattern is pretty useful. It let us keep all the test code in the test module.** The SocketService doesn’t know anything about it.”
<br /><br />
“It’s even more useful than that.” Jerry replied. “Real servers will also implement the SocketServer
interface.”
<br /><br />
“I can see that.” I said. **“Interesting that the needs of a unit test drove us to create a design that would be generally useful.”**
<br /><br />
**“That happens all the time.”** Said Jerry. **“Tests are users too. The needs of the tests are often the same as the needs of the real users.”**
<br /><br />
“But why is it called Mock Object?”
<br /><br />
“Think of it this way. The HelloServer is a substitute for, or a mock-up of, a real server. The pattern allows us to substitute test mock-ups into real application code.”




<h3 id="8">
    The Craftsman #8: Socket Service 3 (Tests are a form of documentation)
</h3>

> ... I got to thinking about how remarkable it was to use **tests as a design tool** ...


> "If you were just coming onto this project, and I showed you these tests, what would they teach you?"
<br /><br />
... **"You mean we wrote these tests as examples to show to others?"**
<br /><br />
"That's part of the reason, Alphonse. Yes. **Others will be able to read these tests and see how to work the code we're writing.** They'll also be able to work through our reasoning. Moreover, they'll be able to
compile and execute these tests in order to prove to themselves that our reasoning was sound."




<h3 id="9">
    The Craftsman #9: Dangerous Threads
</h3>

> ... "I wanted two sessions open simultaneously. "
<br /><br />
"Why?"
<br /><br />
Jerry gave me a curious look and said: "Because then the `serve` method in the `SocketService` class will have been entered twice, in two different threads, before either had had a change to exit. When a function is entered more than once before it exits, it is called **reentrant**."
<br /><br />
"But why do you want to test it?"
<br /><br />
"Because **reentrant functions often give us the most interesting kinds of problems**."


> "We need to put `itsServer.serve` in its own thread so that the loop can return without waiting for it."






<h3 id="10">
    The Craftsman #10: Dangling Threads (Iterations Unbound)
</h3>


> "... Joins can take awhile. **It's not good to leave iterator open for long periods when other threads might try to modify the list.**"

> "Alponse, **whenever you have a container being modified by many different threads, there is a chance that the two threads will collide inside the container**. One thread might be in the middle of adding an
element while another thread is in the middle of deleting one. When that happens the container can get
corrupted and bizarre things can start happening."
<br /><br />
"Oh, so you mean we should synchronize access to the container?"
<br /><br />
"Yes, that's exactly what I mean. We need to make sure that while the container is being modified, no
other thread can access it."
<br /><br />
"OK, I can do that. It's pretty simple!" So I proceeded to surround the `add` statement and the two
remove statements with `synchronized(serverThreads){...}`. I ran the tests, and they all still
worked.



> `private List serverThreads = Collections.synchronizedList(new LinkedList());`
<br /><br />
> "... Have you heard of the Decorator pattern?"
<br /><br />
**"The `synchronizedList` function that we just called wraps the `LinkedList` in a Decorator. All calls to the LinkedList are synchronized by it."**
<br /><br />
"That sounds like a pretty good solution." I said.
<br /><br />
"Yeah, well, you just have to remember to explicitly synchronize any use of an iterator." He grimaced.
<br /><br />
"Really? You mean **iteration isn't synchronized in a synchronized list**?"


<h3 id="11">
    The Craftsman #11: What's main() got to do with it
</h3>

> Jerry looked at me expectantly and asked: "How do you think we should start?"
<br /><br />
"I think I'd like to know how the user will use it." I responded.
<br /><br />
"Excellent! **Starting from the user's point of view is always a good idea.** So what's the simplest thing
the user can do with this tool?"


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

> "Because", said Jeremy, **"the most important trait of a good programmer is the ability to think abstractly. Very few programmers can actually do that. You have just proven that you can."**



