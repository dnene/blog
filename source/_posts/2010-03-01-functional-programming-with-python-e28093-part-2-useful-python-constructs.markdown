---
date: '2010-03-01 22:18:05'
layout: post
slug: functional-programming-with-python-%e2%80%93-part-2-useful-python-constructs
status: publish
title: Functional Programming with Python – Part 2 - Useful python constructs
comments: true
wordpress_id: '983'
categories:
- functional programming
- programming
- python
- software
tags:
- sequences
---

In [Functional Programming with Python – Part 1](http://blog.dhananjaynene.com/2010/02/functional-programming-with-python-part-1/), I focused on providing an overview. In this post I shall focus on the core python language constructs supporting functional programming. If you are experienced pythonista, you may choose to skip this post (and wait for the next post in this series :) )

**Sequences in python are not immutable :**

When using sequences in python the thing to be noted is that sequences are not immutable. This provides you with the following options.

a) Use immutable sequence types : This is only possible by defining different types for sequences than the ones built into the language. 
b) Ignore all methods on the sequences which modify them 
c) Waive functional programming tenet of immutability 

My preferred option when explicitly focusing on writing functional code is to use b) 

**Sequence types in python**

The most commonly used sequence types in python are :




	
  * Tuple : Immutable, Ordered, Indexed collection represented as _(val1, val2)_

	
  * List : Ordered collection represented as _[val1, val2]_

	
  * Dictionary : A key indexed collection represented as _{ key1: val1, key2 : val2 }_

	
  * Set : An unordered collection allowing no duplicates. Represented as _set([val1,val2])_

	
  * str and unicode : While primarily meant to serv as ascii and unicode strings, these data structures also act as sequences of characters._Represented as "abcd" or 'abcd'_



Of the above only tuples are immutable.

There are other sequences used less often as well. An example is _frozenset_ which is an immutable set. 

**Simple iteration**
The _for_ construct allows you to loop through a sequence. eg. 

``` python 
one_to_five = [1,2,3,4,5]
for num in one_to_five : 
    print num
```

**Iterators on dictionaries**

Unlike tuples, lists and sets where iterators essentially traverse through the sequence constituents, there are a number of different iterators on dictionaries. These are :

``` python 
d = {1:"One", 2: "Two", 3:"Three"}
# keys
for val in d : print val
# an alternative for keys
for val in d.keys() : print val
# values
for val in d.values() : print val
# key, value tuples
for key,val in d.items() : print key, val
```

**Slices**

Slices allow creation of a subset on a sequence. They take the syntax _[start:stop:step]_ with the caveat that a negative value for start or stop indicates an index from the end of the sequence measured backwards, whereas a negative step indicates a step in the reverse direction. The following should quickly indicate the use of slices

``` python 
seq=[0,1,2,3,4,5,6,7]

#first 3
print seq[:3]
# last 3
print seq[-3:]
# 2nd to second last
print seq[1:-1]
# reverse the sequence
print seq[::-1]
```

**The iterator protocol**
Since python is an object oriented language as well, it provides strong support for allowing iteration over an object internals. Any object in python can behave like a sequence by providing the following :

a. It must implement the ___iter__()_ method which in turn supports the iterator protocol
b. The iterator protocol requires the returned object, an iterator,  to support the following ie. an ___iter__() _method returning self. and a _next()_ method which returns the next element in the sequence, or raise a StopIteration in case the end of iteration is reached.

_I've tested iterators which do not themselves have an __iter__() method and it still works, but I still do not give in to the temptation of not defining the next() method alone since that would be inconsistent with the documented python specifications _

Let us examine a simple class indicating a range. Note that this is just for demonstration since python itself has better constructs for a range.

``` python 
# An iterator object to sequentially offer the next number
# in a sequence. Raises StopIteration on reaching the end
class RangeIterator(object):
    def __init__(self,begin,end):
        self._next = begin - 1
        self.end = end
    def next(self):
        self._next += 1
        if self._next < self.end :
            return self._next
        else :
            raise StopIteration
            
# A class which allows sequential iteration over a range of
# values (begin inclusive, end exclusive)
class Range(object):
    def __init__(self,begin,end):
        self.begin = begin
        self.end = end
    def __iter__(self):
        return RangeIterator(self.begin, self.end)

oneToFive = Range(1,5)
```

In the above example, ___iter__()_ on Range returns an instance of a RangeIterator.

The way this capability is used is as follows :

``` python 
for number in oneToFive :
    print 'Next number is : %s' % number

print 'Is 2 in oneToFive', 2 in oneToFive
print 'Is 9 in oneToFive', 9 in oneToFive
```

The output you get with it is :

```
Next number is : 1
Next number is : 2
Next number is : 3
Next number is : 4
Is 2 in oneToFive True
Is 9 in oneToFive False
```

**Generators**

In the above situation, the state necessary for the iteration (ie.begin and end attributes) need to be stored. This was stored as a class member. Python also provides a function based construct called a generator. In case of a generator, the function should just do a _yield_ instead of a return. Python implicitly provides a next() method which resumes where the last yield left off and implicitly raises a StopIteration when the function completes. So representing the above class as a function,

``` python 
def my_range(begin, end):
    current = begin
    while current < end :
        yield current
        current += 1
        
for number in my_range(1,5) :
    print 'Next number is : %s' % number

print 'Is 2 in oneToFive', 2 in my_range(1,5)
print 'Is 9 in oneToFive', 9 in my_range(1,5)
```

I do not document the results since these are identical to the earlier code using a RangeIterator.

_Note: I used my_range() and not range() since there already exists another range() already provided by python_

An interesting point to realise is that in both the above situations, the next element to be returned in the sequence was being computed dynamically. To put it in the terminology better consistent with functional programming, it was being lazily evaluated. While python has no mechanism of lazy evaluation of functions, the ability of functions or objects to lazily evaluate the return values are sufficiently adequate to get the benefits of lazy evaluation at least from the perspective of cpu utilisation only on demand, and minimal memory utilisation. 

**List comprehensions**

List comprehensions are one of the most powerful functional programming constructs in python. To quote from python documentation (even as I leave out a very important part of the full quote .. to be covered later), 



> Each list comprehension consists of an expression followed by a for clause, then zero or more for or if clauses. The result will be a list resulting from evaluating the expression in the context of the for and if clauses which follow it. If the expression would evaluate to a tuple, it must be parenthesized.



A sample list comprehension is one which returns a sequence of even numbers between 0 and 9 :

``` python 
# A list comprehension that returns even numbers between 0 and 9
even_comprehension = (num for num in range(10) if num % 2 == 0)
print type(even_comprehension)
print tuple(even_comprehension)
```

The output one gets on running the code above is 

``` python 
type 'generator'
(0, 2, 4, 6, 8)
```

Note that the list comprehension returns a generator which then returns a sequence containing 0, 2, 4, 6 and 8.

The for and if statements can be deeply nested.

**lambda**

Lambdas are anonymous functions with a constraint. Their body can only be a single expression which is also the return value of the function. I will demonstrate their usage in the next section.

**map, filter and reduce**

Three of the functional programming constructs which probably have aroused substantial discussions within the python community are map, filter and reduce.

_map_ takes a sequence, applies a function of each of its value and returns a sequence of the result of the function. Thus if one were to use map with a function which computes the square on a sequence, the result would be a sequence of the squares. Thus

``` python 
def square(num): return num * num
print tuple(map(square,range(5)))
```

results into 

``` python 
(0, 1, 4, 9, 16)
```

I mentioned earlier I will demonstrate usage of a lambda. In this case I could use a lambda by defining the square function anonymously in place as follows :

``` python 
print tuple(map(lambda num : num * num,range(5)))
```

_filter_ takes a predicate and returns only the elements in the sequence for whom the predicate evaluates to true.

Thus

``` python 
print filter(lambda x : x % 2 == 0, range(5))
```

results in the following output

``` python 
[0, 2, 4]
```

Finally the most controversial of them all - _reduce_. Starting with an initial value, reduce reduces the sequence down to a single value by applying a function on each of the elements in the sequence along with the current manifestation of the reduced value. Thus if I wanted to compute the sum of squares of numbers 0 through 4, I could use the following 

``` python 
print reduce(lambda reduced,num : reduced + (num * num), range(5), 0)
```

In the above example, 0 as the right most argument is the initial value. range(5) is the sequence of numbers from 0 thru 4. Finally the anonymous lambda takes two parameters - the first is always the current value of the reduced value (or initial value in case it is being invoked for the very first time) and the second parameter is the element in the sequence. The return value of the function is the value which will get passed to the subsequent invocation of the reduce function with the next element in the sequence as the new reduced value. The reduced value as returned finally by the function is then the returned value from _reduce_

The functional programming folks are likely to find this an extremely natural expression. Yet reduce resulted in a substantial debate within the python community. With the result that reduce is now being _removed_ from python 3.0. (Strictly speaking it is being removed from python core but will be just an import statement away as a part of the functools package). See [The fate of reduce() in Python 3000](http://www.artima.com/weblogs/viewpost.jsp?thread=98196). Why so controversial ? Simply because the usage of map, filter, reduce above could be rewritten as 

``` python 
#map
seq = []
for num in range(5) :
    seq = seq + [num * num]
print seq

#filter
seq = []
for num in range(5) :
    if num % 2 == 0 :
        seq = seq + [num]
print seq

#reduce
total = 0
for num in range(5) : 
    total = total + (num * num)
print total
```

Just look at the two blocks of code and figure out which is easier to read (especially look at reduce). One of the areas where a large number of pythonistas and lispists are likely to disagree is the tradeoffs between brevity and easy readability. Python code is often english like (well, at least to the extent that a programming language can be) and some pythonistas do not like the terse syntax of lisp thats hard to follow. 

I earlier mentioned that I left out a part of the description of list comprehensions from python documentation. Here's that part of the quote.



> List comprehensions provide a concise way to create lists without resorting to use of map(), filter() and/or lambda. The resulting list definition tends often to be clearer than lists built using those constructs. 



Having said that I do believe many including myself will continue to use map, filter, reduce

Other helpful functions in python core that are helpful for functional programming are :




	
  * _all_ : Returns True if all elements in a sequence are true

	
  * _any_ : Returns True if any element in a sequence is true

	
  * _enumerate_ : Returns a sequence containing tuples of element index and element

	
  * _max_ : Returns the maximum value in a sequence

	
  * _min_ : Returns the minimum value in a sequence

	
  * _range_ : Return a sequence for given start, end and step values

	
  * _sorted_ : Returns a sorted form of the sequence. It is possible to specify comparator functions, or key value for sorting, or change direction of sort

	
  * _xrange_ : Same as range except it generates the next sequence element only on demand (lazy evaluation) thus helping conserve memory or work with infinite sequences.



We've seen many of the constructs that are typically useful for functional programming. I left out one big part - the _itertools_ package. This is not a part of python core (its a package which is available with a default python installation). Its a large library and substantially helps functional programming. That along with some more sample python programs shall be the focus of my next blog post in the series. At this point in time I anticipate at least a few more parts after that to focus on a) Immutability b) Concurrency and c) Sample usage.

Hope you found this useful and keep the feedback coming so that I can factor it into the subsequent posts.


