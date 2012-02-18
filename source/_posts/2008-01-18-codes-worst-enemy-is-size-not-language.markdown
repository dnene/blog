---
date: '2008-01-18 00:56:26'
layout: post
slug: codes-worst-enemy-is-size-not-language
status: publish
title: Code's worst enemy is size (not language)
wordpress_id: '10'
categories:
- java
- software
tags:
- java
- software
---

Steve Yegge writes a rather long but interesting post related to "Code's worst enemy":http://steve-yegge.blogspot.com/2007/12/codes-worst-enemy.html .

He starts off making a nice point.

> I happen to hold a hard-won minority opinion about code bases. In particular I believe, quite staunchly I might add, that the worst thing that can happen to a code base is size.

Great point. Not sure if it is a minority but if it is, I am there too. 

He goes on to add some more interesting stuff 

> I say my opinion is hard-won because people don't really talk much about code base size; it's not widely recognized as a problem. In fact it's widely recognized as a non-problem. This means that anyone sharing my minority opinion is considered a borderline lunatic, since what rational person would rant against a non-problem?

....

> My minority opinion is that a mountain of code is the worst thing that can befall a person, a team, a company. I believe that code weight wrecks projects and companies, that it forces rewrites after a certain size, and that smart teams will do everything in their power to keep their code base from becoming a mountain. Tools or no tools. That's what I believe.

However my opinions start to differ soon thereafter.

> The problem with Refactoring as applied to languages like Java, and this is really quite central to my thesis today, is that Refactoring makes the code base larger. I'd estimate that fewer than 5% of the standard refactorings supported by IDEs today make the code smaller. Refactoring is like cleaning your closet without being allowed to throw anything away. If you get a bigger closet, and put everything into nice labeled boxes, then your closet will unquestionably be more organized. But programmers tend to overlook the fact that spring cleaning works best when you're willing to throw away stuff you don't need.

Refactoring is not just adding new classes and methods and therefore introducing the syntatic sugar that accompanies them. Refactoring involves a bit of redesign, a bit of cleanup, a bit of clever consolidation and in many cases these actually *reduce* the code size.

> Java is like a variant of the game of Tetris in which none of the pieces can fill gaps created by the other pieces, so all you can do is pile them up endlessly.


Wake up. There's nothing about the above scenario which is specific to _Java_. You can be in this situation any time you want in any language so long as you are sufficiently careful not to do any refactoring and keep on piling on more functionality. Come to think of it - in the above analogy probably the refactorings are indeed analogous to the blocks taking themselves off the board to create space for more.

> I'll give you the capsule synopsis, the one-sentence summary of the learnings I had from the Bad Thing that happened to me while writing my game in Java: if you begin with the assumption that you need to shrink your code base, you will eventually be forced to conclude that you cannot continue to use Java. Conversely, if you begin with the assumption that you must use Java, then you will eventually be forced to conclude that you will have millions of lines of code.

This is really strange. I pride myself on writing concise and short code, and have learnt to write code in more languages than the fingers on my palms. However even after having spent a better part of the last decade writing in Java, I cannot really imagine myself reaching those conclusion.

> The rational response would be to take a very big step back, put all development on hold, and ask a difficult question: "what should I be using instead of Java?" .... It took me six months to realize it can't be done with Java, not even with the stuff they added to Java 5, and not even with the stuff they're planning for Java 7 (even if they add the cool stuff, like non-broken closures, that the Java community is resisting tooth and nail.) *It can't be done with Java.*

This is almost getting close to being a flame bait. Again, I would be hard pressed to reach a conclusion even remotely close.

I do believe the post deserves to be mentioned for raising the point about about code size and perhaps for bringing in the analogy about Tetris. To a sufficient extent I think the argument is independent of the language (which is unfortunately where Steve headed in the mail). The point is that the emphasis on small code sizes is worth having. It is worth having for the following reasons :

* Emphasis on small code leads to much cleaner design which in turn makes the software much more maintainable and flexible (because of the design and not just the reduced lines of code)
* Emphasis on small code increases the motivation for continuous refactoring and reduces the incentive for the Copy/Paste shortcuts. 
* Emphasis on small code leads to precisely that - small code. 


