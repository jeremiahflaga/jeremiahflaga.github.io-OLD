---
layout: page
title: Great Quotes from "Test-Driven Development with Python" by Harry Percival
categories: [Programming]
tags: [TDD]
permalink: /quotes/tdd-with-python/
---


["Test-Driven Development with Python"](https://www.obeythetestinggoat.com/pages/book.html) by Harry Percival.

### Preface

... Believe me, I second-guessed every rule, I suggested every shortcut, I demanded justifications for every seemingly pointless aspect of TDD, and I came out seeing the wisdom of it all. I’ve lost count of the number of times I’ve thought “Thanks, tests”, as a functional test uncovers a regression we would never have predicted, or a unit test saves me from making a really silly logic error. Psychologically, it’s made development a much less stressful process. It produces code that’s a pleasure to work with.


### Chapter 1

TDD isn’t something that comes naturally. It’s a discipline, like a martial art, and just like in a Kung Fu movie, you need a bad-tempered and unreasonable master to force you to learn the discipline.


### Chapter 2

<dl>
  <dt>Functional Test == Acceptance Test == End-to-End Test</dt>
  <dd>
    What I call functional tests, some people prefer to call acceptance tests, or end-to-end tests. The main point is that these kinds of tests look at how the whole application functions, from the outside. Another term is black box test, because the test doesn’t know anything about the internals of the system under test.
  </dd>
  <dt>User Story: </dt>
  <dd>
    A description of how the application will work from the point of view of the user. Used to structure a functional test.
  </dd>
</dl>


### Chapter 3

Functional tests should help you build an application with the right functionality, and guarantee you never accidentally break it. Unit tests should help you to write code that’s clean and bug free.