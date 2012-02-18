---
date: '2009-01-10 04:29:17'
layout: post
slug: stop-making-soa-complex
status: publish
title: Stop making SOA complex
wordpress_id: '405'
categories:
- architecture
- software
- web
tags:
- soa
---

Ever since Anne Thomas Manes set off a flurry of blog posts and tweets I was completely amazed at the discussions going around what SOA is. So at the risk of (no, with the very intention to) muddying the water furthers I decided to add my 2 cents knowing fully well that many will choose to disagree. **Update:** I am primarily referring to the definitional issues of SOA not the implementation issues. However even from an implementation perspective SOA is not necessarily complex. The implementation of SOA can range from the simple to the very complex depending upon the choices made from the perspective of the scope, the expectations and the technology. 

The definition of SOA is quite simple - the name is the definition - it is "Service Oriented Architecture". Services are generally software things (I don't want to call it modules or objects or components since that will set off another debate) which offer one small focused set of capabilities through a service API. As a generally accepted set of practices such services are often intended to be coarse grained, reusable in multiple contexts and loosely coupled from each other.  When an architecture is oriented around building something that is built on services it is service oriented architecture. Period.

Now that we have looked at the broad outlines of what SOA is, lets consider what SOA "per se" is not (though these aspects are not antithetical to SOA itself - they are largely orthogonal)




	
  1. **SOA is not about business alone :** SOA can be used for business services. But it can just as easily be used for building technical services. I can fully imagine a network management system which is not a critical offering of a business, yet can be built using SOA. Those who focus on SOA being for business only miss out on the big time benefits that can also be had by applying it to the technical infrastructure.

	
  2. **SOA does not require workflow management / BPM etc. :**These are often relevant, but their existence is irrelevant to whether an architecture is SOA. What is critical is if it is built using and around services. There are a whole range of possibilities of building systems or even system sets without getting anywhere close to Workflow Management or BPM. 

	
  3. **SOA need not even be distributed :**While 99% of SOA systems are likely to used remotable services (ie. distributed, over the network) there is no fundamental mandatory requirement that this be so. One could potentially build a desktop based personal finance application which is built using SOA (though I do not necessarily see the economic prudence in going down such a path) 

	
  4. **SOA does not prescribe messaging or event based processing :**There is no such requirement, however many enterprises choose to have such services in their enterprise mix

	
  5. **SOA is not about web services, REST etc.:** DCE, Tuxedo, CORBA, REST, XML, json etc. are all implementation protocols and/or transports and/or data formats for SOA (I have designed and built systems using each one of them, so at least I am not using their names loosely). SOA doesn't dictate that a particular technology / protocol / transport / data format be used. It is the enterprise architecture which makes the choice. In fact, the most successful SOA transport till date is HTTP. Twitter API which offers data formatted using json or XML or HTML over HTTP is a great example of a "simple service" and to the best of my knowledge its method to post a message gets invoked more than many many million times on a busy day. 



SOA is a classic case of a simple idea being force fitted into a grand panacea. If one goes by the premise that a twitter API is indeed a SOA service, how can one justify the expenditure on training, consulting, outsourcing, software stack purchasing, hardware etc. etc ? Its not that the more advanced things like BPM or WS-* or "business services" are not important. It is just that these ride on SOA - they are not SOA.

So why is there a grand confusion on this issue. IMHO it shouldn't have ideally been about SOA at all. It stems from the following :


	
  1. **Service identification is hard :** Identifying good, clean reusable services is a non trival task. Moreover when attempted at an enterprise level, there are too many stakeholders so agreement on what constitutes a service itself can be hard.

	
  2. **Reuse is powerful. It creates territorial issues :** Once identified, the services do not necessarily lend themselves to neat classification in an organisational structure. Moreover a piece of software that is reusable is worth far more than a silo'ed piece of software. This can lead to turf and clout issues whose resolution falls in the domain of Human Oriented Architecture

	
  3. **The Hype :**Thanks to the hype one now needs to get in an army of consultants, trainers, new software and hardware stacks, simply because it is supposed to be something really new and something big. 



So you essentially have service identification and building which does not necessarily align itself with organisational structures and business functions by default within the context of territorial and ownership issues getting raised while absorbing a whole bunch of new consultants, trainers and vendors. And as a result we hear things like - SOA is difficult, SOA is complex, SOA sucks and most importantly we cannot agree on what SOA is. I am not saying that these problems are not important - its just that laying them at the doorstep of SOA is not being entirely honest.

So can I please put in a humble reminder - SOA defines itself quite nicely - it is Service Oriented Architecture, ie. an architecture built around services. 

And finally, couldn't help but include one of my tweets related to current debate on whether SOA is dead - "Knock, Knock, whos there ? SOA. SOA who ? So-out-of-fashion everyones calling me dead."

