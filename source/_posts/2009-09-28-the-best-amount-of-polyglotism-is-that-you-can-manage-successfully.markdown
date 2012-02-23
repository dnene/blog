---
date: '2009-09-28 18:35:20'
layout: post
slug: the-best-amount-of-polyglotism-is-that-you-can-manage-successfully
status: publish
title: The best amount of polyglotism is that you can manage successfully
comments: true
wordpress_id: '829'
categories:
- management
- programming
tags:
- polyglot
- polyglotism
---

Polyglotism was one topic I got into some discussions on twitter earlier this week. So this is to just pen down some of my thoughts on the same. Polyglotism is increasingly being talked about, ever more so since Java started moving from the realm of being a language into a runtime environment to host a number of languages. Yet polyglotism is not new. Most programmers have been mixing their favourite languages with SQL for ages. The advent of the web introduced HTML and javascript as additional languages that web programmers needed to know to be able to successfully write web programs. At the same time other tools started appearing which started to reduce your requirement to know these languages. [Object Relationship Mappers](http://en.wikipedia.org/wiki/Object-relational_mapping) could help you get out of writing SQLs (at least so they claimed :) ) in many cases. And one of the features of [GWT](http://code.google.com/webtoolkit/overview.html) is to ensure that you work primarily on only one language.

But the goal of this post is to neither encourage nor discourage the use of polyglotism. It is to suggest that at the end of the day even if polyglotism really stimulates the technogeek sensors in us, it needs to be evaluated not in technology terms, but in business and economic terms. A point that is also partially supported by [The plague of polyglotism](http://www.codecommit.com/blog/java/the-plague-of-polyglotism). Unless we use tools like GWT and hibernate to keep us monoglots, (which is likely to be a very small proportion), polyglotism is the default state that we shall operate in. So its not worthwhile to fight that trend per se. The important aspect is to ensure that we understand the costs of supporting each new language in the mix. And the reason to do so is to evaulate whether the benefits of adding the language outweigh the costs in the medium to long term. So here are some of the costs (in no specific order)




	
  * **Syntax and libraries** : This is probably one of the lowest costs even though it is the most apparent. For every language we add to the mix, generally for a long running project, at least three developers (if you have reasonable risk management objectives) need to learn the new language syntax and the libraries they work with. Along with this is the cost of books, training courses and materials etc. (yes for many enterprises, many developers but those purely self motivated will need to be formally provided all of these).

	
  * **Idioms and Philosophies** : Knowing the language is not adequate. You need to understand its philosophy, its design intent and its typical idioms. Being able to use a language to write code is not the same as being able to use it idiomatically. And while that may not constrain you in over small code bases in the short term, over a period of time for reasons such as performance, consistency and ability to understand other libraries that you may reuse, you will be forced to deal with these aspects of the language as well. The book[ Dreaming in Code](http://en.wikipedia.org/wiki/Dreaming_in_code) talks about how Java programmers who started writing python could not write idiomatic python code, and thus a fair degree of code had to be redesigned or rewritten. And that is by no means the only such instance. I suspect similar trends are likely to be found if java programmers move to writing ruby or scala code. And its a trend you might have likely observed when people comfortable with writing relatively simpler languages such as Visual Basic or PL/SQL or even more complex languages like C++ moved to writing Java. Knowing the language is not the same as knowing it idiomatically - and that takes time. Which means - do not take on high risk activities when you absorb a new language, it will take a few iterations for your team to absorb the new language - far far more time than it will take to read a book or attend a training course.

	
  * **Skills to manage multiple idioms in one head ** : Unless you are going to have different programmers for different languages, the programmers working on multiple languages at a time will not only have to deal with the multiple idioms, but will need to concurrently apply them. In my experience, this is not a trait we are born with, and it takes some time and experience with it. It will take some time for them to learn that the best practices are not identical across various languages. It will take even more time to figure out why they are different. So there is a good likelihood, you will not be in a situation to have all your developers work on all the languages simultaneously - especially some of the Junior ones. 

	
  * **Skill portfolio management will become more difficult** : For typical enterprise or ISV teams, the managers need to ensure adequate skill availability across all teams. As the number of languages in use increase, this management process will become harder. Which means there is an automatic economic disincentive which rises is in magnitude, each time you add yet another language to the mix.



So is polyglotism good or bad ? I submit such a value attribute cannot be assigned to polyglotism per se. It needs to be really evaluated in your context, preferably beyond a project window into the medium or a long term. What is indeed obvious is that each new language will bring in some very strong capabilities and aspects where it is superior to the other languages in the mix. Equally obvious is that each new language will bring in associated costs. Just like there is no perfect language, there is no perfect number of languages that make sense in every context. Just like you do your homework in terms of language selection, make sure you do the same for your language portfolio not just in technology but in management and economic terms as well.




