---
date: '2009-06-11 01:22:35'
layout: post
slug: rest-soa-woa-or-roa
status: publish
title: 'ReST : SOA, WOA or ROA ?'
comments: true
wordpress_id: '699'
categories:
- architecture
- rest
- rest-musings
tags:
- rest
- roa
- soa
- woa
---

_This is part 4 of a continuing series of posts on ReST. So you might want to read up the earlier ones as well (chronologically they are the three posts before this)_

Nice alphabet soup indeed. But what style of architecture does ReST (Representational State Transfer) correspond to.

Before we get into any definitional issues which are hugely referred to in case of such debates - I shall be referring to the currently available definitions and descriptions as available on Wikipedia viz. Service Oriented Architecture, Web Oriented Architecture and Resource Oriented Architecture.

**Service Oriented Architecture**

As per the definition on Wikipedia (emphasis is mine):


> In computing, service-oriented architecture (SOA) provides methods for systems development and integration where systems package functionality as interoperable services. A SOA infrastructure allows different applications to exchange data with one another.

Service-orientation aims at a loose coupling of services with operating systems, programming languages and other technologies that underlie applications. _SOA separates functions_ into distinct units, or services, which developers make accessible over a network in order that users can combine and reuse them in the production of applications. These services communicate with each other by passing data from one service to another, or by coordinating an activity between two or more services.


On the topic of Service Orientation, it further goes on to state


> Service-orientation is a design paradigm that specifies the _creation of automation logic in the form of services_. It is applied as a strategic goal in developing a service-oriented architecture (SOA). Like other design paradigms, service-orientation provides a means of achieving a separation of concerns.


While REST does attempt to solve many similar goals as SOA, I believe there lies an important distinction. The essential focus of SOA is to separate functions or automation services. An example here is to separate an authentication service from authorisation, monitoring, logging etc. A SOA architecture that consists of a number of SOA services assembles such "_functionalities_" into one feature consistent whole. But is that how REST works ? Only in a very vague sense even if arguably so. REST standardises the functions ie. GET, PUT, POST and DELETE in case of HTTP connectors. It is arguable that REST could be used with a different set of functions, but even in that case the function set is likely to remain consistent. This is further supported by Roy Fielding's post "[It is Okay to use POST](http://roy.gbiv.com/untangled/2009/it-is-okay-to-use-post)" in which he argues


> Some people think that REST suggests not to use POST for updates.  Search my dissertation and you won’t find any mention of CRUD or POST. The only mention of PUT is in regard to HTTP’s lack of write-back caching.  The main reason for my lack of specificity is because the methods defined by HTTP are part of the Web’s architecture definition, not the REST architectural style. Specific method definitions (aside from the retrieval:resource duality of GET) simply don’t matter to the REST architectural style, so it is difficult to have a style discussion about them. _The only thing REST requires of methods is that they be uniformly defined for all resources (i.e., so that intermediaries don’t have to know the resource type in order to understand the meaning of the request). As long as the method is being used according to its own definition, REST doesn’t have much to say about it._


So across the board the functions remain the same and the data types (or media types) change. Sounds familiar ? Thats like supporting SELECT, INSERT, UPDATE, DELETE on a broad range of tables each having a different schema. An argument I make in an earlier post [REST is the DBMS of the Internet](http://blog.dhananjaynene.com/2009/06/rest-is-the-dbms-of-the-internet/). Moreover Roy further goes on to elaborate that in "REST intermediaries don't have to know the resource type in order to understand the meaning of the request" and that "method is being used according to its own definition". Each of this further introduces constraints into traditional service orientation. And these constraints make the field of use so narrow, that even though REST could be argued to be a teeny weeny specific use case of SOA, it could be argued to be Service Oriented to the same extent that a Database could be argued to be Procedure Oriented (since all tables support the procedures SELECT, INSERT, UPDATE, DELETE). In other words for all practical purposes REST is not Service Oriented.

**Web Oriented Architecture**

This is not yet a particularly widely used term yet, but I did come across a reference to it on a recent article in InfoQ "[REST is a Style - WOA is the architecture](http://www.infoq.com/news/2009/06/hinchcliffe-REST-WOA)". Looking it up on Wikipedia leads us to the same sources as referred to by InfoQ - articles written by Dion Hinchcliffe. Wikipedia states it as follows


> Web Oriented Architecture (WOA) is a style of software architecture that extends service-oriented architecture (SOA) to web based applications, and is sometimes considered to be a light-weight version of SOA. WOA is also aimed at maximizing the browser and server interactions by use of technologies such as REST and POX.


But I just argued in the earlier section that REST is only a very very specific use case of SOA, whereas the statement above says WOA extends SOA to web based applications. If we were dealing with objects and not architectural styles here, an argument that X is a specific (constrained) sub type of Y and X extends Y would instantaneously be flagged off as a violation of Liskov's Substitution Principle (LSP). So something isn't quite right here and the whole situation does not add up to a consistent whole for me. I will leave it to the reader to decide whether there exists a flaw in my reasoning here or elsewhere. My assessment is that if WOA is a collection of Web related architecture elements in addition to REST, then the only way to successfully and consistently resolve it is by saying if WOA builds on REST then it cannot be simultaneously extending SOA.

To be fair I couldn't quite find the same worlds (WOA extends SOA) in Hinchcliffe's writings. Referring to one of them "[What is WOA ? Its the future of Service Oriented Architecture (SOA)](http://hinchcliffe.org/archive/2008/02/27/16617.aspx)", he refers to another of his post ["Beating a Dead Horse: What's a SOA Again? All About Service-Orientation..."](http://hinchcliffe.org/archive/2005/08/27/1817.aspx) which in turn states


> It's here that John Reynolds' well-known SOA Elevator pitch comes tantalizingly close to capturing the essence:

>
>> SOA is an architectural style that encourages the creation of loosely coupled business services. Loosely coupled services that are interoperable and technology-agnostic enable business flexibility. An SOA solution consists of a composite set of business services that realize an end-to-end business process. Each service provides an interface-based service description to support flexible and dynamically re-configurable processes.
> 
> 
This business view is right on, and doesn't mean business in a traditional, white-collar way. In this context, "business" means the actual functionality of the system, apart from technical details.


There we go again on services. But Business Services cannot be  GET, PUT, POST, DELETE. I would emphasise again that REST does not expose business services - it exposes some very basic CRUD services. So even if in this case there is no violation of LSP, the essential inconsistency still remains. WOA cannot be REST and SOA at the same time. This inconsistency is a bit worrying. But it is likely that Hinchcliffe meant that WOA is built on REST and is similar to SOA in terms of the goals when he says WOA is the future of SOA. But honestly I could not quite figure out how exactly he would want to describe the relationship between WOA and SOA.

**Resource Oriented Architecture**

The wikipedia page states :


> Resource Oriented Architecture (or, ROA) is a specific set of guidelines of an implementation of the REST architecture.

REST, or Representational State Transfer (see Roy Thomas Fielding's Doctoral Thesis "Architectural Styles and the Design of Network-based Software Architectures"), describes a series of architectural constraints that exemplify how the web's design emerged. Various concrete implementations of these ideas have been created throughout time, but it has been difficult to discuss the REST architecture without blurring the lines between actual software, or the architectural principals behind them.


Since ROA is a set of guidelines of an implementation of a REST architecture, I think its a slam dunk conclusion that REST is consistent with ROA (for the silly reason that ROA seems to be defined using REST :) ).

Per Wikipedia, Leonard Richardson and Sam Ruby further provide the [guidelines for ROA](http://en.wikipedia.org/wiki/Resource_Oriented_Architecture#Guidelines_for_Clarification) in "[RESTful Web Services](http://books.google.com/books?as_isbn=0596529260)", but again since the evolution of ROA stems from REST, it is unsurprising that REST is consistent with ROA.
