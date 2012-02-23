---
date: '2008-02-15 17:56:06'
layout: post
slug: an-even-more-capable-safehashmap
status: publish
title: An even more capable SafeHashMap
comments: true
wordpress_id: '20'
categories:
- java
- software
---

Eric Redmond in his post "A Safe HashMap for Java":http://www.coderoshi.com/2008/02/safe-hashmap-for-java.html describes a SafeHashMap 

Here's a suggestion to extend the capabilities of the same. Lets look at the code.

### Define an interface to create an instance.

First, I create a new interface (We'll get to know why very soon).

``` java    
    public interface InstanceProvider< K,V>
    {
    	public V createNew(K k);
    }
```    




### The SafeMap class itself

Here's the proposed class. The main distinction is that instead of caching an instance and using the clone method to clone, this implementation stores away a reference to the instance provider and triggers it when required.


    
``` java    
    public class SafeMap< K, V> extends HashMap< K, V>
    {
    	// Here's where we cache away the provider
    	private InstanceProvider< K,V> provider;
    
    	// Note that the provider is now 
    	// passed to all the constructors
    	
    	public SafeMap(
    			int initialCapacity, 
    			float loadFactor, 
    			InstanceProvider< K,V> provider)
    	{
    		super(initialCapacity,loadFactor);
    		this.provider = provider;
    	}
    
    	public SafeMap(
    			int initialCapacity, 
    			InstanceProvider< K,V> provider)
    	{
    		this.provider = provider;
    	}
    
    	public SafeMap(
    			InstanceProvider< K,V> provider)
    	{
    		this.provider = provider;
    	}
    
    	public SafeMap(
    			Map< ? extends K, ? extends V> m, 
    			InstanceProvider< K,V> provider)
    	{
    		super(m);
    		this.provider = provider;
    	}
    
    	@Override
    	@SuppressWarnings("unchecked")
    	public V get(Object key)
    	{
    		V value = super.get(key);
    		if (value == null)
    		{
    			// use the provider here
    			value = 
    				provider.createNew(
    						(K) key);
    		}
    		return value;
    	}	
    }
```



### Test Case demonstrating usage.



    
``` java    
    public class TestSafeMap
    {
    	@Test
    	public void testGetObject()
    	{
    		// Am using an anonymous class here. 
    		// If additional parameters are required
    		// to be passed to constructor, one could 
    		// create an abstract class with the constructor
    		// and pass the necessary arguments 
    		Map< String, List< String>> myMap = 
    			new SafeMap< String, List< String>>(
    					new InstanceProvider< String, List< String>>()
    					{
    						public List< String> createNew(String string)
    						{
    							List< String> list = new ArrayList< String>();
    							list.add(string);
    							return list;
    						}
    					}
    		);
    		
    		String key = "hello world";
    		assertEquals(
    				"List size should've been one",
    				1,
    				myMap.get(key).size());
    		assertEquals(
    				"The only element in the list should've been : " + key,
    				key,
    				myMap.get(key).get(0));
    	}
    	
    	@Test
    	public void testArrayInstantiation()
    	{
    		Map< String, String[]> myMap = 
    			new SafeMap< String, String[]>(
    					new InstanceProvider< String, String[]>()
    					{
    						public String[] createNew(
    								String string)
    						{
    							return new String[] {string};
    						}
    					}
    			);
    			
    		String key = "hello world";
    		assertEquals(
    				"List size should've been one",
    				1,
    				myMap.get(key).length);
    		assertEquals(
    				"The only element in the list should've been : " + key,
    				key,
    				myMap.get(key)[0]);
    	}
    }
```    



This way I get a finer level of control on the instantiation of the default instance. There are multiple reasons why one might want that such as :

* The default instance needs to be configured in some way depending upon the key value (shown in the example above)
* The default instance needs to be configured based on some constructor parameters passed to it. In this case create an abstract class which implements the interface, declare a constructor with the necessary arguments, use the abstract class during instantiation, and allow the createNew method to behave appropriately based on the values of the arguments.
* There are some situations such as where the type above is a String[] where the clone method does not work (as in the second test case above). Perhaps the code to conduct the cloning could be modified above to create an array, but I am not too sure (since I've often faced difficulties working with instantiation of array types when using generics).


