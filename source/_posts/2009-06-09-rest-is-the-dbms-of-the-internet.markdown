---
date: '2009-06-09 21:02:26'
layout: post
slug: rest-is-the-dbms-of-the-internet
status: publish
title: REST is the DBMS of the Internet
wordpress_id: '675'
categories:
- architecture
- programming
- rest
- rest-musings
- software
- web
tags:
- rest
---

After my fortunately rather successful post "[Why REST ?](http://blog.dhananjaynene.com/2009/06/why-rest/)", I had planned to write another longish followup roughly titled "Implications of REST on software design and frameworks". However I had an interesting exchange of Twitter DMs (direct messages) after the post which gave me the right words I was looking for to summarise this impact on software design. This simple example was so compelling, that I decided to make that into an independent post and delay the "Implications .." post by another couple of days. So at the risk of giving away the very essence of my subsequent post, here's the summary.

The Twitter DM's exchanged were as follows :


> _@sbidwai :_ is REST like object oriented implementation for services, where as SOA is procedural ? thinking loud..




> _@dnene :_ a slightly better analogy would be SOA is like invoking stored procedures, whereas REST is like invoking SQL on the table




> _@sbidwai :_ agreed in parts.. but most impl will hv much more than CRUD.. eg, twitter rest apis..*




> _@dnene : _CRUD is the interface. To extend the analogy, logic is implemented as triggers not SPs. (My Opinion)


_* CRUD in our shared vocabulary stands for Create, Read Update, Delete._

As I subconsciously wrote the last DM, it suddenly dawned on me that this was the one concise way to express how REST architectures would impact software designs.

To summarise the exchange differently

"**_If WS-* is the RPC of the Internet, REST is the DBMS of the internet_**"

To expand on it a bit more :

Traditional SOA based integration visualises different software artifacts being able to interact with each other through procedures or methods. REST effectively allows each software artifact to behave as a set of tables, and these artifacts talk to each other using SELECT, INSERT, UPDATE and DELETE. (or if you wish GET, PUT, POST, DELETE). And where exactly is is the business logic ? Is it in the stored procedures ? Not Quite. Its in the triggers.
