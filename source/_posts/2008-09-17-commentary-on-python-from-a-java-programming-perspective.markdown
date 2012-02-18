---
date: '2008-09-17 18:44:54'
layout: post
slug: commentary-on-python-from-a-java-programming-perspective
status: publish
title: Commentary on Python from a Java programming perspective
wordpress_id: '73'
categories:
- java
- php
- programming
- python
- web
tags:
- java
- opinion
- programming
- python
---

After having worked with Java (and earlier C++) for a number of years, I have been working with Python for the last few months. Since I came to Python from Java, I thought it might be useful to share my experiences, which might be of interest to many programmers. This is not intended to be a language or feature comparison or something that contrasts the various advantages or disadvantages, but is intended to reflect on the softer aspects of how it feels to write Python programs. Hence I have refrained from including code snippets and other programming constructs in this post, but if you believe I am unclear in some of the comments I am making, do let me know and I shall be glad to update the post or write a new one as required.

**Programming is Easier and Enjoyable**

I think the most dominant impression from the last few months is that python does make programming feel a lot more easier and often more enjoyable. The feeling is not very different between riding a bicycle without gears then riding one with gears. In the latter case one just feels one can cover a lot more distance much more easily though any physicist will tell you the actual effort is not particularly different. It just feels like one has a much bigger toolbox (ie a wider assortment of tools) to work with and therefore the task seems simpler. Why do I think that way ? I believe the following features of python do help (in no particular order) :



	
  * **Concise Coding style** : The code typically is much more concise, with much lesser verbosity

	
  * **Dynamic typing** : You really do not need to worry about declaring data types and making sure the inheritance hierarchies especially for all the interfaces and implementations well laid out. The various objects do not even need to be in the same inheritance hierarchy - so long as they can respond to the method, you can call it. This is a double edge sword, but that doesn't take away the fact that programming under dynamic types environment does seem a lot easier.

	
  * **Easier runtime reflection** : Java seems to have all the reflection capabilities but I think these are just way too painful to use as compared to python. In python the entire set of constructs (classes, sequences etc.) are available for easy reflection. In case you need to use metaprogramming constructs, python really rocks.

	
  * **More built in language capabilities** : Items such a list comprehensions, ability to deal with functions as first class objects etc. give you a broader vocabulary to work with.

	
  * **Clean indentation requirement** : It took me about 2-3 days to get over it but, it seems that python code is much easier to read since if you do not indent it correctly it will be rejected.



**Need to spend time on understanding how to write code in Python**

One of the statements I have heard in different contexts is that Java programmers when they start using python write it as if they are writing a java program using python syntax (unpythonic in python parlance). This actually took me a fair amount of time to understand. I did look at a lot of other python code to attempt to understand this in greater detail. I realised that python offers many more capabilities than java and that as a person who had written java code earlier, I was able to be immediately productive using the constructs in python which mapped onto the equivalent constructs in java easily. However it did take a long time to be able to start using the other constructs. One of the exercises that did help was to get away from the problem at hand and try to work on something entirely different (especially a program which had a strong algorithms element to it with complex data structures), and slowly review each line with how it might be better done in python.I realised it is easy to start coding in python but it takes some effort and time to start using the entire python toolbox effectively. While I reviewed other programs written in python, I did think there was one area where the prior exposure to java helped. Class design. I am not sure if this was an issue with the programs I read and thus there was an issue with the sample references I used. However while these helped me understand how to write code in python differently, I have a strong feeling that the programs I wrote were much stronger in terms of class design. There are many situations especially given the fact that python supports both function oriented programming and object oriented programming, where one wonders what is a better way to design the logic in a particular context. I thought it generally made sense to use proper class based design and use the functional constructs in situations which started getting a little loopy or algorithmic.

**No Compile Cycle**

Another thing I really enjoy about python is - no compile cycle (its implicit). So while in the middle of my editor, I could exit to the shell prompt, run the source immediately without trying to deal with an ant script in between which compiles java code and then recycles the web application. This has boosted my net productivity quite a bit. 

**Productivity**

So are the statements that one can be 5 to 10 times more productive in python supported by my experience. While I haven't gotten through the full life-cycle yet, prima facie I do believe 5 times does seem like a good number. However I would introduce the following caveats. It will not happen for your first project .. more like your second project onwards, primarily since it does require a lot of effort to start understand how to write pythonic code and that does take away a lot of those benefits initially. Secondly it does not factor in refactoring. Point refactoring is much much easier in python, but bulk refactoring is much much more difficult since automatic refactoring is tough. Thus when I am refactoring, I sometimes feel I was able to get it done faster in python, but in many other cases I thought I ended up taking a lot more time.

**Switched to python completely ?**

Not really. But I feel happy I have more options available to me. One thing that really sucks is the poor refactoring capabilities. This is unlikely to be an issue with python alone and is likely to be an issue with all dynamically typed languages. However if you want to use dynamic languages, be prepared to spend some more effort during refactoring since many of the automatic refactoring capabilities you may have gotten used to, may not be available. The other issue is performance. Java wins hands down on performance by a big margin. If I have to ever get back to another project where performance was particularly important and the primary constraint in performance was not network or disk IO but was likely to be the CPU, and it would be acceptable to substantially reduce development productivity in order to get the performance gains, I shall be found to be writing Java code for certain. The next project I work on, I am likely to "first" evaluate whether python fits the bill and switch to other languages if I do not find it appropriate enough.



