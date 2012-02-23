---
date: '2008-02-20 23:50:10'
layout: post
slug: a-rejoinder-to-the-alternative-safehashmap
status: publish
title: A rejoinder to the alternative SafeHashMap
comments: true
wordpress_id: '21'
categories:
- java
---

Eugene responded to my earlier post "An even more capable SafeHashMap":http://blog.dhananjaynene.com/archives/20 with "It is safer not to invent safe hash map / Java":http://www.jroller.com/eu/entry/not_invent_safe_hash_map. While he does make some valid points I did have issues with some others, the issues being Signature Instability, Compilation Troubles, Principle of Least Surprise and Style.



### Extending predefined collection classes

bq. What bugs me is that they both extend HashMap, so the fancy "get or init" feature won't work with other map variants, not to mention that it is relatively bad practice to extend standard collections. 

Granted. If that is the objective, one way around the same would be as follows (though it does unfortunately require an additional step)


    
    
    public class SafeMapWrapper< K, V> implements Map< K, V>
    {
    	// The wrapped map
    	private Map< K,V> map;
    	// Here's where we cache away the provider
    	private InstanceProvider< K,V> provider;
    	
    	public SafeMapWrapper(Map< K,V> map, InstanceProvider< K,V> provider)
    	{
    		this.map = map;
    		this.provider = provider;
    	}
    
    	public V get(Object key)
    	{
    		V value = map.get(key);
    		if (value == null)
    		{
    			value = provider.createNew((K) key);
    			// I missed adding this in my earlier suggestion
    			map.put((K) key, value);
    		}
    		return value;
    	}
    
    	public void clear()
    	{
    		map.clear();
    	}
    
    	// 10 additional such delegating methods required here
    }
    



Another option that comes to mind is to be able to use aspects and intercept the get method. This way one doesn't need to either extend the base collection classes or wrap them with another object. The appropriate option to choose is really a judgement call.

However in Eugene's suggested alternative, here's where I believe are the issues :

### Signature instability

The suggested solution has the following signatures :


    
    
    static < V> Callable< List< V>> listCreator() { // ... }
    
    public static < K,V> V getOrInit(
          Map< K, V> map, K k, Callable< V> c) { // ... }
    



Lets pay attention to the first method. Depending upon whether the value type required is a List<V>, Map<V>, V or a V[], a new method definition would be required (just like a newer definition would've been required in my factory based approach).

However the factory based approach enforced a consistent signature which is that the return type was always V. In this case it would be either a Callable<V> or a CallableList<V>> or a Callable<V[]> or whatever else is required in the context. Depending upon the type of V being returned, *the signatures change*. While signature stability is not a requirement, it would definitely be preferred.

As an example I tried to create an example where instead of wanting to return a List<V>, I decided to return a V[]. Well - I just couldn't implement one since there is no way to create a V[] due to the difficulties in java of creating generic array types (one way to beat it is of course to create yet another factory :) ).

I tried to create an example where instead of returning a List<V>, I would want to return a Map<K,V>. I again ran into a difficulty since the other signatures now had to be changed - the Callable<V> had to now become a Callable<K,V>  (or alternatively a Map.Entry<K,V>). This would now require another instance of the getOrInit() method to get created since the signature is no longer stable.


### Compilation troubles


*Update: The original post has since been update and the comment in this section related to compilation trouble is no longer applicable*


When I tried to actually use the map, here's what I got :

Usage :

    
    
              getOrInit(map, "hello world",  listCreator());
    



The compiler responds with : The method getOrInit(Map<K,V>, K, Callable<V>) in the type SafeMap<K,V> is not applicable for the arguments (Map<String,List<String>>, String, Callable<List<Object>>)

_I could not get the code to compile and would welcome a suggestion asto how to get the code to compile._ 

### Principle of least suprise

Here's what Javadocs have to say about Callable 

bq. The Callable interface is similar to Runnable, in that both are designed for classes whose instances are potentially executed by another thread.

Is there a sufficient case here to use a Callable interface consistent with the way the author intended ? My assessment is that there is no sufficient evidence in this use case at least. So usage of a Callable interface is likely to sometimes surprise the developers using something different from the class than what is necessarily provides. I would certainly avoid it in this context.

### Usage style

Eugene's suggested solution meets Eric's basic requirement of brevity. However the side effect is the introduction of a number of static imports. Static imports form a perfectly valid and workable mechanism of implementing various java constructs in. However they take away from object orientation when used in scenarios where object oriented alternatives exist. Would I personally prefer to use that approach. No. But that would be due to a preferred style and not due to inherent validity or invalidity of the approach.


