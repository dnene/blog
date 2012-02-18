---
date: '2008-06-10 13:43:55'
layout: post
slug: beware-of-polyglot-programming
status: publish
title: Beware of polyglot programming
wordpress_id: '31'
categories:
- java
- software
---

Seems like a nice term doesn't it - Polyglot programming. Some recent posts related to it are [Kill Java, Vol. 2](http://www.jroller.com/aalmiray/entry/kill_java_vol_2) and [Fractal Programming](http://ola-bini.blogspot.com/2008/06/fractal-programming.html) It may seem hot, it may seem in and it may make sense for you. However please allow me to lay down the fine print.

Polyglot programming requires to build bridges across computer languages. These bridges act like translators at the UN. If you are attempting to run the UN you need the translators, you need to accept the cost. But make sure you understand that there is a cost, and justify to yourself that you really need to incur it.

Let me give you a case in point - JSP Tag Libraries. These were meant to help the presentation / UI guys write the cool UIs while allowing the java programmers to focus on the other interesting stuff which was much better done in Java. The Tag Library construct was then a bridge between HTML and Java. Did you ever try to measure the runtime performance cost of tag libraries ? I did and it turned out that for all but the completely  non trivial tag libraries - the cost was too high. I was able to run the controller logic, lookup the data from the cache, update the data in the cache (basically the entire lifecycle of a transaction except for the final updates) and sometimes finish the database updates as well, all in much much lesser time than the time it required to move the data across the tag library when presenting the data. I had a simple rule - if you need screaming concurrency and thruput from a small box - do not touch tag libraries. I however did use tag libraries when the performance demands weren't so high.

Another example of such bridges being sold without proper warnings - EJB v 1.0. The fine print did talk about the remote API costs, but the general paradigm encouraged using the EJB APIs to communicate between the session beans and the entity beans. History does tell us this performance sucked too!

If you work with JNI you will soon realise that a lot of data transformation needs to happen when moving complex data structures between C and Java. Again while much faster than exchanging XML documents over inter process pipelines, this can be expensive too.

Suggestion : focus on the granularity and the triviality of what you are doing. If the communication across the different languages is fine grained or too frequent sit up, take notice and be careful. If the code in the other language (the language that is providing the service and thus is being called) is too small or too trivial, again ask yourself whether this is really required.

Like it or not we are already polyglots. We do DHTML + CSS + Javascript + (Java / Python / Name your Language). It makes sense to be polyglottish. Treat polyglottism as a tool in your toolbox and not as a fashion statement. _Keep the kids in mind and especially when showing them how to move between the static and dynamic programming languages - run your benchmarks and lay down the caveats_. There is indeed a possibility that reckless application of the paradigm might lead you (more likely your unsuspecting readers) to a crawling system - something the users will describe as a PolyClot.  :)
