---
date: '2011-08-15 19:42:34'
layout: post
slug: cperformance-comparison-languages-styles-and-vms-java-scala-python-erlang-clojure-ruby-groovy-javascript
status: publish
title: 'Contrasting Performance : Languages, styles and VMs - Java, Scala, Python,
comments: true
  Erlang, Clojure, Ruby, Groovy, Javascript'
wordpress_id: '1144'
categories:
- functional programming
- java
- programming
- python
- ruby
- scala
- software
tags:
- clojure
- comparison
- erlang
- groovy
- java
- javascript
- jruby
- performance
- pypy
- python
- ruby
- scala
- v8
---

**_Major update_**

This blog post is now formally retracted. A part of the original post remains. As does a record of the contributors and a log of the annotations of the updates. The code also remains on github. What are deleted are the actual published results, and some other sections that are now less relevant after this retraction. Added is the narrative about the reason for retraction. 

The way this post started was a casual exercise in measuring performance of my earlier python code in pypy. I found the results interesting, so soon tried the same in Erlang. Having found the same to be interesting as well, very soon I found myself adding more languages and different coding styles resulting into a set of exercises which caused me to think (naively in hindsight) - hmmm, these results do tell me something. And probably the learnings are useful enough to share. It disappoints me to not be able to continue to share the same. Yet thats what I am just doing.

_Why ?_



	
  * Insufficient detailing of constraints : As Isaac points out in the comments [here](http://blog.dhananjaynene.com/2011/08/cperformance-comparison-languages-styles-and-vms-java-scala-python-erlang-clojure-ruby-groovy-javascript/#comment-9433), I had defined the set of constraints around the coding styles a little loosely. Once I had opened up the results and opened up the entire topic for a number of submissions - these started becoming an issue. As an example the early versions of the benchmarks used lists - yet some of the languages have have no exclusive lists, so one uses array like structures as well. These started a set of suggestions that one should use arrays instead of lists in other languages as well. That was just a starting point being cited as an example of how a lack of clear constraints started influencing the code. It also points out to the difference in rigour required between conducting exercises for self understanding and publishing the same.

	
  * Inability to keep code in other languages updated : It was rather painfully obvious to me that as some of the contributions were being implemented, the same were also candidates for leveraging in other languages. Yet the lack of time prevented me from being able to do so. Thus even as I was adding contributions to remain fair to each implementation, I was being unfair by not applying the underlying efficiencies leveraged by these contributions to other languages simultaneously.

	
  * Running out of time : I am likely to be extremely busy over the coming month. Yet there still were a number of contributions still coming in. Given the priorities I need to work upon, this blog post was not amongst the more important ones. Yet I imagined, if new submissions came in, I simply would no longer have time to deal with the same. Again resulting in a degree of unfairness that would not be acceptable to me.

	
  * Letter or Spirit : Performance Benchmarks need to be written to reflect the letter and the spirit of the language. I would've preferred to measure the performance implications using the features as advertised. There's no point advertising a luxury car packed with a ton of features and a cost, and then simultaneously publishing speed results using a rally car version - which has a lot of the weight thrown out (and would actually cost a lot more than the commercial car to maintain). (The analogy fails partially, since in programming we can combine the two based on varying contexts). The judgement of the right spirit is extremely subjective. A decision I did not find myself (and perhaps most others given the subjectivity) capable of making fairly. And yet keeping benchmarks focused on the letter without the spirit was rather uninteresting for me (YMMV).



I find myself a little wiser and a lot humbled. The learnings above are unlikely to be forgotten. I also find myself much more aware of the performance implications of code thats consistent with my subjective understanding of idiomatic. The original reason I started this exercise has satisfied its objective in terms of an improved personal understanding, perhaps even more so because of a number of contributions I received. Thats a useful learning as well. 

The results that were published earlier on this page need to go. The code continues to remain on github for your perusal and further tweaking based on your definition of letter and spirit.

Sincere thanks to all who contributed. Particularly to Isaac. I learnt a lot from him. Further activity on this post shall cease.

**_Parts of the Original Post_**

There's a better place to specifically look at performance comparisons across languages than this post - [The computer languages benchmarks game](http://shootout.alioth.debian.org/). But this post attempts look at performance comparisons a little differently. Based on coding idioms as well. And for a much narrower range of problems (namely one).

There are languages which are tightly opinionated on a particular way of doing things. And there are languages which allow you to implement a given logic in multiple ways. Yet, depending upon the language (and as we shall see, the runtime), the performance could vary quite substantially based on the nature of the code we write. This post attempts to take a small piece of logic, and implements in upto 3 different styles in 8 languages (10 if you count the runtime variations as well).

[..]

**Problem**

Quoting from [The Josephus Problem](http://danvk.org/josephus.html),

Flavius Josephus was a roman historian of Jewish origin. During the Jewish-Roman wars of the first century AD, he was in a cave with fellow soldiers, 40 men in all, surrounded by enemy Roman troops. They decided to commit suicide by standing in a ring and counting off each third man. Each man so designated was to commit suicide…Josephus, not wanting to die, managed to place himself in the position of the last survivor.

In the general version of the problem, there are n soldiers numbered from 1 to n and each k-th soldier will be eliminated. The count starts from the first soldier. What is the number of the last survivor. In the code I benchmarked, n = 40 and k = 3.

_Update. Note:_ Some are getting confused that I start by striking out the very first soldier in the chain and then starting to count up to the k value. This is one of the variations of the Josephus problem I had introduced this time. All the versions implement this logic consistently.

**Idioms**

I have considered three idioms :



	
  * **Object Oriented :** This code has classes reflecting a person (or a soldier) and the chain. The objects of person maintain reference to their prior and next people in the cirlce (a doubly linked list, and as the counting progresses, whenever they need to eliminate themselves, they do so by updating the next / prev references in the prev / next objects. This style results perhaps in the least operations involving mutation or memory allocation / deallocation. One would've imagined it to be the fastest, but as you will see that is not necessarily true.

	
  * **List reduction :**This code starts with a list of integers, each element representing a soldier. It performs an operation which effectively creates a subset of the list by removing every third soldier. The result of one such pass is a smaller list. Rinse and repeat if the smaller list is more than 1 element long. It emphasises looping over lists (using comprehension or other constructs) and focuses on reducing the list by conducting an operation on the entire list, every pass.

	
  * **Element recursion :**This is a more fine grained logic which emphasises recursion (and often accumulation) for every element in the list. This is particularly apt scenario to use pattern matching (both the erlang and scala code use pattern matching). One would imagine this to be always slower than list reduction since it is much more fine grained and involves many more function calls.



I've attempted to implement code in all languages using the styles above as long as reasonably feasible and appropriate. Since (barring C/C++), Java continues to be the language to beat from a performance perspective, I've attempted to implement roughly equivalent logic in all styles using Java as well. All programs typically run the code once to print the results (to verify correctness), and then 100000 or a million iterations to warmup, and then again repeat the iterations and measuring the elapsed time. There is a slight inconsistency between the various code snippets. The counter either varies between 0 to 39 or between 1 to 40. 

**Contributions**

I can't write the fastest possible code across all these languages. This is the best I could do. However if you can find a better way to implement the code, do let me know in the comments (or send me a pull request on github). I shall certainly include better solutions here if and as they are identified. At the point in time of publishing this, at least two authors had contributed to the code. I imagine (based on my experience with the prior post), more might be interested in suggesting tweaks to further improve performance. These are all listed here.




	
  * Paddy3118 had suggested some python code in the comments in last blog post, which I have substantially reused for the python list-reduction logic

	
  * [Rahul Göma Phuloré (missingfaktor)](http://twitter.com/#!/missingfaktor) contributed substantial improvments to the scala code

	
  * [Viktor Klang](http://twitter.com/#!/viktorklang) contributed a improved version for the scala element recursion code

	
  * [David Nolen (swannodette)](http://twitter.com/#!/swannodette) contributed a substantially improved version for clojure element recursion, and the java like versions for clojure

	
  * [Fred Hebert](http://twitter.com/#!/mononcqc) suggested native compilation by adding "compile(native)." and a couple of other minor improvements over github

	
  * Isaac Guoy offered an improved Java, Python and Javascript versions and code for an alternative oo + element-recursive style. The alternate style code is to be found in the contrib directory.

	
  * [Alex Tkachman](http://twitter.com/#!/alextkachman) offered code for use with Groovy++

	
  * [Stuart Halloway](http://twitter.com/#!/stuarthalloway) submitted clojure element-recursion implementation



**Hardware / Software**

Removed

**Metrics :**

Removed

**Observations :** _(Updated)_

Removed.


Full Source code is available on github at [https://github.com/dnene/josephus](https://github.com/dnene/josephus)

Finally, thanks to a number of folks I had a chance to preview the post with and especially to Saager Mhatre to suggest moving the code from a attached zip file to github.

**Updates**



	
  * Updated metrics for groovy 1.8.1 (instead of earlier groovy 1.7)

	
  * Updated code to reflect suggestions by Eric Rozendaal and another almost similar one by Viktor Klang - Viktor's code was very marginally faster. Leads to a reduction in Scala Element Recursive benchmark

	
  * Updated clojure element recursion code as per suggestion by David Nolen. 

	
  * Thanks to the persistent questioning by Isaac, upgraded the metrics to jRuby 1.6.3. That turned out to be a very good step. There is a substantial improvements in the performance metrics which are now updated in the numbers above.

	
  * Fred Hebert submitted a pull request to turn on native compilation which required native compilation - which in turn required HiPE which Isaac had suggested earlier. After verifying that Erlang-HiPE is a valid synaptic target (thus a different readily available VM), I built the same and updated the readings

	
  * Isaac Gouy offered some helpful suggestions in terms of converting the main block also into a function. Also he demonstrated some potential issues in terms of whether the resulting performance was stable. I have made across the board changes now to run all the benchmarks ten times each for a million iterations and used the last 5 readings after visually ensuring that the readings did not vary much

	
  * Isaac further suggested improvements to the Java List Reduction and Element Recursion techniques which have now been incorporated. He has also contributed a perhaps faster version of OO code, which is less consistent with the other OO code being benchmarked. Need to identify how best to factor that in. Perhaps a yet other contrib section in the source?

	
  * Added newer versions of Java and Javascript contributions from Isaac, and Clojure contributions from David Nolen. I have only recently seen some more alternative implementations for clojure, and have received a Groovy++ contribution .. both I'll explore over the weekend

	
  * Updated Pypy results to Pypy 1.6 now that it has beeen released.

	
  * Added contribution by Stuart Halloway for clojure using LinkedList for element recursion 

	
  * Added code contributed by Alex Tkatchman for Groovy++. This I would remark has exceedingly good performance. You can find it in the contrib section.

	
  * Added further code contributions by Isaac for Python.




