---
date: '2009-08-17 21:03:44'
layout: post
slug: why-should-i-switch-to-scala
status: publish
title: Why should I switch to Scala ?
comments: true
wordpress_id: '815'
categories:
- java
- management
- programming
- scala
---

_This post is a role-play and does not reflect my individual opinion about scala accurately. I am convinced about the capabilities and features of Scala along with the fact that it deserves the mantle of a long term replacement for Java. However language adoption goes beyond technical capabilities, and this post is a speculation on what a typical manager might be dealing with when attempting to decide whether to switch to Scala._



* * *




So I have been reading a lot about Scala lately and even opinions about how it will be a [long term replacement for Java](http://macstrac.blogspot.com/2009/04/scala-as-long-term-replacement-for.html). I've also read some interesting writeups about Scala adoption such as [On Scala's Future](http://antoniocangiano.com/2009/08/07/on-scalas-future/) and [A Tipping Point for Scala](http://fupeg.blogspot.com/2009/08/tipping-point-for-scala.html). While I used to code a lot, my responsibilities today require me to interact with and address a lot of issues including those faced by our customers, our development teams and also engage with my peers and superiors on many other difficulties bedeviling our organisation. This gives me little time to try out Scala. I know I should be doing that, but sincerely I do not have the time. So I rely on the feedback of my team, the trade journals and other influential architects within and outside my organisation.

I have heard about many developers switching from Java to Python / Ruby. However I have heard of relatively only a smaller number of large Java shops which have done the shift - most of the switch stories I've heard reflect a smaller sized teams. I can feel the excitement Scala has generated amongst the development teams - the brevity, the functional programming model introduction, the exciting stuff being done concurrently et al. I have no doubt that, given so much excitement it must really be a good language. 

To introduce my organisation - it is one of those shops which service many projects concurrently. Given the tremendous business and growth, I must confess we do not always have the luxury of being able to hire the most top notch talent. We do have a lot of projects we use Java for, and thats a language our customers are comfortable with. I've had some of the senior people check out Scala to gain some feedback into the language. But at this stage I must say I am inclined to evaluate the shift but not convinced enough to do so. I am sure that I could if convinced drive the change to Scala incrementally.  However my fear stems from the fact that if things don't turn out well, despite all the great advice I've received - its going to be my rear end on the line. So here's some of my concerns regarding evaluating the shift to Scala and there are many of them, so some of you might be able to help me through this thought process.




	
  * **Functional Programming : ** I'm sure in many ways it rocks. But my guys tell me they are not sure how to use it in the typical bread and butter applications which read from database, do some processing and write back to the database. Does Functional Programming help me in this context ? Will my team scale into being able to write functions with no side effects assuming thats a desirable goal ? What if they tie themselves up in knots and my release to the customer is risked ? I can't afford that. Is functional programming even desirable in such contexts ? So I am not sure if in these contexts I should just ditch functional programming and work with just normal imperative programming capabilities of Scala. I am so confused, and afraid.

	
  * **Different Syntax :** While Scala runs on the JRE, its syntax is very different from Java. From what I could gather, it is much easier for a Java programmer to read (make sense of) simple Python code than to read Scala code. Is it true ? So even if I do get compatibility in terms of the runtime environment, would I be picking up a language that is syntactically so different a language that it would involve a substantial relearning curve ? I remember when we had to learn Java and Javascript. For better or for worse these were indeed relatively minor modifications of the C/C++ syntax, compared to what I sense as the syntactic shift between Java and Scala. Am I wrong ? If so, could you help point me to resources which help me understand that Scala code is not much different than Java ?

	
  * **Sample code :** Guys, I need your help. I need to see some good sample code. Some code which reflects how a typical application is architected, designed and programmed in Scala. And I don't need it for a complex multi threaded actor based processing - I just need to see simple J2EE server based departmental applications maybe a simple recruitment tracking or library maintenance application. If I find a good one, I'll just take it and give it to my team and say - there, thats how we're largely going to build it, and even if we make a few changes along the way we at least have a reasonable template that we can build from.

	
  * **Dumbed down environment :** I remember my great adventures with C and vi and make. But my team today is very different. They want great IDEs. They must have syntax highlighting, autocompletion and nice refactoring capabilities. If I ask them to move, some of them might be excited about the change and be willing to overcome these short term hurdles. But there are some of them who will not be keen to do so and may be disinclined to support such a shift. And at the end of the day my ability to conduct this shift is a function of my ability to carry a large proportion of them along with me. Even when I considered a shift from svn to git, the IDE support was a big issue even though quite obviously git capabilities were really exciting. I couldn't push along that change, and in this case we are talking of changing the language.

	
  * **Is this a good time to shift to Scala ? ** I remember the early adopters of Java from 1996 thru 2001. While they gained a lot of experience, JRE and J2EE really matured only post JRE 1.3. Scala seems to be coming out with so many enhancements so fast, I am not sure if it has stabilised. I am told there is a 2.8 coming out in a few months. So if I train my team and Scala continues to change rapidly will I have to keep on retraining my team regularly ? And what about the customers I take to production. Will the frequent upgrades mean I end up supporting multiple customers on multiple versions of Scala ? Maybe Scala is stable but it would be helpful for someone important enough to make a clear statement that there are no new major shifts anticipated anytime soon and that these version shifts are likely to be no faster than the JRE version upgrades (which were fast enough).

	
  * **Support from peers and superiors :** I remember the day I decided to shift to Java. What made the move easy for me was the sheer fact that Java was a big paradigm leap away from the then dominant C++. Not only was it cross platform with binary compatibility thrown in for good measure, Sun ensured that it made all the right noises to appeal to the enterprise architects and all the business managers. I see the senior developers in my team clamouring for the shift to Scala, but my peer managers and my superiors don't display even the fraction of the enthusiasm they displayed during the Java shift. The implication for me is that the risk cover I get when I order the shift is far lesser than what I had when I made the move to Java. Which means if things don't quite work out well, I'm really going to be screwed.

	
  * **Business friendliness : ** I understand all the nice talk about the technical excellence of Scala. But I really need to translate all these great language features into a projected ROI that I can use to convince others about. So I would like to see actual case studies of applications that were moved to Scala and what impact it had on the time and cost so that I can use it to compute my ROI. And what scares me is that learning curve may risk the initial applications long enough to push my breakeven point of shifting to Scala well beyond a 12 month and perhaps even a 24 month period. I fear things might not be as difficult but in absence of known studies, I am likely to lean towards projecting a worst case scenario rather than an optimistic one.



So folks, I am asking for your help. And while a lot of you may think that people like us who balk at the thought of limited IDE support are wimps, please remember that 80% of us don't fit into the top 20%. And if you would like Scala to be popular, you need us as much as we need you. And if you are not too sure, please remember Lisp and Smalltalk are great languages as well.

