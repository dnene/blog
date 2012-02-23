---
date: '2008-04-17 23:28:28'
layout: post
slug: turbocharge-your-string-keyed-hashmaps
status: publish
title: Turbocharge your string keyed hashmaps
comments: true
wordpress_id: '25'
categories:
- java
- ruby
- software
tags:
- java ruby performance
---

This post gives you a small tip which just might make a world of difference to your java hashmap's performance. This trick has been inspired by the "symbol" construct in Ruby language.

I have often considered using hash maps using Strings as keys as quite expensive indeed. And in many ways they often are. However if the keys used in your hashmap are either a well known set at the time of either writing the code or at least when the program starts up, the following is likely to help you make your map performance much much zippier. 

In case you are not familiar with Ruby, it has a special construct called a symbol which is somewhat similar to a constant string. However you can create as many instances of it, but ruby runtime will ensure that multiple instances having the same character data will refer to the same runtime instance.

The design of any key will influence the performance of the hashmap primarily based on the performance of its hashcode and equals methods. The java.utils.HashMap implementation uses the result of hashCode() to narrow down the potential number of keys to be compared and then compares the keys based on whether they are the same instance (ie. occupy the same address space in memory)  or in case they aren't then by invoking the equals() method.

Thus if one wants to use Strings as keys, then there are at least two optimizations that could be potentially targeted :
(a) The hashcode could be cached rather having to be computed each time (Turns out this makes a positive but a rather small difference)
(b) Ensure that the same instance of strings get used for the same string data. (Turns out this does make a substantial difference).

The following two pieces of code indicate the difference.





#### Slower Code


 
    
``` java    
        map.put(new String("mykey"),/* .. some value .. */);
        Object o = map.get(new String("mykey"));
```    





#### Faster Code



    
``` java    
        String key = "mykey";
        map.put(key,/* .. some value .. */);
        // Note : In this case the same instance of the key is
        //            is used in both the get and the put
        Object o = map.get(key);
```     



The big reason why this makes a difference is the following line of code in java.util.HashMap


    
``` java    
    // The following line has two ampersand signs indicating a logical 
    // and. Formatting is destroying the way it looks 
    //     (and I do not know how to fix it)
    if (e.hash == hash && ((k = e.key) == key || key.equals(k))) ...
```    



Thus when comparing the keys, the code first tests whether they are identical and then they are equal. Obviously the test for identity is substantially inexpensive compared to that of equality. Thus the faster code shown above is faster since the keys are identical.

In order to be able to provide the same capability of ensuring that only one instance of a string key with a particular string data is constructed, while taking away the onus from the programmer of having to track the instance creation, a wrapper class called Symbol is used as shown below :

**Update:**I have updated the version to address Khalil's and Dave's concerns and suggestions. The important part of the modification is that the hashmap has been done away with and the getSymbol() method is modified. Earlier code which has been replaced has been commented out.


    
``` java    
    package com.dnene.utils.symbolmap;
    
    /**
     * License : Based on BSD Template
     * 
     * Copyright (c) 2008, Dhananjay Nene
     * All rights reserved.
     * 
     * Redistribution and use in source and binary forms, 
     * with or without modification, are permitted provided 
     * that the following conditions are met:
     *
     *    * Redistributions of source code must retain the 
     *      above copyright notice, this list of conditions 
     *      and the following disclaimer.
     *    * Redistributions in binary form must reproduce 
     *      the above copyright notice, this list of 
     *      conditions and the following disclaimer in the 
     *      documentation and/or other materials provided 
     *      with the distribution.
     *    * The name of the Dhananjay Nene  may not be used 
     *      to endorse or promote products derived from this 
     *      software without specific prior written permission.
     *
     * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND 
     * CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, 
     * INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF 
     * MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE 
     * DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR 
     * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, 
     * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL 
     * DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF 
     * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; 
     * OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY 
     * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT 
     * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT 
     * OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE 
     * POSSIBILITY OF SUCH DAMAGE.
     */
    
    import java.util.HashMap;
    import java.util.Map;
    
    public final class Symbol
    {
    	private final String t;
    	private final int hashcode;
    	// not required after update
            // private static Map< String,Symbol> map = new HashMap< String,Symbol>();
    	
            //  method simplified upon update
    	// public static Symbol getSymbol(String t)
    	// {
    	// 	Symbol symbol = map.get(t);
    	// 	if (symbol == null)
    	// 	{
    	// 		symbol = new Symbol(t);
    	// 		map.put(t, symbol);
    	// 	}
    	// 	return symbol;
    	// }
    	
    	public static Symbol getSymbol(String t)
    	{
    		return new Symbol(t.intern());
    	}
    
    	private Symbol(String t)
    	{
    		this.t = t;
            this.hashcode = t.hashCode();
    	}
    	
    	public final String get()
    	{
    		return t;
    	}
    
    	@Override
    	public int hashCode()
    	{
    		return this.hashcode;
    	}
    
    
    	@Override
    	public final boolean equals(Object that)
    	{
    		return ((that instanceof Symbol) ? 
                               (this.t == (((Symbol)that).t)) : false);
    	}
    
    }
```    



The code snippets shown earlier would be modified as follows to use **Symbol**

Symbol usage :


    
```    
        Symbol key = Symbol.getSymbol("mykey");
        map.put(key,/* .. some value .. */);
        // Note : In this case another instance of the symbol is created
        // Note : You can also use key.get() in this case so long as you use it consistently
        Symbol key = Symbol.getSymbol("mykey");
        Object o = map.get(key);
```    




**Update :**As per Dave's suggestion in using String.intern(), the following could alternatively be used. Note that the performance benefits are to be had only if the symbol construction or the intern() call are made far less frequently than the calls to the getter on the map.

    
```    
        Symbol key = new String("mykey").intern();
        map.put(key,/* .. some value .. */);
        // ... elsewhere in code
        Symbol key2 = new String("mykey").intern();
        Object o = map.get(key2);
```    







### Performance Difference


In my benchmarks involving million keys, the get performance of maps using identical String keys and symbols was almost the same (the symbol based implentation was roughly either slower by 3% or faster by upto 10% with the average performance of the symbol implementation being faster by 4%). I did not benchmark the puts since I did not imagine they would change much. However the Symbol  based implementation get method was consistently faster by at least 30% when compared to the String map performance when the string when the keys used during the 'put' where equal to but not identical to those used during the 'get'. There is an overhead of creating a symbol instance compared to a string, but under most circumstances I believe this should be much more than offset by the gains.   

What surprised me was that the Symbol based String keys beat the get performance of using Long keys quite handsomely and consistently. I am not sure why it works so and am not sure if I had made a mistake. However since using Long keys was not something I was particularly focused on - I did not hunt down the reason for the performance difference between Symbol and Long based keys.



**Update** : adding a sample output of performance benchmarking sample run. The runs consist of hashmaps of million entries each, with each key being 16 characters long, and a million lookups getting done. The times mentioned are in nanoseconds for each run of a million lookups.

    
    
    === Comparison of Symbol to Long ===
    Long lookup time : 246225003
    Symbol lookup time : 148041217
    Performance Ratio : 1.6632193
    Reduction in time : 39%
    === Comparison of Symbol to Identical Strings ===
    Usual lookup time : 151010200
    Symbol lookup time : 148041217
    Performance Ratio : 1.0200552
    Reduction in time : 1%
    === Comparison of Symbol to String Copies ===
    Copy lookup time : 231829056
    Symbol lookup time : 148041217
    Performance Ratio : 1.5659764
    Reduction in time : 36%
    



Am adding the link to the source and the driver files for running your tests independently. 
[Symbol source and driver program](http://blog.dhananjaynene.com/wp-content/uploads/2008/04/files-hashmap-turbocharge1.jar)



### Field of use


This should be useful in a fairly large proportion of typical applications. In most most situations the possible universe of keys in the hashmap are known upfront either when writing the code or when starting up the application. If instead of creating hard coded strings or by using various string key parameters from say an XML file .. just use a Symbol instead. The construction is a little expensive but the map gets run much faster.
  

This solution is not limited to a String. It could actually be used for any data structure which has a high cost of either hash code computation and / or equality check. In fact the version I wrote for myself was a Symbol<T>. The code shown above is only a specialised version where T is a String.

