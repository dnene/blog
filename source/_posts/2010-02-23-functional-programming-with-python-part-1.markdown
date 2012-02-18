---
date: '2010-02-23 23:35:51'
layout: post
slug: functional-programming-with-python-part-1
status: publish
title: Functional Programming with Python - Part 1
wordpress_id: '970'
categories:
- functional programming
- programming
- python
- software
---

Lately there has been a substantial increase in interest and activity in [Functional Programming](http://en.wikipedia.org/wiki/Functional_programming). Functional Programming is sufficiently different from the conventional mainstream programming style called [Imperative Programming](http://en.wikipedia.org/wiki/Imperative_programming) to warrant some discussion on what it is, before we delve into the specifics of how it can be used in Python.

**What is Functional Programming?**

To quote from Wikipedia,


> In computer science, functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids state and mutable data. It emphasizes the application of functions, in contrast to the imperative programming style, which emphasizes changes in state.


For beginners, one of the most fluent starter pages I would recommend for the history and specifics of functional programming is [Functional Programming For The Rest of Us](http://www.defmacro.org/ramblings/fp.html). This is a must read article which provides the reader with a good overview without getting too much into the nitty gritties of functional programming.

Since a detailed discussion on functional programming (henceforth referred to FP) is beyond the scope of this post, I will just briefly summarise the most critical elements of FP.



	
  * **Functions as the basic building blocks :** Unsurprisingly FP requires the construction and usage of functions as the basic building units. In the simplest terms "_int add(int x, int y) { return x + y; }_" is a simple addition function written in 'C'. It takes two parameters x and y, adds them, and returns the result. This is a rather obvious and simple case but I stated it since I would like to refer back to it subsequently in this post.

	
  * **Functional Programming prefers functions without side effects : ** The _add_ function above is a good example of a function without side effects. A function is said to be without side effects if the only changes it makes are those that are manifested in the return values. In other words such a function cannot change any global variables, write to the console, update the database etc. A fairly related term is _referential transparency_. A function is said to be referentially transparent if its invocation can be substituted by the return value in a program without impacting the program in any other way.

	
  * **Immutability **: Pure functional programming often requires you to deal with immutable data structures. Thus the value of any variable is not open to modification (Thus they are called values and not variables). This aspect complements the functions without side effects. Thus the way most changes to state are implemented are not by modifying an object in place (which is how imperative programming deals with it) but by cloning the data structure with some of the values getting modified and the modified data structure being returned by the function. Java programmers are aware of the immutability of the String instances wherein any modifications to the string result in a new String instance being created. Imagine the same happening to all the datatypes across the program.


**Benefits of Functional Programming :**

Some of the nice benefits (I am tempted to say side effects) of functional programming are :



	
  * **Superior ability to deal with concurrency (multi threading) : **Threaded programs are nasty to write. And nastier to debug. In an imperative environment, you not only have to deal with data structures being modified in place by some other parts of the program, in a threaded environment such modifications can happen using peer threads, even as your current thread whose logic you are focusing on is attempting to exercise that logic. Its an extremely unpredictable environment which has resulted in a number of how-to's for safe threaded programming using constructs such as locks, mutexes etc. Functional programming deals with the issue far more elegantly. Instead of controlling and managing unpredictability, it takes it out completely. Because a data structure once constructed will not be modified and because the source of the modifications can be clearly located to the function which instantiated the datastructure, the unpredictability of data changing right under you is gone. This can be a little expensive to manage and FP does sometimes come up with some compromises (or cool features depending on how you view it) such as Software Transactional Memory but a discussion on that is completely beyond the scope of this post.

	
  * **Easier testing and debugging : **Because modifications to data are contained and because a function communicates with the context outside it only via its return values, testing and debugging become far easier. You essentially need to focus on testing each function individually. Similarly during debugging you need to be able to quickly locate the function likely to have the problem, after which you can easily focus on the function to be able to quickly resolve the issue. Mocking out functions can also help testing each function in isolation. In general because of fewer side effects, testing under functional programming is often a lot easier, and the importance of having to do "integration" testing and "module" testing is lesser since testing functions in isolation is likely to identify most issues, far more than in typical imperative programming.


**Why Python?**

Python is not the best functional programming language. But it was not meant to be. Python is a multi paradigm language. Want to write good old 'C' style procedural code? Python will do it for you. C++/Java style object oriented code? Python is at your service as well. Functional Programming ? As this series of posts is about to demonstrate - Python can do a decent job at it as well. Python is probably the most productive language I have worked with (across a variety of different types of programming requirements). Add to that the fact that python is a language thats extremely easy to learn, suffers from excellent readability, has fairly good web frameworks such as [django](http://www.djangoproject.com/), has excellent mathematical and statistical libraries such as [numpy](http://numpy.scipy.org/), and cool network oriented frameworks such as [twisted](http://twistedmatrix.com/trac/). Python may not be the right choice if you want to write 100% FP. But if you want to learn more of FP or use FP techniques along with other paradigms _Python's capabilities are screaming to be heard**.**_

**Sample Program :**

I debated whether I should introduce various elements of functional programming using python in detail and then put it all together in a sample program all in future blog posts of this series, or whether I should start with a sample program which cover various aspects of function programming in this post and then explain various aspects in much more detail in future posts. For better or for worse, I have chosen the latter option. That means I shall be explaining one sample program and shall leave it to future posts in this series to get into greater details.

The sample program I have chosen is that of a simple calculator. A typical calculator supports simple unary or binary mathematical operators and performs floating point operations. Without much ado we now get into the sample program.

_Immutable Data :_

``` python 
from collections import namedtuple

Context = namedtuple('Context','stack, current, op')
def default_context():
    return Context([],0.0,None)
```

Python is not particularly strong at immutable data. However one of the data structures, a tuple is immutable. A _namedtuple_ is another data structure which supports both tuple like access through indices or through named elements in the tuple. For the calculator I shall need a _Context_ which contains a _stack_ for storing any incomplete operations, an attribute _current_ reflecting the current value being shown on the screen and an _op_ which might reflect a pending operation which is typically required for binary operators where the second value still needs to be provided. While namedtuple is a reasonable construct for simple tuple like objects, it would be helpful to have immutable objects as well - but thats to be covered in a future post.

_Simple Functions_

``` python Simple Arithmetic operations
def add(x,y): return x + y
def sub(x,y): return x - y
def mult(x,y): return x * y
def div(x,y): return x/ y
def reverse_sign(x): return -1 * x
def pow(x,y): return x ** y
```

There's not much to describe here. The functions should be self explanatory. For purpose of emphasis I would like to note that in the above code, "add" is now an entry in the namespace which is a reference to a function. This reference can be passed around, assigned to other entries. Thus the code below should work (though it does not form a part of the calculator program). This is to demonstrate how python treats attributes and functions virtually identically consistent with the [Uniform access principle](http://en.wikipedia.org/wiki/Uniform_access_principle).

``` python  function assignment
otheradd = add
add = sub
assert otheradd(7,3) == 10
assert add(7,3) == 4
```

_Currying_
[Currying](http://en.wikipedia.org/wiki/Currying) is a treatment afforded in functional programming which allows a function of n parameters to be treated as a sequence of n sequential functions each of one parameter.

``` python partial application of functions
from functools import partial

square = partial(pow,y=2)
```

Here _partial_ is a function reference. _square_ now refers to another function with its y parameter value being anchored to 2

_Invoking functions dynamically_

``` python Invoking functions dynamically
unary_functions = {'!' : reverse_sign, '@' : square }

def handle_unary_op(ctx,x):
    return ctx._replace(current = unary_functions[x](ctx.current), op = None)

binary_functions = {'+' : add, '-' : sub, '*' : mult, '/' : div}

def handle_binary_op(ctx,x):
    return ctx._replace(op = binary_functions[x])

def handle_float(ctx,x):
    if not ctx.op : 
        return ctx._replace(current = x)
    else : 
        return ctx._replace(current = ctx.op(ctx.current,x), op = None)
```

Note that I created a unary_functions dict (or dictionary or hashmap) where the key is the character which represents the function and the value is the reference to the function.

Also note that in the _handle_unary_op_function, I invoke _ctx._replace_ method. On a named tuple it creates another tuple based on the existing namedtuple data, but with some of the values modified as specified in the keyword paramters passed to __replace_. After looking up the appropriate unary function ie. _unary_functions[x]_, I also invoke it on the current value ie. _unary_functions[x](ctx.current)_. I also defined another dict for binary operators. The _handle_binary_op_ method reflects how the op in the context is set to the appropriate binary function that should be triggered after the subsequent value is known.

Finally the _handle_float_ function either sets the current value to the incoming value or in case the current operator is already set it applies the binary operator to the current value and the incoming value and replaces _current_ with the computed value.

_Additional Code_
When I wrote the calculator program, I wrote the functionality to introduce braces. However that functionality is not particularly important in this explanation. So it is being listed here for completeness.

``` python Brace matching code
def start_brace(ctx): 
    newstack = ctx.stack
    newstack.append((ctx.current,ctx.op))
    return ctx._replace(
                stack = newstack, 
                current = 0.0, op = None)

def end_brace(ctx):
    stack = ctx.stack
    current = ctx.current
    oldcurrent, oldop = stack.pop()
    
    oldctx = Context(stack,oldcurrent,oldop)
    return process_key(oldctx,current)

tokens = { '(': start_brace, ')' : end_brace}

def handle_tokens(ctx,x):
    return tokens[x](ctx)
```

_Processing one key_
I must confess I started off using key to represent the keystrokes, but along the way the key can also represent a complete floating point number (not just a single keystroke). Thus the key parameter can refer to a single character operator or a sequence of characters representing a floating point number

``` python grouping function and processing key press
function_groups = {
    tuple(unary_functions.keys()) : handle_unary_op,
    tuple(binary_functions.keys()) : handle_binary_op,
    tuple(tokens.keys()) : handle_tokens
}

def process_key(ctx,key):
    if isinstance(key,(types.FloatType,types.IntType, types.LongType)) :
        return handle_float(ctx,key)
    elif isinstance(key,(types.StringType)) :
        for function_class in function_groups :
            if key in function_class : return function_groups[function_class](ctx,key)
    return ctx
```

In this case I set up a dictionary where the key is a tuple of all the keys representing a particular class of a function. Note that the _.keys()_ method is a method which returns a list of all the keys in a dictionary. However since list is mutable, it cannot get used as a key into the overall hashmap, hence I convert it into a tuple.

The _process_key_ function takes the incoming key, passes it _handle_float_ if it is a number, or treats it as an operator. If it is the latter it searches for it in all the keys of each operator groups, and if it finds a match, it locates the corresponding handler function from the map and invokes it. Finally in case no match is found it ignores the key.

_Processing a sequence of keys_
``` python Processing a sequence of keys
def process_keys(keys):
    return reduce(lambda ctx,key : process_key(ctx,key), keys, default_context())
```

Here you see a _reduce_ function being invoked. This belongs to the family of _map_ and _filter_ functions which are used extensively in functional programming.  I shall attempt to briefly explain it here, but this family of functions in addition to a number of others will again be dealt with in a future blog post.

To interpret the usage read the above reduce statement right to left. Thus we start with a default context, and for each key in the sequence of keys, we invoke a lambda (thats like an anonymous function), which calls process key with the context and the key. Note that the first parameter to the lambda is either the initial value (the default context) or the return value of the last process_key (which is also a context) and the key is each key in the keys sequence injected sequentially.

To further make it easy I re-represent the same function below differently which is much more readable and easier to understand. This shows one more strength of python. Because of its focus on readability, it actually can be used to write functional programs are much more readable by a large mass of programmers than most of the functional programming languages themselves (readability being subjectively interpreted by me as what is most natural for english or similar language speaking people).
``` python
def process_keys(keys):
    ctx = default_context()
    for key in keys :
        ctx = process_key(ctx,key)
    return ctx
```

_Usage_
As a mechanism to conduct some rudimentary tests on the code written so far, the following code is introduced. Here you can get an overall feel of the program.

``` python Sample usage
if __name__ == "__main__" :
    assert process_keys((2,'+', 3)).current == 5
    assert process_keys((2, '!', '+', 5)).current == 3
    assert process_keys((2, '@')).current == 4
    assert process_keys((2,'+',3,'*',5)).current == 25
    assert process_keys((2,'+','(',3,'*',5,')')).current == 17
```

_Some more slightly advanced Functional Programming_

Finally to tickle your interest even more, here's a slightly more advanced usage of functional programming constructs. Here I shall add all numbers between 1 through 10.

``` python Generating sequential numbers
from itertools import chain

def plus_num_seq(n):
    count = 1
    while count <= n :
        yield '+', count
        count += 1

keys = list(chain.from_iterable(plus_num_seq(10)))[1:]
assert process_keys(keys) == 55
```

The _plus_num_seq_ is a generator. Note the usage of the yield statement. Thus it will continuously generate tuples with the first element of the tuple being the '+' character and the second being the number with the number varying from values 1 through n. The _chain.from_iterable_ flattens the generated list (it thus has 20 items for n = 10, each alternate one being the '+' character starting with the first items). Since we do not need the very first '+' character, I removed it using the [1:] slice operator. 

Just like _reduce_ this style of code is quite typical of functional programming. Thats something I shall detail upon much more in future posts. 

Hope you enjoyed the post. Keep the feedback coming so I can better structure the subsequent posts based on the feedback. 

Note: The full source for the calculator _calculator.py_ can be accessed [here](http://blog.dhananjaynene.com/wp-content/uploads/2010/02/calculator.py_.txt).

Update: I much later also conducted a presentation on the same topic at Pycon India 2010. The slides to the topic can be found at the bottom of [this page](http://in.pycon.org/2010/talks/66-functional-programming-with-python) (direct link: [talk.html](http://in.pycon.org/2010/static/files/talks/66/talk.html))
