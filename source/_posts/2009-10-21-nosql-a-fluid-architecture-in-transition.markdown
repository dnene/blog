---
date: '2009-10-21 23:51:14'
layout: post
slug: nosql-a-fluid-architecture-in-transition
status: publish
title: NoSQL - A fluid architecture in transition
wordpress_id: '867'
categories:
- architecture
tags:
- couchdb
- lightcloud
- mongodb
- nosql
- riak
- tokyotyrant
---

Lot of talk about NoSQL. Much of it well deserved. And while lot of the excitement around it is well understood by those in the know, some of it may actually be confusing to those who are relatively new to the matter. This post is actually for the latter group - not to argue for or against NoSQL - just to put it in perspective.

**What is NoSQL : **Some of the characteristics shared by most if not all the NoSQL engines are as follows :




	
  1. **Schemaless or Hierarchical Schema Storage** NoSQL assumes at its very basis a schemaless or a hierarchichal schema storage system. In most cases this consists of a simple key value pair storage. While some storage engines excel at storing small values (LightCloud/Tokyo Cabinet), some are strong at storing large documents (CouchDB). 

	
  2. **Distributed storage :** This is one of the driving forces of NoSQL growth, though not a distinguishing characteristic of NoSQL. One of the areas these storage systems separate themselves from RDBMS's is their ability to allow better horizontal scalability. This varies from the simple master-master replication for MongoDB, to multi node sharding using consistent hashing with LightCloud (a la memcached) to a multiple master eventual consistency model of Riak. The basic premise in using some of the NoSQL engines is that storage will scale horizontally.

	
  3. **Support for deferred processing :**Many of these engines allow for some degree of deferred processing. Whether this be simple lua scripting in case of LightCloud or map-reduce scripts in case of CouchDB, the general assumption is that some amount of latency in computation times is acceptable, and some of the computations (especially related to analytics based views) will be performed post storage.

	
  4. **Eventual Consistency :** This may seem like a necessary feature of all NoSQL storage systems but it isn't. While clearly some such as CouchDB (in terms of its map-reduce views) and Riak are better placed for supporting and implementing eventual consistency, it is quite feasible to use others such as LightCloud or MongoDB to implement immediate consistency using a single master-master pair. Suffice to note that eventual consistency is not a necessary side effect of using a NoSQL storage system, though it wouldn't be incompatible for the two to work together. 



But the points I would really like to emphasize are :


	
  * **NoSQL is not a direct competitor to RDBMS/SQL :** It is actually a solution to many use cases where using RDBMS was perhaps a poor fit. Thus the decision for an architect is not which of the two competing options (RDBMS or NoSQL) _Update: <del>one should select</del>should be the preferred standard storage strategy_, - it simply is which one is the more appropriate storage system for the application under consideration.

	
  * **NoSQL is still at a fluid stage of its development : ** All the NoSQL storage systems (but for LightCloud/Tokyo Tyrant) are still being quite actively developed. These have not reached v 1.0 _(Update: MongoDB is at v 1.1)_ and it is likely that some time will pass before any of these get beyond the beta and release candidate stage and get a 1.0 in-production stamp. While there is a lot of interest, there still is a substantial amount of experimentation in terms of the right feature sets leading to differently focused developments in different storage systems. To an architect this represents an interesting challenge. I think the way to approach this right now is to not use these in mission critical (eg. life or health impacting) systems, and to focus on reasonable expectation management in terms of ensuring the right kind of SLAs around their availability (simply because many of these haven't yet been put to intense use in production the way say an Oracle or MySQL have been). This is not an attempt to spread "FUD" about NoSQL - far from it it is an exercise in setting appropriate expectations. i would also recommend that it would be appropriate to evaluate the available NoSQL choices only when reasonable SLAs can be worked out for their usage. It is certainly preferred to using NoSQLs rather than using RDBMS's in an inappropriate manner (large objects serialised into BLOBs or into name-value pair tables). However, I would suggest that you do not deeply bind yourself into a particular NoSQL engine. The future development of most of the storage systems is still unknown to a certain extent, as is the future landscape including any shakeouts. Should one recommend usage of a NoSQL engine - it is important to have a clear plan for switching over to an alternative engine should a need arise in the future. While this is easier said than done, deciding the appropriate level of abstraction to use (ie. code to the API directly vs. use a layer of abstraction for engine independence) is best left to designer / architect to dwell upon.





