---
date: '2009-11-02 14:49:02'
layout: post
slug: five-important-trends-on-the-enterprise-architects-radar
status: publish
title: Five important trends on the enterprise architect's radar
comments: true
wordpress_id: '912'
categories:
- architecture
- programming
- rest
- software
tags:
- cloud
- nosql
- polyglotism
- rest
---

It is no secret that the internet architectures are influencing enterprise architectures. This post attempts to summarise some of the recent trends in the internet space, which seem to be carrying some momentum sufficient enough to influence the enterprise. So without further ado, these trends are :




	
  1. **REST :** The [Representational State Transfer](http://en.wikipedia.org/wiki/Representational_State_Transfer) architecture style builds on the essential elements of those constructs which made the internet so globally scalable. A detailed explanation of the rationale and strengths of REST are completely beyond the scope of this article. If your job requires you to be continuously aware of emergent trends and whether they fit your enterprise architecture needs - this is the one must explore trend.

_Impact : _ Web based architectures, Service Oriented Architectures, wide availability and immediate usability of data and processing requests (resources) through simple HTTP URIs and minimal integration effort

	
  2. **Interoperable Cloud :** The interoperable cloud is the ability to create a private cloud and also leverage a public cloud. This has been made possible by offerings such as the [Ubuntu Enterprise Cloud](http://www.ubuntu.com/cloud) which allows you to build a private cloud or use a public cloud such as [Amazon EC2](http://aws.amazon.com/ec2/) while being able to access them using the same set of APIs thanks to open source efforts such as [Eucalyptus](http://open.eucalyptus.com/). This allows you the flexibility of initially using either a private or public cloud and then subsequently shifting to the other, or being able to use both simultaneously. 

_Impact : _ Large servers vs cluster of commodity servers, virtualisation, elastic deployments, flexible hardware procurement / provisioning, infrastructure management in organisational hierarchy.

	
  3. **NoSQL :** While I am unhappy with the name, it has stuck. This refers to a set of options now available to store your data unconstrained by many RDBMS requirements (eg. flexible schema, key value pairs etc.). Some of the databases also allow you to store data in a distributed manner over a number of servers with an intent to support high availability in write intense scenarios even as they may require you to move towards eventual consistency. These options increase your manouverability / flexibility as an architect even as they require you to meet a different set of challenges.

_Impact :_ Relational databases, data storage strategies, data distribution strategies, vertical vs. horizontal scalability, transactionalisation, consistency and availability


	
  4. **Polyglotism :** Developer costs now occupy an increasing percentage of total costs, development time is being an increasingly dominant factor for time to market, and ability of software to change and adapt quickly to newer demands is now a critical success metric. One of the solutions is to write different parts of the software in a different languages most appropriately suited for concise and rapid coding as well as supporting quick reaction changes to each part appropriately. Thus it is conceivable to have some of the business rules written in a dsl written using jruby and some of the algorithms written in clojure in a software built on the JEE platform. 

_Impact :_Development culture and processes, minimum developer skill and scalability, risk management for managing required vs. available skills.

	
  5. **Decentralised processing :** Thanks to many developments which are leading to increasingly distributed processing including REST and NoSQL, applications will need to be a set of collaborating network based components (we've heard this before with distributed objects as well). However especially given some of the lesser guarantees that such architectures can provide around immediate guaranteed processing, latency issues, distributed control and asynchronous processing, a particular piece of business logic may get satisfied in a staggered fashion across a number of collaborating components. This may increase challenges in terms of currency of available data even as it helps actually deliver on the vision of distributed objects and simplifies individual component development. While asynchronous capabilities such as those supported by MQ series and the like have been used in the enterprise for ages, I do anticipate increasing use of lighter messaging constructs such as [PubSubHubbub](http://en.wikipedia.org/wiki/PubSubHubbub) within the enterprise. 

_Impact :_ Application partitioning, network based components, difficulty in supporting fully synchronous workflows.





