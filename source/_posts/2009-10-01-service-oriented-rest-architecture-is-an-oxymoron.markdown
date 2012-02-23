---
date: '2009-10-01 03:04:41'
layout: post
slug: service-oriented-rest-architecture-is-an-oxymoron
status: publish
title: Service oriented REST architecture is an oxymoron
comments: true
wordpress_id: '835'
categories:
- architecture
- rest
- rest-musings
tags:
- soa
- web services
---

It is infrequent for me to react with a level of consternation rather than agreement or disagreement that I felt upon reading [[SOA] Boris on Service, Web and REST](http://www.ebpml.org/blog/200.htm) by Jean-Jacques Dubray. Not because I disagreed strongly with the arguments presented. It is that, I disagree substantially with the assumptions on which these arguments are made. And yet, as I recollect my own thoughts a year ago - a few months post my journey into REST, I realised that there was a time that I did actually believe some of these assumptions. I also realised that it is likely that many others who are dealing with a transition from SOA to REST are also likely to be perhaps sharing similar assumptions. Without much ado let me quickly get to the central assertion of this blog post.

**_Service orientation is neither essential for, nor is it the intention of REST.  
Not only is REST not service oriented, service orientation is irrelevant for REST_**

  
There. But why was it so important to state that ? Allow me to quote from the blog post I referred to.



> I say not surprisingly because RESTafarians have no clear position on "service", they just say REST is the right way to build a Service Oriented Architecture. Yet, REST has no concept of "service" anywhere, just resources and their shiny uniform interface, links and bookmarks. Indeed there are no services in REST. Just read the thesis.



and it further goes on to state



> But I digress, let's go back to "services". Even Bill, in this REST-* proposal is talking about creating a RESTful interface to non RESTful services. That certainly begs the question, how can a service be non RESTful since REST is all about SOA and replaces in its entirety WS-*.



The essential issue here is the flawed assumption that REST attempts to be service oriented or it is all about SOA. Its not. And why so ? Since it is resource oriented. And whats the difference ? Read on, because that's what this post attempts to address.

**Service**

Wikipedia describes a [service](http://en.wikipedia.org/wiki/Service_%28systems_architecture%29) as follows :



> the term service refers to a set of related software functionality, together with the policies that should control their usage.

OASIS (organization) defines service as "a mechanism to enable access to one or more capabilities, where the access is provided using a prescribed interface and is exercised consistent with constraints and policies as specified by the service description."



Now lets attempt to understand a service in a little more dumbed down fashion. Lets hark back to the good old construct of flow charts and process charts. In these charts one basically divided an overall set of functionality into discrete set of functionalities and chained them together through some sequences and decision points. As an example if we were to consider a simplified retail outlet system, it would consist of steps that would support (a) ordering items, (b) receiving and reviewing items, (c) selling items. In a SOA world, these could be mapped into a Ordering Service, Receipt Service and Sales Service (you could of course come up with better names and further decomposition). But each service is essentially one of the decomposed tasks of a larger workflow. If the interface to such service could be standardised and documented it would help it to be reusable across multiple contexts. And to the extent such services are reusable across multiple workflows, the advantage of Service Orientation become obvious. And finally if such a service interfaces are exposed over the web - it is a web service. At the end of the day, each service is a reusable, composable task (or tasklet) performer.

**Resources**

But REST does not attempt to be service oriented. Thats because it does not view the process as a sequence of tasks to be performed. It views it as a sequence of resources under modification. To put it differently, it views the process as a set of actors who exchange resources (or documents for better visualisation) and carry out activities based upon receipt of such resources. Though not as equally apt as a process chart, the analogy here would be a data flow diagram. And what might such resources be ? Well in the above scenario, there's a Purchase Order, a Goods Receipt and an Invoice. Those are the essential abstractions that REST focuses on. These are Resources. Just like Services where there's no one valid set of abstraction of services, one could work out a different set of resources rather than those I listed. But the bottom line is that the essential abstractions are resources *not* services.

**How are they different ?**

You could build a system either way - as services or resources. In terms of being able to successfully build, deploy and maintain a piece of software, both REST and SOA are likely to be equally successful at building the software. But the essential vocabulary through which they decompose their various parts (and therefore describe their interface elements) will be different. And how is that different ?

Let us imagine the ordering service we talked about above. One way to build a SOA ordering service is to establish a interaction procedure which combines an overall protocol and a series of steps (Service API). To reduce potential errors, there is a document upfront which describes in adequate detail how such an interaction should be conducted, what are the data elements to be exchanged at each stage, and what are the necessary sequencing requirements between various steps for such interactions to be concluded successfully (WSDL). The focus here is the tasks being done and the protocol for the task instructions. In case of REST the essential construct will be exchange of one Purchase Order. The purchase order would have sufficient in band instructions about the fact that it is a purchase order and the attributes it has (in-band metadata), and formal documentation if any would be restricted to the structure of the purchase order and its data than than to the sequencing, flow or any protocol level activities. (Thats why sometimes REST looks deceptively simple to be treated as just another CRUD). 

More often than not when called upon to describe a service, the description will describe what the service does, and the service interface will mirror the steps required to perform the activities. Resources on the other hand will simply describe themselves and anyone who looks at a resource description will be none the wiser about what processing exactly happens behind the scene.

Thats why I believe even if both REST and SOA can be used to build software effectively, the essential focus on resources as the central abstraction makes REST much easier to use for the clients. But thats just my opinion - you may form your own.

**You cheated! REST meets the service orientation definitions you listed above**

Yeah, kind of (in theory). There is one way where REST over HTTP is service oriented. Imagine a document service which could store, update, fetch or delete documents. Now replace document with resource in the earlier statement. Thats your typical HTTP service that REST works off of to implement a resource management service - but thats just a single service which is standardised for REST over HTTP.  And all REST implementations will be service oriented to that extent. However the sheer simplicity and ubiquity of this service makes the associated service orientation of REST rather uninteresting and thus largely ignorable.

So next time one wants to debate the merits for REST and/or SOA - feel free to add to the tons of stuff thats already written. But don't measure REST based on service orientation. Service orientation is largely irrelevant for REST. And that per se does not make one better than other - it just makes them different.

_Note:_ There were many other points in the blog post I referred to that I would want to offer different opinions on. But in this case, I believed it was important to keep this post focused on an essential thought that I really wanted to emphasize.



