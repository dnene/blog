---
date: '2010-08-26 21:07:38'
layout: post
slug: a-case-for-non-leaky-dual-abstractions.
status: publish
title: A case for non leaky dual abstractions.
comments: true
wordpress_id: '1042'
categories:
- architecture
- software
tags:
- abstractions
- design
- implementation
- interface
- ood
---

A long long time ago, I worked on a fairly complex piece of design. And like any well behaved designer, I broke it down into a number of abstractions that made it manageable. I gave the abstractions funny sounding names. And before long I found those abstractions finding their way into the user interface. Abstractions which made no sense to the end user. 

That experience taught me a good lesson. In attempting to deal with complexity, I was attempting to come up with the appropriate abstractions in a very bottom up way. So, strategic closure (of the Open Closed Principle), dependency inversion principle, et. al. all found their way into these abstractions I was modeling. These abstractions represented themselves via the various programmatic interfaces in the software design and their roles. At the same time there was a different perspective of the software. The way the end users would've preferred to see that software. The abstractions the way the end users saw the system were sometimes different than the underlying abstractions I modeled. 

In a good design, the two abstractions should line up, and if there is an inconsistency, there is probably an issue with the design. Certainly this could be an issue in some cases. However in many cases the underlying backend might be built to offer far more capabilities than what are being exposed through the early version of the user interfaces. Probably there are multiple intents for which the software is being designed, and the particular user interface on table is just one of them. Probably, the underlying complexity of the back end is way too high which requires a very different nature of bottom up abstractions, which are different from the top down ones. Frankly, the reason doesn't matter. Even after so many years, I look back and am comfortable with the thought that there needed to be two abstractions - one top down and one bottom up, one front end and one backend.

Fast forward many years. Another example helped me further attain some insight into the matter. We had a bunch of mobile smartphones floating around with the traditional PC UI abstractions being carried over into their design. Along came an iPhone - which rethought the interface the way users would've preferred to see the interface. Now iPhone was built on a traditional operating system. So it had the same abstractions that these operating systems have in the backend. However it decided to change some of the front end abstractions - in tune with the target market and the device / form factor peculiarities. Did the iPhone change the backend abstractions baked into its software - most likely not. However it changed the frontend abstractions, just enough to make the experience really simple and easy for its users.

As an engineer, I have lived with the regret of allowing backend abstractions to leak into the front end. But I have learnt something along the way. 




	
  * Don't let the inapplicable frontend abstractions leak into the backend. This is especially true for most reasonably complex software. Strictly top down design can lead to a lot of brittleness in the long run, requiring very substantial surgery eventually.

	
  * Don't let the inapplicable backend abstractions leak into the frontend. In most cases this is a usability nightmare. 'nuff said.

	
  * Realise that you have to simultaneously service independent expectations ie. robustness of usability and robustness of modeling. Work with both the abstractions to mould each of them well: Work your frontend abstractions well enough to mould them to reflect the use cases of the software in the most usable manner.  Work with the backend abstractions to let them emerge from the problem space you are trying to resolve at the backend. Now make sure you work with both of them to map them into each in a reasonably smooth fashion. This is easier said than done. But doing it is whats necessary.



PS: I am not referring to what is conventionally referred to as leaky abstractions, where the implementation leaks into an abstraction. I am referring to a set of frontend and backend abstractions leaking into each other. However if all the backend abstractions were to be treated as an implementation, then this would be a case of leaky abstractions as well.


