---
date: '2009-01-27 06:42:59'
layout: post
slug: software-it-terms-in-early-stages-of-abuse-or-ripe-for-abuse
status: publish
title: Software / IT Terms in early stages of abuse or ripe for Abuse
wordpress_id: '432'
categories:
- software
tags:
- agile
- cloud
- rest
- saas
- soa
- web 2.0
---

**Abuse :**_ improper or excessive use or treatment (Merriam Websters)_

Ahh! That is a term we haven't found being used in IT context too often. Have we ?

**Abuse in the past**

Well let us start with some terms that have been used, and in my opinion abused in the past :

**[Web 2.0](http://en.wikipedia.org/wiki/Web_2.0) :** If there was indeed a term that actually set itself up for abuse this was it. There was nothing in it for people to refer to and hold on to as a basic and essential premise around which it gets applied. Eventually Tim O'Reilly the author of the term had to come out with a clarification [What is Web 2.0](http://www.oreillynet.com/pub/a/oreilly/tim/news/2005/09/30/what-is-web-20.html), in which he said it refers to the _Design Patterns and Business Models for the Next Generation of Software_. Amongst the many trends he pointed out were



	
  * The Web as a Platform

	
  * Harnessing Collective Intelligence

	
  * Data is the next Intel Inside

	
  * End of the software release cycle

	
  * Lightweight programming models

	
  * Rich User Experience


He also defined the core competencies of the Web 2.0 companies

	
  * Services, not packaged software, with cost-effective scalability

	
  * Control over unique, hard-to-recreate data sources that get richer as more people use them

	
  * Trusting users as co-developers

	
  * Harnessing collective intelligence

	
  * Leveraging the long tail through customer self-service

	
  * Software above the level of a single device

	
  * Lightweight user interfaces, development models, AND business models


Did Web 2.0 get abused. You bet. In many ways it was too clearly defined. But inherent in the number of attributes that were associated with Web 2.0 was the risk that anything sharing but a fraction of the attributes would want to call itself Web 2.0 (given the popularity of the term). Moreover it was actually hard not to accidentally abuse it for the same reason. However people have got around that eventually and now treat Web 2.0 as simply a moniker enhancer, and then evaluate the application or company to figure out whether it indeed is Web 2.0. The overuse of the term is leading to it becoming less relevant today

[Service Oriented Architecture (SOA)](http://en.wikipedia.org/wiki/Service_oriented_architecture) : While I'm pretty sure this term has been around since early 90's and especially used in the CORBA and Tuxedo worlds, the earliest online web page I could find which described it is "[Service Oriented" Architectures, Part 1](http://www.gartner.com/DisplayDocument?doc_cd=29201)" which describes it as _A service-oriented architecture is a style of multitier computing that helps organizations share logic and data among multiple applications and usage modes._. A very ironic article was "[BEA Fits Tuxedo With SOA Makeover](http://www.internetnews.com/ent-news/article.php/3520731)" especially considering that CORBA and Tuxedo users were probably amongst the earliest proponents of the term. Instead of seeing SOAP-WS* as one more element in a continuum of possibilities, it became the "in thing" which got weighted down by so many promises, expectations, goals and so much scope creep to its perceived capabilities. Herein lay the seeds of the disappointment one sees today especially in situations when it is doing rather well. 

**Lifecycle of abuse :**
The following are the stages through which a term that is subject to abuse goes through :



	
  * Initial formulation of seed thought and early implementations or practices.

	
  * Further refinement of thought into a notion or hypothesis that has been tested and refined through a few experiences.

	
  * Success visibility by neighbours leading to greater chatter along with active promotion of the term and its benefits.

	
  * A positive spiral of both field successes and good PR leading to a strong mindshare

	
  * High mindshare attracts a large and diverse community of "second stage thought movers"

	
  * Desire to brand or market oneself in terms of using that term to sound "in" or "leading edge"

	
  * Attempts to reevaluate the terms meanings in one's own context to achieve the best fitment leading to a term meaning modification (either newer attributes being introduced or existing attributes associated with the term getting diluted)



A term's essential value proposition is that it manages to convey one sometimes composite and sometimes complex notion succinctly. Yet successful people, successful companies, and successful terms act as great magnets. Once a term is successful, everyone attempts to use it in their own context. This is when the term starts becoming different things to different people. Thats when it goes down the [Blind men and an elephant](http://en.wikipedia.org/wiki/Blind_Men_and_an_Elephant) path. Thats when the term loses its core value proposition, and people using it get steadily more confused over a period of time. One way to avoid this disintegration is to be aware of the process and attempt to slow it down by drawing attention to the core notion and value proposition of the term. 

Similar to the terms above, I believe other terms are now getting ripe for being abused. However sometimes abuse leads to improper expectations and sometimes even to "<Term> is Dead" which is actually an indication of the terms abuse dying rather than its basic set of offerings which continue to do well. A couple of times I have blogged about things *not* dying are [SOA ain’t dead but it certainly is transforming](http://blog.dhananjaynene.com/2009/01/soa-aint-dead-but-it-certainly-is-transforming/) and [Java : the perpetually undead language](http://blog.dhananjaynene.com/2008/12/java-the-perpetually-undead-language/). 

I do see many terms in Software development and IT that are in the stage where they are being abused or are starting to be abused. So before we have to bury them eventually for not meeting the expectations such abuse creates, can we rein in the abuse. I doubt it, but this post is an effort in that direction.

**[REST](http://en.wikipedia.org/wiki/REST) :** This one theoretically should be the least likely to be abused. Simply because it is so well defined in [Roy Fielding's Dissertation](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). But even he had to eventually write in his post [REST APIs must be hypertext-driven](http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven). 



> I am getting frustrated by the number of people calling any HTTP-based interface a REST API. Today’s example is the SocialSite REST API. That is RPC. It screams RPC. There is so much coupling on display that it should be given an X rating.

What needs to be done to make the REST architectural style clear on the notion that hypertext is a constraint? In other words, if the engine of application state (and hence the API) is not being driven by hypertext, then it cannot be RESTful and cannot be a REST API. Period. Is there some broken manual somewhere that needs to be fixed?



Strong words indeed. And if you think if these were against some small application, the social site REST API is the OpenSocial REST API which is a rather prominent standard (and its kind of hard to imagine google and other participants wouldn't realise the distinction). Yet the underlying shift is that for internet based applications people are increasingly relying on simple HTTP based API and since perhaps HTTP is too bland a name to term an API with, they go with the sexier sounding REST. Not really necessary. If I have an HTTP API, its all right to call it an HTTP API. Another term that did start getting used alternatively was POX/HTTP (Plain Old XML over HTTP) but the slight issue with this term is that JSON is starting to be another data payload vehicle so POX doesn't necessarily cut it universally. 




There is another issue as well with using REST as the moniker to describe HTTP based APIs. Roy Fielding eloquently points out that REST is so different from RPC. Yet in a logical sense, many requirements for HTTP APIs are very RPC like. It is possible to represent such an API using REST semantics (instead of user/delete/123 call it userdeletion/create/123 .. a name mangling trick). Moreover REST requires statelessness, a requirement that not all applications necessarily comply with. I am not suggesting they should, just that should they choose to not be completely stateless it would be difficult to term their APIs REST. These were both issues I had referred to in my earlier post [Fomenting unREST : Is RESTfulness a semantics game ? Why does REST require statelessness ?](http://blog.dhananjaynene.com/2008/11/rest-fomenting-unrest-is-restfulness-a-semantics-game-why-does-rest-require-statelessness/).

Clearly REST is not going to cut it as a universal mechanism of describing simple HTTP based APIs. I would suggest call them HTTP APIs, or perhaps think of a better term which is not REST since it seems rather likely that many of the HTTP APIs we shall develop may simply not meet the requirements or expectations of REST. The more we use REST the way it should be, the less we are likely to abuse it. So may I request that we use REST only when the API adheres to all the expectations as specified by Roy Fielding ?

**[Cloud](http://en.wikipedia.org/wiki/Cloud_computing) :**

I am certain the term cloud began thanks to the abstraction of internet that got used in tons of network topology diagrams. So it perhaps referred to the segments of the network that formed the entire plumbing of the internet and all the external network segments which were not relevant to the topology under discussion, and to the applications and services that resided on these "external" network segments (primarily the internet). However further along the way additional attributes started getting implied. An example is the rapidly scalable infrastructure (the scale on demand attribute). This blog is hosted on a pre specified hardware by a web hosting company. Is it on the cloud ? Its a little unclear (for certainly it does not have the scale on demand attribute). Occasionally implied attributes are multi-tenancy. IMO thats stretching it too far, multi tenancy is an optional attribute of applications and services that can be hosted on the cloud, it isn't the attribute of the cloud itself. It would be better to keep the scale on demand attribute away from the cloud term and use it as an additional qualifier (eg. elastic cloud). However one of the effects of that is that it is leading to a [proliferation of cloud related terms](http://itknowledgeexchange.techtarget.com/overheard/overheard-the-new-vocabulary-of-cloud-computing-glossary/) which is if you pardon my pun, making the situation a little too cloudy. Some of the terms are for lack of a more civilised response quite amusing eg. "Internal Cloud". I mean if cloud represents the external segments of the network and the application and services that are hosted on them, why would one call the internal network the internal cloud as if the external network would be the external cloud. But then whats the difference between the network and the cloud. Or does the network becomes the cloud or should it be that the cloud becomes the network. This highly avoidable tendency of clouding everything also has a cloudy term for itself  cloudwashing - slapping the word “cloud” on products and services you already have. See the point ? Having said that, I still think it is preferred to have a proliferation of qualified terms rather than a proliferation of contextually implied attributes into a single term itself. 

So can we keep cloud restricted to the "external network segments and the applications and services hosted on them" ? Thank You. And lets keep out an eye on all the additional terms that further qualify cloud. Many of them might not make sense, so just decloud them.


*******-as-a-service :**

It all probably began with [Software as a Service (Saas)](http://en.wikipedia.org/wiki/Software_as_a_service). Saas rocks and is a term which is catching on. But it is also now getting a ton of other "as a service" followers. Just google for this and the following words crop up as the first part of -as-a-service : platform, desktop, hardware, desktop, sql, data, ui, infrastructure, PC, research, os, malware, identity management, analytics, middleware ... get the picture ? Now in this current global economy where more than a third of the economy is based on services, just imagine the sheer proliferation of terms that are likely to happen simply because someone doesn't say I offer hosted middleware, no it is middleware as a service. This blog will recast itself as Gratis chronological opinion as a service. People are going nuts, and there's no one to stop them. In the meanwhile whats happening to the one which started it all - SAAS ? Theres something called Level 1 SAAS where each customer has its own customized version of the hosted application and runs its own instance of the application on the host's servers. I have read at least one blog post which refers to ISVs needing to customise Saas offerings for individual enterprise customers. So rather than newer implied attributes being added to the term Saas, existing ones are being removed in a manner where any outsourced software hosting will be Saas. Pretty soon, a business unit which outsources software development, maintenance and hosting to another intra corporate business unit will start calling it a Saas offering to keep up with the pressure to seem current. Pretty soon all software will tend to Saas.

Can we please keep Saas restricted to uncustomized offerings made by other service providers which essentially rent out the use of the software they've built or own on an as-is basis (with version upgrades from time to time of course) ?

**[Mashups](http://en.wikipedia.org/wiki/Mashup_(web_application_hybrid)) :**

Lets just for a moment look at how wikipedia describes Mashups.



> a mashup is a web application that combines data from more than one source into a single integrated tool.



I see a number of enterprise vendors starting to offer Mashup products. That in itself is welcome. I however also anticipate a full wave of rebranding of Integration Services, SOA, Portal Servers into Mashups in addition to the few new mashup features being introduced. Also expect such buzzwords to drive newer budgets and customer excitement even though relatively little has changed under the covers. Eventually the term could become all encompassing where it covers all forms of system integration and service aggregation which will eventually make it useless as a term. In the meanwhile authors of existing Web 2.0 Mashups built on Google Maps, Flickr and Twitter will be left wondering, "Hmm.. was this was mashups was all about ?"


**[Agile](http://en.wikipedia.org/wiki/Agile_software_development) :** I don't know that its terribly abused, but when I see presentations that combine elements of Agile with layered sequences (eg documented design followed by review followed by development followed by testing) and heavy management control procedures and call it the right way of doing agile - it smacks of abuse to me. Thats like taking a cheetah, cladding him with medieval knight armour, and saying ain't it a very nice and strong fastest animal on earth ? I don't think it is feasible to adapt a heavy process to agile or vice-versa to somehow take on the attribute of being agile. Agile requires a migration from processes which encourages repeatability through documentation, division of work and control into one which achieves high productivity and quality through capability enhancement. This requires a reduction in the control processes and an increase in the capabilities training in a manner where the quality and control is not maintained through processes built around a replaceable workforce, but instead through a set of (relatively) informal and frequent communication channels built around a increasingly capable team. 

_A lighter parting note._

Finally on a lighter note a friend suggested another term that is being abused.  "Software engineering - a Term that should never have been applied to software and insults engineering." I agree even though I know the comment probably stemmed from the usage of this word in this blog's tagline. While Software Craftsmanship has been discussed for long, I find many software building efforts not worthy of being called craftsmanship. Therefore I contribute a new term to the lexicon - Software Construction And Maintenance. This is one term (or its acronym) I am not concerned of getting abused. In fact it is unlikely to get used.





