---
date: '2009-10-23 04:56:09'
layout: post
slug: stop-calling-me-nosql
status: publish
title: Stop calling me NoSQL
wordpress_id: '884'
categories:
- architecture
- software
tags:
- natural persistence
- nosql
- schemaless databases
---

Dear Reader,

Apologies for sending this note to you completely unannounced and out of the blue. However I find myself in a peculiar situation of having a very weird name being dumped upon me. While I am indifferent to the name per se, I am greatly pained as I realise that it is a completely inappropriate name. What is even more confounding is the very bunch of people who have happily assigned me the name and continue to popularise it belong to that class of people some of whom actually are extremely particular about accurate nomenclature and have no hesitation in creating a 100 letter class or function name by concatenating 20 words just to make sure the name is unambiguous and conveys the intent clearly. 

Ahh.. but I digress and impose upon you without introducing myself adequately first. I am a data storage style. I am not new, but lately far too many a software engineer have started taking a liking for me. Ever since I have been around, I have with great amounts of jealousy watched my cousin the RDBMS being courted by the finest of engineers (in all honesty there were some fine engineers interested in me too, but far too few compared to my cousin). But lately multiple concurrent developments have made a fair amount of attention come my way too. 

You see unlike RDBMS, I don't require that data be clearly split into tables, columns and rows. I can work with data the way it is most naturally represented. As a tree of individual data fields, lists, arrays, dictionaries etc. Also I do not require that you always clearly define each and every possible schema element before being able to store data corresponding to the schema. I can happily accept a schema dynamically or even work without a schema. Some of my early forms were based on key value pairs stored as B-Trees (eg. Berkeley DB). Over the years people have figured out ways to represent the data as a set of decomposed document elements, store data spread across a cluster, replicate it for better availability and fault tolerance, and even perform post storage processing tasks using map-reduce sequences. But really what separates me from my cousin and other storage systems is that I don't make demands on the data - I take it in its naturally found form and then store it, replicate it, slice it, dice it and glean information out of it. And therein lies my true identity - I will work with data the way the data is best represented with all its arbitrary inconsistencies and inabilities to always clearly specify a constraining schema. And the engineers who've spent time with me seem to have enjoyed it quite a bit. 

But the horror of it - they gave me a completely inappropriate moniker - 'NoSQL'. First and foremost I exist to promote a storage style and thats what identifies me. I work with data in its natural and arbitrary forms. Therefore to make it seem like I represent a lack of something else is utterly missing the point. The SQL in NoSQL stands for Structured Query Language, which depends upon Fixed Structure Relational Data. Since I change the very nature of the data being stored, that SQL is not required or relevant is automatic and inconsequential. 

Its like calling a under-the-ocean-mountain_range as NoIgloo. Its dead obvious igloos will not be found there. But calling that mountain range NoIgloo is a big disservice to visitors. You use that as a marketing term, attract people, then tell them that NoIgloo actually has nothing to do with Igloos - its got to do with mountains and oceans, and that they need to first unwind all the confusion they created in their minds due to NoIgloo and then go through a phase of reunderstanding mountains and oceans. And while they came prepared for a possibly warmer place given the name NoIgloo - it actually is a wet place so they need to again change their garments and equipment for the journey. A wholely avoidable situation.

_Update: [Brad Anderson](http://twitter.com/boorad) pointed out this interesting post [NoSQL: A Modest Proposal](http://voodootikigod.com/nosql-a-modest-proposal) which traces the genesis of my name which leaves me very very disappointed. Almost seems to suggest that people are flocking together and naming me not based on something inherently powerful about me - but as a mechanism to demonise my cousin RDBMS. This is most unfortunate, since we actually end up being useful in very different situations and more often than not are likely to complement each other rather than compete with each other. I do hope a better moniker does prevail over time_ 

What I would like is to see a better / more appropriate name for me. Hmm .. call me free form storage, natural persistence or flexi schema storage or perhaps something else even more appropriate (this blog owner prefers "natural persistence"). Each of these conveys far more about me far more accurately than NoSQL does. Basically please please call me something better than NoSQL. So can I request you to carry forward my plea by further forwarding and retweeting this to your friends and ask them how they can so callously call me by such a silly name when they take the utmost precautions in properly naming their classes and methods. Plead with them to stop doing this and please work with others to give me a better name. I think it will cause less confusion over the coming months and years, and the field of software shall recover its glorious tradition of maintaining precision in communication by using accurate naming.

Sincerely,
The one who doesn't want to be called NoSQL

PS : As a background to this there was an interesting conversation earlier today between this blog owner [dnene](http://twitter.com/dnene) and [Kent Beck](http://twitter.com/KentBeck) on twitter, where Kent so kindly and graciously helped carry forward the thought process of helping identify my essential characteristics, and it is in no small part, thanks to this conversation that I was able to articulate myself and my grief. I reproduce that conversation below. (_Update: though in all likelihood Kent's intent was to help clarify the thought rather than contest the names. In hindsight, it makes sense to ask for permission to reproduce conversations .. even when such are on the public twitter stream - something that wasn't done in this case. :( _)




Twitter IDTweet / Message


dnene
NoSQL is such an inappropriate name. NoTables at least makes a little more sense.



KentBeck
@dnene but what would nosql be called if you wanted to say something positive about it?



dnene
@KentBeck Thats a great question .. still thinking .. best thought so far - FlexiStore (though not good enough yet :( )



KentBeck
@dnene what can you do with a nosql store that you can't do with an sql database? why would you be excited to use one?



dnene
@KentBeck I see where u r going with this (a) unconstrained & composite storage (b) store resources not records (c) shard/scale horizontally



.@KentBeck I think there is a merit in attempting to define nosql in terms of what it is rather than what it isn't



KentBeck
@dnene there are many more people confused about what datastore to use than who hate sql. the positive approach appeals to the former.



dnene
@KentBeck Agreed .. and I'm aware of many more who wonder why we need a different datastore than the RDBMSs. NoSQL as a name doesn't help.



KentBeck
@dnene well, why *do* we need a different data store?



dnene
@KentBeck Primary Need : We need support for flexible/arbitrary schemas with complex depths - RDBMSs don't dance well in this space.



@KentBeck Secondary Need : Support for deferred processing required for analytics (eg. Map/Reduce). RDBMS don't do too bad a job here



@KentBeck Tertiary Need (not one that I've felt strongly yet) : Distributed and horizontally scalable storage on commoditized h/w.



KentBeck
@dnene it seems like you're looking for realistically structured data, not data twisted to fit a formula convenient for mathematicians.



dnene
@KentBeck Yes.. thats it! I'm looking for realistically or naturally structured data storage / persistence. Rocks compared to the term nosql



@KentBeck Wonder if the term arbitrarily structured makes sense as well. This has been one heck of a conversation/Q&A; so far +1:)



KentBeck
@dnene glad you found it helpful. you get bonus points if the opposite of the name you pick is unattractive, a la "structured programming"


