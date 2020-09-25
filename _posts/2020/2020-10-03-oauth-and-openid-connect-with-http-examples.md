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
