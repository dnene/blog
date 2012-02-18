---
date: '2009-06-12 05:46:41'
layout: post
slug: musings-on-rest
status: publish
title: Musings on REST
wordpress_id: '710'
categories:
- architecture
- rest
- rest-musings
tags:
- rest-musings
---

This is a summarisation of a four part series of posts I wrote on REST over the past week. This post lists each of them along with a very high level summary and a small snippet from each hopefully sufficient enough to tickle your thoughts and interests.



	
  1. **[Why REST ?](http://blog.dhananjaynene.com/2009/06/why-rest/)**This is a rather long post which provides a narrative of the history of web and service architectures eventually coming together into web services. It refers to many of the strengths that made web architectures so omnipresent, and to the uneasy coming together of the two architectures as web services. It details many REST characteristics, describes how REST provided a style by which the strengths of the web architectures could be retained even as the processing aspects of service architectures could be supported, and finally enumerates some of the benefits of REST.

**On the coming together of Web and Service oriented architectures.**


> Clearly as WWW started getting used far more, people were only too keen to use it for much more than storing or retrieving documents. This led to the development of CGI and subsequently other dynamic web application technologies (eg. LAMP, J2EE etc.) which would allow us to use the web to ‘do something’. Since these were clearly offshoots of the SOA world, being mapped onto the WWW infrastructure, the characteristics of such dynamic applications often had a lot in common with SOA, and they started dropping many characteristics of the traditional static WWW. Thus was born the child of the world wide web and distributed service oriented architectures – web services. This led to newer SOA technologies such as WS-* and SOAP.

Like the typical scenarios after the discovery of any highly profitable opportunity, the early rush was to leverage the opportunity and it was only a little later when the dust died down, that people started wondering if they had sacrificed something in the heat and dust of the moment. That stock taking resulted in the realisation, that some of the very basic characteristics of the extraordinarily successful internet technologies (FTP / SMTP / WWW) had been diluted, and even if such dilution still allowed immediate progress to have occurred, some of them would need to be corrected to be able to continue the explosive growth that had been seen so far. One such exercise in my opinion is the laying down of the REST architecture style.


**On the aspect that even though REST is not as feature rich as SOA, its strength is the simpler abstractions it employs**


> I have generally found that simpler abstractions even though harder to deal with initially, often win in the long run. Notice the fact that the bare bones rendering functionality of HTML/WWW completely trounced the rich UI and application integration capabilities then available (eg. Windows/Java and DCOM/CORBA/RMI). This is not to suggest that the extra capabilities are not required. That is why Rich User Interfaces on WWW continue to be a dominant part of the internet technology wishlist. However the simpler, cleaner and minimalistic abstractions often are far more important than feature richness. A point I would want to make in favour of REST even as I admit that conventional SOA technologies are far more feature rich than REST.




	
  2. **[REST is the DBMS of the Internet](http://blog.dhananjaynene.com/2009/06/rest-is-the-dbms-of-the-internet/)**This post reflects the thought that since REST effective allows one to GET, PUT, POST and DELETE resources, it is similar to being a database which exposes its tables to many applications to SELECT, INSERT, UPDATE and DELETE from. In this analogy, each media type is effectively a new table and the REST interface is primarily of the nature of allowing basic operations on a set of tables (resources).


> To summarise the exchange differently
_
“If WS-* is the RPC of the Internet, REST is the DBMS of the internet“_

To expand on it a bit more :

Traditional SOA based integration visualises different software artifacts being able to interact with each other through procedures or methods. REST effectively allows each software artifact to behave as a set of tables, and these artifacts talk to each other using SELECT, INSERT, UPDATE and DELETE. (or if you wish GET, PUT, POST, DELETE). And where exactly is is the business logic ? Is it in the stored procedures ? Not Quite. Its in the triggers.




	
  3. **[Design Characteristics of REST / Resource Oriented Server Frameworks and Clients](http://blog.dhananjaynene.com/2009/06/design-characteristics-of-rest-resource-oriented-server-frameworks-and-clients/)**This post dwells into the many aspects of design of a REST or Resource Oriented Serverside Framework and attempts to enumerate a large number of their characteristics. One of the aspects it brings up is the role of the Controller. Since the a resource oriented interface primarily consists of basic primitive operations on resources, it suggests that the controller could either be merged with the Resource or support only basic operations and have a one to one relationship with a resource.


> This is where a potential differences with conventional frameworks arise. If I was to think of it from an EJB like perspective, I would model a OrderController as a Session bean and a Order as an entity bean. In case of lightweight POJO based model, I would have an OrderController as the endpoint exposed by say using Struts and model the Order as a entity POJO and map it to the database using Hibernate. In other non java frameworks, I would have a class to represent an OrderController and another one to represent the order along ActiveRecord pattern. But I would argue this separation is not entirely necessary, since what we want is something that implements a single abstraction mapping onto a Resource which also support the primarily lifecycle methods or resource operations of GET, PUT, POST and DELETE. But there is an issue to be worked through here. These resource operations are actually class level and not object level methods. Thus if we have an abstraction to represent the resource instance, the class level methods cannot be defined in the same class except as class level (static) methods. This is a tricky problem, and I would submit the designer may make one of two choices (a) Implement the resource operations as class level methods on the Resource abstraction (ie. they will get or return the resource references as method parameters and not rely on the ‘this’ or ’self’ qualifier for getting access to the resource variables or (b) Implement the resource operations as methods on a separate one-to-one mapped class on the resource abstraction (eg. an OrderHome in case of an EJB like analogy)


Again to extend the analogy of a DBMS, it argues that instead of a lot of logic being in the controllers which are the entry points of the interface (stored procedures), the interface now changes to support basic operations on the resources (tables) and that the logic could perhaps be modeled in a separate class of handler functions (triggers).


> Before I get into the details of this, I encourage you to take a look at my earlier post REST is the DBMS of the internet in case you have not already done so. To summarise it quickly, I have drawn the analogy that a REST based system is like a DBMS where client applications can perform direct SQL such as SELECT, INSERT, UPDATE, DELETE (GET, PUT, POST, DELETE in case of HTTP/REST) on the Tables (Resources in case of REST), and the business logic is implemented as triggers. Thus the framework will need to allow the developer to define such triggers. Such methods will need to support ability to reject the request (in case of downstream validation failures), and update the resource state (to reflect the appropriate resource state after the completion of the downstream processing). It is also feasible to imagine scenarios where such methods are triggered asynchronously. Much of the logic of the traditional controllers which controlled interactions across multiple objects etc. is likely to now be shifted into these methods. I have no particularly good name for such methods. They could be referred to as triggers, event or message handlers, glue methods, extension points etc. For the rest of this post I shall refer to these methods specifically as ‘handlers’.




	
  4. **[ReST : SOA, WOA or ROA ?](http://blog.dhananjaynene.com/2009/06/rest-soa-woa-or-roa/)**This post dwells on how consistent REST is with Service, Web and Resource oriented architectures. It argues that REST could perhaps be argued to be SOA only in a most specific form, and that for all practical purposes REST should not be expressed as SOA.


> And these constraints make the field of use so narrow, that even though REST could be argued to be a teeny weeny specific use case of SOA, it could be argued to be Service Oriented to the same extent that a Database could be argued to be Procedure Oriented (since all tables support the procedures SELECT, INSERT, UPDATE, DELETE). In other words for all practical purposes REST is not Service Oriented.


It brings out some potential inconsistencies in how WOA is currently not only portrayed to be a set of architectural elements in addition to REST but also as a extension of / future of SOA.


> My assessment is that if WOA is a collection of Web related architecture elements in addition to REST, then the only way to successfully and consistently resolve it is by saying if WOA builds on REST then it cannot be simultaneously extending SOA.


...


> So even if in this case there is no violation of LSP, the essential inconsistency still remains. WOA cannot be REST and SOA at the same time. This inconsistency is a bit worrying.


Finally it refers to ROA as the architecture (which actually is defined with REST as the basis) with which it is most consistent with.


> Since ROA is a set of guidelines of an implementation of a REST architecture, I think its a slam dunk conclusion that REST is consistent with ROA (for the silly reason that ROA seems to be defined using REST :) ).

Per Wikipedia, Leonard Richardson and Sam Ruby further provide the guidelines for ROA in “RESTful Web Services“, but again since the evolution of ROA stems from REST, it is unsurprising that REST is consistent with ROA.





