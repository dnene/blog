---
date: '2009-01-02 19:36:42'
layout: post
slug: 2009-is-not-a-prime-number-a-python-program-to-compute-factors
status: publish
title: 2009 is not a prime number. A python program to compute factors.
comments: true
wordpress_id: '335'
categories:
- programming
- python
tags:
- computation
- factors
- prime numbers
---

In my customary "Happy New Year" mail, I sent out a small note : 



> PS: Quiz : Is 2009 a prime number ? - Ans : No



Soon I started getting responses about that wisecrack of mine. The one that really stumped me came from a dear friend which said, 



> Enjoy the new year and happy computing.  BTW is 200000000000000000000000000009 a prime number?



Soon enough found a piece of python code to find factors - [Factor for Python](http://wj32.wordpress.com/2007/10/08/factor-for-python/) which seemed to find all the factors for a number. Wasn't likely to work too fast, so quickly modified it to the following code which finds all the prime factors for a number. Here's the code

**Update:** The first version had a bug which did not show the largest factor. Has since been corrected.


    
``` python    
    """
            Get the factors for a number
            (Note: Not optimised for tail recursion)
    """
    def factor(n):
            if n == 1: return [1]
            i = 2
            limit = n**0.5
            while i <= limit:
                    if n % i == 0:
                            ret = factor(n/i)
                            ret.append(i)
                            return ret
                    i += 1
    
            return [n]
    
    if __name__ == "__main__":
            import sys
            for index in xrange(1,len(sys.argv)):
                    print "Factors for %s : %s" %(sys.argv[index], str(factor(int(sys.argv[index]))))
```   



Quickly gave me the following output (used all of 26.2 seconds to get there) : 

    
```    
    # python getfactors.py 200000000000000000000000000009
    Factors for 200000000000000000000000000009 : [13430577524641L, 2094523, 89, 47, 47, 43, 29, 29]
```    



And in case you are wondering what are the factors for 2009


    
```    
    #  python getfactors.py 2009
    Factors for 2009 : [41, 7, 7]
```    



And which is the next year which is a prime number ?


    
```    
    #  python getfactors.py 2011
    Factors for 2011 : [2011]
```    



Pretty playful hacking for 10 mins I thought.

**Update:** The code above was written in 10 mins, but it kept bothering me .. just wasn't idiomatic python. So once I got some time, I got back to it and decided to write a generator instead. The results obviously stay the same but the time came down from 26 seconds to 4.7 seconds. I of course threw in an additional optimisation which had nothing to do with a generator - basically the value of i no longer restarts from 2, it resumes with the last factor (the same that was yielded and moves on from there). Here's the new code implementing a generator.


    
``` python
    """
      Generator for getting factors for a number
    """
    def factor(n):
      yield 1
      i = 2
      limit = n**0.5
      while i <= limit:
        if n % i == 0:
          yield i
          n = n / i
          limit = n**0.5
        else:
          i += 1
      if n > 1:
        yield n

    
    if __name__ == "__main__":  
      import sys  
      for index in xrange(1,len(sys.argv)):
        print "Factors for %s : %s" %(sys.argv[index], [i for i in factor(int(sys.argv[index]))])
```    







