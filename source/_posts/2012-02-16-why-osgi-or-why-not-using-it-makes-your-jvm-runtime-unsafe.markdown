---
date: '2012-01-21 18:22:36'
layout: post
slug: why-osgi-or-why-not-using-it-makes-your-jvm-runtime-unsafe
status: publish
title: Why OSGi? Or why not using it makes your JVM runtime unsafe.
comments: true
wordpress_id: '1221'
categories:
- programming
- architecture
- java
tags:
- osgi
---
Not sure how long ago I started using OSGi. Perhaps it was 12 months ago or then perhaps 18. And yet I still find it painful using OSGi especially every time I bring in a foreign set of jars into the ecosystem. And yet I continue to be a dogged proponent. Here’s why.

First let us understand one of the many problems OSGi solves. Let us imagine your java application has exactly three classes. One is the class you wrote called “My.java” bundled in a jar called “my.jar”. Another is a class called “Uses.java” whose api and features are leveraged by “My.java” and is in a jar called “uses.jar”. Finally there is yet another class “Transitive.java” which is leveraged by “Uses.java” and lies in a jar called “transitive.jar”.

Ideally when you run your application you should have all the three jars available in your classpath. Since you did not write “Uses.java”, how would you know about its transitive dependency on “transitive.jar” ? If you are fortunate, the build tool used to create “uses.jar” would have created a corresponding “uses.pom” or similar file which would allow your build tool to discover the transitive dependencies and assemble them all together. But what if that wasn’t true?

Turns out these situations are far more frequent than one would imagine. When I started using OSGi, I imagined such cases to be rather unlikely. And yet I have found a large number of situations. Sometimes the unrecorded transitive dependencies are in a profusion of xml libraries, sometimes on alternate logging tools, sometimes even on packages from a different jvm like bea and at least in one case even on dalvik.

So how do you know the libraries you are using have all their transitive dependencies recorded (and in any typical java application today there are literally tens of such jars)? Turns out you don’t.

### The OSGi way :

OSGi requires you to record the metadata for the jar within the jar itself. Thus in the case above, “my.jar” would document the fact that it depended upon packages from “uses.jar”. In turn, the metadata on the “uses.jar” would record the fact it exports a certain set of packages (which “my.jar” uses) and also requires (imports) other packages which incidentally also happens to be exported by “transitive.jar”. The OSGi runtime will make sure all the dependencies are appropriately resolved. Thus in this situation if you attempted to run the application without “transitive.jar”, OSGi would flag that off as an error since some of the package imports would not get satisified and your application will not start until you add the third jar into the classpath.

### The JVM runtime by default is unsafe

Java is often considered safe given its static typing. But that is just half the story. As the example above shows, its runtime is unsafe. And if you are a library author, you are in turn contributing your share by carrying forward that unsafeness into your user’s domain. The compiler makes sure you have compiled the code correctly to the interface of a given type. But the runtime does not guarantee the presence of that type. At a code level even if you think you’ve proved your code works to the extent you could, quite frankly the poor runtime makes such a proof quite illusionary. Since you have no guarantee your code will work even if it compiled successfully. And if you want to test such errors, how on earth will you even test such conditions ?

### Library Vendors : Please make your jars OSGi compatible

For every hour you save by not making your jars OSGi compatible, you are requiring your “n” users who are interested in ensuring a stronger safety of their system to spend “n” hours ie. one each. Believe me, I’ve spent enormous amounts of time doing this as a user. Thats what gives OSGi a bad rap. Thats what makes people say it is complex.

OSGi is really trying to make your application and java runtimes in general safer (There are many other capabilities, but I’ve focused on only one). Do use it when you publish your jars. And do encourage others to do so.


