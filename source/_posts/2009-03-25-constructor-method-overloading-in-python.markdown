---
date: '2009-03-25 10:14:31'
layout: post
slug: constructor-method-overloading-in-python
status: publish
title: Constructor / Method overloading in Python using Function Switching
wordpress_id: '583'
categories:
- programming
- python
- software
tags:
- duck typing
- overloading
---

While not primarily a Functional Programming (FP) language, python has long had a modest support for many of the constructs supporting FP. A while back I was studying Monads and Haskell and was curious to see if some of these constructs were applicable in the problem domains I was working on. I did run into a block very soon since Haskell/Monads have a support for invoking different functions based on type and that support is important for the way many of the constructs are modeled. However method overloading is not feasible at a language level in Python, and thus its difficult to create similar constructs.

A month later I ran into this article [Tighter Ruby Methods with Functional-style Pattern Matching, Using the Case Gem](http://blog.objectmentor.com/articles/2009/03/16/tighter-ruby-methods-with-functional-style-pattern-matching-using-the-case-gem) which again piqued my interest in the same matter. Thats when I looked at the topic once again and came up with the following similar example (similar to the one in the blog post referred to), to demonstrate constructor overloading.

Note that method overloading is not consistent with duck typing approaches for many. However many a time we do run into a situation where conditional logic is required based upon types. As one attempts to bring in a more FP oriented style, I suspect the need for type matching is likely to only increase. As an example you may have a reference to a `stream` variable which you need to interpret differently based on the format, say xml or json. In such case one approach is to have two functions `parse_as_xml(stream)` and `parse_as_json(stream)` What the construct below allows you to do is to have a common function around the two `parse(stream)` which can switch between the two internally.


    
``` python
    class X(object):
        def __init__(self, *val):
            # Matcher functions. These return true or false for the type matching
            # Possible enhancement could be to have  generic function
            check_dict = lambda x : isinstance(x[0],dict)
            check_sequence = lambda x : hasattr(x[0],'__iter__')
            check_args = lambda x : len(x) == 3 
            
            # Type specific initialisers
            def initialise_from_dict(hash):
                self.first_name = hash['first_name']
                self.last_name = hash['last_name']
                self.age = hash['age']
            
            def initialise_from_args(*args):
                self.first_name = args[0]
                self.last_name = args[1]
                self.age = args[2]
                
            def initialise_from_sequence(seq):
                self.first_name = seq[0] 
                self.last_name = seq[1] 
                self.age = seq[2]
            
            # Matching data. This is the switching data based on which the appropriate 
            # function is called.    
            dispatch_data =((check_dict,initialise_from_dict),
                            (check_sequence,initialise_from_sequence),
                            (check_args,initialise_from_args))
            
            # The switch
            for check,func in dispatch_data :
                if check(val) :
                    func(*val)
                    break
                    
        def __str__(self):
            return "%s %s(%s)" % (self.first_name, self.last_name, self.age)
        
    
    print X('hello','world',33)
    print X(('greetings','earthlings',44))
    print X({'first_name' : 'john','last_name' : 'doe', 'age' : 45})
```   



Essentially it boils down to create a list of function pairs, the first to be executed to check if the second should be performed. Some of the characteristics of this approach are :



	
  * Even thought the example is for constructor overloading, the same could be applied to normal function overloading at both class and module level

	
  * No external packages being used

	
  * The different functions which serve the same intent are actually packaged as nested functions within the function thats being attempted to be overloaded. This allows for a cleaner structure. However that is not a requirement. The functions can be moved into the top level name space and the only change that will be required in the example above is the necessity to explicitly pass the "self" argument as the first argument.



One of the possible enhancements to this could be to have a generic pattern matching function and make the first argument in each of the items in the matching sequence (`dispatch_data` in above case) be some pattern data which results in the pattern matcher function returning a `True` or a `False`.

This is not the only way to overload but I found it amongst the easier and simpler structured approaches.  




