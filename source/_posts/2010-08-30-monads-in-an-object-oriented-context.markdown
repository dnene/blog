---
date: '2010-08-30 20:44:58'
layout: post
slug: monads-in-an-object-oriented-context
status: publish
title: Monads in an Object Oriented context
comments: true
wordpress_id: '1056'
categories:
- functional programming
- programming
- software
tags:
- monads
- object orientation
- ood
---

The other day I was referred to a blog post [jQuery is a Monad](http://importantshock.wordpress.com/2009/01/18/jquery-is-a-monad/). That is an interesting post which if you have any interest in Monads should read. My first thought was that jQuery was another implementation of Fluent Interface and it just did not strike me as a monadic construct.

There are blog posts which take an essential thought or a message and by putting together various arguments bring it to some logical conclusion. This post is not one of them. It just describes the results of my thought process which started off contesting the assertion, to accepting it and then realising that either way, it really didn't matter a whole lot. I don't claim it to be sufficiently accurate except to state that it is accurate only to the best of my belief and understanding. Pointers to any inaccuracies or alternate interpretations in the comments shall be gratefully received. And yes, this post in the end reaches a somewhat boring conclusiion.

Having got that little expectation management out of the way, lets get back to monads, jQuery and Object Orientation. Writing a tutorial on Monads is a rite of passage for many. Thats something I've never done. And I don't wish to, but I cannot escape the fact that I'm going to have to introduce monads to those who are as challenged in their understanding as I am. 

**What are Monads ?** 

I'll give you the loose idea. Whenever a function completes any computation, the results of the computations are often returned as the return value of the function. But every once in a while you need a container around that return value. A container to store the state of a computation, sometimes with additional metadata or historical annotations for use later. At its very basis, a monad is a container for storing computations and being the container to be carried between functions. Think of it as a shipping container if you will, which moves materials between factories - the factories being various functions which operate on the contents of that container. 

But it takes more for a monad to be a monad than just being a container. Given any content, the monad should be able to construct itself by wrapping itself around the content. Thats what the source factory invokes when it fills up the container with goods. Some prefer to call it a type constructor - the type here being the container, or the monad (which may internally contain goods of various types, but I get ahead of myself).

In addition once the container reaches the destination factory, unlike typical container, this one does not just allow its contents to be unloaded. Nay, that would make it too simple. In this case, the container has to be taken to a factory where it allows a robotic arm of the factory to plug into a receptacle it provides, which the factory can use to extract the underlying values, often one at a time. The factory further processes these goods, which again come out at the other end of the factory as a different set of containers. And it is quite likely that the type of the goods in the new containers could've changed. 

That in essence is a monad. Easy, right ? I'm sure not and I'm now only going to make it a bit harder.

**The wikipedia, definition of monad [Monad](http://en.wikipedia.org/wiki/Monad_(functional_programming))**

I'm just going to let you read the definition as provided by wikipedia.



> 
A monad is a construction that, given an underlying type system, embeds a corresponding type system (called the monadic type system) into it (that is, each monadic type acts as the underlying type). This monadic type system preserves all significant aspects of the underlying type system, while adding features particular to the monad.
The usual formulation of a monad for programming is known as a Kleisli triple, and has the following components:

> 
> 
	
> 1. A type construction that defines, for every underlying type, how to obtain a corresponding monadic type. In Haskell's notation, the name of the monad represents the type constructor. If M is the name of the monad and t is a data type, then "M t" is the corresponding type in the monad.
> 2. A unit function that maps a value in an underlying type to a value in the corresponding monadic type. The result is the "simplest" value in the corresponding type that completely preserves the original value (simplicity being understood appropriately to the monad). In Haskell, this function is called return due to the way it is used in the do-notation described later. The unit function has the polymorphic type t→M t.
> 3. A binding operation of polymorphic type (M t)→(t→M u)→(M u), which Haskell represents by the infix operator >>=. Its first argument is a value in a monadic type, its second argument is a function that maps from the underlying type of the first argument to another monadic type, and its result is in that other monadic type. The binding operation can be understood as having four stages:
>>  1. The monad-related structure on the first argument is "pierced" to expose any number of values in the underlying type t.
>>  2. The given function is applied to all of those values to obtain values of type (M u).
>>  3. The monad-related structure on those values is also pierced, exposing values of type u.
>>  4. Finally, the monad-related structure is reassembled over all of the results, giving a single value of type (M u).
> 


In object-oriented programming terms, the type construction would correspond to the declaration of the monadic type, the unit function takes the role of a constructor method, and the binding operation contains the logic necessary to execute its registered callbacks (the monadic functions).

In practical terms, a monad (seen as special result values carried throughout the pipeline) stores function results and side-effect representations. This allows side effects to be propagated through the return values of functions without breaking the pure functional model.




**The bind operation**

So coming back to the container analogy, the container is the monad, the goods are the underlying types, and the factory is a function which takes a underlying type and spews out another monad which contains the newly manufactured goods. There is a difference to be noted, the container doesn't go into the factory, its the factory thats plugged into the monad through the bind operation, which triggers the processing. Also note that the output of each such factory is not just goods, but containers of such goods it manufactures.

There, you can now imagine a series of containers and factories, or monads and functions which are sequenced to assemble a pipeline to produce the desired goods / computations.

Before we go on to how this works in OO, I would like to spend some more time on the function thats provided to the binding operation. This function accepts the individual contents of the incoming container and returns a container with the newly manufactured goods. A somewhat similar function is used when using map operations on containers eg. Lists. So if I had a list of integers, and I wrote a function which doubled their values, then the map operation would be written in python as :

``` python
def twice(val): return str(2 * val)

print map(twice,(1,2,3,4,5))

#Expected output is :['2', '4', '6', '8', '10']
```

This is a good example where the container (tuple) is unbundled inside the map operation, each constituent value of the list is passed to the _twice_ function, and all the results of the twice function are reassembled into yet another container (list). The type of the constituents also changed from ints to str (string) along the way. To be a strictly monadic construct, the twice function should've returned not just the double of the value, but a double contained in some other container (either a tuple or a list), and the map function should've extract the individual strings from each of the individual tuples/lists and constructed a final tuple / list out of the same.

While unlikely to be confused as so, let me restate for clarity, the _map_ function is not the monad. The tuple and the lists are the monads here. The map function is an example of the contextual capabilities that monads expect from their environment (eg. via and around the bind operator in haskell). And _twice_ is the operation thats performed on the monad. Again to restate, the monadic chain is like a set of factories that spew out containers, which allow other factories to plug into them and in turn extract the contents and then spew out even more containers.

**Monads and Object Orientation**

So how does this work in an object oriented context ?

Lets take rules 1 and 2 of a monad. If I was to declare a class which had a constructor which took in a value and then wrapped it, then I would satisfy these two rules for being then be able to suggest that the class is a monad. However the rule 3 gets a little bit more interesting. Object oriented languages have the _"."_ operator which is somewhat analogous to the _do block_ which can chain operations. So if I was to write _o.foo()_ that would be equivalent of suggesting that I invoke the method foo() on the object which being a member method of the class has access to all the object internals and thus is able to access the wrapped value and do the necessary computations. Now if _foo()_ were to return any object again of a class which satisfies these very rules, then I would be able to say that this class along with its member function foo() is a reasonable object oriented analogy of the monad. And I would be able to start chaining the methods as in _o.foo().bar().baz()_

Whoa! let me restate that again. If a class, has a constructor which takes a value and wraps that value, and has a number of additional member functions which each operate on these underlying values and return an instance of either the same object or another object of a class which follows the same rules, then I can say that that particular class is a monad. Incidentally thats exactly what fluent interfaces do, except that they do not have any specific expectation of wrapping. And a large number of classes may incidentally fit this description. And if jQuery is a monad, so are all of them.

Well, we did relax a few constraints along the way. First of all the functions are not stand alone functions. They are member methods which have direct access to the underlying wrapped value. Secondly they themselves don't return a monad around an individual item in a collection. They return a monad around the entire collection of values. Thus the complexities and capabilities of the do block and the bind function are substantially simplified when using the "." operator. And finally going back to the container analogy, the class defines and consists of both the containers and the factories. Seems like the threshold for stating a particular class is a monad is actually quite low. Turns out we reached a boring end. And it seems we are not much wiser at least in terms of any specific conclusions or insights. But every once in a while sometime the journey is more exciting than its end. For me this did seem like one.

**Why are objects simple and monads complex ?**

At least for me the above was true. Turns out objects collate related data and functions together into one class. Also the do block and bind operator on an object is very simple. So for many especially coming from the OO school, objects are well understood. On the other hand understanding the requirements of monadic constructs takes quite some time. So there's a lot of gray cells that need to be exercised to start understanding what a monad is and how the various requirements for a monadic construct can be satisfied. And when mapping between monads, and objects which can also be seen to be monads, a lot of that complexity is either partially waived (eg. functions returning monads, the infrastructure around bind operators unbundling and bundling monads) or just simplified (functions and underlying values being colocated in the same class, the "." operator being a far more simplified version of do block). But after some substantial headaches, it does start to seem that perhaps, just perhaps, monads aren't so complicated after all :).

So is jQuery a monad ? I believe one could choose to be very pedantic and point out minor issues with that assertion. Or one could accept that the intent of monadic sequences are well represented in jQuery chains and accept it as one. I started with the former and ended at the latter position. Would I express that jQuery is a monad ? To the monadically challenged - No. That obfuscates far more than it enlightens. To them I would say jQuery is a fluent interface which allows continuous chaining of operations on an underlying set of dom objects. And is there a big "Aha moment" when one realises that jQuery is a monad ? I couldn't find one. While the journey of trying to understand monads and correlate it with monads was very exciting, at least the OO practitioner in me is unlikely to have missed much had I not known that. But I got to understand monads better - and thats well worth the time, and all the headspinning.



 

