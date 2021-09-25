---
layout: post
title: 'Looking for an OAuth and OpenID Connect explanation with HTTP examples??'
subtitle: ''
categories: [Programming]
tags: [Programming, eShopOnContainers, Microservices]
date: 2020-10-03 01:00:00 AM UTC
---

<!-- started September 24, 2020 Philippine Time -->

Here is a somewhat easy-to-understand explanation of the hard-to-understand OAuth and OpenID Connect standards --- and they come with HTTP call examples. I found them from [a playlist](https://www.youtube.com/playlist?list=PLKCk3OyNwIzuD_jxWu-JddooM2yjX5q99) on the YouTube channel named Oracle Learning.


<!--more-->


1. [OAuth Introduction and Terminology](https://www.youtube.com/watch?v=zEysfgIbqlg)
2. [An Introduction To OpenID Connect](https://www.youtube.com/watch?v=6DxRTJN1Ffo)
3. [OpenID Connect Flows](https://www.youtube.com/watch?v=WVCzv50BslE)


### Some Lessons Learned:

**Flows** are the interaction between the participants in the OAuth standard or the OpenId Connect standard.

**Grant Types** are ways for a client application to acquire an access token.

**Each grant type has its own Flow** to acquire an access token. And these flows involve the interaction of public or confidential clients.

OAuth defines two types of clients: **Confidential Client** and **Public Client**.

There are two distinct ways in which the clients communicate with the authorization server: via the **Front Channel** and via the **Back Channel**.

etc.


### Other useful resources:

[An Illustrated Guide to OAuth and OpenID Connect](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc) by David Neal from Okta

[OpenID Connect explained](https://connect2id.com/learn/openid-connect) from Connect2id


#### Easy to understand resources on HTTPS:

[How HTTPS works](https://www.youtube.com/watch?v=w0QbnxKRD0w&ab_channel=thecuriousengineer)

[How Does HTTPS Work - HTTPS Explained](https://www.youtube.com/watch?v=uS36UFiDlhk&ab_channel=GraceHopperAcademy)

[public-key cryptography for non-geeks](https://blog.vrypan.net/2013/08/28/public-key-cryptography-for-non-geeks/) by Vrypan

[What happens during a TLS handshake?](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/) from CloudFlare


### Update August 2021: 

I already forgot what this OAuth and OpenID Connect are... TLS handshake(?)... what is that?...

But here are some very very good materials on authentication/authorisation in ASP.NET Core from Andrew Lock. He also gave examples on OAuth 2.0 and OpenID Connect:

1. [Introduction to Authentication with ASP.NET Core](https://andrewlock.net/introduction-to-authentication-with-asp-net-core/)
2. Exploring the cookie authentication middleware in ASP.NET Core
3. A look behind the JWT bearer authentication middleware in ASP.NET Core
4. An introduction to OAuth 2.0 using Facebook in ASP.NET Core
5. An introduction to OpenID Connect in ASP.NET Core
6. Introduction to Authorisation in ASP.NET Core MVC
7. Custom authorisation policies and requirements in ASP.NET Core
8. Modifying the UI based on user authorisation in ASP.NET Core
9. Resource-based authorisation in ASP.NET Core

> **Authentication** is the process of determining who you are, while **Authorisation** revolves around what you are allowed to do, i.e. permissions

> You can think of **claims** as being a statement about, or a property of, a particular identity. That statement consists of a name and a value. For example you could have a DateOfBirth claim, FirstName claim, EmailAddress claim or IsVIP claim. Note that these statements are about what or who the identity **is**, not what they can **do**.

And here's ["No! You don't need to use ASP.NET Identity!"](https://weblogs.asp.net/jeff/no-you-don-t-need-to-use-asp-net-identity) by Jeff Putz

> The problem, as I see it, is that developers are confusing the act of persisting user information with authentication... ... under the covers, there is code that first verifies the user/password against the database, then sets the auth cookie to indicate who the user is for future requests.
