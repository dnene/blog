---
date: '2008-01-03 14:19:26'
layout: post
slug: dynamically-extend-classpath-during-unit-testing
status: publish
title: Dynamically Extend Classpath during Unit Testing
comments: true
wordpress_id: '8'
categories:
- java
- software
tags:
- java
---

A lot of times I need to load data from configuration files which are supposed to be on the classpath. I also find the need to often change the configuration files for a variety of unit tests. Here's a useful trick I borrowed and restructured from Sun Java Forum Thread [Java Runtime Environment (JRE) - Modify Classpath At Runtime](http://forum.java.sun.com/thread.jspa?threadID=30055)

The following is a Utility class with a static method that gets a class name. In this particular case the assumption is that the directory that needs to be added is same as that in which the class resides (Not necessarily always true - but its a convention I like and have adopted)


    
``` java    
    public class TestUtils
    {
    	public static void addURL(Class clazz)
    	{
    		try
    		{
    
    			URLClassLoader sysloader = (URLClassLoader) ClassLoader
    					.getSystemClassLoader();
    			Class<urlclassloader> sysclass = URLClassLoader.class;
    
    			String path = sysloader.getResource(
    					clazz.getCanonicalName().replace('.', '/') + ".class")
    					.getPath();
    			int lastSlash = path.lastIndexOf('/');
    			path = path.substring(0, lastSlash+1);
    			URL url = new URL("file://" + path);
    
    			Method method = sysclass.getDeclaredMethod("addURL", URL.class);
    			method.setAccessible(true);
    			method.invoke(sysloader, new Object[] { url });
    		}
    		catch (Throwable t)
    		{
    			t.printStackTrace();
    		}
    	}
    }
```    





In the unit test I first start off with importing the addUrl method statically


    
``` java    
    import static my.package.TestUtils.addURL;
```    



Subsequently in the unit test in a static block triggered once from the *_setUp_* method (junit3) or in a method with the *_@BeforeClass_* annotation (junit4) I pass the class


    
    
``` java    
    public class MyTest
    {
    	@BeforeClass
    	public static void initialiseClass()
    	{
    		addURL(MyTest.class);
    	}
    }
``` 



Since I use eclipse, I configure the same to make sure it treats any .xml or .property files that reside in the same directory as the test case class as files that need to be copied to the bin folder (else it will not work in eclipse)

Now I can go ahead and add the configuration files assuming that the folder containing the source for the MyTest class will also be a part of the classpath.

I have found it quite useful. YMMV.


