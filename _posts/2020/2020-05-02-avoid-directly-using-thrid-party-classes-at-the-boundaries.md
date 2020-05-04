---
layout: post
title: 'Avoid directly using 3rd party classes at the boundaries of your architecture'
categories: [Music]
tags: [Music]
date: 2020-05-02 06:00:00 PM UTC
---

<!-- started ‎Friday, ‎May ‎1, ‎2020, ‏‎4:38:00 PM Philippine Time -->
<!-- finished ‎Friday, ‎May ‎3, ‎2020, ‏‎2:05:00 AM Philippine Time -->

That's a big lesson I learned recently:

> Avoid **directly** using third party classes at the **boundaries**<sup id="footnote-indicator-1">[[1]](#footnote-1)</sup> of your architecture. This advice still applies even when you think that these third party classes are very very small and that they will not bring harm to your architecture. (They will, when the requirements change; and the requirements most probably will change.)

<!--more-->

I will start the explanation of this _discovery_ (or, I should say, one of my first-hand experiences of the discovery of _others_) with a story. I might later write on this discovery again minus the story, but in this post I will tell the story behind it. (It seems to me that in our world, there are always stories behind our discovery of truths, and there are always stories behind the stories of our discovery of truths, and so on. And, as I suppose some of our thinkers today say, if we forget the stories we also tend to forget about the truths behind those stories; and we forget why these truths are important; then civilization collapses.)

So here goes the story...

Two nights ago, I was reading ["Patterns, Principles, and Practices of Domain-Driven Design"](https://www.bookdepository.com/Patterns-Principles-Practices-Domain-Driven-Design-Scott-Millett/9781118714706?a_aid=jflaga) by Scott Millett. I came across this example of a Command Handler:

<!-- example of command handler is on page 676 -->

``` csharp
public class CreateOrUpdateCategoryHandler
{
    // ...
    public ICommandResult Execute(CreateOrUpdateCategoryCommand command)
    {
        // ...
    }
}
```

That example came with this note: _"A command handler only returns an acknowledgement of the success or failure of a command; you should not use it to query or report in the domain state."_

Now, if you have encountered some relatively older examples of a command handler, like the [ones given by Steven van Deursen](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/), you might have learned that command handlers should not return anything, or that they should return `void`, like in this example:

``` csharp
public interface ICommandHandler<TCommand>
{
    void Handle(TCommand command);
}
```

More recent examples, such as [those given by Vladimir Khorikov](https://enterprisecraftsmanship.com/posts/cqrs-commands-part-domain-model/), returns objects from the command handler which tells about the success or failure of the operation. (I used this kind of code when I had this problem in the codebase I was working on where I have to return a success or failure indicator from my command handlers.)

``` csharp
public interface ICommandHandler<TCommand>
{
    Result Handle(TCommand command);
}
```

That example from Vladimir Khorikov looks similar to the example I found in the [PPPDDD](https://www.bookdepository.com/Patterns-Principles-Practices-Domain-Driven-Design-Scott-Millett/9781118714706?a_aid=jflaga) book, which can be written like this:

``` csharp
public interface ICommandHandler<TCommand>
{
    ICommandResult Handle(TCommand command);
}
```

The last two examples look the same, right? Hmmmm... :thought_balloon: It might seem like that on first look, but there is a subtle difference: The `Result` class is from a third party library called [CSharpFunctionalExtensions](https://github.com/vkhorikov/CSharpFunctionalExtensions), while `ICommandResult` is not.

And when it comes to third party libraries, I tend to remember this statement from Uncle Bob Martin<sup id="footnote-indicator-2">[[2]](#footnote-2)</sup>: These third party classes, libraries, or frameworks are...

> "... written by people to solve certain problems that they have. Those problems may be similar to yours, but they are not yours. You have different problems..." 

And here's more<sup id="footnote-indicator-2">[[2]](#footnote-2)</sup>:

> "Over the years, I’ve adopted a healthy skepticism about frameworks. While I acknowledge that they can be extremely useful, and save a boatload of time; I also realize that there are costs. Sometimes those costs can mount very high indeed.
<br /><br />
"So my strategy is to keep frameworks... at arm’s length; behind architectural boundaries. I get most of the benefit from them that way; and I can take ruthless advantage of them.
<br /><br />
"But I don’t let those frameworks get too close. I surrender none of my autonomy to them..." --- Uncle Bob Martin 

I forgot those words of wisdom from Uncle Bob just recently :smile:

Oh wait! I did not forget them. Only that I also heard Uncle Bob say something like "we also use libraries inside our core domain, such as the the .NET Collections or the Java Collections library, but this decision involves kind of a conscious decision of using them". Those are not his exact words, but a paraphrase of what he said.<sup id="footnote-indicator-3">[[3]](#footnote-3)</sup>

And so because I had this problem of needing to return a success or failure indicator from my command handlers, I made the _conscious_ decision of using the `Result` class from CSharpFunctionalExtensions as my return type. "What wrong can it cause!?", I told myself, "It's just a small library, a tiny class; it will cause no danger to the app."

And so I started using this as my command handler: 

``` csharp
public interface ICommandHandler<TCommand>
{
    Result Handle(TCommand command);
}
```

A few months later, there was a new requirement: I needed to return a custom error from my command handlers. Whew, that's easy! I will just have to create another kind of command handler and use a custom `Result` with error, `Result<Error>`, as my return type!:

``` csharp
public interface ICommandWithErrorHandler<TCommand>
{
    Result<Error> Execute(TCommand command);
}
```

... Wait!...

[I cannot do `Result<Error>` in CSharpFunctionalExtensions!!!](https://github.com/vkhorikov/CSharpFunctionalExtensions/issues/124)

But I later found out that the way to do custom errors in CSharpFunctionalExtensions is to use `Result<T,E>`, so I have to use an `Empty` class in my command handler to indicate that I don't want to return _state_ from my command handlers:

``` csharp
public interface ICommandWithErrorHandler<TCommand>
{
    Result<Empty, Error> Execute(TCommand command);
}
```

Tsk tsk! Look at that. Imagine having `Result<Empty, Error>` all over your app! It will look very messy, I think.

What's worse is that I now have two kinds of command handlers in my app: one which returns `Result` and another one which returns `Result<Empty, Error>`.

What if there is another requirement in the future which goes something like "the command handlers need to return many error messages at once". I will have to create another kind of command handler for that!

``` csharp
public interface ICommandWithErrorListHandler<TCommand>
{
    Result<Empty, IEnumerable<Error>> Execute(TCommand command);
}
```

Whew!

Solving this problem would have been easier had I used my own custom return type in my command handlers in the first place:


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

This made me come to the realization that using third party libraries at the boundaries of the architecture is like _assuming_ that you know all the problems you will encounter in the future; or that you know all the problems that the ones who will maintain your app will encounter in the future. Sad. :laughing:




<!-- 
-----

**PS:**

If you are wondering why I used the word "avoid" instead of "don't", and why I used the word "advice" instead of "rule" ... the reason is because it seems to me that people these days (which includes me of course :smile:) have the tendency of treating the words "don't" and "rule" as offensive. I can sense it. I'm not yet sure why. But even I am kind of "avoidant" of people who constantly use those words: "don't" and "rule". I'm not saying that those words are not important. They are. But... I don't know...
 -->


-----

<sup id="footnote-1">[1]</sup> 
<small>
I'm not yet sure if this only applies to boundaries that are generic, like when using the `ICommandHandler` interface in all command handlers in an app. But what is the use of having separate classes for command handlers if they do not employ a generic interface, right? Well putting them in separate classes is still good even when they don't have a generic interface, but making them have a generic interface is still better I think. 
</small>
[&#8617;](#footnote-indicator-1)

<sup id="footnote-2">[2]</sup> 
<small>
from blog post ["Framework Bound"](https://blog.cleancoder.com/uncle-bob/2014/05/11/FrameworkBound.html) 
</small>
[&#8617;](#footnote-indicator-2)

<sup id="footnote-3">[3]</sup> 
<small>
I cannot locate for now the place where he said that. I will just update this post when I find it later.
</small>
[&#8617;](#footnote-indicator-3)
