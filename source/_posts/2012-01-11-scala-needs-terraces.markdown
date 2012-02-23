---
date: '2012-01-11 05:22:42'
layout: post
slug: scala-needs-terraces
status: publish
title: Scala needs terraces
comments: true
wordpress_id: '1209'
categories:
- functional programming
- java
- programming
- scala
- software
---

_Terrace (noun) : each of a series of flat areas made on a slope, used for cultivation, or a flight of wide, shallow steps providing standing room for spectators in a stadium_
 
Recently there was a useful discussion triggered off by the post [True Scala complexity](http://yz.mit.edu/wp/true-scala-complexity/) by [Yang Zhang](https://twitter.com/#!/yaaang). Much has been debated about it in the comments on the post and [other forums](http://news.ycombinator.com/item?id=3443436), most of it both articulate and constructive.

Martin Odersky proposed an idea on the ycombinator news thread as follows (next paragraph)



> So, I believe here is what we need do: Truly advanced, and dangerously powerful, features such as implicit conversions and higher-kinded types will in the future be enabled only under a special compiler flag. The flag will come with documentation that if you enable it, you take the responsibility. Even with the flag disabled, Scala will be a more powerful language than any of the alternatives I can think of. And enabling the flag to do advanced stuff is in any case much easier than hacking your own compiler.



The basis of the idea wasn't new. Martin himself had earlier suggested [Scala Levels](http://www.scala-lang.org/node/8610), though the suggested enforcement of two levels via a compiler flag was. While I thought it was a useful idea, strangely enough I found many (especially in the twitter streams I follow) less than enthusiastic. I think there exists a perspective where one can look at this idea with more enthusiasm, and this post details that.

Learning scala is non trivial. For a person who is well versed with Java programming and with little else, this will require learning at least the following (I am listing only a few items)



	
  * Learning traits and objects (and unlearning statics). There's a lot of stuff here including multiple inheritance, member overriding, whether the members should be defs or vals etc. which takes some times to get used to.

	
  * Learning to deal with immutability and occasionally lazy evaluation

	
  * Learning collections constructs such as for comprehensions or, map, flatMap, filter etc.

	
  * Understanding the capabilities that scala offers in terms of generics including being able to define co/contravariant collections, and then learning to use these capabilities effectively

	
  * Being able to understand and leverage many aspects of type theory



Typically when a person makes a sustained effort driven by passion, he can cover a lot of ground very quickly. Thus some might be able to deal with the scala learning curve quickly. While this ability to learn could be achievable for many especially smaller companies, it is likely to be quite difficult for many others. Especially when the team sizes are large and/or developer capabilities might be modest, this is a real issue. One could form an analogy with a Cheetah and an Elephant. Some animals will be able to run fast individually, and others will trod slowly as a herd. One could wish elephants could run as fast as cheetahs but thats unhelpful, since the jungle is defined, and one best deal with it the way one finds it.

An ideal way to introduce scala would be to start with small groups of 3-5. But thats not practical in all cases. Besides for large teams, that means, most will need to wait for a much longer time before being able to use scala and profit from its substantial benefits. So lets say an organisation attempts to introduce scala in a modest team of say 20. Of which 2 are passionate, self driven souls, who tend to work much harder the rest to be able to progress much faster. Because learning to use scala the way scala wished to be used, is not a sprint but is a marathon (or at least a long distance jog), this creates issues. Soon enough the 2 are likely to run far ahead of the rest of the team and will start writing code which the others can't grok. Even as some of the remaining may actually be struggling hard but are likely to be losing enthusiasm simply because they can't find themselves being able to keep pace. An even bigger risk is the 2 continuing to make rapid progress even as they realise so much more remains to be learnt, even as the remainder are unable to start leveraging scala, since they still think there's a lot of distance yet to cover and they become far more focused on catching up rather than leveraging what they have learnt. So while learning proceeds, actual development with direct economic benefit stalls. 

Lets say hypothetically this team decided that they would only focus on reducing java boiler plate (which is a big deal) and continue to use scala in a primarily OO paradigm (since that is what they are used to) and defer learning remainder of scala by at least six months. There's likely tons of benefits to be had by shifting to scala even with this limited scope. Yet all it takes to destroy harmony is one very enthusiastic developer. Who starts inserting (as yet) alien notions such as writing highly functional code, or implementing advanced type theory concepts (eg. scalaz) or starting to use a largely un-understood symbol soup ([scalaz](http://www.cakesolutions.net/teamblogs/2011/11/19/u-s-scalaz-layout/) or the dispatch [periodic table](http://www.flotsam.nl/dispatch-periodic-table.html)). This would make a mockery of the phased learning and force every one else to catch up (even if just to be able to read and maintain somebody else's code).

We've long known hills are not conducive to agriculture. If these hills are our learning curves, fields could be considered to be directly impactful deployment of our skills. Agriculture generally happens flatlands and plateaus but not hillslopes. And yet we've learnt to tame the tall hills using terraces. Instead of climbing for a long time, climb a little & grow a little. If teams can learn for a month, and deploy these skills for the next 6-9 months with the cycle repeating itself, it could help in the following ways :




	
  * Organisations do not need to invest in long training times where the opportunity cost of lost development time is one large fixed cost.

	
  * Intra team disparity is likely to be contained since the deployment period will allow many to catch up with those in the lead

	
  * Instead of scala being viewed as one big leap, it could be viewed as a incremental leaps with managed effort. I saw a remark which said people anyways deal with complexity - its called OO. And Java programmers deal with OO. So they should be able to come to terms with scala quickly. But there is a difference. Java wasn't as big a learning curve leap over C++ as Scala is over Java. And as importantly, a learning curve one has triumphed over already, somehow seems far less onerous than an identically sized one, one still needs to deal with.



In short learning scala needs terraces. Terraces, where some developers will not run far ahead of the pack during the learning stages. Terraces where developers won't have to feel too scared of alien'ish code suddenly appearing on their monitors. Terraces where developers will learn a few things and then deploy these skills over the next few months. These terraces could be made to work in different ways. It could just be a honour system using a set of levels identical or similar to the levels Martin Odersky proposed (though it would be extremely hard to ensure the same without tooling support). Or they could implemented using (as yet unwritten) tool like a scala-lint which will flag off advanced usages as warnings. Or as compiler switches the way Martin proposed. 

There could be other ways to look at it too. Scala could be considered as a sharp weapon. Lethal in the hands of the trained. Self damaging in the hands of the untrained. So compiler switches could simply be a way to graduate from using sticks, to wooden swords, to blunt steel swords to the really sharp ones.

I saw a remark which said a compiler switch would divide the programmers. I seem to think such steps which help create terraces will divide the learning challenges. I also saw another remark which said, implementing compiler switches would play into the hands of languages like Kotlin or Ceylon or Fantom. I believe exactly to the contrary. The lower terraces can compete with Kotlin or Ceylon, and the upper terraces can provide a growth path that many other languages will not have. Not being able to plan a phased and an economically practical learning curve for large teams will make the case for other languages stronger. 





