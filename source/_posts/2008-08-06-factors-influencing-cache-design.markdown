---
date: '2008-08-06 17:10:18'
layout: post
slug: factors-influencing-cache-design
status: publish
title: Factors influencing Cache Design
comments: true
wordpress_id: '42'
categories:
- architecture
- software
tags:
- architecture
- cache
- caching
- design
---

A lot of posts talk about how caching can improve performance and how one can use tools such as memcached for the same. It is a great tool, and I am certain it does wonders, but I do wonder if it and its usage is getting too much press at the cost of other caching options. 

This post talks about the various dimensions that should be carefully examined before deciding upon the caching strategy. I would like to argue that the most difficult part of caching is not necessarily learning or selecting the caching tool but the detailed analysis of the application, its architecture and its data flow, that actually precedes the caching tool selection. I also submit that once this analysis is conducted and the various dimensions as described below well understood, the actual cache design then becomes a relatively simpler and lesser risky activity. Before getting into the dimensions of cache design, there is some important groundwork to be done.

_Note : I am referring to data caching. I am not referring to optimisation of opcodes / byte codes / instruction sets etc. which is a different class of caching._

**Know your application well :**

Let me emphasise this. If you want to decide on the most appropriate caching strategy, you need to have a sufficiently good understanding of your application. This is relatively easy if you are enhancing the caching capabilities in an existing application or are rebuilding an existing application. But if you are building a completely new application, some of the data listed below may not be easily available and you may need to project a little bit after understanding the requirements and a high level data / logic flow. One of the things you really need to have either a clear understanding or at least some reasonable projection of, are the various data elements in the application, how frequently will they be accessed and modified and given the processing flow, which elements make sense to be cached. You will also need to have a good understanding of how critical is it to have the most current data (eg will a 1 min. stale data be acceptable) and the transactional semantics around the data processing (are strict ACID semantics a must ?). 

Note that for the rest of this post when I mention data, I shall be referring to cachable data.

**Know your performance targets :**

Unless you are clear about your performance targets, it will be quite difficult to work out the appropriate strategy. I would suggest you should have clear targets defined (for a specified hardware) before proceeding down this path. Some times the target specifies very ambitious hardware which you may not have access to in early stages of the development. Try to convert these targets into appropriate targets for the hardware you shall be working with (with some reasonable margin of safety).

**Know your architectural guidelines / preferences :**

Will you be using a single server or multiple ? When handling future growth would you prefer to scale up or scale out ? What languages will you be using ? Would you prefer to use a single database instance or would you want to create vertically partitioned shards ? As we shall see, these are extremely important considerations before getting into cache design. Sometimes the decision making is not sequential, so you may end up revisiting these topics once you get into your cache design. 



### _Dimensions_



We shall now explore some of the various dimensions that influence cache design.



#### Data Scope



Various data elements are scoped differently. As an example highly transient in processing data is often transactional scoped, whereas application configuration is application scoped. It is important to understand how the various data elements are scoped.




	
  * **Application :**Data cached at this level is valid for the entire application. Examples of this are the list of users in the system (for a non vertically partitioned application), application configuration parameters etc.

	
  * **Node :**The data might be specific to the node (machine) on which it is being accessed. An example is the list of users managed by this particular node for an application which is vertically partitioned (sharded) based on users. 

	
  * **Process :**When your application consists of multiple processes, some data might be different for different processes. As an example, you may be using a Hi Lo Id generation strategy, and each process might have blocked a different range for the ids to be generated. The data here is mostly different for each process, but consistent within a process.

	
  * **Session :**Some data might be specific to the session. Thus a field called "expiresAt" which indicates a session expiry time will have a value which will be different from session to session, but will often need to be the same for the same session, irrespective of the process or the machine which is accessing it. Sample data here could be compute intensive privileges based upon necessary authorisations.

	
  * **Request :**The data might be specific to the HTTP request itself. eg. requestSourceIpAddress

	
  * **Thread :**The data might be specific to a thread. This is often what ThreadLocal variables are for. 

	
  * **Transaction:**The data might be specific to a transaction in progress. eg. you might have just inserted a new row, and are now inserting another one whose has a foreign key which happens to point to the row you just inserted earlier. Prior to committing the data, its scope and visibility is limited to that of the transaction.



Why is the scope important. Because of the following :

	
  * Cache speeds depend substantially based on the scope of data. Distributed caches such as memcached (required for application scoped data) may run much faster than direct databases but they run 100s of time slower than in process caches. (which can be used for request, thread, transaction scoped data and also for session scoped data when session affinity without session failover is being maintained). 

	
  * Your cache needs to ensure that it does not mix up the scopes and end up returning incorrect data or end up corrupting the data

	
  * You may some times end up storing the data at a different scope than the scope of the data itself. You shall now need to design the strategies to handle the mismatch without introducing any inconsistencies for your users. A typical example is to store application scoped data in process scoped caches but design for inter cache communication to rapidly (near real time) update or invalidate the data across all the process caches.

	
  * Sometimes the same data might be scoped differently based on when it is being accessed. eg. Data in a transactional cache may reflect data that has not yet been committed. In such situations you may want to have a tiered cache eg. the transactional cache transparently consulting the process / application cache for data that has not been modified during the transaction, and updating the process / application cache after the data is committed if necessary.




#### Deployment Configuration



It is important to understand exactly how the various nodes / processes / threads will get used. The implications for cache design are as follows :

**Single Node / Multi Node** : Not all applications are intended to be multi node / clustered. A single node application can help for a much simpler caching strategy since you no longer need to worry about synchronising caches across multiple machines. If you intend to have 1 active + 1 passive node, consider it a single node, but design for warming up the cache for the passive node when it takes over.
**Single process / Multi Process** : One of the most important things that you need to understand if decided, and decide if undecided is whether your application is going to be a single process per node or multi process per node. Single process is easier from a caching perspective because the cache can be more easily built into your process. 



	
  * **single process + single node :** its the simplest scenario to design caching for. You do not need to worry about multi node caches or inter process communication - build the entire cache into your process. 


	
  * **single process + multiple nodes : **you know you will not have to worry about inter process communication but will need to worry about ensuring data consistency for application scoped data across all the nodes.


	
  * **multiple process + multiple nodes :** this is clearly where you need to tread carefully. You will need to decide whether you are going to use inter process communication for caching data which is process scoped and global distributed caches for caching data which is application scoped. You may choose to use a distributed cache for both types of data just to keep things simple.



**Single / Multi Threaded :**Most web applications unless they are explicitly dealing with threading may not have to worry about this much - but if you are having thread scoped data you will need to design for caches which are either scoped at a thread level or use process level caches with some kind of indexing based on the thread id for thread level data.

  



#### Runtime Environment



The language and the runtime environment can substantially influence and constrain the choices you have from a caching perspective. CGI based / shared nothing environments (perl / php ??) may not be easily be able to support process level caches. Python with WSGI can support process level caches and can support multiple threads per process, however features such as the Global Interpreter Lock may make it difficult to run more than a handful threads per process and may not allow you to easily leverage multi core CPUs without breaking out into multiple processes. Java on the other hand makes it easy to run a large number of threads in one process but will require you to pay greater attention to synchronisation, and the cost of serialising Java objects over the network can be rather high compared to the speed of Java runtime processing.




#### Data currency requirements



Sometimes its OK to show an incoming RSS feed which is 5 minutes old and not check for any latest updates (that data will catch up eventually). However if you are feeding the ATM with the current account balance you might need to be absolutely accurate to the second. If you need absolutely current data, remember you are going to have to deal with updating or invalidating the caches each time a cached record is updated. Moreover you are going to have to worry about issues like the small time window between the database record getting committed and the cache getting updated / invalidated. The more liberal the consistency requirements (ie. the easier it is to show slightly old data), the easier it is going to be on you, your cache design, and your caches.

In general if you need very high data currency, caching for a single process/single node is relatively easy but can get quite difficult as you move towards a multiple process / multiple nodes scenario. 



#### Read / Write profiles



Pay careful attention to these. If some part of your data deals predominantly with writes only (eg. logging data) or predominantly writes and v. few reads (eg. audit data) - theres no point to cache it anyway. If most of the traffic is going to be reads, thats great news from a caching perspective. Predominant reads with acceptable slightly stale data is great since you will be able to easily cache the data, get good hit ratios and be able to expire the data based on predefined feasible time windows. The higher the proportion of writes, the sooner is your cached data likely to be invalid, the the higher shall be the overhead to update the caches upon data being written to your application. 



#### Transactional Synchronisation



Sometimes you may want near transactional synchronisation between your database writes and the caches. If you need complete 100% transactional integrity, you might as well not use caching (actually you can still use transactionally scoped caches), since your cache will grind down to the same speed as the database, and its better to rely on your database's built in caches. Points of failure issues become important here - what if the database commit succeeds but the cache update fails because that particular network call to the cache timed out due to network overload. 

**Update :**While commenting on this post, Cameron Purdy [writes](http://www.artima.com/forums/flat.jsp?forum=270&thread=236168#311021)

> Unfortunately, the article focuses on what the "state of the art" was about seven years ago. Distributed caching today provides a far more elegant set of possible solutions than just dirty caches with remote access and invalidation. Transactionally consistent read/write in-memory caching technology across scores of servers is the norm for high-scale TP systems being built in Java and .NET today, and the same technology is used for many of the types of web applications that this article was focused on.

 Cameron makes a valid point and there exist caching tools which can help establish transactional [coherence](http://www.oracle.com/technology/products/coherence/index.html) under distributed environments. State of the art wasn't something I was so implicitly focused on as much as the options that are inexpensively available (read LGPL / ASF). However it would be appropriate to acknowledge that distributed transactional caches do exist and should be evaluated as an option and I missed making a mention of that.  Other caching tools such as  [JBossCache](http://www.jboss.org/jbosscache/) also have respectable capabilities in terms of clustering and transactional management. 



#### Data Hotspots



There are sometimes some data elements that act like hotspots. Imagine a request counter per user (not per session) in an environment where a user can have multiple concurrent sessions going on in a clustered environment. Now imagine that there is a limit per user per day which cannot get crossed at all (not even one extra request should get serviced). The cache that maintains the requests per user may end up being a major hotspot from a data access perspective. This scenario I've made up, but I had to face a similar one where the counter thresholds were completely rigid and non negotiable. 



#### Processing Granularity



It is quite important to understand whether you are going to be processing one request at a time (eg. web based) or are going to be processing data in bulk (eg through large batch files). While one request at a time performance can be substantially improved through caching, the improvements can be much more dramatic with bulk processing, since transaction scoped caches end up having much higher hit rates. 



### _Summary_


I hope I have been able to convey that it is absolutely important to have a strong understanding of your target application architecture, its performance requirements and the data and data flow within the application before getting into designing the appropriate caching strategy. I also hope it is evident that there is no one size fits all caching strategy, and that the strategy you choose will need to based at least on some of the dimensions described here.

I have deliberately kept out of scope characterisation and comparison of caching tools (eg. memcached, ehcache) or caching media (memory, files, database etc.) since that would substantially increase the complexity and length of this post.  
 


### Some thoughts around Cache Design and Architecture



I have developed some preferences and thoughts with regards to caching over a period of time. These may not be universally followed, but I thought it would be useful to document these FWIW. 




	
  * **Make one node work as fast as it can before exercising the option to scale out : ** 
Most applications do not need more than one node to deliver their performance targets. Do not use more than one node unless you really need to.

	
  * **Build a clean caching API and then implement the least expensive caching strategy required. Plan on scaling up the caching sophistication over a period of time ** There are a number of stories around which refer to how some applications suddenly had to respond to rapidly increasing scalability demands and couldn't handle them. If I was to guess, for every application that had to deal with completely unexpected growth and scaling demands, there are at least 10-50 others which were not equally fortunate enough to face that problem. However dealing with this has a cost, and unless you are sufficiently sure you are going to need to scale big time, you may not want to pay that cost upfront. One way to manage this is to have a clean API through which you will always access data (or business objects). Now if you need to scale much more and need to work with more sophisticated caching strategies (eg. move from an in process cache to an interprocess cache to a distributed cache), you can focus on changing the implementation of that API rather than having to make tons of changes all over your application while reducing the initial time and cost of getting the application to the market.

	
  * **Caches need not always be coarse grained :**Often caches are quite coarse grained (eg. user profiles, html snippets etc.). However they need not always be. Sometimes caching results of operations (say just a boolean value) which may otherwise be executed multiple times can offer dramatically superior performance.

	
  * **Serialisation, Interprocess communication and Network transfers required for caching can be expensive in extreme circumstances :**Moving data into and out of a cache especially an interprocess cache or a distributed cache can sometimes be very very expensive. There may be some data hotspots in your application, where you may not even want to convert the data structures or serialise them (especially for intensely accessed data stored in complex data structures). In such situations you may want to completely hand craft the cache rather than using any tool and minimise the network IO (Invalidation is preferable to Replication here, since you only need to move the primary key over the network rather than the entire object) . While such situations are not quite frequent, I have used this on at least one occasion with compelling results. 

	
  * **Caching is no excuse for poor database optimisations : ** Caching is meant to deliver performance improvements beyond those that can be offered by a well tuned database and query design. It cannot be a substitute for poor database optimisations and on occasions a poor query or lack of appropriate indices can more than overcome the benefits introduced by caching.





###  References 


Some useful references from a cache performance perspective are : [Cache Performance Comparison](http://www.mysqlperformanceblog.com/2006/08/09/cache-performance-comparison/), [Comparing memcached and ehcache performance](http://gregluck.com/blog/archives/2007/05/comparing_memca.html) (This is not a strictly fair comparison - whereas  memcached caches the application scoped data synchronously over the network, ehcache is storing the data in process scope and then using asynchronous replication).  Another useful page discussing the various aspects of distributed caching is [The design of a distributed ehcache](http://ehcache.sourceforge.net/documentation/distributed_design.html).

