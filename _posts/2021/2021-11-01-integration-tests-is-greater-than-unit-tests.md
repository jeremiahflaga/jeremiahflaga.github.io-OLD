---
layout: post
title: 'So, integration tests > unit tests'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2021-07-31 12:00:00 AM UTC
published: false
---

<!-- July 15, 2021  5:00 AM Philippine Time -->

<!-- About two years ago,  -->

A few years ago, I had inherited this project which did not have unit tests. During that time, because I had been [brainwashed](/memorabilia/quotes/tdd/) (a [good kind of brainwashing](https://www.nikiflorica.com/post/the-hammering-process-too-hard-too-soft-or-just-right) I hope) to write tests so that 


I started writing tests for the the services or use cases part. But I noticed that sometimes I still break existing functionality to those use cases I have written tests for... 

so I created tests for the ASP.NET Controllers instead... I sometimes break existing functionality when I refactor.

So I created tests from the outside, using HttpClient. I was thinking that that ind of test is called acceptance tests, but I was not so sure, so I called them API Endpoint Tests.

I had some success in doing that, but those tests were later abandoned because the data structures for the API request/response input/output are so big (because they are bing reused in different endpoints) that it takes lots of time to trace which ones are being used in an endpoint...






## Some readings on the importance of integration (or functional) tests

["Should you unit-test API/MVC controllers in ASP.NET Core?"](https://andrewlock.net/should-you-unit-test-controllers-in-aspnetcore/) by Andrew Lock

["Why writing integration tests on a C# API is a productivity booster"](https://timdeschryver.dev/blog/why-writing-integration-tests-on-a-csharp-api-is-a-productivity-booster) by Tim Deschryver

["one good integration test is worth 1,000 unit tests"](https://khalidabuhakmeh.com/secrets-of-a-dotnet-professional) by Khalid Abuhakmeh



## Writing integration tests in ASP.NET WebAPI

["Self-hosted integration tests in ASP.NET"](https://blog.ploeh.dk/2021/01/25/self-hosted-integration-tests-in-aspnet/) by Mark Seemann

["How to test your C# Web API"](https://timdeschryver.dev/blog/how-to-test-your-csharp-web-api) by Tim Deschryver



### Terminology:

["integration (or functional) tests"](https://timdeschryver.dev/blog/why-writing-integration-tests-on-a-csharp-api-is-a-productivity-booster)

["Functional Test == Acceptance Test == End-to-End Test"](https://www.obeythetestinggoat.com/book/chapter_02_unittest.html)

["Acceptance test... similar to an integration test, but with a focus on the use case rather than on the components involved."](http://www.getlaura.com/testing-unit-vs-integration-vs-regression-vs-acceptance/)



=======================






https://blog.ploeh.dk/2021/01/25/self-hosted-integration-tests-in-aspnet/


https://blog.ploeh.dk/2013/10/23/mocks-for-commands-stubs-for-queries/


the one from Vladimir Khorikov

From thomas perrian



Little Mock from Uncle Bob





"Should you unit-test API/MVC controllers in ASP.NET Core?" by Andrew Lock - https://andrewlock.net/should-you-unit-test-controllers-in-aspnetcore/

"one good integration test is worth 1,000 unit tests" - https://khalidabuhakmeh.com/secrets-of-a-dotnet-professional

Writing .NET Database Integration Tests - https://khalidabuhakmeh.com/dotnet-database-integration-tests




Why writing integration tests on a C# API is a productivity booster - https://timdeschryver.dev/blog/why-writing-integration-tests-on-a-csharp-api-is-a-productivity-booster


How to test your C# Web API - https://timdeschryver.dev/blog/how-to-test-your-csharp-web-api