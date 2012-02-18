---
date: '2009-04-13 11:43:04'
layout: post
slug: a-brush-with-functional-programming-and-scala
status: publish
title: A brush with Functional Programming and Scala
wordpress_id: '631'
categories:
- functional programming
- java
- programming
- python
- scala
---

**Initial Struggles**

A few months ago I was working with using Python for complex data processing scenarios. This was stuff I was very well versed in Java, but was  working hard at to make sure that when I wrote code in Python, it would at least pass as half decent pythonic code. My initial struggles with idiomatic Python, soon became a strong interest in functional programming. I initially considered it to probably have a niche applicability in traditional business transaction processing space and of primary importance in algorithmic code, and thats the state where I almost left it. Curiously even after reaching that conclusion I did persist in attempting to figure out how it could be used in a manner where it would be helpful to have functional programming constructs in typical transaction processing scenarios.

**Is functional programming useful in traditional business transaction processing ?**

One of the aspects of the struggle was that I didn't want to have the functional programming code merely replace the existing code. I wanted to figure out if it would be possible to replace it in a manner where it would make a _substantial_ positive difference, and that hunt proved a little elusive for a while. Another aspect was the fact that the traditional OO code depended so intensively on state, that to actually try to figure out how the same would work under Functional Programming was really requiring a substantial leap in terms of thought, a leap I had to really struggle with. Finally my language of choice was Python, a dynamically typed language, and some of the literature surrounding functional programming really assumed static typing, especially that related to patterns and constructs in Haskell (eg. Monads). Such patterns were difficult to apply since the underlying expected static typing support was simply at odds with a language like Python.

It was after persisting for a few weeks, that I started to see how one could apply these techniques _effectively_. I also started figuring out how to Pythonise constructs such as Monads. Once I was past the initial rather steep hurdles, the going became a lot easier, and the progress much much faster. In at least a subset of some typical financial transaction processing scenarios I did get some extremely encouraging (to myself I call them jaw dropping) results. The resultant code suddenly seemed smaller, far far more intuitive to understand and extremely easy to change.

**Working with Object Oriented and Functional Programming simultaneously**

I am not sure whether it was due to my abilities and experience or due to my inabilities to visualise, I decided that pure functional programming was simply not a choice in this context and thus one would need to combine traditional OO and FP constructs. Having reached that decision, it became a little easier to identify the areas which could be better leveraged by FP. However I believe I still have some ways to go before being able to be confident of writing OO and FP together the way each of them should be.

**Scala**

My focus on functional programming and fondness to Java also led me to investigate Scala. I made the mistake of actually reading the reference documentation on its web site first. My initial reactions were that it simply was a weird syntax language which was unlikely to be of help in reducing development time even when writing FP code. Thankfully, I did not give up at that stage and continued to read a lot more. I soon realised that type inferencing and other aspects (eg. no getter setters) really resulted in some substantial savings compared to traditional Java code (it probably cuts down the code size to half). And the syntax really wasn't so cryptic after all, it just required a day to get used to. Moreover one had the entire arsenal of the the FP constructs along with traditional OO constructs to deploy. I did write some processing code and was very happy at the results I got. In this current state, I am excited about learning more about Scala as a language that would be an alternative to Java. It gives me some of the capabilities of more productive languages such as Python, while continuing to leverage the performance and stability of the JVM. Note that Scala's compatibility with the JVM should be thought of differently from that of dynamic language implementations such as Jython on the JVM. Given Scala's static typing capabilities, its runtime structures are far more similar to Java and thus are able to retain somewhat similar performance and memory footprint characteristics as traditional Java programs. I suspect the same is unlikely to be found for other dynamic languages on the JVM.

**Concluding Thoughts
**

Prior to this entire exercise, in my mind there were two primary individual preferences to programming languages. For most scenarios, my preferred choice was Python especially due to the sheer speed with which one could write and maintain code, code that was really compact and readable. And if Python runtime performance was not going to be good enough, use Java. Coming out of this exercise, it does seem that there is a high potential that I might want to switch my preference to Scala instead of Java. I have already validated the positive benefits of functional programming and Scala coding in processing scenarios (those that do a lot of computation and processing in the background). I do need to validate the same for typical web based applications, but on the face of things, I believe I have already identified areas where FP can help in typical web application scenarios as well. And now I have two multi paradigm (OO and FP) languages to be able to leverage FP in alongside OO - Python and Scala.

This is something thats definitely going to be my interest area for the next few months and I intend to write many more specific posts on the topic. Should that be of any interest keep listening on this blog's [RSS feed](http://feeds.feedburner.com/var/log/mind?format=xml) and my [twitter stream](http://twitter.com/dnene).
