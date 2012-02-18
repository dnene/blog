---
date: '2010-01-06 01:49:59'
layout: post
slug: concise-python-code
status: publish
title: Concise python code
wordpress_id: '937'
categories:
- functional programming
- programming
- python
---

I love python for a number of reasons. Brevity is one of them. Readability is another. However writing readable concise code can take some time to getting used to. This post discusses some code snippets especially in the context of some of the comparisons done between python and clojure - another language which excels at brevity and shares yet another feature with python, ie. people either love or hate its syntax, they are rarely indifferent to it.

Please note : The intent of this post is not to compare LOC of python with other languages. It is meant to demonstrate python code implementation which helps describe the capabilities of python in terms of writing clean, concise, readable code. This post continuously refers to other posts and may require you to read the referred posts in conjunction with this post to get maximum value.

**Code and ceremony**

The post [Java vs Clojure – Lets talk ceremony!](http://www.bestinclass.dk/index.php/2009/09/java-vs-clojure-lets-talk-ceremony/) describes a simple problem which requires 28 LOC in Java and 1 or 9 in clojure. It is a simple logic which creates a list, populates it with four values (two of them identical) and creates a list with unique items. There are two alternative clojure implementations, one in 1 line of code and another in 9 - the former using a built in construct in the language which especially helps to write a more concise logic, and the latter by writing code similar to the Java code. Without any further ado, here are the two alternative python implementations

a. By using built in capabilities - In this case we use the set collection
``` python
print set(("foo","bar","baz","foo"))
```

b. Now here's where we will not use a built in operator which automatically chooses the unique elements but again write code similar to the java code. This code is as follows :
``` python
print reduce(lambda x,y:  x + [y] if y not in x else x ,("foo","bar","baz","foo"),[])
```

There - both of them are exactly one line long.

**Project Euler solutions**
The post [Python vs Clojure – Evolving](http://www.bestinclass.dk/index.php/2009/10/python-vs-clojure-evolving/) refers to many sample python implementations solving some of the problems documented on [project euler](http://projecteuler.net) site. We shall revisit these python implementations here.

a. _Find all numbers dividable by 3 or 5 in 0 < x < 1000_:
In this case the clojure code is 4 lines long where as the python implementation is 3 lines long

``` python
print sum(i for i in range(1,1000) if i%3 == 0 or i%5 == 0)
```

Again the implementation is exactly 1 line long - no lambdas, no filters etc.

b. _Get the sum of all even Fibonaccis below 4 mill._

The suggested python implementation is about 17 lines long. The implementation below 7.

``` python
from itertools import takewhile

def fib():
    x,y = 1,1
    while True :
        x,y = y,x+y
        yield x

print sum(i for i in takewhile(lambda x : x < 4000000,fib()) if i%2 == 0)
```

c. _Finding Palindromes_
The suggested python solution uses about 23 lines of code, to 6 in clojure and takes **15** seconds almost **300%** slower than the clojure code (emphasis from the post referred to).

Here's another alternative implementation.
``` python
print max(x * y for x in xrange(100,1000) for y in xrange(100,1000)  if str(x*y) == str(x*y)[::-1])
```

Again if you notice - exactly one line of code. And incidentally on my notebook it clocks in about **0.85** seconds which makes the clojure implementation about **470%** slower than the python implementation. (emphasis mine)

**Top Rank Per Group**

In yet another post [Python vs Clojure – Reloaded](http://www.bestinclass.dk/index.php/2009/10/python-vs-clojure-reloaded/), the author discusses the [Rosetta Top Rank Per Group](http://rosettacode.org/wiki/Top_Rank_Per_Group) implementation of python. Unsurprisingly the python implementation uses 11 lines of code (not including the initialisation) compared to clojure's 6. 

One of the commenters suggests an [eminently readable and simple python solution](http://gist.github.com/214369) in about 6 lines of code. The code snippet is as follows 

``` python
from itertools import groupby
from operator import itemgetter

data = [dict(name="Tyler Bennett", id="E10297", salary=32000, department="D101"), ...]

department = itemgetter('department')
for dep, persons in groupby(sorted(data, key=department), department):
    print "\nDepartment", dep
    print " Employee Name   Employee ID     Salary          Department"       
    for person in sorted(persons, key=itemgetter('salary'):
        print "%(name)-15s %(id)-15s %(salary)-15s %(department)-15s"
```

An alternative far less readable solutions which shaves off one more line but certainly not preferred to the solution above (am just listing it since it shaves off the step of having to create an intermediate data structure) is

``` python
import operator
from itertools import groupby
   
data = [('Employee Name', 'Employee ID', 'Salary', 'Department'),....]

print reduce(lambda x,y : x + y,
         (('\nDepartment %s\n  Employee Name   Employee ID     Salary          Department  ' % dept + 
           reduce(lambda x,y : x + ("\n  " + "%-15s " * len(data[0]) % y), 
                  sorted(recs, key=lambda rec: -rec[-2])[:3],'')) 
         for dept, recs in groupby(sorted(data[1:],key=operator.itemgetter(3)), lambda x : x[3])),'')
```

Some might wonder whether I am joining multiple lines of code into one just to reduce the line count. The code is consistent with the idiomatic usage of python and also with the way python list comprehensions are documented in the formal python proposal for list comprehensions [PEP 202 : List Comprehensions](http://www.python.org/dev/peps/pep-0202/). Besides python is a whitespace sensitive language and the compiler disallows multiple lines of code in one line without using the ';' separator which I have most definitely not used in the suggested solutions.

PS: There's of course an interesting image at the end of the post [Python vs Clojure – Reloaded](http://www.bestinclass.dk/index.php/2009/10/python-vs-clojure-reloaded/) indicating the perception of how clojure code speaks for itself ;). The reason I mention it is that I believe if one is expressing a strong opinion as the one reflected by the picture, one better be far more careful and thorough with verifying the <del>veracity of the assertions</del> appropriateness of the samples compared to. Let me clearly state that I do not know that the clojure loc compared to is an appropriate target so do not infer this post to be a Python vs Clojure LOC as much as a post which emphasises that Python is a good vehicle to write concise readable code. FWIW I am learning clojure and look forward to building more competence with it.

Update: There is another nice post by Stephan Schmidt - [Anatomy of a Flawed Clojure vs. Scala LOC Comparison](http://codemonkeyism.com/scala-vs-clojure-flawed-loc-comparison/) which does reflect an opinion in the context of Scala and a post authored on the same blog as the ones I refer to above.

